# Drive Music – nghe nhạc từ thư mục Google Drive

Web app (PWA) chạy trên Chrome máy tính và cài được lên Android như một app thật.
Toàn bộ miễn phí: Google Drive (lưu nhạc), Google Cloud (đăng nhập), Cloudflare Pages / Netlify / GitHub Pages (host).

## Bước 1 – Lấy Google Client ID (làm 1 lần, ~5 phút)

1. Vào https://console.cloud.google.com → tạo **Project** mới (tên tùy ý).
2. Menu **APIs & Services → Library** → tìm **Google Drive API** → **Enable**.
3. **APIs & Services → OAuth consent screen** (hoặc "Google Auth Platform"):
   - User type: **External**, điền tên app (vd. Drive Music) và email của bạn.
   - Để trạng thái **Testing** (không cần xin Google duyệt).
   - Mục **Test users**: thêm đúng địa chỉ Gmail đang chứa thư mục nhạc.
4. **APIs & Services → Credentials → Create credentials → OAuth client ID**:
   - Application type: **Web application**
   - **Authorized JavaScript origins**: thêm địa chỉ web app của bạn sau khi host xong
     (vd. `https://ten-cua-ban.pages.dev`), và `http://localhost:8080` nếu muốn thử trên máy.
     Chỉ ghi domain, không có dấu `/` ở cuối, không có đường dẫn.
   - Bấm Create → copy **Client ID** (dạng `xxxx.apps.googleusercontent.com`).

## Bước 2 – Đưa web app lên host miễn phí (cần HTTPS)

Chọn 1 trong các cách (đều chỉ cần upload cả thư mục này):

- **Cloudflare Pages**: Workers & Pages → Create → Pages → Upload assets → kéo thả thư mục.
- **Netlify Drop**: vào https://app.netlify.com/drop → kéo thả thư mục.
- **GitHub Pages**: tạo repo, đẩy các file lên, Settings → Pages → chọn nhánh main.

Sau khi có địa chỉ, quay lại Bước 1.4 để thêm địa chỉ đó vào *Authorized JavaScript origins*
(có thể mất vài phút để Google cập nhật).

Thử nhanh trên máy tính (không cần host): mở terminal trong thư mục này, chạy
`python3 -m http.server 8080` rồi vào http://localhost:8080.

## Bước 3 – Dùng

1. Mở web app → ⚙ **Cài đặt** → dán **Client ID** và **link thư mục nhạc** trên Drive
   (mở thư mục trên drive.google.com, copy link trên thanh địa chỉ).
2. Bấm **Lưu & quét thư mục** → đăng nhập Google.
   Lần đầu Google sẽ cảnh báo "app chưa được xác minh": bấm **Nâng cao → Tiếp tục** (app do chính bạn tạo, an toàn).
3. Danh sách bài hát được lưu lại trên máy, lần sau mở là có ngay. Thêm nhạc mới vào Drive thì bấm ⟳.

### Cài lên Android
Mở địa chỉ web app bằng **Chrome** → menu ⋮ → **Cài đặt ứng dụng** (Install app) / Thêm vào màn hình chính.
App mở toàn màn hình, có điều khiển phát nhạc ở thanh thông báo và màn hình khóa.

## Tính năng

- Phát nhạc trực tiếp từ Drive (tua được, không cần tải cả file): mp3, m4a, aac, flac, wav, ogg, opus
- Quét cả thư mục con, xem theo **Tất cả / Thư mục / Yêu thích / Đang phát**
- Tìm kiếm theo tên bài, ca sĩ, thư mục
- Phát / dừng, bài trước / sau, thanh tua, âm lượng
- Trộn bài (shuffle), lặp lại (tắt / tất cả / 1 bài)
- Yêu thích ♥ (lưu trên thiết bị)
- Hẹn giờ tắt nhạc
- Nhớ bài đang nghe, âm lượng, chế độ trộn / lặp
- Điều khiển từ thanh thông báo, màn hình khóa, phím media (Media Session)
- Phím tắt máy tính: Space, ←/→ (tua 5s), N / P, S, R
- Tên bài lấy từ tên file: `Ca sĩ - Tên bài.mp3` sẽ tự tách ca sĩ và tên bài

## Lưu ý

- **Phiên đăng nhập Google kéo dài ~1 giờ.** Hết hạn thì bấm Phát/⟳ lần nữa, cửa sổ đăng nhập sẽ tự hiện
  (thường chỉ nháy qua vì đã cấp quyền). Nếu hết hạn đúng lúc nhạc tự chuyển bài khi màn hình tắt, nhạc sẽ dừng cho tới khi bạn bấm Phát.
- App chỉ có quyền **đọc** Drive (`drive.readonly`), không sửa hay xóa được gì.
- Tên bài lấy từ tên file, không đọc thẻ ID3 và không có ảnh bìa (cố ý bỏ để app đơn giản, ổn định).
- Danh sách phát tự tạo và nghe offline chưa có. Có thể thêm sau nếu cần.
- Muốn dùng trên máy khác: mở cùng địa chỉ web, nhập lại Client ID + link thư mục (yêu thích lưu riêng từng thiết bị).
