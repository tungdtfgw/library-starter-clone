# Feature 2.1.1: Đăng Ký (Registration)

## Mô tả
Tính năng cho phép người dùng mới đăng ký tài khoản trong hệ thống.

## Actor
Mọi người (không cần đăng nhập)

## Flowchart

```mermaid
flowchart TD
    Start([Người dùng truy cập trang đăng ký]) --> ClickRegister[Click nút Đăng ký]
    ClickRegister --> ShowForm[Hiển thị form đăng ký]
    ShowForm --> InputData[Nhập thông tin:<br/>- Email<br/>- Tên<br/>- Mật khẩu<br/>- Xác nhận mật khẩu]
    InputData --> Validate{Validate dữ liệu}
    
    Validate -->|Email không hợp lệ| ErrorEmail[Hiển thị lỗi: Email không đúng định dạng]
    ErrorEmail --> InputData
    
    Validate -->|Tên để trống hoặc > 50 ký tự| ErrorName[Hiển thị lỗi: Tên không hợp lệ]
    ErrorName --> InputData
    
    Validate -->|Mật khẩu < 8 hoặc > 16 ký tự| ErrorPassword[Hiển thị lỗi: Mật khẩu không hợp lệ]
    ErrorPassword --> InputData
    
    Validate -->|Mật khẩu và xác nhận không khớp| ErrorConfirm[Hiển thị lỗi: Mật khẩu không khớp]
    ErrorConfirm --> InputData
    
    Validate -->|Email đã tồn tại| ErrorExists[Hiển thị lỗi: Email đã được sử dụng]
    ErrorExists --> InputData
    
    Validate -->|Tất cả hợp lệ| CheckEmail{Email đã tồn tại?}
    CheckEmail -->|Có| ErrorExists
    CheckEmail -->|Không| CreateAccount[Tạo tài khoản với trạng thái 'Chờ xác nhận']
    CreateAccount --> SaveDB[Lưu vào database]
    SaveDB --> Success[Hiển thị thông báo: Đăng ký thành công]
    Success --> End([Kết thúc])
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorEmail fill:#ffcdd2
    style ErrorName fill:#ffcdd2
    style ErrorPassword fill:#ffcdd2
    style ErrorConfirm fill:#ffcdd2
    style ErrorExists fill:#ffcdd2
    style Success fill:#c8e6c9
```

## Validation Rules
- **Email:** Định dạng email hợp lệ
- **Tên:** Không được để trống, tối đa 50 ký tự
- **Mật khẩu:** Không được để trống, tối thiểu 8 ký tự, tối đa 16 ký tự
- **Xác nhận mật khẩu:** Không được để trống, phải trùng với mật khẩu

## Edge Cases
- Email đã tồn tại trong hệ thống
- Mật khẩu quá yếu (có thể thêm validation)
- Mạng lỗi khi lưu dữ liệu

