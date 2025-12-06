# Feature 2.6.1: Danh Sách Người Dùng (User List)

## Mô tả
Tính năng cho phép quản lý viên xem danh sách người dùng, tìm kiếm, lọc, và quản lý trạng thái tài khoản.

## Actor
Quản lý viên

## Phụ thuộc
- 2.1.2 (Cần đăng nhập với vai trò quản lý viên)

## Flowchart

```mermaid
flowchart TD
    Start([Quản lý viên đăng nhập]) --> AccessUsers[Truy cập trang Quản lý người dùng]
    AccessUsers --> LoadUsers[Tải danh sách người dùng]
    LoadUsers --> DisplayList[Hiển thị danh sách:<br/>- Email<br/>- Tên<br/>- Vai trò Reader/Librarian/Admin<br/>- Ngày tham gia<br/>- Trạng thái Kích hoạt/Vô hiệu hóa]
    
    DisplayList --> UserAction{Chọn hành động}
    
    UserAction -->|Tìm kiếm| Search[Click Tìm kiếm]
    UserAction -->|Lọc| Filter[Chọn vai trò từ dropdown]
    UserAction -->|Vô hiệu hóa/Kích hoạt| ToggleStatus[Click Vô hiệu hóa/Kích hoạt]
    
    Search --> InputKeyword[Nhập từ khóa<br/>Email hoặc Tên]
    InputKeyword --> ExecuteSearch[Thực hiện tìm kiếm]
    ExecuteSearch --> CheckResults{Có kết quả?}
    CheckResults -->|Có| DisplayResults[Hiển thị kết quả tìm kiếm]
    CheckResults -->|Không| ShowNoResults[Hiển thị: Không tìm thấy kết quả]
    DisplayResults --> DisplayList
    ShowNoResults --> DisplayList
    
    Filter --> SelectRole[Chọn vai trò:<br/>- Reader<br/>- Librarian<br/>- Admin<br/>- Tất cả]
    SelectRole --> ApplyFilter[Áp dụng bộ lọc]
    ApplyFilter --> DisplayFiltered[Hiển thị người dùng đã lọc]
    DisplayFiltered --> DisplayList
    
    ToggleStatus --> CheckCurrentUser{Tài khoản đang đăng nhập?}
    CheckCurrentUser -->|Có| ErrorSelf[Hiển thị: Không thể vô hiệu hóa chính mình]
    ErrorSelf --> DisplayList
    
    CheckCurrentUser -->|Không| CheckRole{Vai trò tài khoản?}
    CheckRole -->|Admin| CheckOtherAdmin{Còn Admin khác?}
    CheckOtherAdmin -->|Không| ErrorLastAdmin[Hiển thị: Không thể vô hiệu hóa Admin cuối cùng]
    ErrorLastAdmin --> DisplayList
    
    CheckOtherAdmin -->|Có| CheckStatus{Trạng thái hiện tại?}
    CheckRole -->|Reader/Librarian| CheckStatus
    
    CheckStatus -->|Kích hoạt| ConfirmDisable[Xác nhận vô hiệu hóa]
    CheckStatus -->|Vô hiệu hóa| ConfirmEnable[Xác nhận kích hoạt]
    
    ConfirmDisable --> UserConfirmDisable{Người dùng xác nhận?}
    UserConfirmDisable -->|Không| DisplayList
    UserConfirmDisable -->|Có| CheckActiveBorrows{Có đơn mượn đang hoạt động?}
    CheckActiveBorrows -->|Có| WarningBorrows[Cảnh báo: Tài khoản có đơn mượn đang hoạt động<br/>Vẫn cho phép vô hiệu hóa]
    WarningBorrows --> UserConfirmWarning{Người dùng vẫn muốn vô hiệu hóa?}
    UserConfirmWarning -->|Không| DisplayList
    UserConfirmWarning -->|Có| DisableAccount[Vô hiệu hóa tài khoản]
    CheckActiveBorrows -->|Không| DisableAccount
    
    DisableAccount --> UpdateStatusDisable[Cập nhật trạng thái: Vô hiệu hóa]
    UpdateStatusDisable --> SaveDisable[Lưu thay đổi]
    SaveDisable --> SuccessDisable[Hiển thị: Vô hiệu hóa thành công]
    SuccessDisable --> DisplayList
    
    ConfirmEnable --> EnableAccount[Kích hoạt tài khoản]
    EnableAccount --> UpdateStatusEnable[Cập nhật trạng thái: Kích hoạt]
    UpdateStatusEnable --> SaveEnable[Lưu thay đổi]
    SaveEnable --> SuccessEnable[Hiển thị: Kích hoạt thành công]
    SuccessEnable --> DisplayList
    
    DisplayList --> End([Kết thúc])
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorSelf fill:#ffcdd2
    style ErrorLastAdmin fill:#ffcdd2
    style WarningBorrows fill:#fff9c4
    style SuccessDisable fill:#c8e6c9
    style SuccessEnable fill:#c8e6c9
```

## Chức năng
- Tìm kiếm theo email/tên
- Lọc theo vai trò
- Vô hiệu hóa/Kích hoạt tài khoản

## Edge Cases
- Tài khoản đang đăng nhập → Không cho phép vô hiệu hóa chính mình
- Tài khoản quản lý viên → Có thể ngăn chặn vô hiệu hóa Admin cuối cùng
- Tài khoản có đơn mượn đang hoạt động → Cảnh báo trước khi vô hiệu hóa
- Không có kết quả tìm kiếm → Hiển thị thông báo

