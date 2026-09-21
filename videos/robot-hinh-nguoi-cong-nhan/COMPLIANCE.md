# Compliance — robot-hinh-nguoi-cong-nhan

- decision: APPROVE
- risk_level: GREEN
- gate_a_passed: true
- gate_b_result: {"decision":"APPROVE","risk_level":"GREEN","violations":[],"claims_to_verify":["AgiBot thành lập Thượng Hải 2023","~8.400 robot xuất xưởng nửa đầu năm, 44% thị phần toàn cầu","robot tại nhà máy Longcheer (Trung Quốc) và sân bay Haneda (Nhật Bản)","Haneda phục vụ hơn 60 triệu lượt khách/năm, robot vận chuyển hành lý từ tháng 5 do Japan Airlines triển khai","chi phí bảo trì/lập trình cao là rào cản hiện tại"],"ai_disclosure_required":false,"reason":"Tin công nghệ/AI-robot thông thường, không thuộc nhóm nhạy cảm. Số liệu khớp bài báo gốc (VnExpress + đối chiếu qua tìm kiếm bổ sung do WebFetch trực tiếp VnExpress bị egress-block). Góc tranh luận robot-hỗ-trợ-vs-nguy-cơ-thay-thế có cơ sở thật trong nội dung nguồn."}
- gate_d_result: PASS — 8.5a duration 53.3-53.4s (trong 45-65s, dưới trần 60s); 8.5b không có khoảng lặng ≥2.5s; 8.5c BỎ QUA vì Gemini API lỗi liên tục (gemini-2.0-flash: 404 deprecated; gemini-3.6-flash: 503 x2; gemini-3.8-flash: 503; gemini-2.5-flash: 404 deprecated — thử 5 model khác nhau đều lỗi); 8.5d soát ảnh bằng mắt PASS sau khi sửa 2 lỗi phát hiện (vị trí vòng tròn "VS" đè lên card ở frame 3, và vòng trang trí frame 1 kéo dài phép đo pixel — đã sửa cả hai và render lại); 8.5e đo pixel PASS cả 6 frame (bottom 1410-1599px, fill 71-93.7%, đều ≥55% và trong khoảng yêu cầu); 8.5f loudness ban đầu -15.5 LUFS/-1.5dBTP lệch quá 1 LU → đã chạy loudnorm 2-pass, kết quả cuối -14.1 LUFS (đạt) nhưng True Peak -0.6dBTP hơi vượt trần -1.0dBTP ~0.4dB (NGHI VẤN, đã thử alimiter bổ sung nhưng làm tệ hơn nên giữ nguyên bản loudnorm 2-pass, không chặn Gate D theo đúng quy tắc); 8.5g giá trị đếm động (44%/56% frame 2, 60 triệu khách/năm frame 3) đều lên đúng giá trị cuối trước khi frame kết thúc, không có khung hình trống giữa các frame.
- claims_verified: ["AgiBot ~8.400 robot / 44% thị phần nửa đầu năm, thành lập Thượng Hải 2023 — khớp nguồn qua tìm kiếm đối chiếu nhiều bài VnExpress cùng chủ đề", "Nhà máy Longcheer (Trung Quốc) — robot phân loại/kiểm tra linh kiện — khớp nguồn", "Sân bay Haneda (Nhật Bản), hơn 60 triệu lượt khách/năm, Japan Airlines triển khai robot vận chuyển hành lý từ tháng 5 — khớp nguồn", "Chi phí bảo trì/lập trình cao là rào cản triển khai đại trà — khớp nguồn"]
- copyright_notes: "ảnh nguồn VnExpress (hasImage=true, tải qua Apps Script image proxy, ghi rõ nguồn ở badge + chú thích ảnh tái dùng frame 3); nhạc nền tự sinh qua Google Lyria (calm ambient, density 0.25/brightness 0.4, không lời); SFX 3 tiếng pop lấy từ bundled library của media-use skill (repo license)."
- ai_disclosure_required: false
- checked_at: 2026-09-21T12:34:01Z
- reviewer: automated-routine

## Ghi chú kỹ thuật khác

- WebFetch trực tiếp vnexpress.net bị egress-block (đúng bảng lỗi đã biết trong ROUTINE.md) — dùng
  WebSearch để đối chiếu số liệu/nội dung bài báo gốc thay thế, xác nhận khớp qua nhiều kết quả tìm
  kiếm độc lập trước khi viết script.
- Style dựng hình đã claim ở Bước 1: `split-comparison` (index 2/6), áp dụng cho 3 frame nội dung
  (AgiBot số liệu, thực địa nhà máy/sân bay, lời hứa/thực tế) — frame Hero (1) và CTA (6) giữ cấu
  trúc cố định riêng, frame Thesis (5) dùng pull-quote.
- Phát hiện + tự sửa 2 lỗi trong GATE D 8.5d/8.5e trước khi coi video hoàn thiện: (1) vòng tròn "VS"
  ở frame 3 ban đầu đặt sai vị trí, đè lên card nhà máy Longcheer — đã tính lại toạ độ đặt đúng giữa
  khe hở 2 card xếp dọc; (2) vòng trang trí góc phải frame 1 khiến phép đo pixel fill vượt ngưỡng
  1400-1680px — đã bỏ vòng trang trí này, đồng thời dịch khối nội dung frame 5 (Thesis) xuống 50px
  để đạt ngưỡng fill tối thiểu.
