> **Cảnh báo: tài liệu này tiết lộ toàn bộ bẫy trong bộ dữ liệu.** Nếu muốn tự đánh giá, hãy chạy thử trước rồi mới mở.
>
> Đây là bản nháp AI tạo ra trong lần chạy thật ngày 16/10/2026 (Claude Sonnet 5.5), chép nguyên văn từ Google Docs, kể cả lỗi chữ. Không sửa gì.

---

**BẢN NHÁP DO AI SOẠN, PM PHẢI DUYỆT TRƯỚC KHI GỬI. DỮ LIỆU GIẢ LẬP.**

Tuần báo cáo: thứ Hai 12/10/2026 đến thứ Sáu 16/10/2026. Ngày chạy: 16/10/2026.

# PHẦN 1. DANH SÁCH CHỖ VÊNH (dành cho PM, không gửi khách)

Sắp xếp theo mức ưu tiên xử lý từ trên xuống.

**1. Lỗi hủy thanh toán vẫn sinh đơn "Đã thanh toán": VD-013 ghi Đã đóng nhưng lỗi vẫn tái diễn**

- (a) Danh_sach_van_de ghi VD-013 (mức Cao, phụ trách An) Đã đóng ngày 13/10, ghi chú An đã cập nhật webhook callback VNPay. Nhưng ngày 14/10 Hoàng Thu Hà ghi chị Mai bên Sao Mai gọi phản ánh: khách lẻ hủy giao dịch ở bước quét mã QR trên app ngân hàng, màn hình báo hủy thành công nhưng hệ thống vẫn sinh đơn "Đã thanh toán" và trừ kho. Ngày 16/10 Phạm Quốc Dũng chạy thử checkout VNPay trên điện thoại: hủy giữa chừng nhưng DB vẫn insert record success, đã log ticket Jira nội bộ.
- (b) Nguồn: Danh_sach_van_de VD-013; nhật ký Hoàng Thu Hà 14/10; nhật ký Phạm Quốc Dũng 16/10; nhật ký Nguyễn Văn An 13/10 (không có mục nào nhắc việc sửa webhook); Bao_cao_tuan_2026-10-09 (không nhắc lỗi này với khách dù VD-013 phát hiện ngày 07/10).
- (c) Đề xuất: yêu cầu An xác nhận phạm vi bản sửa (có bao gồm luồng QR trên app ngân hàng không), mở lại VD-013 hoặc tạo mã vấn đề mới, đưa ticket Jira nội bộ của Dũng vào Danh_sach_van_de. Khách đã biết lỗi qua chị Mai, nên PM quyết định cách thông báo trong báo cáo. Bản nháp chưa đưa mục này vì trạng thái theo Danh_sach_van_de là Đã đóng.

**2. Ngày kết thúc Module Kho (H07) lệch giữa bảng tiến độ và nhật ký, kèm thay đổi phạm vi "kiểm kê định kỳ"**

- (a) Bang_tien_do H07 kết thúc dự kiến 23/10/2026. Nhật ký Lê Minh Châu 13/10: anh Hải yêu cầu thêm kiểm kê định kỳ vào module kho, nên module kho "chắc phải 30/10 mới xong". Các ngày 14/10 và 16/10 Châu đã làm bảng kiểm kê, API tạo phiếu và API đối chiếu tồn. Hạng mục kiểm kê không có trong Bang_tien_do và Danh_sach_van_de (VD-009 là yêu cầu khác: trường Ghi chú giao hàng).
- (b) Nguồn: Bang_tien_do H07; nhật ký Lê Minh Châu 13/10, 14/10, 16/10; nhật ký Nguyễn Văn An 16/10 (review PR kiểm kê).
- (c) Đề xuất: PM xác nhận phạm vi kiểm kê có được khách đồng ý chưa, ngày xong H07 là 23/10 hay 30/10, rồi cập nhật Bang_tien_do. Ngày 30/10 trễ hơn 23/10 và có thể ảnh hưởng H09 (Báo cáo doanh thu, bắt đầu 19/10, cũng do Châu phụ trách) và H10. Đây là thay đổi cần trao đổi với khách trước khi đưa vào báo cáo.

