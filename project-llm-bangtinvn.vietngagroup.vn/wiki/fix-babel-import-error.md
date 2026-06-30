# Fix Babel Import Error — Xử Lý Lỗi Babel/React Tự Sinh Import Trên Trình Duyệt

**Tóm tắt**: Hướng dẫn khắc phục lỗi `Cannot use import statement outside a module` khi chạy React trực tiếp trên trình duyệt qua CDN + Babel Standalone. Nguyên nhân do Babel tự sinh lệnh `import` trong chế độ `runtime: "automatic"`.

**Nguồn**: `fix-babel-import-error.md`

**Cập nhật lần cuối**: 2026-06-30

---

## Mô tả sự cố

Khi thiết lập dự án React chạy trực tiếp trên trình duyệt (không qua Webpack, Vite) bằng thẻ `<script>` tải từ CDN, ứng dụng bị trắng trang và Console báo lỗi:

> `Uncaught SyntaxError: Failed to execute 'appendChild' on 'Node': Cannot use import statement outside a module`

**Môi trường xảy ra**:
- React, ReactDOM (bản UMD) qua CDN
- `@babel/standalone` qua CDN để biên dịch JSX bằng `<script type="text/babel">`

---

## Nguyên nhân gốc rễ

1. **Sử dụng sai `type="module"`**: Gán `type="module"` vào thẻ script tải thư viện UMD khiến Babel Standalone bị cô lập, không hoạt động được.
2. **Babel tự chèn lệnh Import (nguyên nhân chính)**: Các phiên bản mới của `@babel/standalone` mặc định dùng `runtime: "automatic"` — chuẩn này tự sinh `import { jsx } from 'react/jsx-runtime'`. Trình duyệt không hiểu bare import → lỗi cú pháp.

---

## Giải pháp

### Bước 1: Cấu hình lại thẻ CDN trong `<head>`

```html
<!-- 1. React & ReactDOM — KHÔNG DÙNG type="module" -->
<script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>

<!-- 2. Khóa phiên bản Babel Standalone 7.23.9 -->
<script src="https://unpkg.com/@babel/standalone@7.23.9/babel.min.js"></script>

<!-- 3. Ép Babel chạy Classic mode (không tự sinh import) -->
<script>
    if (window.Babel) {
        window.Babel.registerPreset('react-classic', {
            presets: [
                [window.Babel.availablePresets.react, { runtime: "classic" }]
            ]
        });
    }
</script>
```

### Bước 2: Cập nhật thẻ script chứa mã React

Thêm `data-presets="env, react-classic"` vào thẻ `<script type="text/babel">`:

```html
<script type="text/babel" data-presets="env, react-classic">
    const { useState, useEffect } = React;
    
    const App = () => {
        return <div>Hệ thống đã hoạt động bình thường!</div>;
    };
    
    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(<App />);
</script>
```

### Bước 3: Xóa cache trình duyệt (bắt buộc)

- **Windows/Linux**: `Ctrl + F5` hoặc `Ctrl + Shift + R`
- **macOS**: `Cmd + Shift + R`
- **Thiết bị nhúng/TV**: Dọn cache trình duyệt hoặc khởi động lại thiết bị

---

## Các trang liên quan

<!-- Không có trang liên quan trong project này -->
