# Feature 2.4.1: Yêu Cầu Trả Sách (Return Request)

## Mô tả
Tính năng cho phép độc giả tạo yêu cầu trả sách để nhân viên xác nhận.

## Actor
Độc giả

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)
- 2.3.1 (Cần có sách đang mượn)

## Flowchart

```mermaid
flowchart TD
    Start([Độc giả đăng nhập]) --> AccessHistory[Truy cập trang Lịch sử mượn sách]
    AccessHistory --> DisplayBorrowing[Hiển thị danh sách sách đang mượn]
    DisplayBorrowing --> ClickReturn[Click Xin trả sách trên sách muốn trả]
    
    ClickReturn --> CheckBorrowStatus{Trạng thái đơn mượn?}
    CheckBorrowStatus -->|Chưa được mượn| ErrorNotBorrowed[Hiển thị: Sách chưa được mượn]
    ErrorNotBorrowed --> End([Kết thúc])
    
    CheckBorrowStatus -->|Đã trả| ErrorAlreadyReturned[Hiển thị: Sách đã được trả]
    ErrorAlreadyReturned --> End
    
    CheckBorrowStatus -->|Bị từ chối| ErrorRejected[Hiển thị: Đơn mượn đã bị từ chối]
    ErrorRejected --> End
    
    CheckBorrowStatus -->|Đang mượn| CheckExistingRequest{Đã có yêu cầu trả<br/>chờ xác nhận?}
    
    CheckExistingRequest -->|Có| ErrorHasRequest[Hiển thị: Đã có yêu cầu trả chờ xác nhận]
    ErrorHasRequest --> End
    
    CheckExistingRequest -->|Chưa| ShowConfirmModal[Hiển thị modal xác nhận:<br/>Bạn có chắc muốn trả sách này?]
    ShowConfirmModal --> UserConfirm{Người dùng xác nhận?}
    
    UserConfirm -->|Không| Cancel[Hủy yêu cầu]
    Cancel --> End
    
    UserConfirm -->|Có| CreateReturnRequest[Tạo yêu cầu trả sách<br/>Trạng thái: Chờ xác nhận]
    CreateReturnRequest --> SaveDB[Lưu vào database]
    SaveDB --> Success[Hiển thị: Yêu cầu trả sách đã được gửi<br/>Vui lòng mang sách đến thư viện]
    Success --> End
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorNotBorrowed fill:#ffcdd2
    style ErrorAlreadyReturned fill:#ffcdd2
    style ErrorRejected fill:#ffcdd2
    style ErrorHasRequest fill:#ffcdd2
    style Success fill:#c8e6c9
```

## Lưu ý
- Độc giả cần mang sách đến thư viện để nhân viên xác nhận
- Một đơn mượn chỉ có thể tạo một yêu cầu trả ở trạng thái "Chờ xác nhận"

## Edge Cases
- Đơn mượn đã ở trạng thái "Đã trả" → Không cho phép tạo yêu cầu mới
- Đơn mượn bị từ chối → Không cho phép tạo yêu cầu trả
- Đã có yêu cầu trả chờ xác nhận → Không cho phép tạo yêu cầu mới

