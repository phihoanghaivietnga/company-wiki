# Kiến Trúc Đa Tầng (Multi-hop)

**Tóm tắt**: Hệ thống vận hành theo kiến trúc 3 chặng cách ly nghiêm ngặt: Production HIS → Global Staging → Facility ODS → Datamart. Mỗi chặng có vai trò, quy tắc cách ly và cơ chế xử lý dữ liệu riêng biệt để đảm bảo an toàn bộ nhớ và tính toàn vẹn tài chính.

**Nguồn**: `raw/MASTER_KNOWLEDGE_BASE.md` (GEM_DATA_FLOW.md section)

**Cập nhật lần cuối**: 2026-05-22

---

## 1. Tổng Quan Kiến Trúc

```
┌─────────────────┐      ┌──────────────────┐      ┌─────────────┐      ┌──────────┐
│ Production HIS  │─────>│ Global Staging   │─────>│Facility ODS │─────>│Datamart  │
│   (Prod DB)     │      │  (stg_nano_v2)   │      │ (hanoi_v2)  │      │ (DM)     │
└─────────────────┘      └──────────────────┘      └─────────────┘      └──────────┘
     Tầng 1                   Tầng 2                    Tầng 3             Tầng 4
    CHỈ ĐỌC            Vùng đệm tạm thời        Lịch sử bền vững        Báo cáo
   (SELECT)           Multi-tenant shared       Per-facility ODS        Phân tích
```

(nguồn: GEM_DATA_FLOW.md)

## 2. Chi Tiết Từng Tầng

### Tầng 1: Production HIS (SQL Server)

**Bản chất**: Nguồn dữ liệu tác nghiệp gốc của bệnh viện. Chứa bảng Dimension (danh mục) và Fact (phát sinh).

**Quân luật**:
- ✅ CHỈ ĐƯỢC: `SELECT`, `BCP OUT` (trích xuất dữ liệu)
- ❌ TUYỆT ĐỐI CẤM: `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`, `ALTER`, `DROP`

**Cơ chế Trích Xuất**:
- Dùng `BaseExtractor` sinh câu `SELECT` động
- Chỉ bốc các cột cần thiết theo `selected_columns` (Whitelist)
- Loại bỏ hoàn toàn cột bẩn hoặc chứa dữ liệu phi cấu trúc lớn
- Giảm tải RAM từ bước đầu tiên

(nguồn: GEM_DATA_FLOW.md)

### Tầng 2: Global Staging / Landing (`stg_nano_v2`)

**Bản chất**: Vùng đệm thô tạm thời, **chia sẻ chung** cho tất cả cơ sở (multi-tenant shared staging). Là vùng trung gian giữa Production và ODS facility riêng lẻ.

**Quân luật**:
- Bắt buộc **`TRUNCATE TABLE`** ngay đầu luồng mỗi lần nạp dữ liệu cơ sở mới
- Mục tiêu: **Chống nhiễm độc** dữ liệu và **rò rỉ dữ liệu chéo** giữa các cơ sở
- Dọn sạch Landing ở cuối luồng để không để lại dữ liệu tạm

**Cơ chế Nạp**:
- Dimension: Load dữ liệu thô từ Prod via [[dimension-loader]]
- Fact: Load dữ liệu incremental từ Prod via [[fact-loader]]

(nguồn: GEM_DATA_FLOW.md)

### Tầng 3: Facility ODS - Operational Data Store

**Bản chất**: Kho lưu trữ lịch sử tác nghiệp **bền vững** của **riêng từng cơ sở**. Cấu trúc bảng mirror 1:1 với Production nhưng bổ sung cột định danh hệ thống.

**Ví dụ ODS Schema**:
```
hanoi_hisnano_v2      (Cơ sở Hà Nội)
hcm_hisnano_v2        (Cơ sở TP.HCM)
halong_hisnano_v2     (Cơ sở Hạ Long)
haiphong_hisnano_v2   (Cơ sở Hải Phòng)
```

**Cột Bổ Sung (Tenant Injection)**:
- `NguonDuLieuKey`: Định danh nguồn dữ liệu (Source ID)
- `CoSoKey`: Định danh cơ sở (Facility ID)
- `MaCoSo`: Mã cơ sở dạng text (Facility Code)

**Quân luật**:
- ❌ TUYỆT ĐỐI CẤM dùng `TRUNCATE` tại ODS
- ✅ CHỈ ĐƯỢC cập nhật dữ liệu bằng `UPSERT` (MERGE) hoặc INSERT
- ✅ Hard delete **chỉ trong cửa sổ D-3** (3 ngày gần nhất) với điều kiện cô lập cơ sở

