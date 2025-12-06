# Feature 2.3.1: Mượn Sách - Độc Giả (Borrow Book - Reader)

## Mô tả
Tính năng cho phép độc giả yêu cầu mượn sách từ hệ thống.

## Actor
Độc giả

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)
- 2.2.4 (Cần xem chi tiết sách)

## Flowchart

```mermaid
flowchart TD
    Start([Độc giả xem chi tiết sách]) --> ClickBorrow[Click nút Mượn Sách]
    ClickBorrow --> CheckConditions{Kiểm tra điều kiện}
    
    CheckConditions --> CheckAvailable{Sách còn sẵn?<br/>Số lượng > 0}
    CheckAvailable -->|Không| ErrorUnavailable[Hiển thị: Sách đã hết]
    ErrorUnavailable --> End([Kết thúc])
    
    CheckAvailable -->|Có| CheckLimit{Đã mượn quá 5 cuốn?}
    CheckLimit -->|Có| ErrorLimit[Hiển thị: Đã đạt giới hạn mượn sách]
    ErrorLimit --> End
    
    CheckLimit -->|Chưa| CheckFine{Có khoản phạt<br/>chưa thanh toán?}
    CheckFine -->|Có| ErrorFine[Hiển thị: Cần thanh toán phạt trước]
    ErrorFine --> End
    
    CheckFine -->|Không| CheckAlreadyBorrowed{Đã mượn sách này<br/>chưa trả?}
    CheckAlreadyBorrowed -->|Có| ErrorAlready[Hiển thị: Đã mượn sách này rồi]
    ErrorAlready --> End
    
    CheckAlreadyBorrowed -->|Chưa| ShowBorrowForm[Hiển thị form mượn sách]
    ShowBorrowForm --> SelectDuration[Chọn thời hạn mượn<br/>Mặc định: 14 ngày<br/>Tối đa: 30 ngày]
    SelectDuration --> ValidateDuration{Thời hạn hợp lệ?}
    
    ValidateDuration -->|> 30 ngày| AutoLimit[Tự động giới hạn về 30 ngày]
    AutoLimit --> CreateRequest
    
    ValidateDuration -->|Hợp lệ| CreateRequest[Tạo đơn mượn<br/>Trạng thái: Chờ xác nhận]
    CreateRequest --> SaveDB[Lưu vào database]
    SaveDB --> Success[Hiển thị: Yêu cầu mượn sách đã được gửi]
    Success --> End
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorUnavailable fill:#ffcdd2
    style ErrorLimit fill:#ffcdd2
    style ErrorFine fill:#ffcdd2
    style ErrorAlready fill:#ffcdd2
    style Success fill:#c8e6c9
```

## Điều kiện mượn sách
1. Sách còn sẵn (số lượng > 0)
2. Độc giả chưa vượt quá số sách mượn tối đa (mặc định 5 cuốn)
3. Không có khoản phạt chưa thanh toán
4. Chưa mượn sách này (đang mượn)

## Edge Cases
- Sách hết → Thông báo sách đã hết
- Đã mượn quá 5 cuốn → Thông báo đã đạt giới hạn
- Có phạt chưa thanh toán → Thông báo cần thanh toán phạt trước
- Đã mượn sách này rồi → Có thể ngăn chặn hoặc cho phép
- Thời hạn mượn > 30 ngày → Tự động giới hạn về 30 ngày

