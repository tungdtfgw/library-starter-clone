# Design Flowcharts - Hệ Thống Quản Lý Thư Viện

Thư mục này chứa các flowchart chi tiết cho tất cả các tính năng của hệ thống quản lý thư viện.

## Cấu trúc

### Tài liệu phân tích
- **00-feature-analysis.md**: Phân tích tổng quan tất cả các tính năng, luồng người dùng, và edge cases

### Flowcharts theo nhóm tính năng

#### 1. Quản lý Tài Khoản (Account Management)
- `feature.2.1.1-dang-ky.md` - Đăng Ký
- `feature.2.1.2-dang-nhap.md` - Đăng Nhập
- `feature.2.1.3-ho-so-ca-nhan.md` - Hồ Sơ Cá Nhân

#### 2. Quản lý Sách (Book Management)
- `feature.2.2.1-quan-ly-the-loai-sach.md` - Quản lý thể loại sách
- `feature.2.2.2-them-sach-moi.md` - Thêm Sách Mới
- `feature.2.2.3-xem-danh-sach-sach.md` - Xem Danh Sách Sách
- `feature.2.2.4-xem-chi-tiet-sach.md` - Xem Chi Tiết Sách
- `feature.2.2.5-sua-xoa-sach.md` - Sửa & Xóa Sách

#### 3. Quản lý Mượn Sách (Borrowing Management)
- `feature.2.3.1-muon-sach-doc-gia.md` - Mượn Sách (Độc Giả)
- `feature.2.3.2-muon-sach-nhan-vien.md` - Mượn Sách (Nhân Viên)
- `feature.2.3.3-xem-lich-su-muon-sach.md` - Xem Lịch Sử Mượn Sách

#### 4. Trả Sách (Return Book)
- `feature.2.4.1-yeu-cau-tra-sach.md` - Yêu Cầu Trả Sách
- `feature.2.4.2-xac-nhan-tra-sach.md` - Xác Nhận Trả Sách

#### 5. Quản lý Nợ & Phạt (Fine Management)
- `feature.2.5.1-quan-ly-muc-phat.md` - Quản lý mức phạt
- `feature.2.5.2-xem-thanh-toan-phat-doc-gia.md` - Xem & Thanh Toán Phạt (Độc Giả)
- `feature.2.5.3-xem-thanh-toan-phat-nhan-vien.md` - Xem & Thanh Toán Phạt (Nhân Viên)

#### 6. Quản lý Người Dùng (User Management)
- `feature.2.6.1-danh-sach-nguoi-dung.md` - Danh Sách Người Dùng
- `feature.2.6.2-gan-vai-tro.md` - Gán Vai Trò

#### 7. Báo Cáo & Thống Kê (Reports & Statistics)
- `feature.2.7.1-bao-cao-tong-quan.md` - Báo Cáo Tổng Quan (Dashboard)
- `feature.2.7.2-bao-cao-chi-tiet.md` - Báo Cáo Chi Tiết

## Tổng số tính năng: 20

## Cách sử dụng

Mỗi file flowchart chứa:
- **Mô tả**: Mô tả ngắn gọn về tính năng
- **Actor**: Người dùng có thể sử dụng tính năng
- **Phụ thuộc**: Các tính năng cần có trước
- **Flowchart**: Sơ đồ luồng sử dụng Mermaid syntax
- **Validation Rules**: Các quy tắc validation
- **Edge Cases**: Các trường hợp đặc biệt cần xử lý

## Xem Flowchart

Các flowchart được viết bằng Mermaid syntax và có thể được xem trên:
- GitHub (tự động render)
- VS Code với extension Mermaid Preview
- Các công cụ online như mermaid.live

## Thứ tự triển khai

Tham khảo file `00-feature-analysis.md` để xem thứ tự triển khai được đề xuất theo các giai đoạn.

