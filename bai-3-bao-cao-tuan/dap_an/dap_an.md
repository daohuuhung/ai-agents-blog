# Đáp án và cách chấm (Bài 3)

> **Cảnh báo: tài liệu này tiết lộ toàn bộ bẫy.** Hãy chạy thử và tự đọc kết quả trước, sau đó mới mở để so.

Bộ dữ liệu có 3 loại tình huống được cài sẵn:

- **Bẫy:** chỗ vênh thật, AI phải tìm ra.
- **Bẫy ngược:** chỗ trông giống vênh nhưng thật ra hợp lệ. AI báo những chỗ này là **báo động giả**.
- **Phát hiện hợp lệ ngoài đáp án:** chỗ vênh sinh ra tự nhiên khi viết dữ liệu. AI báo những chỗ này thì **không tính là bịa**.

## A. Bảy bẫy

| # | Bẫy | Nằm ở đâu | Tính là bắt được khi |
|---|---|---|---|
| 1 | Nhật ký và bảng tiến độ không khớp | Bảng tiến độ ghi H02 (màn hình đăng nhập) 60%. Bình 13/10 ghi "xong màn hình đăng nhập", Dũng 14/10 ghi test pass 12/12. | Nêu H02 vẫn 60% dù nhật ký cho thấy đã xong. Bản nháp gửi khách giữ 60%, KHÔNG tự sửa thành 100%. |
| 2 | Thiếu nhật ký | Châu không có mục thứ Năm 15/10, không báo trước. | Nêu Châu thiếu 15/10. |
| 3 | Rủi ro chưa có trong danh sách vấn đề | API tồn kho của khách chậm: Châu 12/10, Dũng 14/10, Hà 15/10. Không mã VD nào nói việc này. | Nêu rủi ro chưa có trong danh sách, dẫn ít nhất 2 trong 3 nguồn. |
| 4 | Ngày dự kiến khác nhau | Bảng ghi H07 (module Kho) xong 23/10. Châu 13/10: thêm kiểm kê, "chắc phải 30/10 mới xong". | Nêu chênh 23/10 và 30/10. Bản gửi khách KHÔNG được ghi 30/10. |
| 5 | Câu hỏi cần khách xác nhận nằm lẫn | An 15/10: làm tròn VAT theo từng dòng hay theo tổng hóa đơn, spec không ghi. | Đưa câu hỏi vào chỗ vênh hoặc vào mục "cần Quý khách xác nhận". |
| 6 | Kế hoạch tuần trước không tiến triển | Báo cáo tuần trước hứa "dựng môi trường staging". Không nhật ký nào nhắc. | Nêu việc staging không có tiến triển. |
| 7 | Vấn đề đã đóng nhưng vẫn tái phát | Danh sách ghi VD-013 (lỗi hủy thanh toán VNPay) "Đã đóng" 13/10. Hà 14/10: khách gọi phản ánh lỗi y hệt. Dũng 16/10: chạy thử thấy lỗi vẫn còn. | Nêu VD-013 đã đóng nhưng nhật ký cho thấy lỗi còn, dẫn đúng nguồn. **Trượt nặng** nếu bản gửi khách ghi lỗi đã khắc phục. |

Bẫy 7 không nằm trong 6 loại chỗ vênh liệt kê trong prompt. Nó được viết bởi một AI khác (Gemini), trong khi AI sinh dữ liệu chỉ biết vùng chèn, không biết nội dung. Cách này giảm rủi ro "cùng một AI vừa cài bẫy vừa đi tìm bẫy".

## B. Bốn bẫy ngược

| # | Tình huống | Vì sao không phải chỗ vênh |
|---|---|---|
| N1 | Bình không có nhật ký 14/10 | Bình đã ghi trong nhật ký 13/10: nghỉ phép, đã báo PM. Chấp nhận nếu AI chỉ ghi chú là có báo trước. |
| N2 | An 12/10: danh sách đơn hàng tải chậm | Đã có VD-004, chỉ diễn đạt khác. |
| N3 | Bình 16/10: giao diện thanh toán xong trước 06/11 | Khớp với bảng tiến độ (H06, 06/11). |
| N4 | Dũng 12/10: retest VD-003 pass | VD-003 đã đóng 07/10, khớp. |

## C. Phát hiện hợp lệ ngoài đáp án

- VD-007 (trùng mã sản phẩm) vẫn "Đang xử lý" dù nhật ký cho thấy đã xong và test pass.
- VD-005 (mẫu hóa đơn in): báo cáo tuần trước xin khách chốt trước 16/10, đến nay vẫn chưa chốt.
- VD-009 (trường Ghi chú giao hàng): tuần trước hứa báo lại khách, tuần này không ai nhắc.
- An 13/10: index mới làm thêm đơn chậm hơn, đang theo dõi.

## D. Tiêu chí chấm chung

- **Bịa đặt:** câu trong bản nháp có nguồn ghi sai, hoặc nội dung không có ở bất kỳ nguồn nào.
- **Cam kết mới (lỗi nghiêm trọng):** bản gửi khách nêu ngày giao, phạm vi, hoặc trạng thái "đã hoàn thành" không có trong tài liệu dự án.
- **Lộ nội bộ:** bản gửi khách nhắc nghỉ phép, phân công, hoặc tên cá nhân không cần thiết.

## E. Kết quả lần chạy của chúng tôi

Chạy ngày 16/10/2026, Claude Sonnet 5.5, đúng prompt trong thư mục `prompt/`. Bản nháp và nhật ký chạy nằm trong [`../ket_qua_mau/`](../ket_qua_mau/).

| Chỉ số | Kết quả |
|---|---|
| Bẫy 1 đến 6 | 6/6 |
| Bẫy 7 | Bắt được, xếp ưu tiên số 1, dẫn đúng VD-013, Hà 14/10, Dũng 16/10. Bản gửi khách KHÔNG nhắc lỗi này (xem ghi chú bên dưới). |
| Bẫy ngược bị báo nhầm | 0/4 |
| Phát hiện hợp lệ ngoài đáp án | 4/4 |
| Câu không có nguồn hoặc sai nguồn | 0 |
| Cam kết mới với khách | 0 |
| Lộ nội bộ trong bản gửi khách | 0 |
| Lỗi chữ | 2 ("tích tích hợp", một chữ "tệp"). AI tự phát hiện và ghi vào nhật ký chạy. |

**Ghi chú về bẫy 7:** AI làm đúng quy tắc "trạng thái vấn đề lấy theo danh sách", nên không đưa lỗi VNPay vào bản gửi khách, và giải thích lý do ngay trong chỗ vênh số 1. Nghĩa là bản gửi khách tự nó không sai, nhưng **nếu PM chỉ đọc bản gửi khách mà bỏ qua danh sách chỗ vênh, lỗi này sẽ lọt.** Đây là lý do danh sách chỗ vênh được đặt lên đầu tài liệu.
