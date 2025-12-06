# Feature 2.2.2: Thêm Sách Mới (Add New Book)

## Mô tả
Tính năng cho phép nhân viên thư viện thêm sách mới vào hệ thống.

## Actor
Nhân viên thư viện

## Phụ thuộc
- 2.1.2 (Cần đăng nhập)
- 2.2.1 (Cần có thể loại sách)

## Flowchart

```mermaid
flowchart TD
    Start([Nhân viên đăng nhập]) --> ClickAdd[Click Thêm Sách Mới]
    ClickAdd --> ShowForm[Hiển thị form thêm sách]
    ShowForm --> InputData[Nhập thông tin:<br/>- Tên sách<br/>- Tác giả<br/>- Năm xuất bản<br/>- ISBN<br/>- Thể loại<br/>- Mô tả<br/>- Số lượng]
    
    InputData --> Validate{Validate tất cả trường}
    
    Validate -->|Tên sách để trống hoặc > 100 ký tự| ErrorTitle[Hiển thị lỗi: Tên sách không hợp lệ]
    ErrorTitle --> InputData
    
    Validate -->|Tác giả để trống hoặc > 100 ký tự| ErrorAuthor[Hiển thị lỗi: Tác giả không hợp lệ]
    ErrorAuthor --> InputData
    
    Validate -->|ISBN không đúng định dạng ISBN-10/13| ErrorISBN[Hiển thị lỗi: ISBN không hợp lệ]
    ErrorISBN --> InputData
    
    Validate -->|Năm xuất bản < 1900 hoặc > năm hiện tại| ErrorYear[Hiển thị lỗi: Năm xuất bản không hợp lệ]
    ErrorYear --> InputData
    
    Validate -->|Thể loại không tồn tại| ErrorCategory[Hiển thị lỗi: Thể loại không hợp lệ]
    ErrorCategory --> InputData
    
    Validate -->|Mô tả để trống hoặc > 255 ký tự| ErrorDesc[Hiển thị lỗi: Mô tả không hợp lệ]
    ErrorDesc --> InputData
    
    Validate -->|Số lượng <= 0 hoặc không phải số| ErrorQuantity[Hiển thị lỗi: Số lượng không hợp lệ]
    ErrorQuantity --> InputData
    
    Validate -->|Tất cả hợp lệ| CreateBook[Tạo sách mới<br/>Trạng thái: Có sẵn]
    CreateBook --> SaveDB[Lưu vào database]
    SaveDB --> Success[Hiển thị: Thêm sách thành công]
    Success --> End([Kết thúc])
    
    style Start fill:#e1f5ff
    style End fill:#c8e6c9
    style ErrorTitle fill:#ffcdd2
    style ErrorAuthor fill:#ffcdd2
    style ErrorISBN fill:#ffcdd2
    style ErrorYear fill:#ffcdd2
    style ErrorCategory fill:#ffcdd2
    style ErrorDesc fill:#ffcdd2
    style ErrorQuantity fill:#ffcdd2
    style Success fill:#c8e6c9
```

## Validation Rules
- **Tên sách:** Không được để trống, tối đa 100 ký tự
- **Tác giả:** Không được để trống, tối đa 100 ký tự
- **ISBN:** Định dạng chuẩn ISBN-10 hoặc ISBN-13 (nếu có)
- **Năm xuất bản:** Năm hợp lệ (1900 - năm hiện tại)
- **Thể loại:** Phải có thể loại sách (từ danh sách)
- **Mô tả:** Không được để trống, tối đa 255 ký tự
- **Số lượng:** Phải > 0, kiểu số

## Edge Cases
- ISBN không đúng định dạng (ISBN-10 hoặc ISBN-13)
- Năm xuất bản không hợp lệ (<1900 hoặc >năm hiện tại)
- Số lượng <= 0
- Thể loại không tồn tại
- ISBN đã tồn tại (có thể cho phép thêm bản sao hoặc cảnh báo)

