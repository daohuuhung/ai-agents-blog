# Nhật ký công việc - Nguyễn Văn An (Back-end, trưởng nhóm kỹ thuật)

**DỮ LIỆU GIẢ LẬP.** Dự án: Hệ thống quản lý đơn hàng, Công ty CP Phân phối Sao Mai (giả lập).

### Thứ Hai 12/10/2026
**Đã làm:**
- Tối ưu query list đơn hàng (VD-004). Data > 50k dòng load ~6s, thêm index composite (customer_id, created_at), trên dev còn ~2s.
- Thanh toán BE: dựng khung endpoint callback.

**Vướng mắc:**
- Chưa.

**Ngày mai:**
- Đặt hàng: API hủy đơn.

### Thứ Ba 13/10/2026
**Đã làm:**
- API hủy đơn xong, UT được ~80%.
- Review PR màn đăng nhập của Bình, approve.

**Vướng mắc:**
- Index mới làm insert đơn chậm hơn chút, đang theo dõi.

**Ngày mai:**
- Thanh toán BE, phần đối soát giao dịch.

### Thứ Tư 14/10/2026
**Đã làm:**
- Thanh toán BE: xong luồng tạo giao dịch trên sandbox cổng thanh toán.
- Viết thêm UT cho API hủy đơn, đủ 100%.

**Vướng mắc:**
- Không.

**Ngày mai:**
- Đặt hàng: API lịch sử đơn.

### Thứ Năm 15/10/2026
**Đã làm:**
- API lịch sử đơn xong, có phân trang.

**Vướng mắc:**
- Tính VAT đang làm tròn theo từng dòng hàng hay theo tổng hóa đơn? Spec không ghi. Tạm làm tròn từng dòng, lệch vài đồng so với tổng. Cần hỏi lại bên Sao Mai.

**Ngày mai:**
- Merge module đặt hàng, chuẩn bị cho Dũng test tích hợp.

### Thứ Sáu 16/10/2026
**Đã làm:**
- Merge module đặt hàng vào develop. Còn API xuất phiếu giao hàng chưa xong.
- Code review PR kiểm kê của Châu.

**Vướng mắc:**
- Không.

**Ngày mai (thứ Hai):**
- Xong API xuất phiếu giao hàng, quay lại thanh toán BE.
