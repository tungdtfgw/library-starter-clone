# Feature 2.3.2: Mượn Sách - Nhân Viên (Borrow Book - Librarian)

## Mô tả
Tính năng cho phép nhân viên thư viện xem danh sách đơn mượn chờ xác nhận và xác nhận hoặc từ chối đơn mượn.

## Actor
Nhân viên thư viện

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)
- 2.3.1 (Cần có đơn mượn từ độc giả)

## Flowchart

```mermaid
flowchart TD
    Start([Nhân viên đăng nhập]) --> AccessBorrow[Truy cập trang Quản lý mượn trả]
    AccessBorrow --> SelectTab[Chọn tab Chờ xác nhận]
    SelectTab --> LoadPending[Tải danh sách đơn mượn chờ xác nhận]
    LoadPending --> DisplayList[Hiển thị danh sách đơn mượn:<br/>- Tên sách<br/>- Độc giả<br/>- Ngày yêu cầu<br/>- Thời hạn mượn]
    
    DisplayList --> SelectBorrow[Chọn đơn mượn]
    SelectBorrow --> ViewDetail[Xem chi tiết đơn mượn]
    ViewDetail --> CheckStatus{Trạng thái đơn mượn?}
    
    CheckStatus -->|Đã xử lý| ErrorProcessed[Hiển thị: Đơn mượn đã được xử lý]
    ErrorProcessed --> DisplayList
    
    CheckStatus -->|Chờ xác nhận| ChooseAction{Chọn hành động}
    
    ChooseAction -->|Xác nhận| ConfirmBorrow[Click Xác nhận]
    ChooseAction -->|Từ chối| RejectBorrow[Click Từ chối]
    
    ConfirmBorrow --> CheckBookAvailable{Sách còn sẵn?}
    CheckBookAvailable -->|Không| ErrorBookGone[Hiển thị: Sách đã hết<br/>Có thể từ chối đơn mượn]
    ErrorBookGone --> RejectBorrow
    
    CheckBookAvailable -->|Có| UpdateStatus[Cập nhật trạng thái:<br/>Chờ xác nhận → Đã mượn]
    UpdateStatus --> DecreaseAvailable[Giảm số lượng sách có sẵn]
    DecreaseAvailable --> IncreaseBorrowed[Tăng số lượng sách đang mượn]
    IncreaseBorrowed --> SaveConfirm[Lưu thay đổi]
    SaveConfirm --> SuccessConfirm[Hiển thị: Xác nhận mượn sách thành công]
    SuccessConfirm --> DisplayList
    
    RejectBorrow --> ShowRejectForm[Hiển thị form từ chối]
    ShowRejectForm --> InputReason[Nhập lý do từ chối<br/>Bắt buộc]
    InputReason --> ValidateReason{Lý do để trống?}
    
    ValidateReason -->|Có| ErrorEmptyReason[Hiển thị: Vui lòng nhập lý do từ chối]
    ErrorEmptyReason --> InputReason
    
    ValidateReason -->|Không| UpdateReject[Cập nhật trạng thái:<br/>Chờ xác nhận → Bị từ chối]
    UpdateReject --> SaveReason[Lưu lý do từ chối]
    SaveReason --> SaveReject[Lưu thay đổi]
    SaveReject --> SuccessReject[Hiển thị: Từ chối đơn mượn thành công]
    SuccessReject --> DisplayList
    
    DisplayList --> End([Kết thúc])
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorProcessed fill:#ffcdd2
    style ErrorBookGone fill:#ffcdd2
    style ErrorEmptyReason fill:#ffcdd2
    style SuccessConfirm fill:#c8e6c9
    style SuccessReject fill:#c8e6c9
```

## Validation Rules
- **Lý do từ chối:** Bắt buộc phải nhập

## Edge Cases
- Đơn mượn đã được xử lý → Không cho phép xác nhận/từ chối lại
- Sách đã hết sau khi tạo đơn → Có thể từ chối hoặc cảnh báo
- Sách đã bị xóa → Từ chối đơn mượn
- Lý do từ chối để trống → Yêu cầu nhập lý do

