# Feature 2.1.3: Hồ Sơ Cá Nhân (Personal Profile)

## Mô tả
Tính năng cho phép độc giả xem và cập nhật thông tin cá nhân, đổi mật khẩu.

## Actor
Độc giả

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)

## Flowchart

```mermaid
flowchart TD
    Start([Độc giả đăng nhập]) --> AccessProfile[Truy cập trang Hồ Sơ Cá Nhân]
    AccessProfile --> DisplayInfo[Hiển thị thông tin:<br/>- Tên, Email<br/>- Số điện thoại, Địa chỉ<br/>- Ngày tham gia<br/>- Số lần mượn<br/>- Tổng số tiền phạt]
    
    DisplayInfo --> UserAction{Chọn hành động}
    
    UserAction -->|Cập nhật thông tin| UpdateInfo[Click Cập nhật thông tin]
    UserAction -->|Đổi mật khẩu| ChangePassword[Click Đổi mật khẩu]
    UserAction -->|Xem thông tin| DisplayInfo
    
    UpdateInfo --> ShowUpdateForm[Hiển thị form cập nhật]
    ShowUpdateForm --> InputUpdate[Nhập thông tin mới:<br/>- Tên<br/>- Số điện thoại<br/>- Địa chỉ]
    InputUpdate --> ValidateUpdate{Validate dữ liệu}
    
    ValidateUpdate -->|Tên để trống hoặc > 50 ký tự| ErrorName[Hiển thị lỗi: Tên không hợp lệ]
    ErrorName --> InputUpdate
    
    ValidateUpdate -->|Số điện thoại không đúng định dạng| ErrorPhone[Hiển thị lỗi: Số điện thoại không hợp lệ]
    ErrorPhone --> InputUpdate
    
    ValidateUpdate -->|Địa chỉ để trống hoặc > 255 ký tự| ErrorAddress[Hiển thị lỗi: Địa chỉ không hợp lệ]
    ErrorAddress --> InputUpdate
    
    ValidateUpdate -->|Tất cả hợp lệ| SaveUpdate[Lưu thông tin mới]
    SaveUpdate --> SuccessUpdate[Hiển thị: Cập nhật thành công]
    SuccessUpdate --> DisplayInfo
    
    ChangePassword --> ShowPasswordForm[Hiển thị form đổi mật khẩu]
    ShowPasswordForm --> InputPassword[Nhập:<br/>- Mật khẩu cũ<br/>- Mật khẩu mới<br/>- Xác nhận mật khẩu mới]
    InputPassword --> ValidatePassword{Validate}
    
    ValidatePassword -->|Mật khẩu cũ sai| ErrorOldPassword[Hiển thị lỗi: Mật khẩu cũ không đúng]
    ErrorOldPassword --> InputPassword
    
    ValidatePassword -->|Mật khẩu mới < 8 hoặc > 16 ký tự| ErrorNewPassword[Hiển thị lỗi: Mật khẩu mới không hợp lệ]
    ErrorNewPassword --> InputPassword
    
    ValidatePassword -->|Mật khẩu mới và xác nhận không khớp| ErrorConfirm[Hiển thị lỗi: Mật khẩu không khớp]
    ErrorConfirm --> InputPassword
    
    ValidatePassword -->|Mật khẩu mới trùng với mật khẩu cũ| ErrorSame[Hiển thị lỗi: Mật khẩu mới phải khác mật khẩu cũ]
    ErrorSame --> InputPassword
    
    ValidatePassword -->|Tất cả hợp lệ| SavePassword[Cập nhật mật khẩu]
    SavePassword --> SuccessPassword[Hiển thị: Đổi mật khẩu thành công]
    SuccessPassword --> DisplayInfo
    
    DisplayInfo --> End([Kết thúc])
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorName fill:#ffcdd2
    style ErrorPhone fill:#ffcdd2
    style ErrorAddress fill:#ffcdd2
    style ErrorOldPassword fill:#ffcdd2
    style ErrorNewPassword fill:#ffcdd2
    style ErrorConfirm fill:#ffcdd2
    style ErrorSame fill:#ffcdd2
    style SuccessUpdate fill:#c8e6c9
    style SuccessPassword fill:#c8e6c9
```

## Validation Rules
- **Tên:** Không được để trống, tối đa 50 ký tự
- **Số điện thoại:** Định dạng số điện thoại hợp lệ
- **Địa chỉ:** Không được để trống, tối đa 255 ký tự
- **Mật khẩu cũ:** Phải đúng với mật khẩu hiện tại
- **Mật khẩu mới:** Tối thiểu 8 ký tự, tối đa 16 ký tự, phải khác mật khẩu cũ

## Edge Cases
- Số điện thoại không đúng định dạng
- Địa chỉ quá dài
- Mật khẩu mới trùng với mật khẩu cũ
- Mất kết nối khi lưu dữ liệu

