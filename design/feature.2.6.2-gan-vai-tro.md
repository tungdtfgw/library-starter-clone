# Feature 2.6.2: Gán Vai Trò (Assign Role)

## Mô tả
Tính năng cho phép quản lý viên gán vai trò cho người dùng trong hệ thống.

## Actor
Quản lý viên

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)
- 2.6.1 (Cần có danh sách người dùng)

## Flowchart

```mermaid
flowchart TD
    Start([Quản lý viên đăng nhập]) --> AccessUsers[Truy cập trang Quản lý người dùng]
    AccessUsers --> DisplayList[Hiển thị danh sách người dùng]
    DisplayList --> SelectUser[Chọn người dùng]
    SelectUser --> ClickAssign[Click Gán vai trò]
    ClickAssign --> ShowRoleForm[Hiển thị form gán vai trò]
    ShowRoleForm --> DisplayCurrentRole[Hiển thị vai trò hiện tại]
    
    DisplayCurrentRole --> SelectNewRole[Chọn vai trò mới:<br/>- Reader<br/>- Librarian<br/>- Admin]
    SelectNewRole --> CheckCurrentUser{Tài khoản đang đăng nhập?}
    
    CheckCurrentUser -->|Có| ErrorSelf[Hiển thị: Không thể thay đổi vai trò của chính mình]
    ErrorSelf --> DisplayList
    
    CheckCurrentUser -->|Không| CheckSameRole{Vai trò mới = vai trò cũ?}
    CheckSameRole -->|Có| ErrorSame[Hiển thị: Vai trò không thay đổi]
    ErrorSame --> DisplayList
    
    CheckSameRole -->|Không| CheckNewRole{Vai trò mới?}
    
    CheckNewRole -->|Admin| CheckAdminLimit{Đã đạt giới hạn Admin?<br/>Tùy chọn: Giới hạn số lượng Admin}
    CheckAdminLimit -->|Có| ErrorAdminLimit[Hiển thị: Đã đạt giới hạn số lượng Admin]
    ErrorAdminLimit --> DisplayList
    
    CheckAdminLimit -->|Không| ConfirmChange[Xác nhận thay đổi vai trò]
    CheckNewRole -->|Reader/Librarian| ConfirmChange
    
    ConfirmChange --> UserConfirm{Người dùng xác nhận?}
    UserConfirm -->|Không| DisplayList
    UserConfirm -->|Có| UpdateRole[Cập nhật vai trò người dùng]
    UpdateRole --> SaveDB[Lưu thay đổi]
    SaveDB --> Success[Hiển thị: Gán vai trò thành công]
    Success --> DisplayList
    
    DisplayList --> End([Kết thúc])
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorSelf fill:#ffcdd2
    style ErrorSame fill:#ffcdd2
    style ErrorAdminLimit fill:#ffcdd2
    style Success fill:#c8e6c9
```

## Vai trò trong hệ thống
- **Reader:** Chỉ có thể mượn sách, xem lịch sử cá nhân
- **Librarian:** Có thể quản lý sách, xác nhận mượn/trả, theo dõi phạt
- **Admin:** Toàn quyền, quản lý tài khoản, báo cáo, cài đặt

## Edge Cases
- Gán vai trò Admin cho nhiều người → Có thể giới hạn số lượng Admin
- Thay đổi vai trò của chính mình → Ngăn chặn hoặc cảnh báo
- Vai trò không thay đổi → Không cần cập nhật

