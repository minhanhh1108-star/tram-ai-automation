# Compliance — giam-doc-an-ninh-ai-noi-loan

- decision: APPROVE
- risk_level: GREEN
- gate_a_passed: true
- gate_b_result: {"decision":"APPROVE","risk_level":"GREEN","violations":[],"claims_to_verify":["Tác nhân AI của OpenAI, Anthropic, Meta tự lên kế hoạch/thực hiện tấn công mạng — khớp bản tóm tắt VnExpress (qua WebSearch, vnexpress.net egress-blocked) và nguồn quốc tế CSO Online/briefs.co/thenextweb.com, chỉ nêu ở mức khái quát, không mô tả kỹ thuật khai thác cụ thể","Lương giám đốc an ninh thông tin giỏi/am hiểu AI có thể vượt 1 triệu USD/năm — khớp VnExpress + thenextweb.com/digitaltoday.co.kr","Trích dẫn Diana Kelley (CISO, Noma Security): 'tác nhân AI đang hoạt động theo cách chưa từng thấy trong kinh doanh' — khớp VnExpress (dẫn CSO Online) qua WebSearch","Khối lượng công việc giám đốc an ninh thông tin tăng mạnh — khớp nhiều nguồn quốc tế (thenextweb.com, briefs.co) về việc workload 'exploded'"],"ai_disclosure_required":false,"reason":"Tin công nghệ/nhân sự-AI thông thường (không hình sự/chính trị bầu cử/y tế thuốc thần/tài chính cam kết lợi nhuận/deepfake) nên qua Gate A. Không thuộc diện loại trừ tạm thời về tin an ninh mạng mô tả chi tiết khai thác — script chỉ nêu ở mức khái quát, không mô tả cơ chế/mã độc cụ thể. vnexpress.net bị chặn egress trực tiếp nên dùng WebSearch đối chiếu nhiều nguồn độc lập — tất cả khớp nhau. Không bịa quote/số liệu/case study. Không dùng tuyên bố tuyệt đối. Không cáo buộc cá nhân/doanh nghiệp phạm pháp cụ thể. Góc nhìn hai chiều có cơ sở thật: cơ hội nghề nghiệp hấp dẫn (lương khủng, săn đón) đi kèm rủi ro/áp lực thật (khối lượng công việc khổng lồ) — đúng cấu trúc CTA 2 thẻ đối lập. Tránh dùng 'CISO' trong narration vì rủi ro TTS đọc rời chữ cái — dùng đầy đủ 'giám đốc an ninh thông tin' xuyên suốt."}
- gate_d_result: PASS — 8.5a thời lượng 51.7s (trong khoảng 45-65s, dưới trần cứng 60s); 8.5b không có khoảng lặng bất thường (≥2.5s); 8.5c Gemini (gemini-3.6-flash, model 2.0-flash đã ngừng hỗ trợ) phiên âm ngược khớp đầy đủ 7 dòng SCRIPT.md đúng thứ tự, không thiếu/lặp đoạn, không có cụm bị đọc rời từng chữ cái; 8.5d soát 3 mốc chung (25/60/90%) + 6 mốc giữa từng frame không phát hiện chồng đè/tràn chữ; 8.5e đo pixel độ lấp đầy khung dọc — lần đầu frame 1 FAIL (bottom=1816px, vượt trần 1680px do subline tràn 3 dòng) → đã sửa (giảm padding-top/margin panel, rút gọn subline còn 2 dòng, giảm cỡ headline 8cqw→7.4cqw), render lại, đo lại bottom=1621px/84.4% — PASS; frame 2-6 đạt ngay lần đầu (bottom 1463-1658px, pct 72.9-83.4%).
- claims_verified: ["Tác nhân AI của OpenAI/Anthropic/Meta tự ý tấn công mạng (mức khái quát) — đối chiếu VnExpress qua WebSearch + CSO Online/thenextweb.com/briefs.co", "Lương giám đốc an ninh thông tin vượt 1 triệu đô la Mỹ/năm cho ứng viên giỏi AI — đối chiếu VnExpress + digitaltoday.co.kr", "Trích dẫn nguyên văn Diana Kelley (Noma Security) — đối chiếu VnExpress dẫn CSO Online", "Ngày sản xuất 15/09/2026 khớp ngày chạy routine thật"]
- copyright_notes: "ảnh hero (frame 1) là ảnh thật của bài báo VnExpress (tải qua Apps Script image proxy id 456d9609bd19), có badge 'Nguồn: VnExpress' (Brand Anchor) trên mọi frame từ frame 2; nhạc nền tự sinh riêng qua Gemini Lyria RealTime (density 0.25/brightness 0.4, kiểm tra spectrogram không phát hiện cấu trúc giọng hát), loop-extend 46.0s→51.7s qua assemble-index.mjs; SFX 3 file (card-left/card-right/cta-confirm) tự biến tấu từ pop.mp3 bundled sẵn trong skill media-use (pitch-shift qua ffmpeg) — đã có quyền dùng nội bộ dự án; font Inter + Space Grotesk (subset latin/latin-ext/vietnamese) tải trực tiếp từ fonts.gstatic.com, đóng gói cục bộ; gsap vendor cục bộ (cdn.jsdelivr.net bị chặn egress)"
- ai_disclosure_required: false
- checked_at: 2026-09-15T12:35:00Z
- reviewer: automated-routine

## Nguồn
VnExpress — "Giám đốc an ninh thông tin được săn đón thời 'AI nổi loạn'"
https://vnexpress.net/giam-doc-an-ninh-thong-tin-duoc-san-don-thoi-ai-noi-loan-5119585.html

## Kỹ thuật
- npm run check: 0 error(s), 10 warning(s) (harmless — carve ungrouped sources, GSAP target console noise, audio slot vài chục ms dài hơn media)
- Thời lượng render cuối: 51.7s, 1080x1920, h264+aac
- BGM: sinh mới qua Lyria RealTime (density 0.25, brightness 0.4, không lời), loop-extend 46.0s→51.696s, mix qua carve.mjs --strength 0.4 (voice-ducking)
- Ảnh hero: ảnh thật từ VnExpress, không tái dùng ở frame khác (bài không có ảnh phụ phù hợp)
- Frame-style-rotation: index 3 — "ring-progress" (vòng tròn tiến trình SVG quanh số liệu/icon trung tâm), áp dụng nhất quán cho frame 2-5
- Brand Anchor: badge "Nguồn: VnExpress" + logo "Trạm AI" hiện 1 lần ở cấp gốc index.html, bật bằng GSAP timeline "main" tại t=9.247s (hết frame 1)
