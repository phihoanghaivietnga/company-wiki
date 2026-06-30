# TÀI LIÊU XỬ LÝ LỖI: BABEL / REACT TỰ SINH IMPORT TRÊN TRÌNH DUYỆT

## 1. MÔ TẢ SỰ CỐ

Khi thiết lập một dự án React chạy trực tiếp trên trình duyệt (không thông qua các công cụ build như Webpack, Vite) bằng cách sử dụng thẻ `<script>` tải từ CDN, ứng dụng bị trắng trang và Console báo lỗi.

**Thông báo lỗi trên Console:**

> `Uncaught SyntaxError: Failed to execute 'appendChild' on 'Node': Cannot use import statement outside a module`
> File báo lỗi: `transformScriptTags.ts`

**Môi trường xảy ra lỗi:**

* Sử dụng React, ReactDOM (bản UMD) qua CDN.
* Sử dụng `@babel/standalone` qua CDN để biên dịch mã JSX trực tiếp bằng thẻ `<script type="text/babel">`.

---

## 2. NGUYÊN NHÂN GỐC RỄ

Sự cố này thường bắt nguồn từ hai nguyên nhân kết hợp:

1. **Sử dụng sai thuộc tính `type="module"`:** Việc gán `type="module"` vào các thẻ script tải thư viện dạng UMD (Universal Module Definition) khiến trình duyệt cô lập phạm vi của chúng, làm Babel Standalone không thể hoạt động bình thường.
2. **Babel tự động chèn lệnh Import (Lý do chính):** Các phiên bản mới của `@babel/standalone` mặc định sử dụng chuẩn biên dịch JSX mới (`runtime: "automatic"`). Chuẩn này sẽ tự động sinh ra dòng lệnh `import { jsx } from 'react/jsx-runtime'` và lén lút chèn vào mã của bạn. Vì trình duyệt không thể hiểu được lệnh `import` trần (bare import) này trong môi trường script thông thường, nó sẽ ném ra lỗi cú pháp.

---

## 3. GIẢI PHÁP KHẮC PHỤC DỨT ĐIỂM

Để sửa lỗi, chúng ta cần loại bỏ `type="module"`, khóa phiên bản Babel để đảm bảo tính ổn định và ép Babel dịch JSX theo kiểu cũ (Classic mode) để ngăn chặn việc tự sinh lệnh `import`.

### Bước 1: Cấu hình lại các thẻ CDN thư viện trong `<head>`

Thay thế cụm thẻ script cũ bằng đoạn mã sau:

```html
<!-- 1. Tải React & ReactDOM (KHÔNG DÙNG type="module") -->
<script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>

<!-- 2. Khóa phiên bản Babel Standalone (VD: 7.23.9) để tránh bị lỗi khi thư viện update -->
<script src="https://unpkg.com/@babel/standalone@7.23.9/babel.min.js"></script>

<!-- 3. Ép Babel chạy ở chế độ Classic (Không tự sinh ra lệnh import) -->
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

### Bước 2: Cập nhật thẻ script chứa mã nguồn React

Tìm đến thẻ `<script type="text/babel">` chứa logic ứng dụng của bạn và bổ sung thuộc tính `data-presets` để gọi cấu hình `react-classic` vừa tạo ở Bước 1.

```html
<!-- Bổ sung thuộc tính data-presets="env, react-classic" -->
<script type="text/babel" data-presets="env, react-classic">
    
    const { useState, useEffect } = React;
    
    const App = () => {
        return <div>Hệ thống đã hoạt động bình thường!</div>;
    };
    
    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(<App />);
    
</script>

```

### Bước 3: Xóa Cache (Rất Quan Trọng)

Trình duyệt thường lưu bộ nhớ đệm (cache) rất kỹ. Sau khi sửa file HTML, bạn **bắt buộc** phải xóa cache để trình duyệt nhận diện đoạn script cấu hình mới.

* **Trên Windows / Linux:** Bấm `Ctrl + F5` hoặc `Ctrl + Shift + R`.
* **Trên macOS:** Bấm `Cmd + Shift + R`.
* **Trên Tivi/Thiết bị nhúng:** Dọn dẹp bộ nhớ/cache của trình duyệt tích hợp hoặc khởi động lại thiết bị (rút nguồn cắm lại nếu cần thiết).