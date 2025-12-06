# Feature 2.2.3: Xem Danh Sách Sách (View Book List)

## Mô tả
Tính năng cho phép tất cả người dùng (không cần đăng nhập) xem danh sách sách với các chức năng tìm kiếm, lọc, sắp xếp.

## Actor
Tất cả người dùng (không cần login)

## Phụ thuộc
Không có

## Flowchart

```mermaid
flowchart TD
    Start([Người dùng truy cập trang Danh sách sách]) --> LoadBooks[Tải danh sách sách]
    LoadBooks --> DisplayList[Hiển thị danh sách:<br/>- Tên sách<br/>- Tác giả<br/>- Năm xuất bản<br/>- Thể loại<br/>- Số lượng có sẵn<br/>- Số lượng đang mượn]
    
    DisplayList --> UserAction{Chọn hành động}
    
    UserAction -->|Tìm kiếm| Search[Click Tìm kiếm]
    UserAction -->|Lọc| Filter[Chọn thể loại từ dropdown]
    UserAction -->|Sắp xếp| Sort[Chọn tiêu chí sắp xếp]
    UserAction -->|Phân trang| Paginate[Click trang số]
    UserAction -->|Xem chi tiết| ViewDetail[Click vào sách]
    
    Search --> InputKeyword[Nhập từ khóa<br/>Tên sách hoặc Tác giả]
    InputKeyword --> ExecuteSearch[Thực hiện tìm kiếm]
    ExecuteSearch --> CheckResults{Có kết quả?}
    CheckResults -->|Có| DisplayResults[Hiển thị kết quả tìm kiếm]
    CheckResults -->|Không| ShowNoResults[Hiển thị: Không tìm thấy kết quả]
    DisplayResults --> DisplayList
    ShowNoResults --> DisplayList
    
    Filter --> SelectCategory[Chọn thể loại]
    SelectCategory --> ApplyFilter[Áp dụng bộ lọc]
    ApplyFilter --> CheckFilterResults{Có sách trong thể loại?}
    CheckFilterResults -->|Có| DisplayFiltered[Hiển thị sách đã lọc]
    CheckFilterResults -->|Không| ShowNoFilter[Hiển thị: Không có sách trong thể loại này]
    DisplayFiltered --> DisplayList
    ShowNoFilter --> DisplayList
    
    Sort --> SelectSort[Chọn tiêu chí:<br/>- Tên A-Z<br/>- Năm xuất bản Mới nhất<br/>- Lượt mượn Phổ biến nhất]
    SelectSort --> ApplySort[Áp dụng sắp xếp]
    ApplySort --> DisplaySorted[Hiển thị sách đã sắp xếp]
    DisplaySorted --> DisplayList
    
    Paginate --> LoadPage[Tải trang mới<br/>10 sách/trang]
    LoadPage --> DisplayList
    
    ViewDetail --> NavigateDetail[Chuyển đến trang Chi tiết sách]
    NavigateDetail --> End([Kết thúc])
    
    DisplayList --> End
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ShowNoResults fill:#fff9c4
    style ShowNoFilter fill:#fff9c4
    style DisplayResults fill:#c8e6c9
    style DisplayFiltered fill:#c8e6c9
    style DisplaySorted fill:#c8e6c9
```

## Chức năng
- **Tìm kiếm:** Theo tên sách hoặc tác giả
- **Lọc:** Theo thể loại
- **Sắp xếp:** 
  - Tên (A-Z)
  - Năm xuất bản (Mới nhất)
  - Lượt mượn (Phổ biến nhất)
- **Phân trang:** 10 sách trên 1 trang

## Edge Cases
- Không có kết quả tìm kiếm
- Không có sách nào trong thể loại được chọn
- Trang trống (không có sách)
- Kết hợp nhiều bộ lọc cùng lúc