**3. API tồn kho của Sao Mai chậm, chặn kiểm thử, nhưng chưa có trong Danh_sach_van_de**

- (a) Châu 12/10: API timeout liên tục, 8-10s, retry 3 lần vẫn fail. Dũng 14/10: TC-INT-03 và TC-INT-04 fail do timeout phía khách (>10s), case tích hợp tồn kho bị block. Hà 15/10: gọi chị Lan, bên khách chuyển bộ phận hạ tầng kiểm tra, chưa hẹn ngày phản hồi. Danh_sach_van_de không có mã nào cho việc này; Bang_tien_do H08 vẫn 30%, kết thúc 23/10, không có ghi chú rủi ro.
- (b) Nguồn: nhật ký Lê Minh Châu 12/10; nhật ký Phạm Quốc Dũng 14/10; nhật ký Hoàng Thu Hà 15/10; Bang_tien_do H08, H10; Danh_sach_van_de (thiếu mã).
- (c) Đề xuất: tạo mã vấn đề mới (nên là mức Cao), đánh giá ảnh hưởng đến ngày 23/10 của H08 và đến H10 (kết thúc 13/11), quyết định có nêu trong mục Vấn đề và rủi ro hay không (bản nháp đã nêu theo dữ kiện, không cam kết ngày).

**4. Màn hình đăng nhập (H02): bảng ghi 60%, nhật ký cho thấy đã xong**

- (a) Bang_tien_do H02 là 60% (kết thúc dự kiến 13/10, đã qua). Nhật ký Trần Thị Bình 13/10: xong màn hình đăng nhập, An approve PR, merge develop. Dũng 14/10: test login pass 12/12 case, không có bug.
- (b) Nguồn: Bang_tien_do H02; nhật ký Trần Thị Bình 13/10; nhật ký Nguyễn Văn An 13/10; nhật ký Phạm Quốc Dũng 13/10, 14/10.
- (c) Đề xuất: PM xác nhận và cập nhật H02 (khả năng 100%). Bản nháp vẫn dùng 60% theo bảng.

**5. Mẫu hóa đơn in (VD-005): đã đến hạn 16/10 mà khách chưa chốt**

- (a) Báo cáo tuần 09/10 đề nghị khách chốt mẫu trước 16/10/2026. Hà đã nhắc chị Lan lần thứ hai ngày 12/10, nhật ký ghi khách vẫn chưa chốt. Danh_sach_van_de VD-005 vẫn Mở, ghi chú "Đã nhắc khách". Không nguồn nào cho thấy khách đã trả lời.
- (b) Nguồn: Bao_cao_tuan_2026-10-09 mục 6; nhật ký Hoàng Thu Hà 12/10; Danh_sach_van_de VD-005.
- (c) Đề xuất: PM quyết định hạn mới (bản nháp ghi "chưa có thông tin" về hạn mới, không tự đặt ngày) và đánh giá ảnh hưởng đến H11/in hóa đơn.

**6. Câu hỏi VAT làm tròn nằm trong nhật ký, đặc tả VAT đã ghi 100%**

- (a) An 15/10: tính VAT làm tròn theo từng dòng hàng hay theo tổng hóa đơn, spec không ghi, tạm làm tròn từng dòng, lệch vài đồng, cần hỏi Sao Mai. Bang_tien_do H11 (Đặc tả hóa đơn VAT) đã 100%, Hà 12/10 ghi hoàn thiện đặc tả và gửi PM duyệt. Chưa có nguồn nào cho thấy câu hỏi đã gửi khách; Danh_sach_van_de không có mã.
- (b) Nguồn: nhật ký Nguyễn Văn An 15/10; nhật ký Hoàng Thu Hà 12/10; Bang_tien_do H11.
- (c) Đề xuất: đưa câu hỏi vào mục "cần Quý khách xác nhận" (bản nháp đã đưa), cân nhắc bổ sung vào đặc tả VAT và tạo mã vấn đề.

