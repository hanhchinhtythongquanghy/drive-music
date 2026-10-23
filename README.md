# Drive Music – nghe nhạc từ thư mục Google Drive

Web app (PWA) chạy trên Chrome máy tính và cài được lên Android như một app thật.
Miễn phí hoàn toàn: Google Drive (lưu nhạc), Google Cloud (API key), Cloudflare Pages / Netlify / Vercel / GitHub Pages (host).

App có 2 chế độ, tự chọn theo ô bạn điền trong ⚙ Cài đặt:
- **API key** (khuyên dùng, hướng dẫn bên dưới): không cần đăng nhập, không bị hết phiên. Thư mục phải chia sẻ "Bất kỳ ai có đường liên kết".
- **OAuth Client ID**: thư mục giữ riêng tư nhưng phải đăng nhập Google và hết phiên mỗi ~1 giờ (xem cuối file).

## Cách 1 – Dùng API key

### Bước 1 – Chia sẻ thư mục nhạc
1. Mở thư mục nhạc trên Google Drive → chuột phải → **Chia sẻ**.
2. Mục "Quyền truy cập chung" chọn **Bất kỳ ai có đường liên kết** → vai trò **Người xem**.
3. Copy link thư mục (dạng `https://drive.google.com/drive/folders/XXXXXXXX`).

Lưu ý: ai có link thư mục đều nghe được nhạc, đừng để file riêng tư trong đó. Thư mục con được quét tự động
(nằm trong thư mục cha đã chia sẻ thì cũng được chia sẻ theo).

### Bước 2 – Tạo API key
1. Vào https://console.cloud.google.com → tạo **Project** mới (tên bất kỳ).
2. **APIs & Services → Library** → tìm **Google Drive API** → **Enable**.
3. **APIs & Services → Credentials → Create credentials → API key**.
4. Bấm vào key vừa tạo để giới hạn (nên làm vì key nằm trong trang web):
   - **API restrictions** → Restrict key → chọn **Google Drive API**.
   - **Application restrictions** → **Websites** → thêm tên miền web của bạn sau khi host,
     ví dụ `https://ten-cua-ban.pages.dev/*`, và `http://localhost:8000/*` nếu muốn thử trên máy.

### Bước 3 – Đưa web lên mạng (chọn một, đều miễn phí, cần HTTPS)
Đây là web tĩnh, không có lệnh build, thư mục gốc chính là thư mục xuất bản.
- **Cloudflare Pages**: Create project → Direct Upload → kéo thả thư mục.
- **Netlify**: https://app.netlify.com/drop → kéo thả thư mục.
- **Vercel**: đẩy lên repo GitHub → Import → Framework "Other", để trống Build Command và Output Directory.
- **GitHub Pages**: đẩy lên repo → Settings → Pages → nhánh `main`, thư mục `/ (root)`.

Thử nhanh trên máy tính: mở terminal trong thư mục này, gõ `python3 -m http.server 8000`, vào http://localhost:8000.

### Bước 4 – Kết nối
Mở web → ⚙ **Cài đặt** → dán **API key** và **link thư mục** → **Lưu & quét thư mục**.
Danh sách bài lưu trên máy nên lần sau mở là có nhạc ngay. Thêm nhạc mới vào Drive thì bấm ⟳.
(Ô Client ID để trống.)

### Cài lên Android
Mở địa chỉ web bằng **Chrome** → menu ⋮ → **Cài đặt ứng dụng** (Install app) / Thêm vào màn hình chính.

## Cách 2 – Dùng OAuth Client ID (thư mục riêng tư)

1. Cloud Console → OAuth consent screen: External, trạng thái **Testing**, thêm Gmail của bạn vào **Test users**.
2. Credentials → Create credentials → **OAuth client ID** → Web application.
   **Authorized JavaScript origins** phải khớp chính xác địa chỉ web đang mở
   (vd. `https://ten-cua-ban.pages.dev`, không có `/` cuối, không có đường dẫn).
   Sai chỗ này sẽ báo lỗi `origin_mismatch`. Chờ vài phút sau khi lưu.
3. Dán **Client ID** (để trống ô API key) rồi quét; đăng nhập Google, chọn "Nâng cao → Tiếp tục" ở cảnh báo app chưa xác minh.
4. Phiên đăng nhập kéo dài ~1 giờ, hết thì bấm Phát/⟳ để đăng nhập lại.

## Tính năng
- Phát trực tiếp từ Drive, tua được: mp3, m4a, aac, flac, wav, ogg, opus
- Quét cả thư mục con; xem theo Tất cả / Thư mục / Yêu thích / Đang phát
- Tìm kiếm theo tên bài, ca sĩ, thư mục
- Phát/dừng, bài trước/sau, thanh tua, âm lượng
- Trộn bài, lặp lại (tắt / tất cả / 1 bài), yêu thích ♥, hẹn giờ tắt nhạc
- Nhớ bài đang nghe, âm lượng, chế độ trộn/lặp
- Điều khiển từ thanh thông báo, màn hình khóa, phím media
- Phím tắt máy tính: Space, ←/→ (tua 5s), N / P, S, R
- Tên file dạng `Ca sĩ - Tên bài.mp3` tự tách ca sĩ và tên bài

## Lưu ý
- Không đọc thẻ ID3, không có ảnh bìa; chưa có danh sách phát tự tạo và nghe offline.
- Yêu thích lưu riêng trên từng thiết bị.
- Nếu nhiều bài liên tiếp không phát được: kiểm tra API key đúng, đã bật Drive API, tên miền nằm trong danh sách giới hạn,
  và thư mục đang để "Bất kỳ ai có đường liên kết". File công khai bị nghe quá nhiều có thể bị Google tạm giới hạn lượt tải.
