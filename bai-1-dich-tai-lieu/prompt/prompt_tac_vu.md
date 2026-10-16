# Prompt cho tác vụ định kỳ (Bài 1)

Bản hướng dẫn này dùng cho tác vụ định kỳ **chạy trên máy tính của bạn**, dịch tài liệu mới trong thư mục `input` và lưu bản dịch, **giữ nguyên định dạng gốc**, vào thư mục `output`.

Mặc định: dịch **tiếng Nhật sang tiếng Việt**. Muốn đổi cặp ngôn ngữ, xem phần [Cách đổi cặp ngôn ngữ](#cách-đổi-cặp-ngôn-ngữ) bên dưới.

Trước khi dán vào tác vụ, thay 2 chỗ `<DUONG_DAN_...>` bằng đường dẫn thật của hai thư mục trên máy bạn, ví dụ `C:\Users\ten-ban\My Drive\input`.

```
Nhiệm vụ: dịch các file mới trong thư mục input từ NGÔN NGỮ NGUỒN sang NGÔN NGỮ ĐÍCH, giữ nguyên định dạng file. Làm việc trực tiếp trên máy tính của người dùng qua các công cụ mcp__remote-devices__* (nạp bằng ToolSearch nếu chưa có). Không dùng connector Google Drive để tải/đăng file.

THIẾT LẬP NGÔN NGỮ (muốn đổi cặp ngôn ngữ thì sửa 4 dòng này):
- Ngôn ngữ nguồn: tiếng Nhật
- Ngôn ngữ đích: tiếng Việt
- Hậu tố tên file bản dịch: _vi
- Mã ngắn trong JSON: nguồn = "src", đích = "dst"

THƯ MỤC:
- Nguồn: <DUONG_DAN_INPUT>
- Đích: <DUONG_DAN_OUTPUT>
Trong device_bash, hai thư mục này nằm tại $HOME/mnt/input và $HOME/mnt/output. Nếu chưa được kết nối thì gọi get_device_info rồi device_request_folder_access cho đúng hai thư mục này. Chỉ làm việc trong hai thư mục này; không xóa, không ghi đè, không sửa file nguồn.

BƯỚC 1 - LIỆT KÊ: dùng device_list_dir cho cả hai thư mục (bao gồm thư mục con nếu có, giữ cấu trúc tương ứng ở đích). File nguồn được coi là đã dịch nếu ở đích có "<tên-gốc><hậu tố>.<đuôi>". Chỉ xử lý file chưa có bản dịch. Bỏ qua file đã có hậu tố bản dịch, file tạm (~$*, .tmp*), file có kích thước 0. Nếu không có file mới thì báo "Không có file mới" và kết thúc.

BƯỚC 2 - CHUẨN BỊ CÔNG CỤ (mỗi lần chạy, vì phiên mới không giữ file): trong device_bash, tạo thư mục $HOME/tools với 3 file sau (chạy lệnh tạo bằng heredoc 'EOF'):
(a) pptx_extract_lib.py
from pptx import Presentation
def iter_text_frames(shapes):
    for sh in shapes:
        if sh.shape_type == 6:
            yield from iter_text_frames(sh.shapes)
        if getattr(sh, "has_text_frame", False) and sh.has_text_frame:
            yield sh.text_frame
        if getattr(sh, "has_table", False) and sh.has_table:
            for row in sh.table.rows:
                for cell in row.cells:
                    yield cell.text_frame
def all_frames(prs):
    for s in prs.slides:
        yield from iter_text_frames(s.shapes)
        if s.has_notes_slide:
            yield s.notes_slide.notes_text_frame
(b) pptx_extract.py: đọc argv[1]=file pptx, argv[2]=json ra; duyệt all_frames, mỗi paragraph ghép text các run; với text không rỗng và chưa gặp thì thêm {"id": n, "src": text} vào danh sách; ghi JSON (ensure_ascii=False).
(c) pptx_apply.py: argv[1]=pptx gốc, argv[2]=json đã có trường "dst", argv[3]=pptx ra; tạo map src->dst; duyệt all_frames, với mỗi paragraph có text khớp thì gán runs[0].text = bản dịch và xóa các run còn lại (r._r.getparent().remove(r._r)); lưu; in số đoạn đã thay và số đoạn chưa dịch.
Cần python-pptx, libreoffice/soffice, pdftotext, pypdf; PyMuPDF cài bằng: pip install --break-system-packages pymupdf (nếu chưa có).

BƯỚC 3 - DỊCH TỪNG FILE (làm tuần tự, mỗi file xong mới sang file tiếp; một file lỗi thì ghi lại rồi bỏ qua, không dừng cả lượt):
PPTX: (1) chạy pptx_extract.py trên file nguồn (đường dẫn $HOME/mnt/input/...) ra JSON ở $HOME/tools; (2) tự dịch toàn bộ các đoạn "src" sang ngôn ngữ đích, chia lô khoảng 100-150 đoạn mỗi lô, ghi thêm trường "dst" vào JSON (viết lô dịch ra file riêng rồi gộp bằng python); (3) chạy pptx_apply.py ra file tạm trong $HOME/tools; (4) kiểm tra: số đoạn chưa dịch = 0, còn sót chữ của ngôn ngữ nguồn thì dịch bù; mở thử bằng soffice --headless --convert-to pdf để chắc file không hỏng; (5) chép file kết quả vào thư mục đích với tên "<tên-gốc><hậu tố>.pptx" bằng cp trong device_bash. Không dịch số liệu, công thức, URL, tên riêng đã viết bằng chữ Latin (tên sản phẩm, tên công nghệ...). Văn phong ngôn ngữ đích tự nhiên, đúng giọng văn bản công việc; thuật ngữ nhất quán trong từng file.
QUY TẮC RIÊNG CHO CẶP NHẬT - VIỆT (đổi cặp ngôn ngữ thì sửa hoặc bỏ đoạn này): tên công ty/tổ chức Nhật giữ Romaji kèm tên Việt hoặc nguyên văn trong ngoặc ở lần đầu nếu cần; xưng hô "Quý khách/chúng tôi" khi bản gốc dùng 御中/当社; áp dụng skill anthropic-skills:vietnamese-business-writing nếu có; tiếng Việt thường dài hơn tiếng Nhật nên ưu tiên dịch ngắn gọn để tránh tràn chữ.
PDF: dùng PyMuPDF (fitz): với từng khối chữ, dịch rồi thay thế tại chỗ (redaction vùng chữ gốc, chèn bản dịch bằng font hỗ trợ ngôn ngữ đích có sẵn ở /usr/share/fonts, ví dụ DejaVu Sans hoặc Noto Sans cho tiếng Việt; co cỡ chữ cho vừa khung). Giữ nguyên hình ảnh/nền. Lưu thành "<tên-gốc><hậu tố>.pdf" ở thư mục đích. Nếu PDF là bản scan không có lớp chữ thì bỏ qua và báo lý do.
Sau khi ghi, dùng device_list_dir xác nhận file đã nằm ở thư mục đích và kích thước hợp lý.

NGUYÊN TẮC: không ghi vào ngoài hai thư mục trên; không xóa file; không đụng tới thông tin xác thực hay cài đặt hệ thống. Nếu thiếu quyền hoặc công cụ, báo rõ lý do thay vì tìm cách vòng.

BƯỚC 4 - BÁO CÁO cuối phiên, ngắn gọn: số file đã dịch (liệt kê tên), file bỏ qua/lỗi kèm lý do, và lưu ý nếu PPTX có thể tràn chữ cần người dùng kiểm tra bằng mắt.
```

## Cách đổi cặp ngôn ngữ

Bản hướng dẫn được viết sẵn cho cặp **Nhật sang Việt**. Muốn dịch cặp khác, bạn chỉ cần sửa ba chỗ:

| Sửa ở đâu | Mặc định | Ví dụ: Anh sang Việt | Ví dụ: Việt sang Nhật |
|---|---|---|---|
| Dòng "Ngôn ngữ nguồn" | tiếng Nhật | tiếng Anh | tiếng Việt |
| Dòng "Ngôn ngữ đích" | tiếng Việt | tiếng Việt | tiếng Nhật |
| Dòng "Hậu tố tên file bản dịch" | `_vi` | `_vi` | `_ja` |
| Đoạn "QUY TẮC RIÊNG CHO CẶP NHẬT - VIỆT" | giữ | bỏ đoạn này | viết lại, ví dụ: dùng kính ngữ です・ます; giữ tên Việt bằng chữ Latin kèm phiên âm Katakana ở lần đầu nếu cần; tiếng Nhật thường ngắn hơn tiếng Việt |

Lưu ý thêm khi đổi cặp ngôn ngữ:

- **Phông chữ cho PDF.** Bản dịch PDF được chèn lại bằng phông chữ có sẵn trên máy. Nếu ngôn ngữ đích là tiếng Nhật, tiếng Trung hay tiếng Hàn, cần một phông hỗ trợ chữ của ngôn ngữ đó (ví dụ họ phông Noto Sans CJK). Nếu máy chưa có, bản dịch PDF có thể hiện ô vuông thay cho chữ.
- **Độ dài câu.** Mỗi ngôn ngữ có độ dài khác nhau. Dịch sang ngôn ngữ dài hơn bản gốc thì dễ tràn chữ trong slide; hãy giữ câu "ưu tiên dịch ngắn gọn" trong quy tắc.
- **Mã ngắn "src" và "dst"** chỉ là tên trường dùng nội bộ trong quá trình dịch, không cần đổi.

## Giải thích ngắn

- **Chạy trên máy tính của bạn, không qua Google Drive:** kết nối Google Drive chỉ tạo được tài liệu Google, không giữ được định dạng PowerPoint hay PDF gốc. Muốn bản dịch giữ nguyên định dạng, tác vụ phải làm việc trực tiếp với thư mục trên máy, nơi có các công cụ đọc và ghi PowerPoint, PDF.
- **Tạo lại công cụ mỗi lần chạy:** mỗi lần chạy là một phiên mới, không nhớ gì và không giữ lại file của lần trước.
- **Nhận biết file đã dịch bằng hậu tố tên:** agent không cần nhớ lần trước đã dịch gì, chỉ cần nhìn thư mục đích.
- **Không xóa, không ghi đè, không sửa file nguồn:** tác vụ định kỳ chạy không hỏi duyệt, nên giới hạn việc được làm phải nằm ngay trong hướng dẫn.
- **Một file lỗi không làm dừng cả lượt:** lỗi được ghi vào báo cáo cuối phiên để bạn xem lại.
