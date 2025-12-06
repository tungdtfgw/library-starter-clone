# Phân Tích Tính Năng - Hệ Thống Quản Lý Thư Viện

## Tổng Quan
Tài liệu này phân tích tất cả các tính năng từ PRD và liệt kê các luồng người dùng, luồng thay thế, và các trường hợp lỗi/edge cases.

---

## 1. QUẢN LÝ TÀI KHOẢN (Account Management)

### 1.1 Đăng Ký (2.1.1)
**Actor:** Mọi người  
**Phụ thuộc:** Không có

**Luồng chính:**
1. Người dùng click "Đăng ký"
2. Nhập thông tin: Email, Tên, Mật khẩu, Xác nhận mật khẩu
3. Hệ thống validate dữ liệu
4. Hệ thống tạo tài khoản với trạng thái "Chờ xác nhận"
5. Hiển thị thông báo thành công

**Luồng thay thế:**
- Validation thất bại → Hiển thị lỗi cụ thể
- Email đã tồn tại → Thông báo email đã được sử dụng

**Edge cases:**
- Email không đúng định dạng
- Mật khẩu quá ngắn/dài
- Mật khẩu và xác nhận không khớp
- Tên để trống hoặc quá dài

---

### 1.2 Đăng Nhập (2.1.2)
**Actor:** Mọi người  
**Phụ thuộc:** 2.1.1 (Cần có tài khoản)

**Luồng chính:**
1. Người dùng nhập Email & Mật khẩu
2. Hệ thống xác thực thông tin
3. Tạo JWT token (thời hạn 24h)
4. Chuyển hướng đến dashboard theo vai trò (Độc giả/Nhân viên)

**Luồng thay thế:**
- Email/Mật khẩu sai → Hiển thị lỗi đăng nhập
- Tài khoản chưa được kích hoạt → Thông báo cần kích hoạt
- Tài khoản bị vô hiệu hóa → Thông báo tài khoản bị khóa

**Edge cases:**
- Email không tồn tại
- Mật khẩu sai
- Token hết hạn (sau 24h)

---

### 1.3 Hồ Sơ Cá Nhân (2.1.3)
**Actor:** Độc giả  
**Phụ thuộc:** 2.1.2 (Cần đăng nhập)

**Luồng chính - Xem hồ sơ:**
1. Độc giả đăng nhập
2. Truy cập trang "Hồ Sơ Cá Nhân"
3. Hiển thị: Tên, Email, Số điện thoại, Địa chỉ, Ngày tham gia, Số lần mượn, Tổng số tiền phạt

**Luồng chính - Cập nhật thông tin:**
1. Click "Cập nhật thông tin"
2. Chỉnh sửa các trường: Tên, Số điện thoại, Địa chỉ
3. Validate dữ liệu
4. Lưu thay đổi
5. Hiển thị thông báo thành công

**Luồng chính - Đổi mật khẩu:**
1. Click "Đổi mật khẩu"
2. Nhập mật khẩu cũ, mật khẩu mới, xác nhận mật khẩu mới
3. Validate
4. Cập nhật mật khẩu
5. Hiển thị thông báo thành công

**Luồng thay thế:**
- Validation thất bại → Hiển thị lỗi
- Mật khẩu cũ sai → Thông báo mật khẩu cũ không đúng

**Edge cases:**
- Số điện thoại không đúng định dạng
- Địa chỉ quá dài
- Mật khẩu mới trùng với mật khẩu cũ

---

## 2. QUẢN LÝ SÁCH (Book Management)

### 2.1 Quản lý thể loại sách (2.2.1)
**Actor:** Nhân viên thư viện  
**Phụ thuộc:** 2.1.2 (Cần đăng nhập)

**Luồng chính - Xem danh sách:**
1. Nhân viên đăng nhập
2. Truy cập trang "Quản lý thể loại"
3. Hiển thị bảng danh sách thể loại

**Luồng chính - Thêm thể loại:**
1. Click "Thêm thể loại sách"
2. Nhập tên thể loại
3. Validate
4. Lưu thể loại mới
5. Hiển thị thông báo thành công

**Luồng chính - Sửa thể loại:**
1. Sửa trực tiếp trên bảng
2. Validate
3. Lưu thay đổi

**Luồng chính - Xóa thể loại:**
1. Click "Xóa" trên bảng
2. Kiểm tra xem có sách nào thuộc thể loại này không
3. Nếu không có → Xác nhận xóa
4. Nếu có → Thông báo không thể xóa

