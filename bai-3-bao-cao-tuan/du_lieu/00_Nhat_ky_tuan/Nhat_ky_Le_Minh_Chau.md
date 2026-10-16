# Nhật ký công việc - Lê Minh Châu (Back-end, module Kho)

**DỮ LIỆU GIẢ LẬP.** Dự án: Hệ thống quản lý đơn hàng, Công ty CP Phân phối Sao Mai (giả lập).

### Thứ Hai 12/10/2026
**Đã làm:**
- import sp từ file excel của khách. xử lý trùng mã giữa 2 kho (VD-007), được 1 nửa.
- nối API tồn kho Sao Mai. timeout liên tục, có lúc 8-10s mới trả.

**Vướng mắc:**
- API tồn kho chậm. retry 3 lần vẫn fail.

**Ngày mai:**
- tiếp trùng mã.

### Thứ Ba 13/10/2026
**Đã làm:**
- xong trùng mã sp giữa 2 kho. chờ Dũng test.
- anh Hải bảo thêm kiểm kê định kỳ vào module kho.

**Vướng mắc:**
- thêm kiểm kê thì module kho chắc phải 30/10 mới xong.

**Ngày mai:**
- thiết kế bảng kiểm kê.

### Thứ Tư 14/10/2026
**Đã làm:**
- thiết kế bảng kiểm kê, tạo migration.
- API tạo phiếu kiểm kê.

**Vướng mắc:**
- không

**Ngày mai:**
- API đối chiếu tồn.

### Thứ Sáu 16/10/2026
**Đã làm:**
- API đối chiếu tồn kho xong bản đầu. tạo PR, anh An review.
- fix bug Dũng báo: import lỗi khi file có dòng trống cuối.

**Vướng mắc:**
- -

**Ngày mai (thứ Hai):**
- tiếp kiểm kê.
