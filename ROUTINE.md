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

## Cập nhật chính sách

Sửa `automation/policy/COMPLIANCE-GATE.md` (hoặc 2 file gốc) → commit → lần chạy routine tiếp theo
tự động dùng bản mới, không cần sửa lại prompt của routine.
