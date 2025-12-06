# Feature 2.7.2: Báo Cáo Chi Tiết (Detailed Reports)

## Mô tả
Tính năng cho phép quản lý viên và nhân viên thư viện xem và xuất các báo cáo chi tiết.

## Actor
Quản lý viên, Nhân viên thư viện

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)
- 2.7.1 (Cần có dashboard và dữ liệu đầy đủ)

## Flowchart

```mermaid
flowchart TD
    Start([Người dùng đăng nhập]) --> CheckRole{Vai trò?}
    
    CheckRole -->|Quản lý viên| AccessReport[Truy cập trang Báo cáo]
    CheckRole -->|Nhân viên thư viện| AccessReport
    CheckRole -->|Độc giả| ErrorAccess[Không có quyền truy cập]
    ErrorAccess --> End([Kết thúc])
    
    AccessReport --> DisplayReportTypes[Hiển thị các loại báo cáo:<br/>- Báo cáo Sách<br/>- Báo cáo Mượn Trả<br/>- Báo cáo Phạt<br/>- Báo cáo Sách Mất/Hư]
    
    DisplayReportTypes --> SelectReportType[Chọn loại báo cáo]
    SelectReportType --> ShowTimeFilter[Hiển thị bộ lọc thời gian]
    ShowTimeFilter --> SelectTimeRange[Chọn khoảng thời gian:<br/>- Ngày<br/>- Tuần<br/>- Tháng<br/>- Quý<br/>- Năm]
    
    SelectTimeRange --> InputDateRange[Nhập ngày bắt đầu và kết thúc]
    InputDateRange --> ValidateDateRange{Khoảng thời gian hợp lệ?}
    
    ValidateDateRange -->|Không| ErrorInvalidDate[Hiển thị: Khoảng thời gian không hợp lệ]
    ErrorInvalidDate --> InputDateRange
    
    ValidateDateRange -->|Có| ApplyFilter[Áp dụng bộ lọc]
    ApplyFilter --> LoadReportData[Tải dữ liệu báo cáo]
    LoadReportData --> CheckData{Có dữ liệu trong khoảng thời gian?}
    
    CheckData -->|Không| ShowNoData[Hiển thị: Không có dữ liệu trong khoảng thời gian này]
    ShowNoData --> SelectTimeRange
    
    CheckData -->|Có| ProcessReport[Xử lý dữ liệu báo cáo]
    ProcessReport --> DisplayReport[Hiển thị báo cáo]
    
    DisplayReport --> CheckReportType{Loại báo cáo?}
    
    CheckReportType -->|Báo cáo Sách| ShowBookReport[Hiển thị:<br/>- Tổng số sách<br/>- Tình trạng sách<br/>- Số lần mượn]
    CheckReportType -->|Báo cáo Mượn Trả| ShowBorrowReport[Hiển thị:<br/>- Số lần mượn/trả<br/>- Theo ngày/tháng/quý]
    CheckReportType -->|Báo cáo Phạt| ShowFineReport[Hiển thị:<br/>- Tổng doanh thu phạt<br/>- Người nợ ngoài hạn]
    CheckReportType -->|Báo cáo Sách Mất/Hư| ShowLostDamagedReport[Hiển thị:<br/>- Danh sách sách cần thay thế]
    
    ShowBookReport --> UserAction{Chọn hành động}
    ShowBorrowReport --> UserAction
    ShowFineReport --> UserAction
    ShowLostDamagedReport --> UserAction
    
    UserAction -->|Xuất CSV| ClickExport[Click Xuất CSV]
    UserAction -->|Thay đổi bộ lọc| SelectTimeRange
    UserAction -->|Xem chi tiết| ViewDetail[Xem chi tiết từng mục]
    
    ClickExport --> CheckPermission{Có quyền xuất báo cáo?}
    CheckPermission -->|Không| ErrorNoPermission[Ẩn nút xuất hoặc hiển thị lỗi]
    ErrorNoPermission --> DisplayReport
    
    CheckPermission -->|Có| GenerateCSV[Tạo file CSV]
    GenerateCSV --> CheckFileSize{Kích thước file?}
    CheckFileSize -->|Quá lớn| WarningLargeFile[Cảnh báo: File quá lớn<br/>Có thể giới hạn hoặc cảnh báo]
    WarningLargeFile --> UserConfirmLarge{Người dùng vẫn muốn xuất?}
    UserConfirmLarge -->|Không| DisplayReport
    UserConfirmLarge -->|Có| DownloadFile[Tải file CSV về máy]
    CheckFileSize -->|Bình thường| DownloadFile
    
    DownloadFile --> SuccessExport[Hiển thị: Xuất báo cáo thành công]
    SuccessExport --> DisplayReport
    
    ViewDetail --> NavigateDetail[Chuyển đến trang chi tiết]
    NavigateDetail --> End
    
    DisplayReport --> End
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorAccess fill:#ffcdd2
    style ErrorInvalidDate fill:#ffcdd2
    style ShowNoData fill:#fff9c4
    style ErrorNoPermission fill:#ffcdd2
    style WarningLargeFile fill:#fff9c4
    style SuccessExport fill:#c8e6c9
```

## Các báo cáo có sẵn
1. **Báo cáo Sách:** Tổng số sách, tình trạng, số lần mượn
2. **Báo cáo Mượn Trả:** Số lần mượn/trả theo ngày/tháng/quý
3. **Báo cáo Phạt:** Tổng doanh thu phạt, người nợ ngoài hạn
4. **Báo cáo Sách Mất/Hư:** Danh sách sách cần thay thế

## Tính năng
- Xuất báo cáo ra CSV
- Lọc theo khoảng thời gian (Ngày, Tuần, Tháng, Quý, Năm)

## Edge Cases
- Khoảng thời gian không hợp lệ → Validation và thông báo lỗi
- File CSV quá lớn → Có thể giới hạn hoặc cảnh báo
- Không có quyền xuất báo cáo → Ẩn nút xuất
- Không có dữ liệu trong khoảng thời gian → Hiển thị thông báo
- Lỗi khi xuất file → Hiển thị thông báo lỗi

