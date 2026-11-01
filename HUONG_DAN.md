# Hướng dẫn dùng Tủ Sách Cá Nhân (PKV)

Chỉ cần file `pkv.html`. Mở trực tiếp bằng trình duyệt là dùng được, dữ liệu lưu ngay trên máy. Muốn dùng trên nhiều thiết bị thì kết nối GitHub (Phần 2). `Code.gs` là bản cũ đồng bộ qua Google Sheets, app hiện không còn dùng.

## Phần 1 — Khóa ứng dụng (tùy chọn)

- Menu **⋯ → Khóa ứng dụng → Đặt mật khẩu**. Từ đó mỗi lần mở web sẽ hỏi mật khẩu; tải lại trang trong cùng một tab thì không hỏi lại.
- Khóa **chỉ che giao diện**, dữ liệu trên máy không bị mã hóa. Người biết mở DevTools vẫn đọc được.
- **Quên mật khẩu**: bấm "Quên mật khẩu?" dưới màn khóa, gõ `XOA`. App xóa toàn bộ dữ liệu trên máy; bạn nhập lại từ file sao lưu JSON hoặc kết nối lại GitHub. Hãy xuất sao lưu định kỳ.

## Phần 2 — Đồng bộ qua GitHub (kho riêng)

### Tạo kho riêng
1. github.com → **New repository**, tên ví dụ `pkv-data`, chọn **Private**, tick **Add a README** → Create.

### Tạo Fine-grained token
1. Avatar → **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.
2. Đặt tên (ví dụ `pkv`) và chọn hạn dùng (tối đa 1 năm; hết hạn thì tạo token mới).
3. **Repository access → Only select repositories** → chọn đúng kho `pkv-data`.
4. **Permissions → Repository permissions → Contents → Read and write**. Không cần quyền nào khác.
5. **Generate token**, chép token (`github_pat_…`, chỉ hiện một lần).

### Kết nối trong app
1. Menu **⋯ → Cài đặt kết nối** (hoặc bấm nút **Đồng bộ** trên thanh công cụ).
2. Nhập tên người dùng GitHub, tên kho, đường dẫn file (mặc định `data.json`) và dán token → **Lưu & đồng bộ**.
3. Thiết bị thứ hai: làm lại đúng các bước này. Nếu dữ liệu đã được mã hóa, app hỏi **mật khẩu khóa** một lần để giải mã.

### Cách đồng bộ hoạt động
- Mỗi lượt: tải file về → **gộp** → đẩy lên nếu có khác biệt. App tự đồng bộ khi mở, sau mỗi lần lưu/chấm ôn, và khi quay lại tab. Nút **Đồng bộ** để làm ngay.
- Gộp theo `id` ghi chú: hai máy sửa cùng một ghi chú thì bản **có `updated_at` mới hơn** thắng (giữ đồng hồ các máy đúng giờ). Xóa mềm cũng được đồng bộ.
- Lịch ôn: bài có **nhiều lượt ôn hơn** thắng; nhật ký ngày học được hợp lại.
- Nếu hai máy đẩy cùng lúc, app tự tải lại và gộp lại tối đa 3 lần.
- Đã đặt mật khẩu khóa thì file trên GitHub được **mã hóa AES-256-GCM** (khóa suy ra từ mật khẩu bằng PBKDF2, 600.000 vòng); người ngoài thấy file cũng không đọc được. Chưa đặt mật khẩu thì file là JSON thường trong kho riêng. Đổi mật khẩu thì lượt đồng bộ kế tiếp mã hóa lại bằng khóa mới; tắt khóa **không** gỡ mã hóa.
- Mật khẩu yếu thì mã hóa yếu. Nên dùng cụm từ dài. Quên mật khẩu mà không còn máy nào đang mở được thì không giải mã được file trên GitHub; khi đó dùng file sao lưu JSON.
- Token được lưu trong trình duyệt của từng thiết bị (không mã hóa), nhưng chỉ có quyền trên đúng một kho. Nghi ngờ lộ token thì vào GitHub thu hồi (Revoke) và tạo token mới.

