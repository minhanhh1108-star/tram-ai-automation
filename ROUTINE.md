# Trạm AI — Routine sản xuất + đăng video tự động

Repo này lưu trạng thái cấu hình chính sách/kiểm duyệt cho routine cloud
`tram-ai-video-production` (Claude Code routine, chạy 3 lần/ngày — 7h/12h/19h giờ Việt Nam). Bản
thân routine không lưu trong repo (cấu hình qua Claude Code Routines API), nhưng nó clone repo
này ở mỗi lần chạy để đọc chính sách kiểm duyệt và ghi lại nhật ký audit.

## Kiến trúc

```
Lấy tin (news_queue qua Apps Script)
    ↓
GATE A — automation/policy/COMPLIANCE-GATE.md § Gate A (lọc tin trước khi viết script)
    ↓
Viết storyboard + script
    ↓
GATE B — automation/policy/COMPLIANCE-GATE.md § Gate B (kiểm duyệt script trước khi lồng tiếng/dựng)
    ↓
Lồng tiếng (proxy qua Apps Script — không lộ API key ra cloud routine)
    ↓
Dựng frame HyperFrames + nhạc nền
    ↓
Render + npm run check (gate kỹ thuật)
    ↓
GATE C — automation/policy/COMPLIANCE-GATE.md § Gate C (ghi videos/<slug>/COMPLIANCE.md, commit)
    ↓
Upload vào Google Drive TRAM_AI_VIDEO_QUEUE
    ↓
Apps Script (trigger postScheduledVideo, 7h/12h/19h) tự đăng lên Facebook/YouTube/Instagram/Threads
```

## Thư mục

- `automation/policy/` — 2 tài liệu chính sách gốc (Meta V3, Google/YouTube V1) +
  `COMPLIANCE-GATE.md` (checklist rút gọn, routine đọc trực tiếp file này).
- `videos/<slug>/COMPLIANCE.md` — nhật ký kiểm duyệt của từng video đã qua Gate B, ghi ở Gate C.

## Hạ tầng liên quan (không nằm trong repo này)

- Apps Script "Tram AI News Fetch" — lấy tin, proxy lồng tiếng an toàn (ELEVENLABS_API_KEY trong
  Script Properties), đăng bài 4 kênh theo lịch.
- Google Drive folder `TRAM_AI_VIDEO_QUEUE` — hàng đợi video chờ đăng.
- Google Cloud project `tram-ai-youtube-automation` — OAuth YouTube (đã publish production, không
  còn giới hạn refresh token 7 ngày).

## Environment (claude.ai/code) — network access

Routine chạy trong sandbox Linux, mọi request HTTPS đi qua 1 egress proxy chỉ cho phép domain nằm
trong allowlist của environment (`env_01TMbLEyRM3acJv6GcxNADPB`, tên "Default"). Environment này
**bắt buộc phải đặt Network access = "Custom"** (không dùng mặc định "Trusted" — mức đó KHÔNG bao
gồm `script.google.com` nên toàn bộ pipeline sẽ dừng ngay ở Bước 1).

Allowlist tối thiểu cần có (Custom):
- `script.google.com`, `script.googleusercontent.com` — endpoint Apps Script "Tram AI News Fetch"
  (lấy tin, đánh dấu used, proxy lồng tiếng). Đây là domain bắt buộc nhất — routine hoàn toàn phụ
  thuộc endpoint này cho cả Bước 1 và Bước 4.
- `github.com`, `raw.githubusercontent.com`, `objects.githubusercontent.com` — git push GATE C.
- `registry.npmjs.org` và các registry gói khác — thường đã được proxy bypass thẳng (không cần
  thêm), dùng cho `npx hyperframes`.

Không cần thêm domain CDN ảnh báo (`vnecdn.net`, `icdn.dantri.com.vn`...) hay `api.elevenlabs.io` —
Trạm AI không gọi thẳng các domain đó (khác kiến trúc của kênh "Bot Bán Hàng · Kinh Doanh", repo
`github.com/quangnv-cloud/bot-ban-hang-kinh-doanh`, xem `PIPELINE-PLAYBOOK.md` của họ để tham khảo
kiến trúc gốc — cùng ý tưởng "đẩy mọi thứ nhạy cảm ra Apps Script" nhưng họ gọi ElevenLabs/Gemini
trực tiếp nên cần allowlist thêm domain đó, Trạm AI thì không).

### Bảng lỗi đã gặp

