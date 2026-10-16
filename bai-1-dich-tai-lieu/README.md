# Bài 1: Dịch tài liệu giữ nguyên định dạng

Hướng dẫn tự dựng một AI Agent dịch tài liệu chạy theo lịch, từng bước, không cần biết lập trình. Mặc định dịch **tiếng Nhật sang tiếng Việt**, có thể đổi sang cặp ngôn ngữ khác.

---

## Mục lục

1. [Bài toán](#1-bài-toán)
2. [Bạn sẽ làm gì](#2-bạn-sẽ-làm-gì)
3. [Vì sao phải chạy trên máy tính của bạn](#3-vì-sao-phải-chạy-trên-máy-tính-của-bạn)
4. [Chuẩn bị](#4-chuẩn-bị)
5. [Các bước thực hiện](#5-các-bước-thực-hiện)
6. [Đọc kết quả](#6-đọc-kết-quả)
7. [Tự kiểm tra](#7-tự-kiểm-tra)
8. [Đổi cặp ngôn ngữ](#8-đổi-cặp-ngôn-ngữ)
9. [Khi có sự cố](#9-khi-có-sự-cố)
10. [Câu hỏi thường gặp](#10-câu-hỏi-thường-gặp)

---

## 1. Bài toán

Bạn làm việc với đối tác hoặc khách hàng nước ngoài. Tài liệu đến liên tục: slide thuyết trình, bản đề xuất, tài liệu kỹ thuật. Cả team cần đọc bằng tiếng Việt, và cần đọc **đúng bố cục gốc**: slide vẫn là slide, PDF vẫn là PDF, để còn đối chiếu trang với trang khi họp với khách.

Nếu dịch trong giờ làm việc, thời gian và hạn mức AI của bạn sẽ tiêu vào việc dịch thay vì việc chính.

**Ý tưởng của bài:** chiều tối, bạn thả tài liệu vào thư mục `input`. Đêm đó, AI Agent tự chạy, dịch từng tài liệu và lưu bản dịch cùng định dạng vào thư mục `output`. Sáng hôm sau, bản dịch đã sẵn sàng cho cả team.

## 2. Bạn sẽ làm gì

```
  Bạn thả tài liệu vào        AI chạy theo lịch trên         Sáng hôm sau
  thư mục "input"     ->      máy tính của bạn       ->      bản dịch nằm trong "output"
                              dịch từng tài liệu,             cùng định dạng với bản gốc
                              giữ nguyên định dạng
```

| Thư mục | Dùng để làm gì |
|---|---|
| `input` | Nơi bạn thả tài liệu cần dịch. AI chỉ đọc, không sửa, không xóa |
| `output` | AI lưu bản dịch vào đây, tên có thêm hậu tố, ví dụ `De_xuat.pptx` thành `De_xuat_vi.pptx` |

AI nhận biết tài liệu nào đã dịch bằng cách xem thư mục `output` đã có bản `_vi` tương ứng hay chưa. Vì vậy bạn cứ thả thêm tài liệu mới vào `input` bất cứ lúc nào; lần chạy sau chỉ dịch tài liệu mới.

**Định dạng được hỗ trợ trong bản hướng dẫn này:** PowerPoint (`.pptx`) và PDF có sẵn lớp chữ. PDF dạng ảnh scan sẽ bị bỏ qua và được ghi lý do trong báo cáo.

## 3. Vì sao phải chạy trên máy tính của bạn

Tác vụ định kỳ của Claude có thể chạy ở hai nơi:

| | Chạy trên đám mây | Chạy trên máy tính của bạn |
|---|---|---|
| Máy tắt có chạy không | Có | **Không**, máy phải bật vào giờ chạy |
| Làm việc với dữ liệu qua | Kết nối (connector), ví dụ Google Drive | Thư mục trực tiếp trên máy |
| Giữ nguyên định dạng PowerPoint, PDF | Không. Kết nối Google Drive chỉ tạo được tài liệu Google | **Có**, vì AI dùng được các công cụ đọc và ghi PowerPoint, PDF trên máy |

Bài này cần giữ nguyên định dạng, nên tác vụ phải chạy trên máy tính của bạn. Đổi lại, **máy phải bật vào giờ chạy** (không tắt máy, không để ngủ).

> **Mẹo cho cả team cùng xem:** nếu bạn dùng ứng dụng **Google Drive cho máy tính** (Google Drive for Desktop) ở chế độ đồng bộ đầy đủ (Mirror), hãy đặt hai thư mục `input` và `output` bên trong ổ Google Drive trên máy. Bản dịch sẽ tự đồng bộ lên Drive, và cả team mở được từ Drive mà không cần bạn gửi.

## 4. Chuẩn bị

Bạn cần:

1. **Một tài khoản Claude** có tính năng **tác vụ định kỳ** (scheduled task, tức là bảo Claude tự làm một việc vào giờ hẹn).
2. **Ứng dụng Claude cho máy tính** (Claude desktop), cài trên máy chứa hai thư mục, và đang đăng nhập đúng tài khoản. Tác vụ làm việc với máy của bạn thông qua ứng dụng này.
3. **Hai thư mục** tên **đúng** là `input` và `output` trên máy.

> **Tên menu có thể khác nhau tùy phiên bản.** Hãy tìm mục "Scheduled tasks" hoặc "Routines", và tùy chọn cho tác vụ chạy trên máy tính này (ví dụ "Require this computer").

> **Lời khuyên an toàn:** tác vụ chạy tự động và **không hỏi bạn xác nhận** từng bước. Bản hướng dẫn đã dặn AI chỉ làm việc trong hai thư mục, không xóa, không ghi đè. Dù vậy, hãy luôn giữ bản gốc của tài liệu ở một nơi khác, và chỉ cấp cho tác vụ quyền truy cập đúng hai thư mục này.

## 5. Các bước thực hiện

### Bước 1: Tạo hai thư mục

Tạo hai thư mục **đúng tên** `input` và `output`, ví dụ:

```
C:\Users\ten-ban\My Drive\input
C:\Users\ten-ban\My Drive\output
```

Tên thư mục quan trọng, vì bản hướng dẫn cho AI tìm hai thư mục theo đúng tên `input` và `output`.

### Bước 2: Chuẩn bị hướng dẫn cho AI (prompt)

1. Mở tài liệu [`prompt/prompt_tac_vu.md`](prompt/prompt_tac_vu.md), chép toàn bộ nội dung trong khung vào một trình soạn thảo văn bản.
2. Thay `<DUONG_DAN_INPUT>` và `<DUONG_DAN_OUTPUT>` bằng đường dẫn thật của hai thư mục ở Bước 1.
3. Nếu muốn dịch cặp ngôn ngữ khác, sửa phần **THIẾT LẬP NGÔN NGỮ** ở đầu (xem [phần 8](#8-đổi-cặp-ngôn-ngữ)).

Bản hướng dẫn nói với AI những điều chính sau:

- Chỉ làm việc trong hai thư mục `input` và `output`. **Không xóa, không ghi đè, không sửa tài liệu gốc.**
- Chỉ dịch tài liệu **chưa có bản dịch** trong `output`.
- Dịch PowerPoint bằng cách tách chữ ra, dịch, rồi gắn lại vào đúng vị trí; dịch PDF bằng cách thay chữ ngay tại chỗ, giữ nguyên hình ảnh và nền.
- Không dịch số liệu, công thức, đường dẫn web, tên riêng viết bằng chữ Latin.
- Mỗi tài liệu làm xong mới sang tài liệu tiếp; một tài liệu lỗi thì ghi lại và làm tiếp, không dừng cả lượt.
- Cuối phiên, viết báo cáo ngắn: đã dịch những gì, bỏ qua gì, vì sao.

### Bước 3: Chạy thử trong một phiên làm việc thường

Trước khi đặt lịch, hãy chạy thử một lần khi bạn đang ngồi trước máy:

1. Thả **một hoặc hai** tài liệu nhỏ vào `input`.
2. Mở Claude, bắt đầu một phiên làm việc mới **có liên kết với máy tính này**, dán bản hướng dẫn vào và gửi.
3. Lần đầu, Claude sẽ hỏi quyền truy cập hai thư mục. Hãy cho phép **đúng hai thư mục** `input` và `output`.
4. Chờ chạy xong, mở bản dịch trong `output` ra xem.

Bước này giúp bạn cấp quyền thư mục một lần, và thấy trước chất lượng bản dịch trước khi để nó tự chạy ban đêm.

### Bước 4: Tạo tác vụ định kỳ

1. Mở Claude, vào mục **tác vụ định kỳ**.
2. Tạo tác vụ mới, đặt tên dễ nhớ, ví dụ "Dịch tài liệu ban đêm".
3. **Dán** bản hướng dẫn đã sửa ở Bước 2 vào ô nội dung.
4. Chọn để tác vụ **chạy trên máy tính này**, và đảm bảo hai thư mục `input`, `output` nằm trong danh sách thư mục được phép.
5. Chọn lịch, ví dụ **22:03 mỗi ngày làm việc**. Nên chọn phút lẻ thay vì giờ tròn, vì giờ tròn thường đông và dễ chạy trễ.
6. Lưu tác vụ.

### Bước 5: Để máy bật qua đêm

Đến giờ hẹn, máy phải đang bật, ứng dụng Claude cho máy tính đang mở và đăng nhập. Nếu máy tắt hoặc đang ngủ, lượt chạy đó sẽ không thực hiện.

## 6. Đọc kết quả

Sáng hôm sau:

| Nơi | Kết quả mong đợi |
|---|---|
| `input` | Giữ nguyên, không có gì bị đổi tên hay xóa |
| `output` | Mỗi tài liệu mới có một bản dịch, tên thêm hậu tố, ví dụ `Bao_cao.pdf` thành `Bao_cao_vi.pdf` |
| Lịch sử tác vụ | Báo cáo cuối phiên: số tài liệu đã dịch, tài liệu bị bỏ qua và lý do |

Nếu không có tài liệu mới, báo cáo chỉ ghi "Không có file mới". Đó là tín hiệu tác vụ vẫn chạy bình thường.

## 7. Tự kiểm tra

AI dịch nhanh nhưng không hoàn hảo. Hãy tập thói quen kiểm tra:

1. **Mở bằng mắt từng slide.** Chữ tiếng Việt thường dài hơn tiếng Nhật, nên có thể tràn khung. Báo cáo cuối phiên sẽ nhắc nếu có nguy cơ, nhưng bạn vẫn nên lướt qua.
2. **So một đoạn với bản bạn tự dịch.** Lấy một tài liệu bạn đã từng dịch, cho AI dịch lại, rồi so sánh thuật ngữ và văn phong.
3. **Kiểm tra số liệu.** Số liệu, công thức, đường dẫn web phải giữ nguyên.
4. **Kiểm tra PDF.** Bản dịch PDF được chèn lại bằng phông chữ khác bản gốc và có thể bị co cỡ chữ cho vừa khung. Hãy xem kỹ những trang nhiều chữ.
5. **Đọc báo cáo cuối phiên.** Tài liệu nào bị bỏ qua, vì sao.

## 8. Đổi cặp ngôn ngữ

Mở [`prompt/prompt_tac_vu.md`](prompt/prompt_tac_vu.md), phần **Cách đổi cặp ngôn ngữ**. Tóm tắt:

1. Sửa dòng **Ngôn ngữ nguồn** và **Ngôn ngữ đích**, ví dụ tiếng Anh sang tiếng Việt.
2. Sửa **hậu tố tên bản dịch** nếu cần, ví dụ `_ja` khi dịch sang tiếng Nhật.
3. Sửa hoặc bỏ đoạn **QUY TẮC RIÊNG CHO CẶP NHẬT - VIỆT** cho hợp với cặp ngôn ngữ mới.
4. Nếu ngôn ngữ đích là tiếng Nhật, tiếng Trung hay tiếng Hàn, kiểm tra máy đã có phông chữ hỗ trợ chưa, nếu không bản dịch PDF có thể hiện ô vuông.

## 9. Khi có sự cố

| Hiện tượng | Nguyên nhân thường gặp | Cách xử lý |
|---|---|---|
| Đến giờ mà tác vụ không chạy | Máy tắt, đang ngủ, hoặc ứng dụng Claude cho máy tính chưa mở | Để máy bật, tắt chế độ ngủ vào giờ chạy, mở sẵn ứng dụng |
| Báo cáo ghi không truy cập được thư mục | Chưa cấp quyền cho hai thư mục, hoặc tên thư mục không đúng `input`, `output` | Làm lại Bước 3 để cấp quyền; kiểm tra tên thư mục |
| PDF bị bỏ qua | PDF là ảnh scan, không có lớp chữ | Bản hướng dẫn này không xử lý PDF scan |
| Chữ tràn khung trong slide | Bản dịch dài hơn bản gốc | Sửa tay những slide bị tràn; có thể thêm vào quy tắc yêu cầu dịch ngắn gọn hơn |
| PDF dịch hiện ô vuông | Máy thiếu phông chữ cho ngôn ngữ đích | Cài phông chữ phù hợp, ví dụ họ phông Noto |
| Tài liệu đã dịch rồi vẫn bị dịch lại | Bản dịch trong `output` bị đổi tên hoặc chuyển đi | Giữ nguyên tên bản dịch có hậu tố trong `output` |

## 10. Câu hỏi thường gặp

**AI có sửa hay xóa tài liệu gốc của tôi không?**
Theo bản hướng dẫn thì không: AI chỉ đọc `input` và chỉ ghi bản dịch mới vào `output`. Nhưng vì tác vụ chạy không hỏi xác nhận, hãy luôn giữ bản gốc ở nơi khác.

**Tôi có thể dịch Word, Excel hay HTML không?**
Bản hướng dẫn hiện tại chỉ viết cho PowerPoint và PDF. Bạn có thể bổ sung quy tắc cho định dạng khác theo cùng cách làm: tách chữ, dịch, gắn lại, rồi kiểm tra tài liệu mở được.

**Vì sao không dùng Google Drive trên đám mây cho tiện?**
Vì kết nối Google Drive không giữ được định dạng PowerPoint hay PDF gốc. Nếu bạn chỉ cần đọc hiểu nội dung, không cần giữ định dạng, thì chạy trên đám mây qua Google Drive là đủ và máy tắt vẫn chạy.

**Tôi có cần biết lập trình không?**
Không. Bạn chỉ cần tạo thư mục, sửa vài dòng trong bản hướng dẫn, và đặt lịch.

**Mỗi lần chạy tốn bao nhiêu hạn mức?**
Tùy số lượng và độ dài tài liệu. Hãy đo vài đêm đầu ở trang mức sử dụng của Claude trước khi để chạy thường xuyên.
