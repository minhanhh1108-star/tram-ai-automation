# Compliance — anthropic-canh-bao-ai-mat-kiem-soat

- decision: APPROVE
- risk_level: YELLOW
- gate_a_passed: true
- gate_b_result: {"decision":"APPROVE","risk_level":"YELLOW","violations":[],"claims_to_verify":[],"ai_disclosure_required":false,"reason":"Tin công nghệ/AI chính thống (VnExpress, dẫn lại bài luận công khai của Dario Amodei và phản ứng của Sam Altman) đã được đối chiếu chéo qua WebSearch — không có số liệu/trích dẫn bịa. Khung tranh luận 'an toàn vs tốc độ, phương Tây vs Trung Quốc' có cơ sở thật trong bài (chính Amodei gọi đây là 'bài toán khó nhất'), không tự suy diễn thêm. Không cáo buộc cá nhân/doanh nghiệp cụ thể sai phạm, không đề cập chính trị nội bộ Mỹ. Gắn YELLOW vì có khung địa chính trị nhẹ (cạnh tranh Mỹ-Trung trong AI) dù không cáo buộc ai — mức YELLOW vẫn APPROVE theo Gate B, không cần sửa."}
- gate_d_result: PASS — 8.5a thời lượng 57.73s (trong khoảng 45-65s, dưới trần 60s); 8.5b không có khoảng lặng bất thường (≥2.5s); 8.5c Gemini phiên âm ngược khớp đầy đủ 7 dòng script, đúng thứ tự, chỉ lệch nhỏ do STT (cẩn trọng→thận trọng, số/chữ số); 8.5d soát hình ảnh phát hiện lỗi hiển thị ở Frame 2 (counter đếm động dừng ở "12" cạnh nhãn tĩnh "–12" tạo thành "12 –12" vô nghĩa, thay vì "6–12" đúng ý "6 đến 12 tháng") — đã sửa counter target về 6, render lại, xác nhận hiển thị đúng "6–12 THÁNG"; các frame còn lại (1,3,4,5,6 + mốc 25%/60%/90%) không phát hiện chồng đè/tràn chữ.
- checked_at: 2026-09-14T03:49:54Z
- reviewer: automated-routine
- post_publish_fix: 2026-09-14T04:10:00Z — người dùng phản hồi dòng subline thứ 2 của Frame 1 ("nóng hơn bao giờ hết") bị cắt mất ở đáy khung 1920px. Nguyên nhân: khối nội dung trong `.f1-panel` (masthead offset + pills margin-top 9cqh + textblock margin-top 3.6cqh + headline 9.2cqw/3 dòng + subline 5.4cqw/2 dòng) cao hơn không gian khả dụng của panel (45%×1920 trừ padding đáy), bị `.f1-scene{overflow:hidden}` cắt — `npm run check` Layout không phát hiện (0 issues cả trước/sau). Đã sửa: tỷ lệ ảnh/panel 55/45→48/52, pills margin-top 9cqh→5.5cqh, textblock margin-top 3.6cqh→2.2cqh, gap 1.6cqh→1.2cqh, headline 9.2cqw→8cqw, subline 5.4cqw→5.2cqw/line-height 1.4→1.35, panel padding-bottom 5cqh→3.6cqh. Render lại (57.733s, không đổi), xác nhận bằng mắt cả 2 dòng subline hiện đầy đủ. Video/thumbnail trong repo đã cập nhật; 4 bài đã đăng (Facebook/YouTube/Instagram/Threads) từ Bước 10 KHÔNG được cập nhật lại (giới hạn API các nền tảng khi update media sau khi đăng) — bài đã đăng vẫn dùng bản có lỗi.

## Nguồn
VnExpress — "Lo AI mất kiểm soát, CEO Anthropic kêu gọi giảm tốc độ phát triển mô hình"
https://vnexpress.net/lo-ai-mat-kiem-soat-ceo-anthropic-keu-goi-giam-toc-do-phat-trien-mo-hinh-5119678.html

## Kỹ thuật
- npm run check: 0 error(s), 2 warning(s) (harmless — audio slot ~0.05s dài hơn media, carve ungrouped sources)
- Thời lượng render cuối: 57.733s, 1080x1920, h264+aac
- BGM: sinh mới qua Lyria RealTime (bpm 112, brightness 0.55, density 0.45), loop-extend 52.0s→57.73s, mix qua carve.mjs (voice-ducking)
- Ảnh hero: ảnh thật từ VnExpress (Dario Amodei), tái dùng ở đúng 1 frame khác (frame 2) kèm chú thích "Ảnh: VnExpress"