| Triệu chứng | Nguyên nhân | Cách xử lý |
|---|---|---|
| Routine dừng ngay Bước 1: `curl script.google.com` → `403`/`CONNECT tunnel failed` | Environment network access đang ở mức "Trusted" (mặc định), không có `script.google.com` trong allowlist | Đổi Network access sang "Custom", thêm `script.google.com` + `script.googleusercontent.com` (xem mục trên) |
| `apt-get install ffmpeg` báo `404 Not Found` với vài gói phụ (`libva*`, `mesa-*-drivers`, `libssh-gcrypt-4`...) | Package index apt trong container bị cũ so với mirror, không liên quan network policy | Chạy `apt-get update` trước, sau đó `apt-get install -y ffmpeg` |
| `POST` tới Apps Script (`?action=...`) luôn trả về body rỗng hoặc trang lỗi "Sorry, unable to open the file at this time" — dù `doPost` trong `Code.gs` thực thi đúng (log Executions báo "Đã hoàn thành") | Apps Script trả `302 → script.googleusercontent.com/macros/echo?...`. `curl` mặc định (không `-L`) không theo redirect → body rỗng. `curl -L`/`--post302` theo redirect nhưng xử lý sai request → lỗi. Đây là lớp lỗi tương tự bên kênh "Bot Bán Hàng" đã gặp với Facebook Graph API 302. | **KHÔNG dùng `curl -L` cho POST tới Apps Script.** Gửi POST không theo redirect, tự đọc header `Location`, rồi GET riêng tới URL đó: `curl -sS -D h.txt -o /dev/null -X POST "$EXEC" ...` → `LOC=$(grep -i '^location:' h.txt \| sed 's/^location: //I' \| tr -d '\r')` → `curl -sS "$LOC"` mới là JSON thật. Áp dụng cho MỌI POST tới endpoint này (đánh dấu tin đã dùng, `generate_narration`). |
| Tải ảnh minh hoạ (`imageUrl`) trực tiếp từ CDN báo bị chặn egress | CDN báo (subdomain khác nhau tuỳ báo) không nằm trong allowlist và không ổn định để đuổi theo | Nếu Apps Script hỗ trợ route kiểu `?image=<id>` (tải hộ từ phía Google, trả base64) thì dùng route đó thay vì curl thẳng CDN — xem cách kênh "Bot Bán Hàng" đã làm |
| `git push` vào repo này bị từ chối dù repo public | Repo public chỉ cấp quyền đọc; quyền ghi cần cài Claude GitHub App lên account/org chứa repo rồi reconnect GitHub connector | Cài GitHub App (`github.com/apps/claude/installations/select_target`) lên account sở hữu `tram-ai-automation`, reconnect ở claude.ai Settings → Connectors |
| `GET` thường (không kèm `?action=`, kể cả lấy danh sách tin và `?image=<id>`) tới Apps Script cũng trả `302` header rỗng, không chỉ riêng `POST` | Toàn bộ endpoint Apps Script (mọi verb) đều đi qua redirect `script.googleusercontent.com/macros/echo?...`, không riêng gì `doPost` | Áp dụng ĐÚNG pattern POST→Location→GET ở mục "LƯU Ý KỸ THUẬT" của routine cho **mọi** request tới endpoint này, kể cả GET — không có ngoại lệ "GET thường gọi thẳng" |
| `WebFetch` bài báo qua `link` (Bước 1) báo `EGRESS_BLOCKED` cho `vnexpress.net`, `tech.zingnews.vn`, `vietnamnet.vn` | 3 domain nguồn tin chính KHÔNG nằm trong Custom allowlist của environment (khác với domain ảnh CDN, vốn đã né được nhờ proxy `?image=`) — đây là domain đọc bài, không có proxy tương đương | Dùng `WebSearch` (không đi qua egress proxy bị chặn) để tìm tóm tắt/trích dẫn bài báo thay cho `WebFetch` trực tiếp — verify kỹ số liệu/quote qua nhiều kết quả search trước khi viết script. Muốn hết hẳn vấn đề này thì thêm 3 domain trên vào Custom allowlist |
| `npm run check` / render báo `Failed to load npm/gsap@3.14.2/dist/gsap.min.js: net::ERR_TUNNEL_CONNECTION_FAILED` rồi `gsap is not defined` | `cdn.jsdelivr.net` (và `unpkg.com`) — nơi `assemble-index.mjs` mặc định nhúng `<script src>` cho GSAP — không nằm trong Custom allowlist, dù `registry.npmjs.org` (dùng cho `npx`/`npm install`) vẫn thông | Cài `gsap` qua npm (registry vẫn thông): `npm install gsap@3.14.2 --no-save`, copy `node_modules/gsap/dist/gsap.min.js` vào `vendor/gsap.min.js` trong project, rồi sửa `index.html` (sau khi `assemble-index.mjs` chạy xong) đổi `<script src="https://cdn.jsdelivr.net/...">` thành `<script src="vendor/gsap.min.js">`. Cần làm lại bước sửa này mỗi lần `assemble-index.mjs` được chạy lại (nó ghi đè `index.html`). Muốn hết hẳn thì thêm `cdn.jsdelivr.net` vào Custom allowlist |
| Bước 10 (upload Drive) không thể đưa `renders/video.mp4` (và thường cả `thumbnail.jpg`) vào `TRAM_AI_VIDEO_QUEUE` — không có lỗi rõ ràng, chỉ đơn giản là bất khả thi | Công cụ Google Drive connector (`create_file`) chỉ nhận nội dung nhúng thẳng (`base64Content`/`textContent`) trong tham số gọi tool, không hỗ trợ upload theo đường dẫn file cục bộ hay upload resumable/theo chunk. Một video ~1-6MB base64 hoá thành ~1.3-8.7 triệu ký tự — vượt xa giới hạn output một lượt trả lời của model, và việc "gõ lại" chuỗi base64 dài cỡ đó có rủi ro sai lệch/hỏng file thật. File nhỏ (JSON caption vài trăm byte) thì upload được bình thường | Chưa có cách tự động hoá an toàn trong môi trường hiện tại. Tạm thời: dùng `SendUserFile` để giao trực tiếp `video.mp4`/`thumbnail.jpg`/`<slug>.json` cho người dùng, rồi họ tự kéo thả vào thư mục Drive `TRAM_AI_VIDEO_QUEUE`. Đừng để lại file `.json` mồ côi trong thư mục Drive nếu video không upload kèm được (xoá/trash nó) — Apps Script có thể hiểu nhầm thành 1 item hàng đợi thiếu video. Muốn tự động hoá thật cần 1 công cụ Drive khác hỗ trợ upload từ đường dẫn cục bộ hoặc resumable upload |

## Cập nhật chính sách

Sửa `automation/policy/COMPLIANCE-GATE.md` (hoặc 2 file gốc) → commit → lần chạy routine tiếp theo
tự động dùng bản mới, không cần sửa lại prompt của routine.
