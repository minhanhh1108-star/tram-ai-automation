# Compliance — trung-quoc-bac-bo-canh-bao-ai

- decision: APPROVE
- risk_level: YELLOW
- gate_a_passed: true
- gate_b_result: {"decision":"APPROVE","risk_level":"YELLOW","violations":[],"claims_to_verify":["Amodei công bố bài viết kêu gọi làm chậm AI ngày 12/9 — xác nhận qua WebSearch (VnExpress, Dân Trí, NBC News, Yahoo Finance)","Phát ngôn viên Bộ Ngoại giao Trung Quốc Quách Gia Côn gọi cảnh báo là 'gieo rắc sợ hãi' ngày 14/9 — xác nhận nhiều nguồn","Global Times gọi đây là kịch bản Chiến tranh Lạnh AI nhắm vào Trung Quốc — xác nhận qua Dân Trí/tincongnghe.net"],"ai_disclosure_required":false,"reason":"Tin công nghệ/AI chính thống (VnExpress), đối chiếu chéo qua WebSearch với nhiều nguồn độc lập — không bịa số liệu/trích dẫn. Khung tranh luận 'cảnh báo an toàn thật lòng vs chiêu cạnh tranh địa chính trị' có cơ sở thật trong bài. Gắn YELLOW vì có khung địa chính trị Mỹ-Trung (giống tiền lệ video anthropic-canh-bao-ai-mat-kiem-soat), không phải tin bầu cử/chính trị nội bộ — mức YELLOW vẫn APPROVE."}
- gate_d_result: PASS — 8.5a thời lượng 57.13s (trong khoảng 45-65s, dưới trần mềm 60s); 8.5b không phát hiện khoảng lặng ≥2.5s; 8.5c Gemini (gemini-3.6-flash, model 2.0-flash đã ngừng hỗ trợ) phiên âm ngược khớp đầy đủ 7 dòng SCRIPT.md đúng thứ tự, không thiếu/lặp/đọc rời chữ cái; 8.5d soát 3 mốc chung (25/60/90%) + 6 mốc giữa từng frame không phát hiện chồng đè/tràn/cắt chữ, contrast rõ, Brand Anchor không bị che; 8.5e đo pixel độ lấp đầy khung dọc cho cả 6 frame — lần đầu frame 1 (bottom=1692px) và frame 6 (bottom=1764px) vượt trần 1680px do panel/logo neo quá thấp, và frame 2-5 (bottom 1235-1379px) hụt sàn 1400px do dùng flex `justify-content:center` trên toàn khung 1920px (tâm khối chữ quá cao) → sửa: frame 1 tăng padding-bottom panel 5cqh→6.6cqh; frame 6 nâng `bottom` của logo neo thấp 8cqh→13cqh; frame 2-5 đổi sang định vị `position:absolute; top:1080px; transform:translateY(-50%)` (frame 2 tinh chỉnh thêm top:1160px vì khối nội dung ngắn nhất) để tâm khối chữ rơi đúng dải yêu cầu — render lại 3 lần, kết quả cuối: frame1 bottom=1677px/87.3%, frame2 bottom=1435px/72.0%, frame3 bottom=1417px/71.0%, frame4 bottom=1499px/75.3%, frame5 bottom=1474px/74.0%, frame6 bottom=1668px/84.1% — tất cả đạt PASS (bottom trong 1400-1680px, pct≥55%).
- claims_verified: ["Amodei công bố bài viết kêu gọi làm chậm AI ngày 12/9 — đối chiếu VnExpress qua WebSearch + Dân Trí/NBC News/Yahoo Finance", "Phát ngôn viên Bộ Ngoại giao Trung Quốc Quách Gia Côn gọi cảnh báo là 'gieo rắc sợ hãi và đối đầu' ngày 14/9 — đối chiếu nhiều nguồn", "Global Times gọi đây là kịch bản Chiến tranh Lạnh AI nhắm vào Trung Quốc — đối chiếu Dân Trí/tincongnghe.net", "Ngày sản xuất 16/09/2026 khớp ngày chạy routine thật"]
- copyright_notes: "ảnh hero (frame 1, tái dùng ở thumbnail) là ảnh thật của bài báo VnExpress (tải qua Apps Script image proxy id cc44f5c26be2), badge 'Nguồn: VnExpress' (Brand Anchor) hiện từ frame 2 tới hết video, ảnh không dùng quá 1 frame nội dung (chỉ frame 1); nhạc nền tự sinh riêng qua Gemini Lyria RealTime (density 0.25/brightness 0.4, không lời — xác nhận bằng phiên âm Gemini 8.5c không phát hiện giọng hát lẫn trong nhạc nền), loop-extend 52.0s→57.1s qua assemble-index.mjs; SFX 3 file (cta-left/cta-right/cta-confirm) biến tấu pitch-shift qua ffmpeg từ 1 file bundled sẵn trong skill media-use (resolve --type sfx) — đã có quyền dùng nội bộ dự án; font Inter + Montserrat dùng bản pre-bundle của HyperFrames compiler (không cần @font-face/CDN ngoài, tránh phụ thuộc mạng); gsap vendor cục bộ (cdn.jsdelivr.net có thể bị chặn egress, dùng vendor/gsap.min.js cài qua npm)"
- ai_disclosure_required: false
- checked_at: 2026-09-16T00:40:45Z
- reviewer: automated-routine

## Nguồn

VnExpress — "Trung Quốc gọi loạt cảnh báo về AI là 'gieo rắc sợ hãi'"
https://vnexpress.net/trung-quoc-goi-loat-canh-bao-ve-ai-la-gieo-rac-so-hai-5120441.html

## Kỹ thuật

- npm run check: 0 error(s), 2 warning(s) (harmless — audio_carve_ungrouped_sources, clip_media_fit lệch 0.05s), 1 info (Ken Burns zoom ảnh hero tràn khung có chủ đích)
- Thời lượng render cuối: 57.133s, 1080x1920, h264+aac, 8.5MB
- BGM: sinh mới qua Lyria RealTime (density 0.25, brightness 0.4, không lời, instrumental only), loop-extend 52.0s→57.1s qua assemble-index.mjs ensureBgmCovers, mix volume 0.09, carve.mjs --strength 0.4 (voice-ducking, floor -9.6dB)
- Ảnh hero: ảnh thật VnExpress, dùng ở frame 1 hero (nửa trên ảnh, nửa dưới panel Deep Navy) — không tái dùng ở frame khác trong video này (không có card ảnh phụ vì layout hero đã dùng hết ngân sách frame 1)
- Frame-style-rotation: index 4 — "timeline-chronology" (mini-timeline mốc sự kiện có nhãn ngày/thực thể, một mốc sáng nổi bật), áp dụng nhất quán cho frame 2-5 (frame 1 hero và frame 6 CTA không thuộc vòng xoay theo đúng quy tắc)
- Brand Anchor: badge "Nguồn: VnExpress" (trái) + logo "TRẠM AI" (phải) ở cấp gốc index.html, bật bằng GSAP timeline "main" tại t=13.349s (hết frame 1 Hero+Context), giữ nguyên tới hết video
- Gate D 8.5e (pixel-fill): xem chi tiết ở gate_d_result — đã sửa và render lại 3 lần trước khi đạt PASS toàn bộ 6 frame
