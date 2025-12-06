# Feature 2.1.2: Đăng Nhập (Login)

## Mô tả
Tính năng cho phép người dùng đăng nhập vào hệ thống với email và mật khẩu.

## Actor
Mọi người (không cần đăng nhập)

## Phụ thuộc
- 2.1.1 (Cần có tài khoản để đăng nhập)

## Flowchart

```mermaid
flowchart TD
    Start([Người dùng truy cập trang đăng nhập]) --> ShowForm[Hiển thị form đăng nhập]
    ShowForm --> InputData[Nhập Email & Mật khẩu]
    InputData --> Validate{Validate dữ liệu}
    
    Validate -->|Email không hợp lệ| ErrorEmail[Hiển thị lỗi: Email không đúng định dạng]
    ErrorEmail --> InputData
    
    Validate -->|Mật khẩu để trống hoặc không hợp lệ| ErrorPassword[Hiển thị lỗi: Mật khẩu không hợp lệ]
    ErrorPassword --> InputData
    
    Validate -->|Tất cả hợp lệ| CheckAccount{Tài khoản tồn tại?}
    CheckAccount -->|Không| ErrorNotFound[Hiển thị lỗi: Email hoặc mật khẩu không đúng]
    ErrorNotFound --> InputData
    
    CheckAccount -->|Có| CheckPassword{Mật khẩu đúng?}
    CheckPassword -->|Không| ErrorNotFound
    
    CheckPassword -->|Đúng| CheckStatus{Trạng thái tài khoản?}
    CheckStatus -->|Chờ xác nhận| ErrorPending[Hiển thị lỗi: Tài khoản chưa được kích hoạt]
    ErrorPending --> InputData
    
    CheckStatus -->|Vô hiệu hóa| ErrorDisabled[Hiển thị lỗi: Tài khoản đã bị vô hiệu hóa]
    ErrorDisabled --> InputData
    
    CheckStatus -->|Kích hoạt| GenerateToken[Tạo JWT token<br/>Thời hạn: 24 giờ]
    GenerateToken --> SaveSession[Lưu session]
    SaveSession --> CheckRole{Vai trò người dùng?}
    
    CheckRole -->|Độc giả| RedirectReader[Chuyển hướng đến Dashboard độc giả]
    CheckRole -->|Nhân viên| RedirectLibrarian[Chuyển hướng đến Dashboard nhân viên]
    CheckRole -->|Quản lý viên| RedirectAdmin[Chuyển hướng đến Dashboard quản lý viên]
    
    RedirectReader --> End([Kết thúc])
    RedirectLibrarian --> End
    RedirectAdmin --> End
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorEmail fill:#ffcdd2
    style ErrorPassword fill:#ffcdd2
    style ErrorNotFound fill:#ffcdd2
    style ErrorPending fill:#ffcdd2
    style ErrorDisabled fill:#ffcdd2
    style RedirectReader fill:#c8e6c9
    style RedirectLibrarian fill:#c8e6c9
    style RedirectAdmin fill:#c8e6c9
```

## Validation Rules
- **Email:** Định dạng email hợp lệ
- **Mật khẩu:** Không được để trống, tối thiểu 8 ký tự, tối đa 16 ký tự

## Edge Cases
- Email không tồn tại trong hệ thống
- Mật khẩu sai
- Tài khoản chưa được kích hoạt
- Tài khoản bị vô hiệu hóa
- Token hết hạn (sau 24 giờ)

