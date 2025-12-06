# Feature 2.2.5: Sửa & Xóa Sách (Edit & Delete Book)

## Mô tả
Tính năng cho phép nhân viên thư viện sửa và xóa sách trong hệ thống.

## Actor
Nhân viên thư viện

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)
- 2.2.4 (Cần xem chi tiết sách)

## Flowchart - Sửa Sách

```mermaid
flowchart TD
    Start([Nhân viên xem chi tiết sách]) --> ClickEdit[Click nút Sửa]
    ClickEdit --> ShowEditForm[Hiển thị form sửa với dữ liệu hiện tại]
    ShowEditForm --> InputEdit[Nhập/chỉnh sửa thông tin:<br/>- Tên sách<br/>- Tác giả<br/>- Năm xuất bản<br/>- ISBN<br/>- Thể loại<br/>- Mô tả<br/>- Số lượng]
    
    InputEdit --> Validate{Validate dữ liệu}
    
    Validate -->|Tên sách để trống hoặc > 100 ký tự| ErrorTitle[Hiển thị lỗi: Tên sách không hợp lệ]
    ErrorTitle --> InputEdit
    
    Validate -->|Tác giả để trống hoặc > 100 ký tự| ErrorAuthor[Hiển thị lỗi: Tác giả không hợp lệ]
    ErrorAuthor --> InputEdit
    
    Validate -->|ISBN không đúng định dạng| ErrorISBN[Hiển thị lỗi: ISBN không hợp lệ]
    ErrorISBN --> InputEdit
    
    Validate -->|Năm xuất bản không hợp lệ| ErrorYear[Hiển thị lỗi: Năm xuất bản không hợp lệ]
    ErrorYear --> InputEdit
    
    Validate -->|Thể loại không tồn tại| ErrorCategory[Hiển thị lỗi: Thể loại không hợp lệ]
    ErrorCategory --> InputEdit
    
    Validate -->|Mô tả để trống hoặc > 255 ký tự| ErrorDesc[Hiển thị lỗi: Mô tả không hợp lệ]
    ErrorDesc --> InputEdit
    
    Validate -->|Số lượng <= 0| ErrorQuantity[Hiển thị lỗi: Số lượng không hợp lệ]
    ErrorQuantity --> InputEdit
    
    Validate -->|Tất cả hợp lệ| SaveEdit[Lưu thay đổi]
    SaveEdit --> Success[Hiển thị: Sửa sách thành công]
    Success --> RefreshDetail[Làm mới trang chi tiết]
    RefreshDetail --> End([Kết thúc])
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorTitle fill:#ffcdd2
    style ErrorAuthor fill:#ffcdd2
    style ErrorISBN fill:#ffcdd2
    style ErrorYear fill:#ffcdd2
    style ErrorCategory fill:#ffcdd2
    style ErrorDesc fill:#ffcdd2
    style ErrorQuantity fill:#ffcdd2
    style Success fill:#c8e6c9
```

## Flowchart - Xóa Sách

```mermaid
flowchart TD
    Start([Nhân viên xem chi tiết sách]) --> ClickDelete[Click nút Xóa]
    ClickDelete --> CheckBorrows{Có đơn mượn hoạt động?}
    
    CheckBorrows -->|Có| ErrorHasBorrows[Hiển thị lỗi: Không thể xóa vì có đơn mượn hoạt động]
    ErrorHasBorrows --> End([Kết thúc])
    
    CheckBorrows -->|Không| ConfirmDelete[Hiển thị dialog xác nhận xóa]
    ConfirmDelete --> UserConfirm{Người dùng xác nhận?}
    
    UserConfirm -->|Không| Cancel[Hủy xóa]
    Cancel --> End
    
    UserConfirm -->|Có| DeleteBook[Xóa sách khỏi database]
    DeleteBook --> Success[Hiển thị: Xóa sách thành công]
    Success --> NavigateList[Chuyển về danh sách sách]
    NavigateList --> End
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorHasBorrows fill:#ffcdd2
    style Success fill:#c8e6c9
```

## Validation Rules
- Tương tự như Feature 2.2.2 (Thêm Sách Mới)

## Edge Cases
- Sách đang được mượn → Không thể xóa
- Sách có đơn mượn chờ xác nhận → Có thể xóa hoặc không (tùy business logic)
- Mất kết nối khi lưu dữ liệu