**7. VD-007 (trùng mã sản phẩm giữa hai kho) vẫn "Đang xử lý" trong khi nhật ký cho thấy đã xử lý xong**

- (a) Châu 13/10: xong trùng mã sản phẩm giữa 2 kho, chờ Dũng test. Dũng 15/10: TC-KHO-05 đến 09 pass. Danh_sach_van_de VD-007 vẫn Đang xử lý, không có ghi chú.
- (b) Nguồn: Danh_sach_van_de VD-007; nhật ký Lê Minh Châu 12/10, 13/10; nhật ký Phạm Quốc Dũng 15/10.
- (c) Đề xuất: PM xác nhận với Châu và Dũng rồi cập nhật trạng thái. Bản nháp vẫn ghi Đang xử lý theo Danh_sach_van_de.

**8. Đánh giá tổng thể "đúng kế hoạch" cần PM quyết định**

- (a) Bang_tien_do dòng Tổng thể: 52% (tuần trước 48%, đánh giá "đúng kế hoạch"), kết thúc 27/11. Trong khi đó có các tín hiệu có thể ảnh hưởng tiến độ: H07 có thể lùi sang 30/10 (chỗ vênh 2), API tồn kho của khách chậm và chưa có ngày xử lý (chỗ vênh 3), H02 chưa cập nhật (chỗ vênh 4).
- (b) Nguồn: Bang_tien_do dòng Tổng thể, H07, H08; Bao_cao_tuan_2026-10-09 mục 2; nhật ký Lê Minh Châu 13/10; nhật ký Hoàng Thu Hà 15/10.
- (c) Đề xuất: PM tự đánh giá "đúng kế hoạch / chậm" sau khi xử lý các mục trên. Bản nháp để trống phần đánh giá này.

**9. Kế hoạch tuần trước không có tiến triển: dựng môi trường staging (H13) và đánh giá trường Ghi chú giao hàng (VD-009)**

- (a) Báo cáo 09/10 hứa dựng staging trên hạ tầng khách trong tuần tới và sẽ báo lại khách về trường "Ghi chú giao hàng". Tuần này không nhật ký nào của 5 người nhắc đến cả hai việc. Bang_tien_do H13 vẫn 0% (bắt đầu 12/10, phối hợp IT Sao Mai, kết thúc 23/10); Danh_sach_van_de VD-009 vẫn Mở, "Chờ đánh giá ảnh hưởng".
- (b) Nguồn: Bao_cao_tuan_2026-10-09 mục 4 và 6; Bang_tien_do H13; Danh_sach_van_de VD-009; nhật ký An, Bình, Hà, Dũng, Châu cả tuần (không có mục liên quan).
- (c) Đề xuất: PM hỏi An về H13 (phụ thuộc IT Sao Mai, có thể cùng nguyên nhân hạ tầng với chỗ vênh 3) và hỏi Hà về tiến độ đánh giá VD-009; quyết định cách nêu với khách vì đã hứa báo lại.

**10. Index mới làm chậm insert đơn (VD-004), chưa ghi vào danh sách vấn đề**

- (a) An 12/10: thêm index composite, truy vấn từ khoảng 6s còn khoảng 2s trên dev. An 13/10: index mới làm insert đơn chậm hơn chút, đang theo dõi. Danh_sach_van_de VD-004 chỉ ghi "Đang tối ưu index", chưa có tác dụng phụ; không có mục nào sau 13/10 cập nhật kết quả theo dõi.
- (b) Nguồn: nhật ký Nguyễn Văn An 12/10, 13/10; Danh_sach_van_de VD-004.
- (c) Đề xuất: yêu cầu An báo kết quả theo dõi insert, bổ sung ghi chú vào VD-004. Báo cáo 09/10 hứa "dự kiến cải thiện trong tuần tới"; bản nháp chỉ nêu kết quả trên môi trường dev.

**11. Thiếu nhật ký ngày 15/10 của Lê Minh Châu (không báo trước); Trần Thị Bình thiếu 14/10 (có báo trước)**

