# Hướng Dẫn Auto Runner - Chạy Tự Động

**Tóm tắt**: Auto Runner là engine tự động điều phối chạy ETL theo lịch (crontab/scheduler), tuần tự từng facility, nạp Dimension rồi Fact mà không cần can thiệp thủ công. Thích hợp cho vận hành hằng ngày (Production).

**Nguồn**: `raw/MASTER_KNOWLEDGE_BASE.md` (GEM_AUTO_PIPELINE.md, orchestrator-dong-bo section)

**Cập nhật lần cuối**: 2026-05-22

---

## 1. Kiến Trúc Auto Runner

```
┌─────────────────────────────────────────┐
│  GitHub Actions / Cron Scheduler        │
│  (Chạy lúc 02:00 AM mỗi ngày)          │
└──────────┬──────────────────────────────┘
           │
           ↓
┌─────────────────────────────────────────┐
│  SyncOrchestrator.run()                 │
│  (Điều phối tuần tự)                    │
└──────────┬──────────────────────────────┘
           │
           ├─→ [Facility 1: hanoi]
           │   ├─→ DimensionLoader
           │   │   ├─ Prod → ODS
           │   │   └─ ODS → Datamart
           │   └─→ FactLoader
           │       ├─ Prod → Landing
           │       ├─ Landing → ODS
           │       └─ ODS → Datamart
           │
           ├─→ [Facility 2: hcm]
           │   ├─→ DimensionLoader
           │   └─→ FactLoader
           │
           └─→ [Facility 3: halong]
               ├─→ DimensionLoader
               └─→ FactLoader
```

(nguồn: GEM_AUTO_PIPELINE.md)

---

## 2. Cấu Hình Lịch Chạy

### Phương Pháp 1: GitHub Actions (Khuyên Dùng)

**File**: `.github/workflows/sync_to_drive.yml`

```yaml
name: Auto ETL Sync

on:
  schedule:
    # Chạy lúc 02:00 AM UTC mỗi ngày
    # (tương đương 09:00 AM +7 Vietnam)
    - cron: '0 2 * * *'
  
  workflow_dispatch:  # Cho phép chạy thủ công qua GitHub UI

jobs:
  sync:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v2
      
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.11'
      
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
      
      - name: Run ETL Orchestrator
        env:
          PROD_CONNECTION_HANOI: ${{ secrets.PROD_CONNECTION_HANOI }}
          PROD_CONNECTION_HCM: ${{ secrets.PROD_CONNECTION_HCM }}
          DATAMART_CONNECTION_STRING: ${{ secrets.DATAMART_CONNECTION_STRING }}
        run: |
          python -m src.jobs.sync_orchestrator
      
      - name: Sync to Google Drive (if successful)
        run: |
          python scripts/upload_to_drive_from_local.py
```

**Lợi Ích**:
- ✅ Chạy trên cloud (không cần server local)
- ✅ Log lưu trữ trong GitHub
- ✅ Gửi email thông báo lỗi tự động

(nguồn: .github/workflows)

### Phương Pháp 2: Cron Local Server

**Trên Linux/Ubuntu**:
```bash
# Chỉnh sửa crontab
crontab -e

# Thêm dòng: Chạy lúc 02:00 AM mỗi ngày
0 2 * * * cd /home/etl_user/etl_nano_v2 && /usr/bin/python -m src.jobs.sync_orchestrator >> /var/log/etl_sync.log 2>&1
```

**Trên Windows (Task Scheduler)**:
1. Mở Task Scheduler
2. Tạo task mới:
   - Trigger: Daily 02:00 AM
   - Action: `python -m src.jobs.sync_orchestrator`
   - Working dir: `C:\etl_nano_v2`

(nguồn: GEM_AUTO_PIPELINE.md)

---

## 3. Cấu Hình Facility Chạy

### YAML: config/tables.yaml

```yaml
etl_settings:
  odbc_chunk_size: 1000
  active_facilities:
    - hanoi      # Chạy cơ sở Hà Nội
    - hcm        # Chạy cơ sở TP.HCM
    # - halong   # Comment ra nếu không chạy

facilities:
  hanoi:
    co_so_key: 1
    nguon_du_lieu_key: 10
    ma_co_so: "HN"
    staging_schema: "hanoi_hisnano_v2"
  
  hcm:
    co_so_key: 2
    nguon_du_lieu_key: 20
    ma_co_so: "HCM"
    staging_schema: "hcm_hisnano_v2"
```

**Cách Thay Đổi Facility Chạy**:

1. Muốn bỏ cơ sở Hà Nội: Comment dòng `- hanoi`
2. Muốn thêm cơ sở mới: Thêm dòng + tạo `facilities` entry
3. Save `.yml` → Orchestrator sẽ đọc facility mới lần chạy tiếp theo