**Luồng thay thế:**
- Có sách thuộc thể loại → Không cho phép xóa
- Tên thể loại trùng → Thông báo trùng tên

**Edge cases:**
- Tên thể loại để trống
- Tên thể loại quá dài (>50 ký tự)

---

### 2.2 Thêm Sách Mới (2.2.2)
**Actor:** Nhân viên thư viện  
**Phụ thuộc:** 2.1.2, 2.2.1

**Luồng chính:**
1. Click "Thêm Sách Mới"
2. Nhập: Tên sách, Tác giả, Năm xuất bản, ISBN, Thể loại, Mô tả, Số lượng
3. Validate tất cả trường
4. Lưu sách với trạng thái "Có sẵn"
5. Hiển thị "Thêm sách thành công"

**Luồng thay thế:**
- Validation thất bại → Hiển thị lỗi cụ thể
- ISBN đã tồn tại → Có thể cảnh báo hoặc cho phép thêm bản sao

**Edge cases:**
- ISBN không đúng định dạng (ISBN-10 hoặc ISBN-13)
- Năm xuất bản không hợp lệ (<1900 hoặc >năm hiện tại)
- Số lượng <= 0
- Thể loại không tồn tại

---

### 2.3 Xem Danh Sách Sách (2.2.3)
**Actor:** Tất cả người dùng (không cần login)  
**Phụ thuộc:** Không có

**Luồng chính:**
1. Truy cập trang "Danh sách sách"
2. Hiển thị: Tên sách, Tác giả, Năm xuất bản, Thể loại, Số lượng có sẵn, Số lượng đang mượn
3. Phân trang (10 sách/trang)

**Luồng tìm kiếm:**
1. Nhập từ khóa (tên sách hoặc tác giả)
2. Click "Tìm kiếm"
3. Hiển thị kết quả

**Luồng lọc:**
1. Chọn thể loại từ dropdown
2. Áp dụng bộ lọc
3. Hiển thị kết quả đã lọc

**Luồng sắp xếp:**
1. Chọn tiêu chí sắp xếp (Tên A-Z, Năm xuất bản, Lượt mượn)
2. Áp dụng sắp xếp
3. Hiển thị kết quả

**Edge cases:**
- Không có kết quả tìm kiếm
- Không có sách nào trong thể loại được chọn
- Trang trống (không có sách)

---

### 2.4 Xem Chi Tiết Sách (2.2.4)
**Actor:** Tất cả người dùng (không cần login)  
**Phụ thuộc:** 2.2.3

**Luồng chính:**
1. Click vào sách từ danh sách
2. Hiển thị: Tên, Tác giả, ISBN, Năm xuất bản, Mô tả, Số lượng có sẵn, Số lượng đang mượn
3. Nếu là nhân viên → Hiển thị lịch sử mượn
4. Nếu là độc giả đã đăng nhập → Hiển thị nút "Mượn sách"

**Edge cases:**
- Sách không tồn tại → 404
- Sách đã bị xóa → Thông báo sách không còn tồn tại

---

### 2.5 Sửa & Xóa Sách (2.2.5)
**Actor:** Nhân viên thư viện  
**Phụ thuộc:** 2.1.2, 2.2.4

**Luồng chính - Sửa sách:**
1. Xem chi tiết sách
2. Click "Sửa"
3. Chỉnh sửa thông tin
4. Validate
5. Lưu thay đổi
6. Hiển thị thông báo thành công

**Luồng chính - Xóa sách:**
1. Xem chi tiết sách
2. Click "Xóa"
3. Kiểm tra xem có đơn mượn hoạt động không
4. Nếu không có → Xác nhận xóa
5. Nếu có → Thông báo không thể xóa

**Luồng thay thế:**
- Có đơn mượn hoạt động → Không cho phép xóa
- Validation thất bại → Hiển thị lỗi

**Edge cases:**
- Sách đang được mượn → Không thể xóa
- Sách có đơn mượn chờ xác nhận → Có thể xóa hoặc không (tùy business logic)

---

## 3. QUẢN LÝ MƯỢN SÁCH (Borrowing Management)

### 3.1 Mượn Sách - Độc Giả (2.3.1)
**Actor:** Độc giả  
**Phụ thuộc:** 2.1.2, 2.2.4

