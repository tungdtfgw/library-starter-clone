# Feature 2.5.2: Xem & Thanh Toán Phạt - Độc Giả (View & Pay Fine - Reader)

## Mô tả
Tính năng cho phép độc giả xem danh sách khoản phạt và thanh toán phạt.

## Actor
Độc giả

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)
- 2.4.2 (Cần có phiếu phạt)
- 2.5.1 (Cần có mức phạt)

## Flowchart

```mermaid
flowchart TD
    Start([Độc giả đăng nhập]) --> AccessFine[Truy cập trang Khoản phạt]
    AccessFine --> LoadFines[Tải danh sách khoản phạt]
    LoadFines --> CheckFines{Có khoản phạt?}
    
    CheckFines -->|Không| ShowEmpty[Hiển thị: Không có khoản phạt nào]
    ShowEmpty --> End([Kết thúc])
    
    CheckFines -->|Có| DisplayList[Hiển thị danh sách khoản phạt:<br/>- Nguyên nhân phạt<br/>- Số tiền<br/>- Ngày phạt<br/>- Trạng thái]
    
    DisplayList --> FilterStatus{Lọc theo trạng thái?}
    FilterStatus -->|Có| ApplyFilter[Áp dụng bộ lọc:<br/>- Chưa thanh toán<br/>- Chờ xác nhận<br/>- Đã thanh toán<br/>- Từ chối]
    ApplyFilter --> DisplayFiltered[Hiển thị kết quả đã lọc]
    DisplayFiltered --> DisplayList
    
    FilterStatus -->|Không| SelectFine[Chọn phiếu phạt]
    SelectFine --> CheckStatus{Trạng thái phiếu phạt?}
    
    CheckStatus -->|Đã thanh toán| ShowPaid[Hiển thị: Phiếu phạt đã được thanh toán]
    ShowPaid --> DisplayList
    
    CheckStatus -->|Chờ xác nhận| ShowPending[Hiển thị: Đang chờ nhân viên xác nhận]
    ShowPending --> DisplayList
    
    CheckStatus -->|Từ chối| ShowRejected[Hiển thị lý do từ chối<br/>Có thể thanh toán lại]
    ShowRejected --> ClickPayAgain[Click Thanh toán lại]
    ClickPayAgain --> ProcessPayment
    
    CheckStatus -->|Chưa thanh toán| ClickPay[Click Thanh toán]
    ClickPay --> ShowPaymentInfo[Hiển thị thông tin thanh toán:<br/>- Số tiền<br/>- Số tài khoản ngân hàng<br/>- Nội dung chuyển khoản]
    
    ShowPaymentInfo --> ProcessPayment[Thực hiện chuyển khoản<br/>Bên ngoài hệ thống]
    ProcessPayment --> UserConfirmPayment{Đã chuyển khoản?}
    
    UserConfirmPayment -->|Chưa| WaitPayment[Chờ người dùng chuyển khoản]
    WaitPayment --> UserConfirmPayment
    
    UserConfirmPayment -->|Có| ClickConfirm[Click Đã thanh toán]
    ClickConfirm --> UpdateStatus[Cập nhật trạng thái:<br/>Chưa thanh toán → Chờ xác nhận]
    UpdateStatus --> SaveDB[Lưu vào database]
    SaveDB --> Success[Hiển thị: Yêu cầu thanh toán đã được gửi<br/>Vui lòng chờ nhân viên xác nhận]
    Success --> DisplayList
    
    DisplayList --> End
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ShowEmpty fill:#fff9c4
    style ShowPaid fill:#fff9c4
    style ShowPending fill:#fff9c4
    style Success fill:#c8e6c9
```

## Thông tin hiển thị
- Danh sách khoản phạt chưa thanh toán
- Nguyên nhân phạt, Số tiền, Ngày phạt, Trạng thái

## Edge Cases
- Phiếu phạt đã hết hạn → Có thể hiển thị cảnh báo
- Nhiều phiếu phạt chưa thanh toán → Hiển thị tổng số tiền
- Phiếu phạt đã thanh toán → Không hiển thị nút thanh toán
- Phiếu phạt bị từ chối → Cho phép thanh toán lại