- (a) Châu có 4 mục (12, 13, 14, 16/10), thiếu 15/10, không có thông báo nghỉ ở nguồn nào. Ngày 16/10 mục Vướng mắc để trống. Bình có 4 mục (13, 15, 16/10 và 12/10), thiếu 14/10, nhưng nhật ký 13/10 ghi nghỉ phép 1 ngày đã báo anh Hải, nên không phải thiếu bất thường. An, Hà, Dũng đủ 5 mục.
- (b) Nguồn: nhật ký Lê Minh Châu; nhật ký Trần Thị Bình 13/10.
- (c) Đề xuất: hỏi Châu về ngày 15/10 (nghỉ hay quên ghi, và có vướng mắc nào bị bỏ sót, nhất là API tồn kho). Đây là thông tin nội bộ, không đưa vào báo cáo khách.

---

# PHẦN 2. BẢN NHÁP BÁO CÁO TUẦN GỬI KHÁCH

(Các ghi chú nguồn trong ngoặc vuông để PM kiểm tra, xóa trước khi gửi.)

**BÁO CÁO TUẦN** **Dự án:** Hệ thống quản lý đơn hàng **Kính gửi:** Công ty CP Phân phối Sao Mai **Người lập:** Đỗ Thanh Hải, Quản lý dự án, Công ty TNHH Giải pháp Phần mềm Hồng Hà

## 1. Kỳ báo cáo

Từ 12/10/2026 đến 16/10/2026.

## 2. Tiến độ tổng thể

Tiến độ tổng thể đạt 52% [Bang_tien_do dòng Tổng thể]. Đánh giá so với kế hoạch: chưa có thông tin, PM bổ sung trước khi gửi [xem chỗ vênh 8].

## 3. Kết quả tuần này

- Màn hình đăng nhập và quên mật khẩu: đạt 60% theo bảng tiến độ [Bang_tien_do H02, xem chỗ vênh 4]. Kiểm thử màn hình đăng nhập đạt 12/12 trường hợp [Phạm Quốc Dũng 14/10].
- Module Đặt hàng: đạt 85%. Đã hoàn thành chức năng hủy đơn và lịch sử đơn hàng, đã đưa vào nhánh phát triển để chuẩn bị kiểm thử tích hợp. Còn chức năng xuất phiếu giao hàng [Bang_tien_do H04; Nguyễn Văn An 13/10, 15/10, 16/10].
- Module Thanh toán (phía hệ thống): đạt 35%. Đã hoàn thành luồng tạo giao dịch trên môi trường thử nghiệm của cổng thanh toán [Bang_tien_do H05; Nguyễn Văn An 14/10].
- Giao diện Thanh toán: đạt 20%. Đã dựng giao diện trang thanh toán bản máy tính và bản điện thoại, đang làm biểu mẫu thông tin giao hàng [Bang_tien_do H06; Trần Thị Bình 15/10, 16/10].
- Module Kho: đạt 55% [Bang_tien_do H07]. Đã xử lý trùng mã sản phẩm giữa hai kho khi nhập dữ liệu và kiểm thử nhập dữ liệu đạt; đã sửa lỗi nhập dữ liệu khi tệp có dòng trống cuối [Lê Minh Châu 13/10, 16/10; Phạm Quốc Dũng 15/10, 16/10].
- Tích hợp API tồn kho của Quý khách: đạt 30% [Bang_tien_do H08]. Các trường hợp kiểm thử tích hợp tồn kho chưa chạy được do thời gian phản hồi của API (xem mục 5) [Phạm Quốc Dũng 14/10].
- Kiểm thử tích hợp: đạt 15%. Đã chuẩn bị các trường hợp kiểm thử luồng đặt hàng [Bang_tien_do H10; Phạm Quốc Dũng 16/10].
- Đặc tả hóa đơn VAT: hoàn thành 100% [Bang_tien_do H11].
- Hướng dẫn sử dụng module Đặt hàng: đạt 40%, đã xong phần tạo đơn, tra cứu đơn và sửa đơn [Bang_tien_do H12; Hoàng Thu Hà 16/10].