**Luồng chính:**
1. Độc giả xem chi tiết sách
2. Click "Mượn Sách"
3. Kiểm tra điều kiện:
   - Sách còn sẵn (số lượng > 0)
   - Chưa vượt quá số sách mượn tối đa (5 cuốn)
   - Không có khoản phạt chưa thanh toán
4. Nếu đủ điều kiện → Chọn thời hạn mượn (mặc định 14 ngày, tối đa 30 ngày)
5. Tạo đơn mượn với trạng thái "Chờ xác nhận"
6. Hiển thị thông báo thành công

**Luồng thay thế:**
- Sách hết → Thông báo sách đã hết
- Đã mượn quá 5 cuốn → Thông báo đã đạt giới hạn
- Có phạt chưa thanh toán → Thông báo cần thanh toán phạt trước
- Sách không còn sẵn → Thông báo không thể mượn

**Edge cases:**
- Độc giả đã mượn sách này rồi (đang mượn) → Có thể ngăn chặn hoặc cho phép
- Thời hạn mượn > 30 ngày → Tự động giới hạn về 30 ngày

---

### 3.2 Mượn Sách - Nhân Viên (2.3.2)
**Actor:** Nhân viên thư viện  
**Phụ thuộc:** 2.1.2, 2.3.1

**Luồng chính - Xem danh sách chờ xác nhận:**
1. Nhân viên đăng nhập
2. Truy cập trang "Quản lý mượn trả" → Tab "Chờ xác nhận"
3. Hiển thị danh sách đơn mượn chờ xác nhận

**Luồng chính - Xác nhận mượn:**
1. Xem đơn mượn
2. Click "Xác nhận"
3. Cập nhật trạng thái đơn mượn thành "Đã mượn"
4. Giảm số lượng sách có sẵn
5. Tăng số lượng sách đang mượn
6. Hiển thị thông báo thành công

**Luồng chính - Từ chối mượn:**
1. Xem đơn mượn
2. Click "Từ chối"
3. Nhập lý do từ chối (bắt buộc)
4. Validate lý do
5. Cập nhật trạng thái đơn mượn thành "Bị từ chối"
6. Lưu lý do từ chối
7. Hiển thị thông báo thành công

**Luồng thay thế:**
- Lý do từ chối để trống → Yêu cầu nhập lý do
- Sách đã hết sau khi tạo đơn → Có thể từ chối hoặc cảnh báo

**Edge cases:**
- Đơn mượn đã được xử lý → Không cho phép xác nhận/từ chối lại
- Sách đã bị xóa → Từ chối đơn mượn

---

### 3.3 Xem Lịch Sử Mượn Sách (2.3.3)
**Actor:** Độc giả  
**Phụ thuộc:** 2.1.2, 2.3.1

**Luồng chính:**
1. Độc giả đăng nhập
2. Truy cập trang "Lịch sử mượn sách"
3. Hiển thị:
   - Sách đang mượn: Tên, Tác giả, Ngày mượn, Hạn trả, Số ngày còn lại
   - Sách đã trả: Tên, Ngày mượn, Ngày trả
   - Sách bị từ chối: Tên, Tác giả, Lý do từ chối

**Luồng lọc:**
1. Chọn trạng thái (Chờ xác nhận, Đang mượn, Quá hạn, Đã trả, Bị từ chối)
2. Áp dụng bộ lọc
3. Hiển thị kết quả

**Luồng gia hạn:**
1. Xem sách đang mượn (chưa hết hạn)
2. Click "Gia hạn"
3. Kiểm tra xem đã gia hạn chưa (chỉ được gia hạn 1 lần)
4. Nếu chưa gia hạn → Gia hạn thêm 7 ngày
5. Cập nhật hạn trả
6. Hiển thị thông báo thành công

**Luồng yêu cầu trả sách:**
1. Xem sách đang mượn
2. Click "Xin trả sách"
3. Xác nhận trong modal
4. Tạo yêu cầu trả sách với trạng thái "Chờ xác nhận"

**Luồng thay thế:**
- Đã gia hạn rồi → Không cho phép gia hạn thêm
- Sách đã hết hạn → Không cho phép gia hạn
- Đã có yêu cầu trả chờ xác nhận → Không cho phép tạo yêu cầu mới

**Edge cases:**
- Sách quá hạn → Hiển thị cảnh báo và số ngày quá hạn
- Không có sách nào → Hiển thị thông báo trống

---

## 4. TRẢ SÁCH (Return Book)