## Phần 3 — Đưa app lên GitHub Pages (tùy chọn, để mở trên điện thoại)

1. Tạo một kho **riêng biệt** cho app, ví dụ `pkv-app`, chọn **Public** (Pages miễn phí cần kho công khai). Tuyệt đối không dùng kho dữ liệu `pkv-data` cho việc này.
2. Upload `pkv.html` (chỉ file này) → **Settings → Pages** → Deploy from a branch → `main` / `(root)` → Save.
3. Sau 1–2 phút app mở được ở địa chỉ `https://<tên-github>.github.io/pkv-app/pkv.html` (đặt tên file là `index.html` thì bỏ được phần `pkv.html` ở cuối). Kho công khai vẫn an toàn vì token và dữ liệu **không nằm trong code**.
4. Điện thoại: mở địa chỉ, kết nối GitHub như Phần 2, rồi *Thêm vào màn hình chính* (Android Chrome: menu ⋮; iPhone Safari: nút Chia sẻ).

## Dùng hằng ngày

- Mở app: màn chào cho biết hôm nay có bao nhiêu bài cần ôn và bao nhiêu bài chưa duyệt. `Space` để bắt đầu.
- Duyệt ghi chú: `D` (✓ Duyệt); ghi chú đã duyệt vào lịch ôn sau 1 / 3 / 7 / 30 ngày. Ôn: `Space` lật thẻ, `1` chưa nhớ, `2` đã nhớ.
- **Thống kê**: chuỗi ngày học, số liệu theo chủ đề, nút "Ôn theo chủ đề này".
- Phím tắt: `/` hoặc `Ctrl+K` tìm, `Alt+N` thêm, `↑↓` chọn, `Ctrl+Enter` lưu, `E` sửa, `C` chép, `F` tập trung, `Esc` thoát.
- Tìm không cần gõ dấu: `quan tri`, `dieu 117`.
- Sao lưu định kỳ: **⋯ → Xuất sao lưu (JSON)** (gồm ghi chú, lịch ôn, số lượt ôn, nhật ký). Khôi phục: **⋯ → Nhập ghi chú**. Nạp ghi chú do NotebookLM/Gemini viết cũng ở đây, xem `PROMPT_NOTEBOOKLM.md`.
- Ghi chú pháp luật: ghi rõ văn bản + điều + **năm/phiên bản** ở ô Nguồn, dùng tag như `cần kiểm tra hiệu lực` cho điều dễ bị sửa đổi.

## Xử lý sự cố

| Hiện tượng | Nguyên nhân thường gặp |
|---|---|
| "Token sai hoặc hết hạn" | Token nhập sai, đã hết hạn hoặc bị thu hồi: tạo token mới |
| "Token thiếu quyền" | Chưa chọn **Contents: Read and write**, hoặc vượt giới hạn gọi API (đợi vài phút) |
| "Không thấy kho" | Sai tên người dùng / tên kho, hoặc token chưa được cấp quyền cho kho đó (Repository access) |
| "Cần mật khẩu · bấm Đồng bộ" | File trên GitHub đã mã hóa và máy này chưa có khóa: bấm **Đồng bộ** rồi nhập mật khẩu |
| "File GitHub lạ" | File ở đường dẫn đó không phải dữ liệu của app. App không ghi đè; đổi đường dẫn hoặc xóa file |
| "Xung đột, thử lại" | Hai máy đồng bộ cùng lúc quá nhiều lần: bấm Đồng bộ lại |
| "Mất khóa mã hóa" | Trình duyệt đã xóa dữ liệu IndexedDB: **⋯ → Đổi mật khẩu** (đặt lại cùng mật khẩu) rồi đồng bộ |
