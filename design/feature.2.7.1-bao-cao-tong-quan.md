# Feature 2.7.1: Báo Cáo Tổng Quan - Dashboard

## Mô tả
Tính năng hiển thị dashboard với các thống kê tổng quan về hệ thống thư viện.

## Actor
Quản lý viên, Nhân viên thư viện

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)
- 2.2.2 (Cần có dữ liệu sách)
- 2.3.1 (Cần có dữ liệu mượn)
- 2.4.2 (Cần có dữ liệu trả)

## Flowchart

```mermaid
flowchart TD
    Start([Người dùng đăng nhập]) --> CheckRole{Vai trò?}
    
    CheckRole -->|Quản lý viên| AccessDashboardAdmin[Truy cập Dashboard]
    CheckRole -->|Nhân viên thư viện| AccessDashboardLibrarian[Truy cập Dashboard]
    CheckRole -->|Độc giả| ErrorAccess[Không có quyền truy cập]
    ErrorAccess --> End([Kết thúc])
    
    AccessDashboardAdmin --> LoadData[Tải dữ liệu thống kê]
    AccessDashboardLibrarian --> LoadData
    
    LoadData --> CheckData{Có dữ liệu?}
    CheckData -->|Không| ShowEmpty[Hiển thị: Chưa có dữ liệu]
    ShowEmpty --> End
    
    CheckData -->|Có| ProcessStats[Xử lý thống kê]
    ProcessStats --> DisplayStats[Hiển thị các thống kê]
    
    DisplayStats --> ShowBooks[Hiển thị Tổng số sách:<br/>- Có sẵn<br/>- Đang mượn<br/>- Bị mất<br/>- Hư hỏng]
    DisplayStats --> ShowReaders[Hiển thị Tổng số độc giả:<br/>- Hoạt động<br/>- Vô hiệu hóa]
    DisplayStats --> ShowTodayBorrows[Hiển thị Tổng đơn mượn hôm nay]
    DisplayStats --> ShowTopBooks[Hiển thị Top 5 sách phổ biến nhất]
    DisplayStats --> ShowOverdue[Hiển thị Danh sách độc giả nợ quá hạn]
    
    ShowBooks --> CheckError{Có lỗi khi tải?}
    ShowReaders --> CheckError
    ShowTodayBorrows --> CheckError
    ShowTopBooks --> CheckError
    ShowOverdue --> CheckError
    
    CheckError -->|Có| ShowError[Hiển thị thông báo lỗi]
    ShowError --> End
    
    CheckError -->|Không| DisplayComplete[Dashboard hiển thị đầy đủ]
    DisplayComplete --> UserAction{Chọn hành động}
    
    UserAction -->|Xem chi tiết| NavigateDetail[Chuyển đến trang chi tiết]
    UserAction -->|Làm mới| Refresh[Làm mới dữ liệu]
    UserAction -->|Xem báo cáo chi tiết| NavigateReport[Chuyển đến Báo cáo chi tiết]
    
    Refresh --> LoadData
    NavigateDetail --> End
    NavigateReport --> End
    
    DisplayComplete --> End
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorAccess fill:#ffcdd2
    style ShowEmpty fill:#fff9c4
    style ShowError fill:#ffcdd2
    style DisplayComplete fill:#c8e6c9
```

## Thông tin hiển thị
- **Tổng số sách:** Có sẵn / Đang mượn / Bị mất / Hư hỏng
- **Tổng số độc giả:** Hoạt động / Vô hiệu hóa
- **Tổng đơn mượn hôm nay**
- **Top 5 sách phổ biến nhất**
- **Danh sách độc giả nợ quá hạn**

## Edge Cases
- Không có dữ liệu → Hiển thị 0 hoặc "Chưa có dữ liệu"
- Lỗi khi tải dữ liệu → Hiển thị thông báo lỗi
- Dữ liệu rỗng → Hiển thị placeholder
- Thời gian tải lâu → Hiển thị loading indicator

