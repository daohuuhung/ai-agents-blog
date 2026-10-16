# Nhật ký công việc - Phạm Quốc Dũng (Kiểm thử)

**DỮ LIỆU GIẢ LẬP.** Dự án: Hệ thống quản lý đơn hàng, Công ty CP Phân phối Sao Mai (giả lập).

### Thứ Hai 12/10/2026
**Đã làm:**
- Retest bug font tiếng Việt PDF phiếu giao hàng (VD-003): pass.
- Viết test case module Kho: TC-KHO-01 đến TC-KHO-18.

**Vướng mắc:**
- Không.

**Ngày mai:**
- Test màn đăng nhập khi Bình merge.

### Thứ Ba 13/10/2026
**Đã làm:**
- Bình merge login chiều nay, bắt đầu test: TC-LOGIN-01 đến 12, đã chạy 9 case, pass 9. Còn 3 case khóa tài khoản để mai.

**Vướng mắc:**
- Không.

**Ngày mai:**
- Chạy nốt login. Test tích hợp tồn kho.

### Thứ Tư 14/10/2026
**Đã làm:**
- Login: pass hết 12/12 case, không có bug.
- Test tích hợp tồn kho với API Sao Mai: TC-INT-03, TC-INT-04 fail do timeout phía khách (>10s). Không phải bug bên mình.

**Vướng mắc:**
- Case tích hợp tồn kho bị block, chờ API bên khách.

**Ngày mai:**
- Test import trùng mã sp.

### Thứ Năm 15/10/2026
**Đã làm:**
- Test import trùng mã: TC-KHO-05 đến 09 pass. Phát hiện bug: file excel có dòng trống cuối thì import báo lỗi, đã báo Châu.

**Vướng mắc:**
- Không.

**Ngày mai:**
- Retest bug dòng trống. Viết case tích hợp luồng đặt hàng.

### Thứ Sáu 16/10/2026
**Đã làm:**
- Retest bug dòng trống: pass.
- Viết test case tích hợp luồng đặt hàng: TC-INT-10 đến TC-INT-20.
- Chạy thử luồng checkout VNPay trên điện thoại: user cancel payment giữa chừng mà DB vẫn insert record success. Đã log ticket bên Jira nội bộ để dev ngó.

**Vướng mắc:**
- Không.

**Ngày mai (thứ Hai):**
- Chạy case tích hợp đặt hàng.
