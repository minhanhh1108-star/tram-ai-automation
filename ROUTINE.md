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
| Tải ảnh minh hoạ (`imageUrl`) trực tiếp từ CDN báo bị chặn egress | CDN báo (subdomain khác nhau tuỳ báo) không nằm trong allowlist và không ổn định để đuổi theo | Nếu Apps Script hỗ trợ route kiểu `?image=<id>` (tải hộ từ phía Google, trả base64) thì dùng route đó thay vì curl thẳng CDN — xem cách kênh "Bot Bán Hàng" đã làm |
| `git push` vào repo này bị từ chối dù repo public | Repo public chỉ cấp quyền đọc; quyền ghi cần cài Claude GitHub App lên account/org chứa repo rồi reconnect GitHub connector | Cài GitHub App (`github.com/apps/claude/installations/select_target`) lên account sở hữu `tram-ai-automation`, reconnect ở claude.ai Settings → Connectors |

## Cập nhật chính sách

Sửa `automation/policy/COMPLIANCE-GATE.md` (hoặc 2 file gốc) → commit → lần chạy routine tiếp theo
tự động dùng bản mới, không cần sửa lại prompt của routine.
