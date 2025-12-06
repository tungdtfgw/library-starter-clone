# Feature 2.3.3: Xem Lịch Sử Mượn Sách (View Borrowing History)

## Mô tả
Tính năng cho phép độc giả xem lịch sử mượn sách, gia hạn sách, và tạo yêu cầu trả sách.

## Actor
Độc giả

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)
- 2.3.1 (Cần có đơn mượn)

## Flowchart

```mermaid
flowchart TD
    Start([Độc giả đăng nhập]) --> AccessHistory[Truy cập trang Lịch sử mượn sách]
    AccessHistory --> LoadHistory[Tải lịch sử mượn sách]
    LoadHistory --> DisplayHistory[Hiển thị danh sách:<br/>- Sách đang mượn<br/>- Sách đã trả<br/>- Sách bị từ chối]
    
    DisplayHistory --> UserAction{Chọn hành động}
    
    UserAction -->|Lọc theo trạng thái| FilterStatus[Chọn trạng thái:<br/>- Chờ xác nhận<br/>- Đang mượn<br/>- Quá hạn<br/>- Đã trả<br/>- Bị từ chối]
    UserAction -->|Gia hạn| ExtendBook[Click Gia hạn]
    UserAction -->|Xin trả sách| RequestReturn[Click Xin trả sách]
    UserAction -->|Xem lý do từ chối| ViewRejectReason[Xem lý do từ chối]
    
    FilterStatus --> ApplyFilter[Áp dụng bộ lọc]
    ApplyFilter --> DisplayFiltered[Hiển thị kết quả đã lọc]
    DisplayFiltered --> DisplayHistory
    
    ExtendBook --> CheckBookStatus{Trạng thái sách?}
    CheckBookStatus -->|Đã hết hạn| ErrorExpired[Hiển thị: Không thể gia hạn sách đã hết hạn]
    ErrorExpired --> DisplayHistory
    
    CheckBookStatus -->|Chưa hết hạn| CheckExtended{Đã gia hạn chưa?}
    CheckExtended -->|Đã gia hạn| ErrorAlreadyExtended[Hiển thị: Chỉ được gia hạn 1 lần]
    ErrorAlreadyExtended --> DisplayHistory
    
    CheckExtended -->|Chưa gia hạn| ExtendDays[Gia hạn thêm 7 ngày]
    ExtendDays --> UpdateDueDate[Cập nhật hạn trả]
    UpdateDueDate --> SaveExtend[Lưu thay đổi]
    SaveExtend --> SuccessExtend[Hiển thị: Gia hạn thành công]
    SuccessExtend --> DisplayHistory
    
    RequestReturn --> CheckReturnStatus{Đã có yêu cầu trả<br/>chờ xác nhận?}
    CheckReturnStatus -->|Có| ErrorHasReturn[Hiển thị: Đã có yêu cầu trả chờ xác nhận]
    ErrorHasReturn --> DisplayHistory
    
    CheckReturnStatus -->|Chưa| ShowConfirmModal[Hiển thị modal xác nhận]
    ShowConfirmModal --> UserConfirm{Người dùng xác nhận?}
    UserConfirm -->|Không| DisplayHistory
    UserConfirm -->|Có| CreateReturnRequest[Tạo yêu cầu trả sách<br/>Trạng thái: Chờ xác nhận]
    CreateReturnRequest --> SaveReturn[Lưu yêu cầu]
    SaveReturn --> SuccessReturn[Hiển thị: Yêu cầu trả sách đã được gửi]
    SuccessReturn --> DisplayHistory
    
    ViewRejectReason --> ShowReason[Hiển thị lý do từ chối]
    ShowReason --> DisplayHistory
    
    DisplayHistory --> End([Kết thúc])
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorExpired fill:#ffcdd2
    style ErrorAlreadyExtended fill:#ffcdd2
    style ErrorHasReturn fill:#ffcdd2
    style SuccessExtend fill:#c8e6c9
    style SuccessReturn fill:#c8e6c9
```

## Thông tin hiển thị
- **Sách đang mượn:** Tên, Tác giả, Ngày mượn, Hạn trả, Số ngày còn lại
- **Sách đã trả:** Tên, Ngày mượn, Ngày trả
- **Sách bị từ chối:** Tên, Tác giả, Lý do từ chối

## Chức năng
- Lọc theo trạng thái
- Xem lý do từ chối (nếu có)
- Gia hạn sách (+7 ngày, tối đa 1 lần)
- Tạo yêu cầu trả sách

## Edge Cases
- Sách quá hạn → Hiển thị cảnh báo và số ngày quá hạn
- Đã gia hạn rồi → Không cho phép gia hạn thêm
- Đã có yêu cầu trả chờ xác nhận → Không cho phép tạo yêu cầu mới
- Không có sách nào → Hiển thị thông báo trống

