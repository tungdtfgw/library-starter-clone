# Feature 2.5.1: Quản lý mức phạt (Fine Level Management)

## Mô tả
Tính năng cho phép quản lý viên quản lý các mức phạt: xem, thêm, sửa, xóa.

## Actor
Quản lý viên

## Phụ thuộc
- 2.1.2 (Cần đăng nhập với vai trò quản lý viên)

## Flowchart

```mermaid
flowchart TD
    Start([Quản lý viên đăng nhập]) --> AccessFine[Truy cập trang Quản lý mức phạt]
    AccessFine --> DisplayList[Hiển thị bảng danh sách mức phạt]
    
    DisplayList --> UserAction{Chọn hành động}
    
    UserAction -->|Thêm mức phạt| AddFine[Click Thêm mức phạt]
    UserAction -->|Sửa mức phạt| EditFine[Click Sửa trên bảng]
    UserAction -->|Xóa mức phạt| DeleteFine[Click Xóa trên bảng]
    
    AddFine --> ShowAddForm[Hiển thị form thêm mức phạt]
    ShowAddForm --> InputAdd[Nhập thông tin:<br/>- Tên mức phạt<br/>- Số tiền<br/>- Ngày phạt<br/>Mặc định: Ngày hiện tại]
    InputAdd --> ValidateAdd{Validate}
    
    ValidateAdd -->|Tên để trống| ErrorEmptyName[Hiển thị lỗi: Tên không được để trống]
    ErrorEmptyName --> InputAdd
    
    ValidateAdd -->|Tên > 25 ký tự| ErrorLongName[Hiển thị lỗi: Tên quá dài]
    ErrorLongName --> InputAdd
    
    ValidateAdd -->|Số tiền <= 0| ErrorAmount[Hiển thị lỗi: Số tiền phải > 0]
    ErrorAmount --> InputAdd
    
    ValidateAdd -->|Số tiền không phải số| ErrorAmountType[Hiển thị lỗi: Số tiền không hợp lệ]
    ErrorAmountType --> InputAdd
    
    ValidateAdd -->|Ngày phạt không hợp lệ| ErrorDate[Hiển thị lỗi: Ngày phạt không hợp lệ]
    ErrorDate --> InputAdd
    
    ValidateAdd -->|Tất cả hợp lệ| SaveAdd[Lưu mức phạt mới]
    SaveAdd --> SuccessAdd[Hiển thị: Thêm mức phạt thành công]
    SuccessAdd --> DisplayList
    
    EditFine --> ShowEditForm[Hiển thị form sửa với dữ liệu hiện tại]
    ShowEditForm --> InputEdit[Nhập thông tin mới]
    InputEdit --> ValidateEdit{Validate}
    
    ValidateEdit -->|Tên để trống| ErrorEmptyName
    ValidateEdit -->|Tên > 25 ký tự| ErrorLongName
    ValidateEdit -->|Số tiền <= 0| ErrorAmount
    ValidateEdit -->|Số tiền không phải số| ErrorAmountType
    ValidateEdit -->|Ngày phạt không hợp lệ| ErrorDate
    ValidateEdit -->|Tất cả hợp lệ| SaveEdit[Lưu thay đổi]
    SaveEdit --> SuccessEdit[Hiển thị: Sửa mức phạt thành công]
    SuccessEdit --> DisplayList
    
    DeleteFine --> ConfirmDelete[Xác nhận xóa]
    ConfirmDelete --> UserConfirm{Người dùng xác nhận?}
    UserConfirm -->|Không| DisplayList
    UserConfirm -->|Có| CheckUsage{Mức phạt đang được sử dụng?}
    CheckUsage -->|Có| WarningUsed[Cảnh báo: Mức phạt đang được sử dụng<br/>Vẫn cho phép xóa hoặc ngăn chặn]
    WarningUsed --> UserConfirmDelete{Người dùng vẫn muốn xóa?}
    UserConfirmDelete -->|Không| DisplayList
    UserConfirmDelete -->|Có| DeleteDB[Xóa mức phạt]
    CheckUsage -->|Không| DeleteDB
    DeleteDB --> SuccessDelete[Hiển thị: Xóa mức phạt thành công]
    SuccessDelete --> DisplayList
    
    DisplayList --> End([Kết thúc])
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorEmptyName fill:#ffcdd2
    style ErrorLongName fill:#ffcdd2
    style ErrorAmount fill:#ffcdd2
    style ErrorAmountType fill:#ffcdd2
    style ErrorDate fill:#ffcdd2
    style WarningUsed fill:#fff9c4
    style SuccessAdd fill:#c8e6c9
    style SuccessEdit fill:#c8e6c9
    style SuccessDelete fill:#c8e6c9
```

## Validation Rules
- **Tên mức phạt:** Không được để trống, tối đa 25 ký tự
- **Số tiền:** Phải > 0, kiểu số
- **Ngày phạt:** Ngày hợp lệ (mặc định là ngày hiện tại)

## Edge Cases
- Tên mức phạt để trống
- Tên mức phạt quá dài (>25 ký tự)
- Số tiền <= 0 hoặc không phải số
- Ngày phạt không hợp lệ
- Mức phạt đang được sử dụng → Có thể ngăn chặn xóa hoặc cảnh báo