(nguồn: [[quan-ly-he-thong]])

---

## 4. Luồng Chạy Chi Tiết

### Thực Tế Từng Facility

```python
# Pseudocode
active_facilities = config['etl_settings']['active_facilities']  # ['hanoi', 'hcm']

for facility in active_facilities:
    try:
        log(f"[START] {facility}")
        
        # Bước 1: Nạp Dimension
        dimension_loader = DimensionLoader(facility, config)
        dimension_loader.execute_load()
        log(f"[SUCCESS] Dimension {facility}")
        
        # Bước 2: Nạp Fact
        fact_loader = FactLoader(facility, config)
        fact_loader.execute_load()
        log(f"[SUCCESS] Fact {facility}")
        
        # Bước 3: Dọn Landing (automatic trong Fact Loader finally)
        log(f"[COMPLETE] {facility}")
        
    except Exception as e:
        log(f"[ERROR] {facility}: {str(e)}")
        raise  # Dừng ngay, không tiếp tục facility kế tiếp
```

(nguồn: [[orchestrator-dong-bo]] - ADR-06)

### Ví Dụ Log Output

```
2026-05-22 02:00:00.000 [orchestrator] [START] Điều phối 2 facility
2026-05-22 02:00:01.123 [hanoi] [DimensionLoader] [START] Nạp Dimension
2026-05-22 02:00:05.456 [hanoi] [DimensionLoader] [INFO] Nạp DimBenhNhan (150k rows)
2026-05-22 02:00:10.789 [hanoi] [DimensionLoader] [SUCCESS] MERGE Datamart
2026-05-22 02:00:12.000 [hanoi] [FactLoader] [START] Nạp Fact
2026-05-22 02:00:15.123 [hanoi] [FactLoader] [INFO] ThuPhiDichVu (10k rows)
2026-05-22 02:00:18.456 [hanoi] [FactLoader] [INFO] ThuPhiBaoHiem (8k rows)
2026-05-22 02:00:21.789 [hanoi] [FactLoader] [INFO] ThuPhiTangGiam (2k rows)
2026-05-22 02:00:25.000 [hanoi] [FactLoader] [SUCCESS] MERGE Datamart
2026-05-22 02:00:30.000 [hcm] [DimensionLoader] [START] Nạp Dimension
...
2026-05-22 02:15:00.000 [orchestrator] [SUCCESS] Hoàn tất 2 facility
2026-05-22 02:15:05.000 [orchestrator] [START] Sync to Google Drive
2026-05-22 02:20:00.000 [orchestrator] [SUCCESS] Sync Drive complete
```

(nguồn: GEM_AUTO_PIPELINE.md)

---

## 5. Giám Sát & Alert

### Kết Quả Thành Công

**Dấu Hiệu OK**:
```
✓ Log cuối: "[orchestrator] [SUCCESS] Hoàn tất X facility"
✓ Thời gian chạy: < 30 phút (nếu có 4 facility + Dimension full + Fact incremental)
✓ Không có [ERROR] hay [FAILED]
```

### Kết Quả Lỗi

**Dấu Hiệu Lỗi**:
```
✗ Log cuối: "[ERROR] XXX: [Message]"
✗ Thời gian chạy: > 60 phút hoặc timeout
✗ Một số facility thành công, một số lỗi (bất thường)
```

### Cách Nhận Thông Báo

**GitHub Actions**:
- Actions page: https://github.com/[org]/[repo]/actions
- Tìm workflow run lỗi → View details
- GitHub có thể gửi email nếu cấu hình

**Email Alert (Tùy Chọn)**:
```python
# Thêm vào orchestrator
import smtplib

try:
    orchestrator.run()
except Exception as e:
    # Gửi email alert
    send_email(
        to='admin@hospital.vn',
        subject=f'ETL Error: {str(e)[:50]}',
        body=f'Orchestrator failed:\n{traceback.format_exc()}'
    )
    raise
```

(nguồn: GEM_AUTO_PIPELINE.md)

---

## 6. Tần Suất Chạy & Quy Hoạch

### Quy Hoạch Hàng Ngày

```
00:00 (Nửa đêm)
├─ 02:00 AM: ▶ Auto ETL Sync (start)
├─ 02:30 AM: ✓ Hoàn tất (hầu hết case)
└─ 03:00 AM: ✓ Upload Google Drive

09:00 AM
├─ Manual kiểm tra báo cáo Datamart
└─ Nếu cần: Manual Runner chạy Fact cụ thể

15:00 (3 PM)
└─ (Optional) Manual kiểm tra tài chính

21:00 (9 PM)
└─ Theo dõi: Log file `etl_sync.log` (nếu có lỗi)
```

