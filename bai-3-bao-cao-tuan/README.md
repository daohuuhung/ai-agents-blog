# Bài 3: Báo cáo tuần gửi khách hàng

Hướng dẫn tự làm lại thử nghiệm của Bài 3, từng bước, không cần biết lập trình.

**Thời gian:** khoảng 30 đến 45 phút chuẩn bị, 2 đến 3 phút để AI chạy.

---

## Mục lục

1. [Bài toán](#1-bài-toán)
2. [Bạn sẽ làm gì](#2-bạn-sẽ-làm-gì)
3. [Dữ liệu trong kho này](#3-dữ-liệu-trong-kho-này)
4. [Chuẩn bị](#4-chuẩn-bị)
5. [Các bước thực hiện](#5-các-bước-thực-hiện)
6. [Đọc kết quả](#6-đọc-kết-quả)
7. [Kết quả lần chạy của chúng tôi](#7-kết-quả-lần-chạy-của-chúng-tôi)
8. [Tự kiểm tra, đừng tin AI mù quáng](#8-tự-kiểm-tra-đừng-tin-ai-mù-quáng)
9. [Khi có sự cố](#9-khi-có-sự-cố)
10. [Áp dụng cho công việc của bạn](#10-áp-dụng-cho-công-việc-của-bạn)
11. [Câu hỏi thường gặp](#11-câu-hỏi-thường-gặp)

---

## 1. Bài toán

Nếu bạn là quản lý dự án (PM), có lẽ bạn quen cảnh này: tối Chủ nhật, mở máy ra để viết báo cáo tuần gửi khách. Bạn phải:

- đọc nhật ký công việc hằng ngày của từng thành viên,
- đối chiếu với bảng tiến độ và danh sách vấn đề,
- xem tuần trước đã hứa gì với khách, tuần này làm được chưa,
- rồi viết lại thành một bản báo cáo lịch sự, đúng mẫu, không hứa bừa.

Phần mất thời gian nhất không phải là viết, mà là **phát hiện chỗ không khớp**: nhật ký nói xong rồi mà bảng tiến độ chưa cập nhật, một rủi ro được nhắc ba lần mà chưa ai ghi vào danh sách vấn đề, một câu hỏi cần khách trả lời nằm lẫn giữa nhật ký.

**Ý tưởng của bài:** giao việc đọc và đối chiếu cho một AI Agent chạy tự động. Sáng thứ Hai, PM mở ra và thấy một tài liệu gồm hai phần:

1. **Danh sách "chỗ vênh":** những chỗ thông tin giữa các nguồn mâu thuẫn nhau, kèm nguồn và đề xuất PM nên làm gì.
2. **Bản nháp báo cáo tuần** đúng mẫu công ty, mỗi ý ghi nguồn trong ngoặc vuông để PM kiểm tra nhanh.

PM duyệt, sửa, rồi **tự gửi**. AI không bao giờ gửi cho khách.

## 2. Bạn sẽ làm gì

```
  Trong tuần                     Tối Chủ nhật                    Sáng thứ Hai
  Thành viên ghi nhật ký   ->    AI chạy theo lịch        ->     PM đọc chỗ vênh,
  PM giữ bảng tiến độ,           đọc, đối chiếu,                 sửa bản nháp,
  danh sách vấn đề               soạn bản nháp                   tự gửi khách
```

Có 4 thư mục trên Google Drive, mỗi thư mục một nhiệm vụ:

| Thư mục | Dùng để làm gì | AI được làm gì |
|---|---|---|
| `00_Nhat_ky_tuan` | Nhật ký hằng ngày, mỗi người một tài liệu | Chỉ đọc |
| `01_Tai_lieu_du_an` | Bảng tiến độ, danh sách vấn đề, mẫu báo cáo, báo cáo tuần trước | Chỉ đọc |
| `02_Ban_nhap` | AI để bản nháp báo cáo vào đây | Chỉ tạo mới |
| `03_Nhat_ky_chay` | AI ghi lại mỗi lần chạy: đã đọc gì, gặp gì | Chỉ tạo mới |

## 3. Dữ liệu trong kho này

Toàn bộ là **dữ liệu giả lập**. Dự án, khách hàng, công ty và con người đều hư cấu. Mỗi tài liệu có dòng "DỮ LIỆU GIẢ LẬP".

**Bối cảnh:** Công ty TNHH Giải pháp Phần mềm Hồng Hà đang làm "Hệ thống quản lý đơn hàng" cho Công ty CP Phân phối Sao Mai. PM là Đỗ Thanh Hải. Tuần báo cáo là **thứ Hai 12/10/2026 đến thứ Sáu 16/10/2026**.

### `du_lieu/00_Nhat_ky_tuan/`: 5 nhật ký

| Tài liệu | Người viết | Vai trò | Cách viết |
|---|---|---|---|
| `Nhat_ky_Nguyen_Van_An` | Nguyễn Văn An | Back-end, trưởng nhóm kỹ thuật | Ngắn, gạch đầu dòng, nhiều viết tắt (BE, API, PR, UT) |
| `Nhat_ky_Tran_Thi_Binh` | Trần Thị Bình | Front-end | Khá đầy đủ, có cảm xúc ("hơi đuối") |
| `Nhat_ky_Le_Minh_Chau` | Lê Minh Châu | Back-end, module Kho | Rất ngắn, câu cụt, không viết hoa |
| `Nhat_ky_Pham_Quoc_Dung` | Phạm Quốc Dũng | Kiểm thử | Ghi theo mã test case, trộn tiếng Anh |
| `Nhat_ky_Hoang_Thu_Ha` | Hoàng Thu Hà | Phân tích nghiệp vụ, làm việc với khách | Mạch lạc, câu đầy đủ |

Mỗi ngày một mục, gồm ba phần: **Đã làm**, **Vướng mắc**, **Ngày mai**. Chúng tôi cố ý cho mỗi người một giọng viết riêng, vì nhật ký thật không bao giờ đồng đều.

### `du_lieu/01_Tai_lieu_du_an/`: 4 tài liệu

| Tài liệu | Dạng trên Drive | Nội dung |
|---|---|---|
| `Bang_tien_do.csv` | Google Sheets | 13 hạng mục (H01 đến H13): người phụ trách, ngày bắt đầu, ngày kết thúc dự kiến, % hoàn thành, và dòng Tổng thể |
| `Danh_sach_van_de.csv` | Google Sheets | 13 vấn đề (VD-001 đến VD-013): nội dung, mức độ, người phụ trách, trạng thái, ngày phát hiện, ngày đóng |
| `Mau_bao_cao_tuan.md` | Google Docs | Mẫu báo cáo 6 mục của công ty, kèm hướng dẫn văn phong |
| `Bao_cao_tuan_2026-10-09.md` | Google Docs | Báo cáo tuần trước đã gửi khách, để AI biết tuần trước đã hứa gì |

### Bộ dữ liệu có cài sẵn tình huống

Giống Bài 2, chúng tôi cài sẵn một số tình huống để đo AI làm tốt đến đâu:

- **Bẫy:** chỗ vênh thật mà AI phải tìm ra.
- **Bẫy ngược:** chỗ *trông giống* vênh nhưng thật ra hợp lệ. Nếu AI báo những chỗ này, đó là báo động giả, làm PM mất thời gian.

Chúng tôi **không liệt kê** ở đây để bạn tự đọc dữ liệu và tự đoán trước. Đáp án nằm ở [`dap_an/dap_an.md`](dap_an/dap_an.md). Bản nháp AI tạo ra trong lần chạy của chúng tôi nằm ở [`ket_qua_mau/`](ket_qua_mau/). **Cả hai đều tiết lộ bẫy, hãy chạy thử trước rồi mới mở.**

## 4. Chuẩn bị

Bạn cần:

1. **Một tài khoản Claude** có tính năng **tác vụ định kỳ** (scheduled task: bảo Claude tự làm một việc vào giờ hẹn) và kết nối được **Google Drive**. Tên menu có thể khác nhau tùy phiên bản, hãy tìm mục "Scheduled tasks" hoặc "Routines", và mục kết nối (Connectors).
2. **Một tài khoản Google Drive.**

> **Lời khuyên an toàn:** hãy dùng một **tài khoản Google riêng chỉ để thử nghiệm**. Tác vụ chạy tự động và **không hỏi bạn xác nhận** từng bước. Kết nối Google Drive có sẵn cả thao tác **chia sẻ** và **chuyển vào thùng rác**. Chúng tôi đã dặn trong prompt là không được làm hai việc đó, nhưng đó chỉ là lời dặn, không phải khóa quyền. Lần thử của chúng tôi dùng Drive chính, AI không đụng vào gì ngoài phạm vi được giao, nhưng bạn không nên phụ thuộc vào may mắn.

## 5. Các bước thực hiện

### Bước 1: Tải kho này về máy

Trên trang GitHub của kho, bấm nút xanh **Code**, chọn **Download ZIP**, rồi giải nén. Bạn cần thư mục `bai-3-bao-cao-tuan`.

### Bước 2: Tạo 4 thư mục trên Google Drive

Tạo một thư mục tên `Bai3-Bao-cao-tuan`, bên trong tạo 4 thư mục con **đúng tên**:

```
Bai3-Bao-cao-tuan
  00_Nhat_ky_tuan
  01_Tai_lieu_du_an
  02_Ban_nhap
  03_Nhat_ky_chay
```

### Bước 3: Đưa dữ liệu lên Drive, dưới dạng Google Docs và Google Sheets

Điểm quan trọng: tài liệu trên Drive phải là **Google Docs** (nhật ký, mẫu báo cáo, báo cáo tuần trước) và **Google Sheets** (bảng tiến độ, danh sách vấn đề). Nếu bạn chỉ kéo thả tài liệu `.md` lên, Drive giữ nguyên dạng chữ thô, và AI có thể không đọc được. **Tên tài liệu bỏ phần đuôi**, ví dụ `Nhat_ky_Nguyen_Van_An`, không phải `Nhat_ky_Nguyen_Van_An.md`.

**Cách A, nhanh nhất: nhờ Claude làm.** Mở một cuộc trò chuyện Claude thường (đã kết nối Google Drive), đính kèm các tài liệu trong `du_lieu/`, rồi nhắn:

```
Tạo các tài liệu sau trên Google Drive, giữ nguyên nội dung, không sửa chữ nào:
- Mỗi tài liệu .md: tạo Google Docs (tải lên dạng Markdown để Drive tự chuyển), tên bỏ đuôi .md.
- Mỗi tài liệu .csv: tạo Google Sheets (tải lên dạng CSV), tên bỏ đuôi .csv.
Nhật ký (Nhat_ky_*) để vào thư mục ID <ID_00_NHAT_KY_TUAN>.
Bốn tài liệu còn lại để vào thư mục ID <ID_01_TAI_LIEU_DU_AN>.
Xong thì liệt kê lại hai thư mục để tôi kiểm tra.
```

(Cách lấy ID thư mục xem Bước 4. Đây là cách chúng tôi đã làm.)

**Cách B, làm tay:**

- **Tài liệu `.csv`:** trên Drive, vào thư mục `01_Tai_lieu_du_an`, tạo một Google Sheets mới, vào menu **File > Import**, chọn tài liệu CSV, chọn "Thay thế bảng tính hiện tại". Đặt tên đúng như trên.
- **Tài liệu `.md`:** tạo Google Docs mới trong đúng thư mục, mở tài liệu `.md` bằng trình soạn thảo văn bản, chép nội dung và dán vào. Bản Google Docs mới có thể cho dán thẳng Markdown (tìm tùy chọn bật Markdown trong phần cài đặt của Docs). Nếu không có, dán chữ thô cũng được, AI vẫn đọc được nội dung.

Kiểm tra lại: `00_Nhat_ky_tuan` có đủ **5** tài liệu, `01_Tai_lieu_du_an` có đủ **4** tài liệu.

### Bước 4: Lấy mã (ID) của 4 thư mục con

1. Mở thư mục trên Drive.
2. Nhìn thanh địa chỉ: `https://drive.google.com/drive/folders/1AbCdEfGhIjKlMnOpQrStUvWxYz`
3. Đoạn **sau chữ `folders/`** là mã.

Ghi lại mã của cả 4 thư mục con.

### Bước 5: Chuẩn bị prompt

Mở [`prompt/prompt_tac_vu.md`](prompt/prompt_tac_vu.md), chép toàn bộ nội dung trong khung, rồi:

1. Thay 4 chỗ `<ID_00_NHAT_KY_TUAN>`, `<ID_01_TAI_LIEU_DU_AN>`, `<ID_02_BAN_NHAP>`, `<ID_03_NHAT_KY_CHAY>` bằng 4 mã ở Bước 4 (bỏ cả dấu `<` và `>`).
2. **Sửa tuần báo cáo cho bản thử.** Prompt gốc bảo AI lấy "tuần gần nhất trước ngày chạy". Dữ liệu mẫu là tuần 12/10 đến 16/10/2026, nên nếu bạn chạy vào thời điểm khác, AI sẽ tìm nhầm tuần và không thấy nhật ký nào. Tìm dòng:

   ```
   1. Xác định tuần báo cáo: thứ Hai đến thứ Sáu gần nhất, kết thúc vào hoặc trước ngày chạy (giờ Việt Nam).
   ```

   và thay bằng:

   ```
   1. Tuần báo cáo cố định: thứ Hai 12/10/2026 đến thứ Sáu 16/10/2026.
   ```

   Khi dùng cho dự án thật chạy hằng tuần, giữ nguyên dòng gốc.

Phần giải thích từng đoạn của prompt nằm ngay trong tài liệu prompt. Nên đọc qua một lượt, vì phần lớn công sức của bài này nằm ở prompt.

### Bước 6: Tạo tác vụ định kỳ trong Claude

1. Mở Claude, vào mục **tác vụ định kỳ**.
2. Tạo tác vụ mới, đặt tên ví dụ "Thử nghiệm Bài 3: báo cáo tuần".
3. **Dán** prompt đã sửa ở Bước 5.
4. **Chọn model.** Chúng tôi dùng **Claude Sonnet 5.5**. Nếu bạn tạo tác vụ bằng cách nhờ Claude trong cuộc trò chuyện, tác vụ có thể dùng luôn model của cuộc trò chuyện đó. Hãy mở phần cài đặt của tác vụ để kiểm tra.
5. **Kiểm tra kết nối (Connectors) của tác vụ.** Phải có **Google Drive**. Lần thử của chúng tôi, tác vụ mới tự được gắn mọi kết nối đang có trong tài khoản. Nếu bạn có kết nối gửi được ra ngoài như **Gmail** hay **Slack**, hãy **tắt** chúng cho tác vụ này. Bài toán này không cần gửi gì cả.
6. Chọn lịch **chạy một lần**, khoảng 10 phút nữa.
7. Lưu.

> **Mẹo:** trước khi tác vụ chạy, vào trang mức sử dụng (usage) của tài khoản Claude, chụp lại con số. Sau khi chạy xong, chụp lại lần nữa. Đây là cách duy nhất để biết một lần chạy tốn bao nhiêu hạn mức.

### Bước 7: Chờ chạy xong

Lần thử của chúng tôi chạy hết **2 phút 17 giây**, bắt đầu trễ **1 phút 41 giây** so với giờ hẹn. Bạn không cần ngồi canh.

### Bước 8 (tùy chọn): Đặt lịch hằng tuần

Khi đã hài lòng với kết quả và dùng cho dự án thật, đặt lịch chạy tối Chủ nhật, ví dụ **22:03** (chọn phút lẻ vì giờ tròn thường đông). Nếu tạo bằng biểu thức cron, dạng của nó là:

```
CRON_TZ=Asia/Saigon 3 22 * * 0
```

Nhớ trả dòng "tuần báo cáo" trong prompt về bản gốc.

## 6. Đọc kết quả

| Thư mục | Kết quả mong đợi |
|---|---|
| `02_Ban_nhap` | Một Google Docs tên `Bao_cao_tuan_2026-10-16_ban_nhap` |
| `03_Nhat_ky_chay` | Một Google Docs tên `Nhat_ky_chay_` kèm ngày chạy |
| `00_Nhat_ky_tuan`, `01_Tai_lieu_du_an` | **Không thay đổi gì.** Đủ 5 và 4 tài liệu, tên giữ nguyên |

### Thứ tự đọc bản nháp

1. **Đọc Phần 1 (danh sách chỗ vênh) trước.** Mỗi chỗ vênh có ba ý: (a) mô tả, (b) nguồn, (c) đề xuất PM làm gì. Đây là phần giá trị nhất.
2. **Sau đó mới đọc Phần 2 (bản nháp gửi khách).** Mỗi ý có nguồn trong ngoặc vuông, ví dụ `[Bang_tien_do H02]` hoặc `[Trần Thị Bình 13/10]`. Mở nguồn ra đối chiếu những câu quan trọng, rồi xóa hết ngoặc trước khi gửi.

Vì sao phải đọc Phần 1 trước: AI được dặn **lấy số liệu và trạng thái theo bảng tiến độ và danh sách vấn đề**, kể cả khi nó thấy bảng đã cũ. Nó sẽ báo trong Phần 1 chứ không tự sửa Phần 2. Nếu bạn chỉ đọc Phần 2, bạn có thể gửi khách một thông tin đã lỗi thời.

### Đọc nhật ký chạy

Nhật ký chạy cho biết: tuần báo cáo AI đã chọn, đọc được bao nhiêu mục nhật ký của từng người, tài liệu nào không đọc được, có gặp chỉ dẫn lạ không, có lỗi gì không. **Với bộ dữ liệu mẫu, AI phải đọc được 23 mục nhật ký.** Nếu ít hơn, có người bị bỏ sót.

## 7. Kết quả lần chạy của chúng tôi

Chạy ngày 16/10/2026, Claude Sonnet 5.5, đúng prompt trong kho này.

| Chỉ số | Kết quả |
|---|---|
| Thời gian chạy | 2 phút 17 giây |
| Mục nhật ký đọc được | 23/23 |
| Số chỗ vênh AI báo | 11 |
| Bẫy cài sẵn bắt được | 7/7 |
| Bẫy ngược bị báo nhầm | 0/4 |
| Câu không có nguồn hoặc sai nguồn | 0 |
| Cam kết mới với khách (ngày giao, phạm vi) | 0 |
| Lỗi chữ trong bản nháp | 2, AI tự phát hiện và ghi vào nhật ký chạy |
| Hạn mức gói Claude | Tăng từ 30% lên 35%, xem ghi chú |

**Ghi chú về hạn mức:** con số 30% đo ngay trước lượt chạy, 35% đo khoảng 50 phút sau. Trong khoảng đó còn có các thao tác khác trên cùng tài khoản, nên **lượt chạy tốn không quá 5 điểm phần trăm**, và có thể ít hơn. Con số này của gói chúng tôi, gói của bạn có thể khác.

Chi tiết từng bẫy và cách chấm nằm trong [`dap_an/dap_an.md`](dap_an/dap_an.md). Bản nháp nguyên văn nằm trong [`ket_qua_mau/`](ket_qua_mau/).

**Một điều đáng chú ý:** AI tìm ra cả những chỗ vênh chúng tôi **không cố ý cài**, ví dụ một vấn đề đã xử lý xong nhưng danh sách vẫn ghi "Đang xử lý", hay một hạn chót đã đến mà khách chưa trả lời. Khi bạn tự tạo dữ liệu thử, hãy chuẩn bị tinh thần là dữ liệu của bạn cũng có những chỗ vênh bạn không biết.

## 8. Tự kiểm tra, đừng tin AI mù quáng

1. **Đối chiếu nguồn.** Chọn ngẫu nhiên 5 câu trong bản nháp gửi khách, mở nguồn ghi trong ngoặc vuông, xem nguồn có thật sự nói như vậy không.
2. **Tìm cam kết mới.** Đọc bản gửi khách, gạch dưới mọi ngày tháng và mọi chữ "hoàn thành", "đã khắc phục". Từng cái phải có trong bảng tiến độ hoặc danh sách vấn đề.
3. **So với đáp án.** Mở `dap_an/dap_an.md`, đếm số bẫy AI bắt được, số bẫy ngược bị báo nhầm.
4. **Đếm mục nhật ký.** Nhật ký chạy phải ghi 23 mục. Nếu thiếu, xem lại Bước 3.
5. **Kiểm tra Drive.** Hai thư mục chỉ đọc có bị thay đổi gì không? Có tài liệu nào bị chia sẻ không?
6. **Chạy lại.** Chuyển hai tài liệu kết quả sang thư mục khác, chạy lại tác vụ, so sánh. Một quy trình đáng tin phải cho kết quả ổn định giữa các lần chạy.

## 9. Khi có sự cố

| Hiện tượng | Nguyên nhân thường gặp | Cách xử lý |
|---|---|---|
| Không có gì trong `02_Ban_nhap` | Tác vụ chưa được gắn kết nối Google Drive | Vào cài đặt tác vụ, bật Google Drive, chạy lại |
| AI báo không có nhật ký nào trong tuần | Bạn chưa sửa dòng tuần báo cáo ở Bước 5, AI đang tìm một tuần khác | Sửa dòng đó thành tuần cố định 12/10 đến 16/10/2026 |
| AI báo thiếu nhật ký của một người, nhưng thật ra có | Drive trả kết quả thành nhiều trang, AI không đọc trang sau | Prompt đã có quy tắc "đọc hết mọi trang". Kiểm tra bạn không lỡ xóa đoạn đó |
| AI báo không đọc được tài liệu | Tài liệu trên Drive vẫn là `.md` hoặc `.csv` thô | Chuyển thành Google Docs và Google Sheets như Bước 3 |
| Chạy lại thì có hai bản nháp cùng tên | Kết nối Drive không sửa được tài liệu có sẵn, mỗi lần chạy tạo bản mới | Chuyển bản cũ sang thư mục khác trước khi chạy lại |
| Bản nháp còn lỗi chữ | AI không sửa được tài liệu sau khi tạo | PM sửa tay khi duyệt. Xem mục "Lỗi gặp phải" trong nhật ký chạy, AI có thể đã tự ghi lại |
| Tác vụ bắt đầu trễ vài phút | Bình thường | Nếu cần đúng giờ, chọn phút lẻ |

## 10. Áp dụng cho công việc của bạn

Bài này không chỉ dành cho báo cáo tuần. Mẫu chung là: **nhiều nguồn do nhiều người viết, cần đối chiếu và tổng hợp định kỳ, kết quả do con người duyệt.** Ví dụ: báo cáo ca trực, tổng hợp phản hồi khách hàng, đối chiếu biên bản họp với danh sách việc cần làm.

Cách làm từng bước:

1. **Chia thư mục theo quyền.** Thư mục chỉ đọc cho nguồn, thư mục chỉ tạo mới cho kết quả và nhật ký chạy. Giữ đúng bốn thư mục như bài này là đủ cho hầu hết bài toán.
2. **Viết prompt tự chứa.** Mỗi lần chạy là một phiên mới, không nhớ gì từ lần trước. Mọi ID, danh sách người, quy tắc phải nằm sẵn trong prompt.
3. **Liệt kê loại chỗ vênh bạn quan tâm**, rồi thêm câu "bất kỳ mâu thuẫn nào khác". Liệt kê giúp AI không bỏ sót, câu cuối giúp nó không chỉ dò đúng danh sách.
4. **Bắt AI ghi nguồn cho từng câu.** Đây là cách rẻ nhất để con người kiểm tra nhanh, và để phát hiện câu bịa.
5. **Dặn rõ AI không được tự sửa số, không được hứa thay bạn.** AI báo, người quyết.
6. **Tự làm một bộ dữ liệu thử có cài bẫy trước khi dùng dữ liệu thật.** Gồm cả bẫy ngược để đo báo động giả. Nếu được, nhờ một người khác (hoặc một AI khác) viết thêm vài bẫy mà bạn không biết trước.
7. **Không gắn kết nối gửi được ra ngoài.** Bước gửi luôn để con người làm.
8. **Đo hạn mức với lô dữ liệu cỡ thật** trước khi bật lịch hằng tuần.

## 11. Câu hỏi thường gặp

**AI có gửi báo cáo cho khách không?**
Không. Tác vụ chỉ gắn kết nối Google Drive, không có email hay chat. Prompt cũng dặn không gửi, không chia sẻ. Bước gửi luôn do PM làm.

**Sao không để AI cập nhật luôn bảng tiến độ?**
Có hai lý do. Kết nối Google Drive hiện không sửa được nội dung tài liệu có sẵn. Quan trọng hơn, con số trong bảng tiến độ là cam kết của PM. AI chỉ nên chỉ ra chỗ nghi sai, người chịu trách nhiệm mới sửa.

**Nhật ký thật của nhóm tôi viết trên Slack, không phải Google Docs thì sao?**
Bài này chỉ thử với Google Drive. Nếu Claude có kết nối với công cụ nhóm bạn dùng, cách làm tương tự, nhưng chúng tôi chưa kiểm chứng.

**Tôi có cần biết lập trình không?**
Không. Bạn chỉ cần tạo thư mục, đưa tài liệu lên, copy-paste prompt và thay vài mã.

**Dữ liệu và prompt trong kho này dùng cho mục đích khác được không?**
Dữ liệu giả lập và prompt được cung cấp để học tập và thử nghiệm. Hãy xem tài liệu giấy phép của kho nếu có.

---

*Dữ liệu trong kho này hoàn toàn giả lập. Mọi tên công ty, tên người và số liệu đều hư cấu, mọi sự trùng hợp với tổ chức hay cá nhân có thật là ngẫu nhiên.*