### 4.1 Yêu Cầu Trả Sách (2.4.1)
**Actor:** Độc giả  
**Phụ thuộc:** 2.1.2, 2.3.1

**Luồng chính:**
1. Độc giả vào trang "Lịch sử mượn sách"
2. Xem danh sách sách đang mượn
3. Click "Xin trả sách" trên sách muốn trả
4. Xác nhận trong modal
5. Kiểm tra xem đã có yêu cầu trả chờ xác nhận chưa
6. Nếu chưa có → Tạo yêu cầu trả sách với trạng thái "Chờ xác nhận"
7. Hiển thị thông báo thành công

**Luồng thay thế:**
- Đã có yêu cầu trả chờ xác nhận → Thông báo đã có yêu cầu
- Sách chưa được mượn → Không hiển thị nút "Xin trả sách"

**Edge cases:**
- Đơn mượn đã ở trạng thái "Đã trả" → Không cho phép tạo yêu cầu mới
- Đơn mượn bị từ chối → Không cho phép tạo yêu cầu trả

---

### 4.2 Xác Nhận Trả Sách (2.4.2)
**Actor:** Nhân viên thư viện  
**Phụ thuộc:** 2.1.2, 2.4.1

**Luồng chính:**
1. Nhân viên vào trang "Quản lý mượn trả" → Tab "Chờ xác nhận trả"
2. Xem danh sách yêu cầu trả sách chờ xác nhận
3. Nhận sách vật lý từ độc giả
4. Click "Xác nhận trả"
5. Chọn tình trạng sách: Bình thường / Hư hỏng / Mất

**Luồng - Trả bình thường:**
1. Chọn "Bình thường"
2. Xác nhận trả
3. Cập nhật đơn mượn thành "Đã trả"
4. Tăng số lượng sách có sẵn
5. Giảm số lượng sách đang mượn
6. Kiểm tra có trả muộn không
7. Nếu muộn → Tạo phiếu phạt "Trả muộn"

**Luồng - Trả hư hỏng:**
1. Chọn "Hư hỏng"
2. Chọn mức phạt từ danh sách
3. Nhập ghi chú (bắt buộc)
4. Kiểm tra có trả muộn không
5. Nếu muộn → Tạo phiếu phạt "Trả muộn"
6. Tạo phiếu phạt cho hư hỏng
7. Cập nhật đơn mượn thành "Đã trả"
8. Tăng số lượng sách có sẵn (hoặc không, tùy business logic)
9. Giảm số lượng sách đang mượn

**Luồng - Trả mất:**
1. Chọn "Mất"
2. Chọn mức phạt từ danh sách
3. Nhập ghi chú (bắt buộc)
4. Tạo phiếu phạt cho mất sách
5. Cập nhật đơn mượn thành "Đã trả"
6. Giảm số lượng sách đang mượn
7. Không tăng số lượng sách có sẵn (vì sách đã mất)

**Luồng thay thế:**
- Chưa chọn mức phạt → Yêu cầu chọn mức phạt
- Ghi chú để trống (khi hư hỏng/mất) → Yêu cầu nhập ghi chú
- Không có mức phạt nào → Thông báo cần tạo mức phạt trước

**Edge cases:**
- Sách trả muộn + hư hỏng → Tạo 2 phiếu phạt
- Sách trả muộn + mất → Tạo 2 phiếu phạt (muộn + mất)
- Sách trả sớm → Không tạo phiếu phạt muộn
- Mức phạt không tồn tại → Thông báo lỗi

---

## 5. QUẢN LÝ NỢ & PHẠT (Fine Management)

### 5.1 Quản lý mức phạt (2.5.1)
**Actor:** Quản lý viên  
**Phụ thuộc:** 2.1.2

**Luồng chính - Xem danh sách:**
1. Quản lý viên đăng nhập
2. Truy cập trang "Quản lý mức phạt"
3. Hiển thị bảng danh sách mức phạt

**Luồng chính - Thêm mức phạt:**
1. Click "Thêm mức phạt"
2. Nhập: Tên mức phạt, Số tiền, Ngày phạt (mặc định ngày hiện tại)
3. Validate
4. Lưu mức phạt mới
5. Hiển thị thông báo thành công

**Luồng chính - Sửa mức phạt:**
1. Sửa trực tiếp trên bảng
2. Validate
3. Lưu thay đổi

**Luồng chính - Xóa mức phạt:**
1. Click "Xóa" trên bảng
2. Xác nhận xóa
3. Xóa mức phạt

