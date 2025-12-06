# Feature 2.5.3: Xem & Thanh Toán Phạt - Nhân Viên (View & Pay Fine - Librarian)

## Mô tả
Tính năng cho phép nhân viên thư viện xem và xác nhận/từ chối thanh toán phạt từ độc giả.

## Actor
Nhân viên thư viện

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)
- 2.5.2 (Cần có phiếu phạt chờ xác nhận)

## Flowchart

```mermaid
flowchart TD
    Start([Nhân viên đăng nhập]) --> AccessFine[Truy cập trang Quản lý khoản phạt]
    AccessFine --> LoadFines[Tải danh sách khoản phạt]
    LoadFines --> DisplayList[Hiển thị danh sách:<br/>- Chưa thanh toán<br/>- Chờ xác nhận<br/>- Đã thanh toán<br/>- Từ chối]
    
    DisplayList --> SelectFine[Chọn khoản phạt]
    SelectFine --> ViewDetail[Xem chi tiết khoản phạt:<br/>- Độc giả<br/>- Nguyên nhân phạt<br/>- Số tiền<br/>- Ngày phạt<br/>- Trạng thái<br/>- Thông tin chuyển khoản]
    
    ViewDetail --> CheckStatus{Trạng thái phiếu phạt?}
    
    CheckStatus -->|Đã thanh toán| ShowPaid[Hiển thị: Đã thanh toán]
    ShowPaid --> DisplayList
    
    CheckStatus -->|Từ chối| ShowRejected[Hiển thị lý do từ chối]
    ShowRejected --> DisplayList
    
    CheckStatus -->|Chờ xác nhận| CheckPayment[Kiểm tra số tiền đã chuyển khoản]
    CheckPayment --> CompareAmount{So sánh số tiền:<br/>Số tiền chuyển khoản<br/>với Số tiền phạt}
    
    CompareAmount -->|Số tiền đúng| ConfirmPayment[Click Đã thanh toán]
    CompareAmount -->|Số tiền không đúng| RejectPayment[Click Từ chối]
    
    ConfirmPayment --> UpdateConfirm[Cập nhật trạng thái:<br/>Chờ xác nhận → Đã thanh toán]
    UpdateConfirm --> SaveConfirm[Lưu thay đổi]
    SaveConfirm --> SuccessConfirm[Hiển thị: Xác nhận thanh toán thành công]
    SuccessConfirm --> DisplayList
    
    RejectPayment --> ShowRejectForm[Hiển thị form từ chối]
    ShowRejectForm --> InputReason[Nhập lý do từ chối<br/>Bắt buộc]
    InputReason --> ValidateReason{Lý do để trống?}
    
    ValidateReason -->|Có| ErrorEmptyReason[Hiển thị: Vui lòng nhập lý do từ chối]
    ErrorEmptyReason --> InputReason
    
    ValidateReason -->|Không| UpdateReject[Cập nhật trạng thái:<br/>Chờ xác nhận → Từ chối]
    UpdateReject --> SaveReason[Lưu lý do từ chối]
    SaveReason --> SaveReject[Lưu thay đổi]
    SaveReject --> SuccessReject[Hiển thị: Từ chối thanh toán thành công]
    SuccessReject --> DisplayList
    
    DisplayList --> End([Kết thúc])
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ShowPaid fill:#fff9c4
    style ShowRejected fill:#fff9c4
    style ErrorEmptyReason fill:#ffcdd2
    style SuccessConfirm fill:#c8e6c9
    style SuccessReject fill:#c8e6c9
```

## Validation Rules
- **Lý do từ chối:** Bắt buộc phải nhập

## Edge Cases
- Phiếu phạt đã được xử lý → Không cho phép xác nhận/từ chối lại
- Số tiền thừa → Có thể chấp nhận hoặc từ chối
- Số tiền thiếu → Từ chối và yêu cầu thanh toán lại
- Lý do từ chối để trống → Yêu cầu nhập lý do