## 4. Kế hoạch tuần tới

- Hoàn thiện chức năng xuất phiếu giao hàng của module Đặt hàng và tiếp tục module Thanh toán [Nguyễn Văn An 16/10; Bang_tien_do H04, H05].
- Hoàn thiện biểu mẫu thông tin giao hàng và kết nối với API thanh toán khi sẵn sàng [Trần Thị Bình 16/10].
- Chạy các trường hợp kiểm thử tích tích hợp luồng đặt hàng [Phạm Quốc Dũng 16/10].
- Tiếp tục kết nối API tồn kho của Quý khách [Bang_tien_do H08].
- Hoàn thành phần hướng dẫn sử dụng về hủy đơn và đổi trả [Hoàng Thu Hà 16/10].
- Bắt đầu báo cáo doanh thu theo kế hoạch ngày 19/10/2026 [Bang_tien_do H09].
- Dựng môi trường staging trên hạ tầng của Quý khách, phối hợp với bộ phận IT của Quý khách [Bang_tien_do H13].

## 5. Vấn đề và rủi ro

- Truy vấn danh sách đơn hàng chậm khi dữ liệu lớn: đang xử lý. Sau khi tối ưu chỉ mục, thời gian tải trên môi trường phát triển giảm từ khoảng 6 giây xuống khoảng 2 giây; chúng tôi tiếp tục theo dõi [Danh_sach_van_de VD-004; Nguyễn Văn An 12/10, 13/10, xem chỗ vênh 10].
- Trùng mã sản phẩm giữa hai kho khi nhập dữ liệu: đang xử lý [Danh_sach_van_de VD-007, xem chỗ vênh 7].
- Thời gian phản hồi của API tồn kho phía Quý khách chậm (khoảng 8 đến 10 giây), làm các trường hợp kiểm thử tích hợp tồn kho chưa chạy được. Bộ phận IT của Quý khách đã tiếp nhận và chuyển bộ phận hạ tầng kiểm tra; hiện chưa có thông tin về thời điểm phản hồi [Lê Minh Châu 12/10; Phạm Quốc Dũng 14/10; Hoàng Thu Hà 15/10, xem chỗ vênh 3].
- Mẫu hóa đơn in chưa được chốt [Danh_sach_van_de VD-005].
- Đề nghị bổ sung trường "Ghi chú giao hàng": đang chờ đánh giá ảnh hưởng [Danh_sach_van_de VD-009, xem chỗ vênh 9].

## 6. Nội dung cần Quý khách xác nhận

- Mẫu hóa đơn in: báo cáo tuần trước đề nghị Quý khách chốt mẫu trước ngày 16/10/2026. Kính mong Quý khách xác nhận mẫu; hạn mong muốn mới: chưa có thông tin, PM bổ sung [Bao_cao_tuan_2026-10-09 mục 6; Danh_sach_van_de VD-005, xem chỗ vênh 5].
- Quy tắc làm tròn thuế VAT: làm tròn theo từng dòng hàng hay theo tổng hóa đơn. Đặc tả hiện chưa quy định, hiện tạm làm tròn theo từng dòng hàng và có thể lệch vài đồng so với tổng. Kính mong Quý khách xác nhận; hạn mong muốn: chưa có thông tin, PM bổ sung [Nguyễn Văn An 15/10, xem chỗ vênh 6].
- Thời điểm phản hồi về việc xử lý thời gian phản hồi của API tồn kho: chưa có thông tin, PM bổ sung hạn mong muốn [Hoàng Thu Hà 15/10, xem chỗ vênh 3].
- Đề nghị bổ sung trường "Ghi chú giao hàng": chúng tôi đang đánh giá ảnh hưởng và sẽ báo lại Quý khách [Bao_cao_tuan_2026-10-09 mục 6; Danh_sach_van_de VD-009].

Trân trọng, Đỗ Thanh Hải
