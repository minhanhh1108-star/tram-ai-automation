# Compliance — tai-tro-ba-ty-dong-ai

- decision: APPROVE
- risk_level: GREEN
- gate_a_passed: true
- gate_b_result: {"decision":"APPROVE","risk_level":"GREEN","violations":[],"claims_to_verify":["Mức tài trợ tối đa 3 tỷ đồng/nhiệm vụ — khớp Dân trí, VJST, Bộ KH&CN, Vietnamplus, Nhân Dân","Đơn vị chủ quản: Quỹ Đổi mới công nghệ quốc gia (NATIF), thuộc Bộ Khoa học và Công nghệ","Đối tượng: doanh nghiệp/tổ chức có tư cách pháp nhân, ứng dụng AI cho sản phẩm/dịch vụ/quy trình/mô hình kinh doanh mới hoặc cải tiến","Thời gian thực hiện tối đa 12 tháng","Yêu cầu chứng minh khả năng huy động vốn hợp pháp ngoài ngân sách nhà nước (mst.gov.vn)"],"ai_disclosure_required":false,"reason":"Tin chính sách tài trợ AI của nhà nước (NATIF/Bộ KH&CN); không thuộc nhóm loại trừ Gate A. Số liệu đối chiếu khớp nhiều nguồn qua WebSearch (WebFetch bị egress-block cho vnexpress.net/dantri.com.vn/mst.gov.vn). Góc tranh luận hai chiều (cơ hội mở cho mọi doanh nghiệp vs. rào cản vốn đối ứng/năng lực hồ sơ) có cơ sở thật từ chính điều kiện chương trình, dùng framing có điều kiện, không khẳng định tuyệt đối. Không bịa số liệu/quote, không viết tắt. Giọng ElevenLabs generic, ảnh thật có nguồn."}
- gate_d_result: PASS — 8.5a duration 63.9s (trong khoảng 45-65s); 8.5b không có khoảng lặng bất thường; 8.5c Gemini (gemini-3.6-flash, model cũ gemini-2.0-flash đã bị deprecate) phiên âm khớp đầy đủ 7 dòng script, không thiếu/lặp/đọc lắp; 8.5d soát ảnh 3 mốc + trung điểm từng frame không phát hiện chồng chữ/cắt chữ; 8.5e đo pixel: cả 6 frame đạt bottom trong khoảng 1400-1680px và fill 79-82% (>=55%); 8.5g số đếm động "3 tỷ đồng" lên đúng giá trị cuối, không có khoảng trống hoàn toàn giữa các frame ở mốc chuyển cảnh; 8.5f loudness sau loudnorm 2-pass đạt -14.0 LUFS đúng chuẩn, True Peak -0.8 dBTP lệch nhẹ 0.2dB so với trần -1.0 dBTP sau khi re-encode AAC (NGHI VẤN, không chặn theo quy tắc Gate D — không tự FAIL vì riêng bước loudness).
- claims_verified: ["3 tỷ đồng tài trợ tối đa/nhiệm vụ — khớp VnExpress + Dân trí + VJST + Bộ KH&CN", "Quỹ Đổi mới công nghệ quốc gia (NATIF) thuộc Bộ Khoa học và Công nghệ — khớp nguồn", "Thời gian thực hiện tối đa 12 tháng — khớp nguồn", "Hạn nộp hồ sơ 10/10/2026 — khớp nguồn", "Yêu cầu chứng minh vốn đối ứng ngoài ngân sách nhà nước — khớp mst.gov.vn"]
- copyright_notes: "ảnh thật nguồn VnExpress (robot hình người tại triển lãm công nghệ, TP Hồ Chí Minh), dùng ở frame 1 (hero) + frame 4 (tái dùng thu nhỏ, có chú thích nguồn); nhạc nền tự sinh qua Gemini Lyria (--density 0.25 --brightness 0.4, calm ambient, no drums/vocals); SFX pop-left/pop-right/pop-cta lấy từ catalog bundled sẵn của media-use skill (click-soft.mp3, pop.mp3, chime.mp3 đã cắt còn 0.5s)"
- ai_disclosure_required: false
- checked_at: 2026-09-18T00:37:20Z
- reviewer: automated-routine

## Ghi chú kỹ thuật khác

- Style khối nội dung (BƯỚC 6 xoay vòng): `timeline-chronology` — trục thời gian dọc 4 mốc (Đối tượng & hạn nộp → Điều kiện vốn đối ứng → Hệ quả tiếp cận → Kết luận), kèm bảng "lộ trình" tóm tắt 4 bước ở cuối mỗi frame nội dung để đạt yêu cầu lấp đầy khung dọc (Gate D 8.5e) — cải tiến so với rail đơn thuần.
- Model Gemini `gemini-2.0-flash` đã ngừng hoạt động (deprecated) từ phía Google; dùng `gemini-3.6-flash` theo gợi ý trả về từ chính API lỗi 404.
- WebFetch bị egress-block cho toàn bộ domain báo (vnexpress.net, dantri.com.vn, mst.gov.vn) trong environment này; toàn bộ fact-check dùng WebSearch đối chiếu nhiều nguồn độc lập thay thế, đúng quy trình dự phòng đã ghi trong ROUTINE.md.