**Luồng thay thế:**
- Validation thất bại → Hiển thị lỗi
- Số tiền <= 0 → Thông báo lỗi

**Edge cases:**
- Tên mức phạt để trống
- Tên mức phạt quá dài (>25 ký tự)
- Ngày phạt không hợp lệ
- Mức phạt đang được sử dụng → Có thể ngăn chặn xóa hoặc cảnh báo

---

### 5.2 Xem & Thanh Toán Phạt - Độc Giả (2.5.2)
**Actor:** Độc giả  
**Phụ thuộc:** 2.1.2, 2.4.2, 2.5.1

**Luồng chính - Xem danh sách phạt:**
1. Độc giả đăng nhập
2. Truy cập trang "Khoản phạt"
3. Hiển thị: Danh sách khoản phạt chưa thanh toán
4. Thông tin: Nguyên nhân phạt, Số tiền, Ngày phạt, Trạng thái

**Luồng chính - Thanh toán:**
1. Chọn phiếu phạt ở trạng thái "Chưa thanh toán"
2. Click "Thanh toán"
3. Thực hiện chuyển khoản qua ngân hàng (bên ngoài hệ thống)
4. Click "Đã thanh toán"
5. Cập nhật trạng thái phiếu phạt thành "Chờ xác nhận"
6. Hiển thị thông báo đã gửi yêu cầu xác nhận

**Luồng thay thế:**
- Không có phiếu phạt nào → Hiển thị thông báo trống
- Phiếu phạt đã thanh toán → Không hiển thị nút thanh toán

**Edge cases:**
- Phiếu phạt đã hết hạn → Có thể hiển thị cảnh báo
- Nhiều phiếu phạt chưa thanh toán → Hiển thị tổng số tiền

---

### 5.3 Xem & Thanh Toán Phạt - Nhân Viên (2.5.3)
**Actor:** Nhân viên thư viện  
**Phụ thuộc:** 2.1.2, 2.5.2

**Luồng chính - Xem danh sách:**
1. Nhân viên đăng nhập
2. Truy cập trang "Quản lý khoản phạt"
3. Hiển thị danh sách khoản phạt chưa thanh toán và chờ xác nhận

**Luồng chính - Xác nhận thanh toán:**
1. Xem chi tiết khoản phạt
2. Kiểm tra số tiền đã chuyển khoản
3. Nếu số tiền đúng → Click "Đã thanh toán"
4. Cập nhật trạng thái phiếu phạt thành "Đã thanh toán"
5. Hiển thị thông báo thành công

**Luồng chính - Từ chối thanh toán:**
1. Xem chi tiết khoản phạt
2. Kiểm tra số tiền đã chuyển khoản
3. Nếu số tiền không đúng → Click "Từ chối"
4. Nhập lý do từ chối (bắt buộc)
5. Cập nhật trạng thái phiếu phạt thành "Từ chối"
6. Lưu lý do từ chối
7. Hiển thị thông báo thành công

**Luồng thay thế:**
- Lý do từ chối để trống → Yêu cầu nhập lý do
- Số tiền không khớp → Từ chối và yêu cầu thanh toán lại

**Edge cases:**
- Phiếu phạt đã được xử lý → Không cho phép xác nhận/từ chối lại
- Số tiền thừa → Có thể chấp nhận hoặc từ chối

---

## 6. QUẢN LÝ NGƯỜI DÙNG (User Management)

### 6.1 Danh Sách Người Dùng (2.6.1)
**Actor:** Quản lý viên  
**Phụ thuộc:** 2.1.2

**Luồng chính:**
1. Quản lý viên đăng nhập
2. Truy cập trang "Quản lý người dùng"
3. Hiển thị: Email, Tên, Vai trò (Reader/Librarian/Admin), Ngày tham gia, Trạng thái (Kích hoạt/Vô hiệu hóa)

**Luồng tìm kiếm:**
1. Nhập từ khóa (email hoặc tên)
2. Click "Tìm kiếm"
3. Hiển thị kết quả

**Luồng lọc:**
1. Chọn vai trò từ dropdown
2. Áp dụng bộ lọc
3. Hiển thị kết quả

**Luồng vô hiệu hóa/kích hoạt:**
1. Click "Vô hiệu hóa" hoặc "Kích hoạt" trên tài khoản
2. Xác nhận thao tác
3. Cập nhật trạng thái tài khoản
4. Hiển thị thông báo thành công

