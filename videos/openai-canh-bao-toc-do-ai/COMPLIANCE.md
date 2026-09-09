# Compliance — openai-canh-bao-toc-do-ai

- decision: APPROVE
- risk_level: GREEN
- gate_a_passed: true
- gate_b_result: {"decision":"APPROVE","risk_level":"GREEN","violations":[],"claims_to_verify":[],"ai_disclosure_required":false,"reason":"Tin công nghệ AI thông thường (không tài chính/y tế/chính trị/hình sự). Phát biểu của Jakub Pachocki (Giám đốc khoa học OpenAI) về việc 'chưa ai sẵn sàng cho hệ quả của AI', khả năng AI thao túng chuỗi suy luận khiến khó giám sát, và lời kêu gọi tự nguyện làm chậm + giám sát độc lập — đều khớp nhiều nguồn quốc tế (BusinessToday, Spokesman, Digit.in, Fortune, Seoul Economic Daily, CoinCentral) xác nhận qua WebSearch (WebFetch trực tiếp vnexpress.net bị chặn egress, dùng WebSearch thay thế theo đúng gotcha đã ghi trong ROUTINE.md). Không dùng tuyên bố tuyệt đối, không bịa số liệu/quote/case study. Tiêu đề hero frame trích đúng nguyên văn tiêu đề bài báo (trong ngoặc kép). Không cần AI disclosure (giọng ElevenLabs generic). Ảnh minh hoạ là ảnh thật của chính bài báo (logo GPT-6 Astra/OpenAI trên điện thoại — đúng chủ thể bài viết, không liên quan thương hiệu nhà phát triển mô hình đang chạy routine, không có xung đột lợi ích). Có góc nhìn riêng: đặt lại câu hỏi tăng tốc hay chậm lại vì an toàn — đúng cấu trúc CTA 2 thẻ đối lập, mâu thuẫn có cơ sở thật trong bài (lời kêu gọi tự nguyện chậm lại của chính người kêu gọi công nghệ)."}
- gate_d_result: PASS — thời lượng 46.83s (trong khoảng 45-65s, dưới trần cứng 60s); không phát hiện khoảng lặng ≥2.5s; Gemini (gemini-3.6-flash, model 2.0-flash đã ngừng hỗ trợ) phiên âm khớp đúng thứ tự và đủ ý cả 7 dòng SCRIPT.md, không lặp/mất đoạn, không có cụm bị đọc rời từng chữ cái; soát 3 khung hình (25%/60%/90%) không lỗi chữ tràn/chồng, không mảng đen bất thường.
- checked_at: 2026-09-09T05:27:49Z
- reviewer: automated-routine

## Ghi chú thêm

- Nguồn tin: VnExpress — "Sếp OpenAI: 'Chưa ai sẵn sàng cho hệ quả của AI'"
  https://vnexpress.net/sep-openai-chua-ai-san-sang-cho-he-qua-cua-ai-5117866.html
- Ảnh minh hoạ bài báo (hasImage:true) tải thành công qua proxy Apps Script, dùng làm hero frame 1
  (nửa trên ảnh thật: điện thoại hiển thị logo "GPT-6 Astra / OpenAI" trước màn hình laptop có hình
  minh hoạ robot AI — đúng chủ thể bài báo, không phải thương hiệu nhà phát triển mô hình đang chạy
  routine này nên không có xung đột lợi ích).
- BGM sinh mới qua Lyria (Google RealTime), bản gốc 42.0s (ngắn hơn yêu cầu do timeout kết nối) được
  loop-extend bằng ffmpeg (acrossfade 1.2s) lên 82.8s rồi cắt còn đúng 46.85s + fade-out 3s cuối —
  không phải fallback catalog. Kiểm tra bằng spectrogram (showspectrumpic): chỉ thấy dải tần
  percussion/bass đều đặn + pad tần cao, không có cấu trúc formant/vibrato đặc trưng giọng hát.
- SFX cho frame CTA: catalog bundled offline chỉ có 1 file gốc duy nhất (không có HeyGen auth cho
  SFX đa dạng) — tự tạo 3 biến thể phân biệt bằng ffmpeg (pitch-shift qua asetrate/atempo): thẻ trái
  giữ nguyên cao độ gốc, thẻ phải hạ cao độ (asetrate×0.82), nút CTA nâng cao độ + tăng âm lượng
  (asetrate×1.18, volume×1.6) để tạo cảm giác "xác nhận" rõ hơn — đúng tinh thần yêu cầu 3 SFX phân
  biệt dù nguồn catalog hạn chế.
- Font: Inter + Space Grotesk tải trực tiếp từ fonts.gstatic.com (subset vietnamese, biến thể) và
  đóng gói cục bộ vào assets/fonts/ — không dùng font hệ thống, đảm bảo dấu tiếng Việt hiển thị đúng
  trong môi trường render headless Chrome (không có font hệ thống cài sẵn).
- gsap tải qua npm rồi vendor cục bộ vào vendor/gsap.min.js (cdn.jsdelivr.net bị chặn egress, đúng
  gotcha đã ghi trong ROUTINE.md).
- Video + thumbnail sẽ được commit riêng ở Bước 10 (video.mp4, thumbnail.jpg) để đăng trực tiếp lên
  4 kênh qua GitHub raw URL — không dùng Google Drive.
