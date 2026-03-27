# Website Đăng ký Thông tin Khách hàng (Airtable Integration + DOCX Export)

Website đăng ký thông tin khách hàng sử dụng **GitHub Pages** + **Airtable** để lưu dữ liệu, đồng thời có thể **xuất mẫu biểu DOCX** ngay trên trình duyệt khi người dùng chọn **In mẫu biểu**.

## 🌐 LINK WEBSITE
- **Form đăng ký:** https://dodoanloc.github.io/regis/
- **Trang quản lý:** https://dodoanloc.github.io/regis/admin.html

---

## ✨ TÍNH NĂNG MỚI

### Form đăng ký (`index.html`)
- ✅ Nút **Đăng ký** để lưu dữ liệu như hiện tại
- ✅ Nút **In mẫu biểu** cạnh nút Đăng ký
- ✅ **Chỉ khi chọn loại đăng ký là `Loa Thần Tài`** thì nút In mẫu biểu mới khả dụng
- ✅ Khi bấm **In mẫu biểu**, form hiện thêm các trường:
  - Tên chủ hộ
  - Số CCCD
  - Ngày cấp
  - Nơi cấp
  - Địa chỉ
- ✅ Sau khi bấm **Đăng ký**:
  - ghi dữ liệu vào Airtable
  - nếu đang bật chế độ in mẫu biểu thì xuất file **DOCX**
  - hiển thị thông báo thành công màu xanh ngay trên cụm nút, không chuyển sang trang thank-you

### Trang quản lý (`admin.html`)
- ✅ Hiển thị thêm các cột:
  - Tên chủ hộ
  - Số CCCD
  - Ngày cấp
  - Nơi cấp
  - Địa chỉ
  - Yêu cầu in mẫu biểu
- ✅ Xuất Excel gồm cả các trường bổ sung

---

## 📁 CẤU TRÚC DỰ ÁN

```txt
regis/
├── index.html
├── admin.html
├── thank-you.html   # hiện không còn dùng trong flow chính
├── error.html
├── assets/
│   ├── pizzip.min.js
│   └── docxtemplater.js
├── templates/
│   ├── mau_bieu.docx        # bạn cần thêm file này
│   └── README.txt
└── README.md
```

---

## 📋 CẤU HÌNH AIRTABLE

Repo hiện đang ghi vào Airtable table đang dùng. Để lưu được đầy đủ dữ liệu mới, anh cần tạo thêm các cột sau trong Airtable:

| Tên cột | Kiểu dữ liệu gợi ý |
|---|---|
| Loại đăng ký | Single line text |
| Số tài khoản | Single line text |
| Số điện thoại | Single line text |
| Tên khách hàng | Single line text |
| Cán bộ quản lý | Single line text |
| Ghi chú | Long text |
| Tên chủ hộ | Single line text |
| Số CCCD | Single line text |
| Ngày cấp | Date |
| Nơi cấp | Single line text |
| Địa chỉ | Long text |
| Yêu cầu in mẫu biểu | Single line text hoặc Single select |
| Ngày đăng ký | Created time |

> Nếu thiếu các cột mới, Airtable có thể báo lỗi khi submit.

---

## 🖨️ CẤU HÌNH TEMPLATE DOCX

Đặt file Word mẫu vào:

```txt
templates/mau_bieu.docx
```

Hiện repo local đã được gắn file mẫu anh gửi cho luồng **Loa Thần Tài**.

Trong file DOCX, dùng các placeholder sau:

```txt
{loai_dang_ky}
{account_number}
{phone}
{customer_name}
{can_bo_quan_ly}
{ghi_chu}
{ten_chu_ho}
{so_cccd}
{ngay_cap}
{noi_cap}
{dia_chi}
{ngay_dang_ky}
{gio_dang_ky}
```

Ví dụ trong Word:
- Tên chủ hộ: `{ten_chu_ho}`
- CCCD số: `{so_cccd}`
- Ngày cấp: `{ngay_cap}`
- Nơi cấp: `{noi_cap}`
- Địa chỉ thường trú: `{dia_chi}`

Khi người dùng bấm **In mẫu biểu** rồi **Đăng ký**, webapp sẽ:
1. lưu record vào Airtable
2. đọc `templates/mau_bieu.docx`
3. thay placeholder bằng dữ liệu thực tế
4. tải file DOCX về máy người dùng

---

## 🚀 CÁCH TRIỂN KHAI

### 1. Thêm template Word
Copy file mẫu biểu của anh vào:

```txt
templates/mau_bieu.docx
```

### 2. Tạo thêm cột trong Airtable
Đảm bảo có các cột mới đã liệt kê bên trên.

### 3. Commit và push
```bash
git add .
git commit -m "feat: add docx print flow for regis"
git push origin main
```

---

## ⚠️ LƯU Ý
- Nếu thiếu file `templates/mau_bieu.docx`, webapp vẫn lưu được dữ liệu nhưng sẽ báo lỗi khi xuất DOCX.
- Vì đang chạy GitHub Pages tĩnh, việc xuất DOCX diễn ra **trên trình duyệt**.
- Repo hiện vẫn dùng Airtable token ở frontend, nên chỉ phù hợp demo/nội bộ. Nếu dùng thật, nên chuyển sang backend/proxy.

---

## GỢI Ý BƯỚC TIẾP THEO
Nếu anh muốn em làm tiếp, em có thể:
1. **nhét luôn mẫu `.docx` thật vào repo** nếu anh gửi file
2. **push code lên GitHub**
3. **refactor tiếp** để dùng chung danh sách cán bộ và bớt lặp code
4. **chuyển sang backend/serverless** để vừa an toàn hơn vừa dễ chống trùng hơn