**Luồng thay thế:**
- Không có kết quả tìm kiếm → Hiển thị thông báo
- Tài khoản đang đăng nhập → Có thể ngăn chặn vô hiệu hóa chính mình

**Edge cases:**
- Tài khoản quản lý viên → Có thể ngăn chặn vô hiệu hóa
- Tài khoản có đơn mượn đang hoạt động → Có thể cảnh báo trước khi vô hiệu hóa

---

### 6.2 Gán Vai Trò (2.6.2)
**Actor:** Quản lý viên  
**Phụ thuộc:** 2.1.2, 2.6.1

**Luồng chính:**
1. Xem danh sách người dùng
2. Click "Gán vai trò" trên tài khoản
3. Chọn vai trò mới: Reader / Librarian / Admin
4. Xác nhận thay đổi
5. Cập nhật vai trò người dùng
6. Hiển thị thông báo thành công

**Luồng thay thế:**
- Vai trò không thay đổi → Không cần cập nhật
- Tài khoản đang đăng nhập → Có thể ngăn chặn thay đổi vai trò của chính mình

**Edge cases:**
- Gán vai trò Admin cho nhiều người → Có thể giới hạn số lượng Admin
- Thay đổi vai trò của chính mình → Có thể ngăn chặn hoặc cảnh báo

---

## 7. BÁO CÁO & THỐNG KÊ (Reports & Statistics)

### 7.1 Báo Cáo Tổng Quan - Dashboard (2.7.1)
**Actor:** Quản lý viên, Nhân viên thư viện  
**Phụ thuộc:** 2.1.2, 2.2.2, 2.3.1, 2.4.2

**Luồng chính:**
1. Đăng nhập với vai trò Quản lý viên hoặc Nhân viên
2. Truy cập Dashboard
3. Hiển thị các thống kê:
   - Tổng số sách: Có sẵn / Đang mượn / Bị mất / Hư hỏng
   - Tổng số độc giả: Hoạt động / Vô hiệu hóa
   - Tổng đơn mượn hôm nay
   - Top 5 sách phổ biến nhất
   - Danh sách độc giả nợ quá hạn

**Luồng thay thế:**
- Không có dữ liệu → Hiển thị 0 hoặc "Chưa có dữ liệu"
- Lỗi khi tải dữ liệu → Hiển thị thông báo lỗi

**Edge cases:**
- Dữ liệu rỗng → Hiển thị placeholder
- Thời gian tải lâu → Hiển thị loading indicator

---

### 7.2 Báo Cáo Chi Tiết (2.7.2)
**Actor:** Quản lý viên, Nhân viên thư viện  
**Phụ thuộc:** 2.1.2, 2.7.1

**Luồng chính:**
1. Truy cập trang "Báo cáo"
2. Chọn loại báo cáo:
   - Báo cáo Sách
   - Báo cáo Mượn Trả
   - Báo cáo Phạt
   - Báo cáo Sách Mất/Hư
3. Chọn khoảng thời gian (Ngày, Tuần, Tháng, Quý, Năm)
4. Áp dụng bộ lọc
5. Hiển thị báo cáo

**Luồng xuất báo cáo:**
1. Sau khi xem báo cáo
2. Click "Xuất CSV"
3. Tải file CSV về máy

**Luồng thay thế:**
- Không có dữ liệu trong khoảng thời gian → Hiển thị thông báo
- Lỗi khi xuất file → Hiển thị thông báo lỗi

**Edge cases:**
- Khoảng thời gian không hợp lệ → Validation và thông báo lỗi
- File CSV quá lớn → Có thể giới hạn hoặc cảnh báo
- Không có quyền xuất báo cáo → Ẩn nút xuất

---

## Tổng Kết

### Số lượng tính năng: 20 tính năng

### Phân loại theo Actor:
- **Mọi người (Public):** 2 tính năng (Đăng ký, Đăng nhập)
- **Độc giả (Reader):** 6 tính năng
- **Nhân viên (Librarian):** 7 tính năng
- **Quản lý viên (Admin):** 5 tính năng

### Phân loại theo độ phức tạp:
- **Đơn giản:** 8 tính năng (Xem danh sách, Xem chi tiết, v.v.)
- **Trung bình:** 8 tính năng (Thêm, Sửa, Xóa, v.v.)
- **Phức tạp:** 4 tính năng (Xác nhận trả sách, Báo cáo, v.v.)

