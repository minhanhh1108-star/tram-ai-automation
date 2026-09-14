# Compliance — meta-tai-co-cau-ai

- decision: APPROVE
- risk_level: GREEN
- gate_a_passed: true
- gate_b_result: {"decision":"APPROVE","risk_level":"GREEN","violations":[],"claims_to_verify":["số nhân sự gộp vào Applied AI dao động 6.500-7.000 tuỳ nguồn — đã hedge bằng 'gần bảy nghìn' và 'được cho là', counter trong video chỉ hiển thị '7 NGHÌN' (làm tròn), không khẳng định số chính xác"],"ai_disclosure_required":false,"reason":"Tin công nghệ/kinh doanh AI thông thường (không hình sự/chính trị/y tế/tài chính cam kết lợi nhuận). Mọi số liệu và phát biểu (Andrew Bosworth mô tả tinh thần nhân viên, so sánh Cambridge Analytica, gần 7.000 nhân sự Applied AI, giới hạn khoảng 20 direct report, hơn 130 tỷ đô la đầu tư AI 2026) khớp nhiều nguồn quốc tế (Business Insider, Wired, TechRadar, Inc., IBTimes UK) xác nhận qua WebSearch — vnexpress.net/tincongnghe.net/techradar.com đều bị chặn egress trực tiếp nên không WebFetch được, dùng WebSearch thay thế theo đúng gotcha đã ghi trong ROUTINE.md. Không bịa quote/số liệu/case study. Góc nhìn hai chiều có cơ sở thật trong chính bài báo: doanh nghiệp đổ hàng trăm tỷ đô la vào tham vọng AI trong khi nội bộ thừa nhận hỗn loạn — không phải mâu thuẫn tự suy diễn."}
- gate_d_result: PASS — 8.5a duration 56.9s (trong khoảng 45-65s, dưới trần cứng 60s); 8.5b không phát hiện khoảng lặng bất thường (>=2.5s); 8.5c Gemini (gemini-3.6-flash, model 2.0-flash đã ngừng hỗ trợ) phiên âm ngược khớp đầy đủ, đúng thứ tự cả 7 dòng SCRIPT.md, không thiếu/lặp đoạn, không có cụm bị đọc rời từng chữ cái; 8.5d soát 3 khung hình (25%/60%/90%) + khung giữa mỗi trong 6 frame bằng mắt — chữ rõ, không tràn/chồng, contrast tốt (39/39 WCAG AA qua npm run check), không mảng đen bất thường. Phát hiện + tự sửa 1 vòng: bản render đầu tiên có 4/6 frame nội dung (2,3,4,5) chỉ lấp khoảng 25-41% chiều cao khung dọc (dồn ở nửa trên, để trống lớn phía dưới) — đã tăng gap/cỡ chữ và thêm 1 chi tiết (note-pill frame 2) cho cả 4 frame trong cùng 1 lượt, render lại, xác nhận cả 6 frame đạt khoảng 52-58% chiều cao trước khi chốt.
- checked_at: 2026-09-14T14:58:33Z
- reviewer: automated-routine

## Ghi chú thêm

- Nguồn tin: VnExpress — "Meta kêu gọi nhân viên quay lại làm quản lý"
  https://vnexpress.net/meta-keu-goi-nhan-vien-quay-lai-lam-quan-ly-5119383.html
- Ảnh minh hoạ bài báo (hasImage:true) tải thành công qua proxy Apps Script, dùng làm hero frame 1
  (nửa trên ảnh thật: điện thoại hiển thị logo "Meta AI" trước backdrop logo "Meta" — đúng chủ thể
  bài báo). Tái dùng đúng 1 lần nữa ở frame 4 (card nhỏ viền cyan, chú thích "Ảnh: VnExpress") vì
  đây là frame nói về khoản đầu tư AI của chính Meta — liên quan trực tiếp nhất tới nội dung ảnh.
- BGM sinh mới qua Lyria (Google RealTime), bản gốc 50.0s (ngắn hơn yêu cầu 58.9s do timeout kết nối
  sau 67s) được loop-extend bằng ffmpeg (acrossfade 1.2s) lên 98.8s rồi cắt còn đúng 56.9s + fade-out
  3s cuối — không phải fallback catalog. Kiểm tra bằng spectrogram (showspectrumpic): chỉ thấy dải
  percussion/bass đều đặn + pad tần thấp-trung, không có cấu trúc formant/vibrato đặc trưng giọng
  hát. Dùng đúng bộ tham số đã hiệu chỉnh (--bpm 112 --brightness 0.55 --density 0.45).
- SFX cho frame CTA: catalog bundled offline chỉ có 1 file gốc duy nhất (không có HeyGen auth cho SFX
  đa dạng) — tự tạo 3 biến thể phân biệt bằng ffmpeg (pitch-shift qua asetrate): thẻ trái giữ nguyên
  cao độ gốc, thẻ phải hạ cao độ (asetrate×0.82), nút CTA nâng cao độ + tăng âm lượng
  (asetrate×1.18, volume×1.6).
- Font: Inter + Space Grotesk (biến thể variable, subset vietnamese) tải trực tiếp từ
  fonts.gstatic.com và đóng gói cục bộ vào assets/fonts/ — không dùng font hệ thống. Khai báo
  @font-face inline trong <style> của TỪNG frame (không chỉ link file dùng chung) vì `npm run check`
  không parse được @font-face khai báo qua <link rel="stylesheet"> ở sub-composition, dù runtime vẫn
  áp dụng đúng lúc render (link được hoisted) — khai báo trùng lặp để lint sạch.
- gsap tải qua npm rồi vendor cục bộ vào vendor/gsap.min.js (cdn.jsdelivr.net bị chặn egress).
- Đã tự phát hiện lỗi asset path `../../public/hero.jpg` vi phạm
  `invalid_parent_traversal_in_asset_path` ở Bước `npm run check` lần đầu — sửa toàn bộ path trong
  6 frame về dạng root-relative (`public/...`, `assets/...`) trước khi render.
- Số đếm động GSAP áp dụng cho cả frame 2 (0→7, đơn vị "NGHÌN NHÂN SỰ") và frame 4 (0→130, đơn vị
  "TỶ ĐÔ LA") — không chỉ riêng frame có số liệu nổi bật nhất, để nhất quán quy tắc "số liệu trung
  tâm phải đếm động" cho mọi frame có số liệu trung tâm.
- Video + thumbnail commit riêng ở Bước 10 (video.mp4, thumbnail.jpg) để đăng trực tiếp lên 4 kênh
  qua GitHub raw URL — không dùng Google Drive.
