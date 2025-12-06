# Feature 2.2.4: Xem Chi Tiết Sách (View Book Details)

## Mô tả
Tính năng cho phép tất cả người dùng xem thông tin chi tiết của một cuốn sách.

## Actor
Tất cả người dùng (không cần login)

## Phụ thuộc
- 2.2.3 (Cần có danh sách sách trước)

## Flowchart

```mermaid
flowchart TD
    Start([Người dùng click vào sách từ danh sách]) --> CheckBook{Sách tồn tại?}
    
    CheckBook -->|Không| Error404[Hiển thị lỗi 404: Sách không tồn tại]
    Error404 --> End([Kết thúc])
    
    CheckBook -->|Có| CheckDeleted{Sách đã bị xóa?}
    CheckDeleted -->|Có| ErrorDeleted[Hiển thị: Sách không còn tồn tại]
    ErrorDeleted --> End
    
    CheckDeleted -->|Không| LoadDetails[Tải thông tin chi tiết sách]
    LoadDetails --> DisplayBasic[Hiển thị thông tin cơ bản:<br/>- Tên, Tác giả<br/>- ISBN, Năm xuất bản<br/>- Mô tả chi tiết<br/>- Số lượng bản đang có<br/>- Số lượng đang mượn]
    
    DisplayBasic --> CheckUserRole{Vai trò người dùng?}
    
    CheckUserRole -->|Chưa đăng nhập| DisplayPublic[Hiển thị thông tin công khai]
    CheckUserRole -->|Độc giả đã đăng nhập| DisplayReader[Hiển thị thông tin + Nút Mượn sách]
    CheckUserRole -->|Nhân viên thư viện| DisplayLibrarian[Hiển thị thông tin + Lịch sử mượn]
    CheckUserRole -->|Quản lý viên| DisplayAdmin[Hiển thị thông tin + Lịch sử mượn]
    
    DisplayReader --> CheckAvailable{Sách còn sẵn?}
    CheckAvailable -->|Có| ShowBorrowButton[Hiển thị nút Mượn sách]
    CheckAvailable -->|Không| ShowUnavailable[Hiển thị: Sách đã hết]
    
    DisplayLibrarian --> LoadHistory[Tải lịch sử mượn:<br/>- Độc giả<br/>- Ngày mượn<br/>- Ngày hết hạn]
    LoadHistory --> DisplayHistory[Hiển thị lịch sử mượn]
    
    DisplayAdmin --> LoadHistory
    
    ShowBorrowButton --> UserAction{Chọn hành động}
    UserAction -->|Mượn sách| NavigateBorrow[Chuyển đến trang Mượn sách]
    UserAction -->|Quay lại| NavigateBack[Quay lại danh sách]
    
    DisplayPublic --> NavigateBack
    ShowUnavailable --> NavigateBack
    DisplayHistory --> NavigateBack
    
    NavigateBorrow --> End
    NavigateBack --> End
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style Error404 fill:#ffcdd2
    style ErrorDeleted fill:#ffcdd2
    style ShowUnavailable fill:#fff9c4
    style ShowBorrowButton fill:#c8e6c9
```

## Thông tin hiển thị
- **Tất cả người dùng:** Tên, Tác giả, ISBN, Năm xuất bản, Mô tả chi tiết, Số lượng có sẵn, Số lượng đang mượn
- **Nhân viên/Quản lý viên:** Thêm Lịch sử mượn (Độc giả, Ngày mượn, Ngày hết hạn)
- **Độc giả đã đăng nhập:** Thêm nút "Mượn sách" (nếu sách còn sẵn)

## Edge Cases
- Sách không tồn tại → 404
- Sách đã bị xóa → Thông báo sách không còn tồn tại
- Sách hết → Ẩn nút "Mượn sách"