**Cơ chế Cập Nhật**:
- Dimension: Full MERGE sau Tầng 2 (thay thế hoàn toàn)
- Fact: Incremental MERGE với hard delete an toàn

(nguồn: GEM_DATA_FLOW.md)

### Tầng 4: Datamart (DM)

**Bản chất**: Kho dữ liệu công khai cho báo cáo, phân tích, BI dashboard.

**Quân luật**:
- ✅ MERGE từ ODS
- ✅ Cô lập theo `NguonDuLieuKey` + `MaCoSo` trong điều kiện `ON` để không xóa nhầm dữ liệu cơ sở khác
- ✅ Hard delete theo cửa sổ D-3 nếu áp dụng

**Dữ Liệu Tài Chính**:
- Tất cả các bảng doanh thu (ThuPhiDichVu, ThuPhiBaoHiem, ThuPhiTangGiam) phải đi qua Tầng 3 (ODS) trước
- Datamart chỉ nhận dữ liệu từ ODS đã xác minh

(nguồn: GEM_DATA_FLOW.md)

## 3. Luồng Dữ Liệu Theo Loại Bảng

### Dimension (Danh Mục) - 2-Hop

```
Production
    ↓ (full load)
Global Staging (stg_nano_v2)
    ↓ (MERGE replace all)
ODS Facility (hanoi_hisnano_v2, etc.)
    ↓ (MERGE replace all)
Datamart
```

**Đặc điểm**: 
- Full-load toàn bộ dữ liệu mỗi lần (không incremental)
- 4 dimension chính: DimBenhNhan, DimBenh, DimLoaiGoiDichVu, DimDichVu
- Quản lý qua [[dimension-loader]]

(nguồn: GEM_CODE_MAP.md)

### Fact (Phát Sinh) - 3-Hop

```
Production (D-3 to today)
    ↓ (BCP out / ODBC bulk)
Global Staging (stg_nano_v2)
    ↓ (MERGE + hard delete trong D-3)
ODS Facility
    ↓ (MERGE + hard delete trong D-3, batch 10k rows)
Datamart
```

**Đặc điểm**:
- Incremental với cửa sổ trượt D-3 (3 ngày gần nhất)
- Dùng cột ngày động (ví dụ: `NgayDenKham`, `NgayThu`)
- Hard delete chỉ áp dụng **trong phạm vi D-3** để bảo vệ lịch sử
- Early arriving facts: fallback seed key `-1` cho dimension chưa đến
- Quản lý qua [[fact-loader]]

(nguồn: GEM_DATA_FLOW.md)

## 4. Chốt Chặn An Toàn

### 4.1 Bảo Vệ Production
- Production connection **chỉ dùng** cho `SELECT` / `BCP OUT`
- Mọi lệnh thay đổi dữ liệu chỉ chạy trên Staging/ODS/Datamart

(nguồn: agents.md)

### 4.2 Cô Lập Cơ Sở (Multi-tenant)
- Mỗi facility có ODS riêng
- Hard delete Datamart bắt buộc kiểm tra `NguonDuLieuKey` + `MaCoSo`
- Tránh xóa nhầm dữ liệu cơ sở khác

(nguồn: GEM_DATA_FLOW.md)

### 4.3 Bảo Vệ Lịch Sử Tài Chính
- Hard delete Fact chỉ được phép **trong 3 ngày gần nhất**
- Không được xóa dữ liệu cũ hơn 3 ngày
- Đảm bảo audit trail cho kiểm toán

(nguồn: PROJECT_CHRONICLE.md - ADR-08)

### 4.4 Dọn Landing
- `TRUNCATE stg_nano_v2.[TenBang]` ở **đầu** luồng (trước nạp)
- `TRUNCATE stg_nano_v2.[TenBang]` ở **cuối** luồng (trong finally block)
- Tránh rò rỉ dữ liệu giữa các phiên chạy

(nguồn: PROJECT_CHRONICLE.md - ADR-08)

---

## Các Trang Liên Quan

- [[dimension-loader]] - Chi tiết nạp full-load Dimension
- [[fact-loader]] - Chi tiết nạp incremental Fact
- [[orchestrator-dong-bo]] - Điều phối luồng 3 chặng
- [[chinh-sach-bao-mat-db]] - Quy tắc bảo mật chi tiết
