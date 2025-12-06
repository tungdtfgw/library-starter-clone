# Feature 2.4.2: Xác Nhận Trả Sách (Confirm Return)

## Mô tả
Tính năng cho phép nhân viên thư viện xác nhận trả sách và xử lý các trường hợp sách bình thường, hư hỏng, hoặc mất.

## Actor
Nhân viên thư viện

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)
- 2.4.1 (Cần có yêu cầu trả sách)

## Flowchart

```mermaid
flowchart TD
    Start([Nhân viên đăng nhập]) --> AccessReturn[Vào trang Quản lý mượn trả]
    AccessReturn --> SelectTab[Chọn tab Chờ xác nhận trả]
    SelectTab --> LoadPending[Tải danh sách yêu cầu trả sách chờ xác nhận]
    LoadPending --> DisplayList[Hiển thị danh sách yêu cầu trả]
    
    DisplayList --> SelectReturn[Chọn yêu cầu trả sách]
    SelectReturn --> ViewDetail[Xem chi tiết yêu cầu trả]
    ViewDetail --> ReceiveBook[Nhận sách vật lý từ độc giả]
    ReceiveBook --> ClickConfirm[Click Xác nhận trả]
    
    ClickConfirm --> SelectCondition[Chọn tình trạng sách]
    SelectCondition --> CheckCondition{Tình trạng sách?}
    
    CheckCondition -->|Bình thường| ProcessNormal[Xử lý trả bình thường]
    CheckCondition -->|Hư hỏng| ProcessDamaged[Xử lý trả hư hỏng]
    CheckCondition -->|Mất| ProcessLost[Xử lý trả mất]
    
    ProcessNormal --> CheckLate{Trả muộn?}
    CheckLate -->|Không| UpdateNormal[Cập nhật đơn mượn: Đã trả<br/>Tăng số lượng sách có sẵn<br/>Giảm số lượng sách đang mượn]
    CheckLate -->|Có| CreateLateFine[Tạo phiếu phạt Trả muộn]
    CreateLateFine --> UpdateNormal
    
    UpdateNormal --> SaveNormal[Lưu thay đổi]
    SaveNormal --> SuccessNormal[Hiển thị: Xác nhận trả sách thành công]
    SuccessNormal --> DisplayList
    
    ProcessDamaged --> SelectFineLevel[Chọn mức phạt từ danh sách]
    SelectFineLevel --> ValidateFine{Mức phạt hợp lệ?}
    ValidateFine -->|Không| ErrorNoFine[Hiển thị: Vui lòng chọn mức phạt]
    ErrorNoFine --> SelectFineLevel
    
    ValidateFine -->|Có| InputNote[Nhập ghi chú<br/>Bắt buộc]
    InputNote --> ValidateNote{Ghi chú để trống?}
    ValidateNote -->|Có| ErrorEmptyNote[Hiển thị: Vui lòng nhập ghi chú]
    ErrorEmptyNote --> InputNote
    
    ValidateNote -->|Không| CheckLateDamaged{Trả muộn?}
    CheckLateDamaged -->|Có| CreateLateFineDamaged[Tạo phiếu phạt Trả muộn]
    CheckLateDamaged -->|Không| CreateDamagedFine[Tạo phiếu phạt Hư hỏng]
    CreateLateFineDamaged --> CreateDamagedFine
    
    CreateDamagedFine --> UpdateDamaged[Cập nhật đơn mượn: Đã trả<br/>Tăng số lượng sách có sẵn<br/>Giảm số lượng sách đang mượn]
    UpdateDamaged --> SaveDamaged[Lưu thay đổi]
    SaveDamaged --> SuccessDamaged[Hiển thị: Xác nhận trả sách thành công]
    SuccessDamaged --> DisplayList
    
    ProcessLost --> SelectFineLevelLost[Chọn mức phạt từ danh sách]
    SelectFineLevelLost --> ValidateFineLost{Mức phạt hợp lệ?}
    ValidateFineLost -->|Không| ErrorNoFine
    ValidateFineLost -->|Có| InputNoteLost[Nhập ghi chú<br/>Bắt buộc]
    InputNoteLost --> ValidateNoteLost{Ghi chú để trống?}
    ValidateNoteLost -->|Có| ErrorEmptyNote
    ValidateNoteLost -->|Không| CreateLostFine[Tạo phiếu phạt Mất sách]
    
    CreateLostFine --> UpdateLost[Cập nhật đơn mượn: Đã trả<br/>Giảm số lượng sách đang mượn<br/>KHÔNG tăng số lượng sách có sẵn]
    UpdateLost --> SaveLost[Lưu thay đổi]
    SaveLost --> SuccessLost[Hiển thị: Xác nhận trả sách thành công]
    SuccessLost --> DisplayList
    
    DisplayList --> End([Kết thúc])
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorNoFine fill:#ffcdd2
    style ErrorEmptyNote fill:#ffcdd2
    style SuccessNormal fill:#c8e6c9
    style SuccessDamaged fill:#c8e6c9
    style SuccessLost fill:#c8e6c9
```

## Validation Rules
- **Tình trạng sách:** Bắt buộc chọn (bình thường, hư hỏng, mất)
- **Mức phạt:** Bắt buộc khi tình trạng là "hư hỏng", trả muộn hoặc "mất"
- **Ghi chú:** Bắt buộc khi tình trạng là "hư hỏng" hoặc "mất", tối đa 500 ký tự

## Edge Cases
- Sách trả muộn + hư hỏng → Tạo 2 phiếu phạt (muộn + hư hỏng)
- Sách trả muộn + mất → Tạo 2 phiếu phạt (muộn + mất)
- Sách trả sớm → Không tạo phiếu phạt muộn
- Mức phạt không tồn tại → Thông báo lỗi
- Không có mức phạt nào → Yêu cầu tạo mức phạt trước