(nguồn: GEM_AUTO_PIPELINE.md)

### Nếu Cần Thay Đổi Lịch

```yaml
# Tham khảo cron expression
# ┌───────────── minute (0-59)
# │ ┌───────────── hour (0-23)
# │ │ ┌───────────── day of month (1-31)
# │ │ │ ┌───────────── month (1-12)
# │ │ │ │ ┌───────────── day of week (0-6, Sunday=0)
# │ │ │ │ │

# Chạy lúc 03:00 AM
  - cron: '0 3 * * *'

# Chạy lúc 06:00 PM (18:00)
  - cron: '0 18 * * *'

# Chạy mỗi 6 tiếng một lần
  - cron: '0 */6 * * *'

# Chạy thứ 2 đến thứ 6 (Mon-Fri) lúc 02:00 AM
  - cron: '0 2 * * 1-5'
```

(nguồn: GitHub Actions cron syntax)

---

## 7. Vận Hành & Bảo Trì

### Kiểm Tra Hàng Ngày

| Thời Gian | Hành Động | Ghi Chú |
|---|---|---|
| 08:00 AM | Check log `etl_sync.log` | Xem có lỗi không |
| 09:00 AM | Mở Datamart query tool | Kiểm tra dữ liệu mới |
| 14:00 | (Optional) Review báo cáo | Đối chiếu số liệu |

### Troubleshooting

**Nếu Auto Runner Lỗi Hôm Nay**:

1. Kiểm tra log: `tail -f /var/log/etl_sync.log` (Linux) hoặc GitHub Actions
2. Xác định facility/loader bị lỗi
3. Dùng [[huong-dan-manual-runner]] chạy lại bảng cụ thể
4. Sau khi OK → chạy lại toàn bộ facility

(nguồn: [[loi-va-khac-phuc]])

**Nếu Cần Dừng Auto Runner Tạm Thời**:

```yaml
# Trong config/tables.yaml
etl_settings:
  active_facilities: []  # Trống = không chạy facility nào
```

Hoặc:
```bash
# GitHub Actions: Disable workflow từ Actions tab
```

---

## 8. Performance Tuning

### Nếu Chạy Quá Lâu (> 45 phút)

**Kiểm Tra**:
- Production DB có lag không? (Query từ Management Studio)
- Network stable không?
- Chunk size có phù hợp không?

**Tối Ưu**:
```yaml
# Tăng chunk size (nếu RAM đủ)
etl_settings:
  odbc_chunk_size: 2000  # Từ 1000 → 2000
```

**Hoặc**: Giảm số facility chạy hôm nay, chạy facility khác hôm sau

(nguồn: [[quan-ly-he-thong]] - chunk_size)

### Nếu Muốn Chạy Nhanh Hơn

```yaml
# Thêm 2 cron jobs: 1 cho Dimension, 1 cho Fact
# Dimension lúc 02:00, Fact lúc 03:00
  - cron: '0 2 * * *'  # Dimension only
  - cron: '0 3 * * *'  # Fact only
```

Nhưng cần code để hỗ trợ chế độ này (ADR hiện tại chưa support)

(nguồn: GEM_AUTO_PIPELINE.md - Future improvement)

---

## 9. Disaster Recovery

### Nếu Database Crash

```
1. Restart Production DB
2. Chờ 5 phút để stabilize
3. Auto Runner sẽ chạy lại lúc 02:00 AM
4. Hoặc: Manual trigger GitHub Action ngay
```

### Nếu Cần Rollback Dữ Liệu

```sql
-- Restore Datamart từ backup
RESTORE DATABASE Datamart FROM DISK = 'backup_2026_05_21.bak'

-- Rồi chạy lại Auto Runner
```

---

## 10. Tóm Tắt

| Khía Cạnh | Chi Tiết |
|---|---|
| **Lịch Chạy** | 02:00 AM mỗi ngày (crontab/GitHub Actions) |
| **Thời Gian** | 15-30 phút (tuỳ facility + tốc độ DB) |
| **Facility** | Cấu hình qua `config/tables.yaml` |
| **Log** | Lưu file `/var/log/etl_sync.log` (Linux) |
| **Alert** | Email alert nếu cấu hình, hoặc check GitHub Actions |
| **Troubleshoot** | Dùng Manual Runner để debug |

---

## Các Trang Liên Quan

- [[orchestrator-dong-bo]] - Chi tiết SyncOrchestrator
- [[huong-dan-manual-runner]] - Chạy thủ công
- [[quan-ly-he-thong]] - Cấu hình YAML
- [[loi-va-khac-phuc]] - Debug & troubleshoot
