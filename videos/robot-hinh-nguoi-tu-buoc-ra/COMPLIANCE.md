# Compliance — robot-hinh-nguoi-tu-buoc-ra

- decision: APPROVE
- risk_level: GREEN
- gate_a_passed: true
- gate_b_result: {"decision":"APPROVE","risk_level":"GREEN","violations":[],"claims_to_verify":[],"ai_disclosure_required":false,"reason":"Tin công nghệ/robot thông thường (không hình sự/chính trị/y tế thuốc thần/tài chính cam kết lợi nhuận/deepfake). Sự kiện Xpeng (Trung Quốc) mở dây chuyền sản xuất hàng loạt đầu tiên trên thế giới cho robot hình người, robot Iron tự bước ra ngày 8/9, tự động hoá hơn 80% công đoạn cốt lõi, dự kiến giao hàng Trung Quốc + quốc tế từ 2027 — khớp nhiều nguồn quốc tế xác nhận qua WebSearch (CnEVPost, Electrek, BusinessToday, Stuff South Africa; WebFetch trực tiếp vnexpress.net bị chặn egress, dùng WebSearch thay thế theo đúng gotcha đã ghi trong ROUTINE.md). Không dùng tuyên bố tuyệt đối, không bịa số liệu/quote/case study. Ảnh minh hoạ là ảnh thật của chính bài báo (robot Iron trong nhà máy Xpeng), có badge nguồn. Không cần AI disclosure (giọng ElevenLabs generic). Có góc nhìn riêng: đặt lại câu hỏi 'bước tiến công nghệ hay nỗi lo việc làm' — đúng cấu trúc CTA 2 thẻ đối lập, mâu thuẫn có cơ sở thật trong các nguồn (lo ngại về tác động thị trường lao động được nêu rõ trong các bài phân tích quốc tế về sự kiện này)."}
- gate_d_result: PASS — thời lượng 58.8s (trong khoảng 45-65s, dưới trần cứng 60s); không phát hiện khoảng lặng ≥2.5s; Gemini (gemini-3.6-flash, model 2.0-flash đã ngừng hỗ trợ) phiên âm khớp đúng thứ tự và đủ ý cả 7 dòng SCRIPT.md, không lặp/mất đoạn, không có cụm bị đọc rời từng chữ cái; soát 3 khung hình (25%/60%/90%) + khung hình giữa riêng từng frame (1,2,3,4,6) + đầu/cuối frame 6 — không lỗi chữ tràn/chồng, không mảng đen bất thường, masthead không bị che.
- checked_at: 2026-09-09T12:36:10Z
- reviewer: automated-routine

## Ghi chú thêm

- Nguồn tin: VnExpress — "Robot hình người đầu tiên tự bước ra từ dây chuyền sản xuất"
  https://vnexpress.net/robot-hinh-nguoi-dau-tien-tu-buoc-ra-tu-day-chuyen-san-xuat-5118285.html
- Ảnh minh hoạ bài báo (hasImage:true) tải thành công qua proxy Apps Script, dùng làm hero frame 1
  (nửa trên ảnh thật) và tái dùng thu nhỏ (480×300, bo góc, viền cyan, chú thích "Ảnh: VnExpress")
  ở frame 2 — đúng 2 frame theo quy tắc, không quá.
- **Sự cố quota ElevenLabs giữa chừng**: lồng tiếng lần đầu (script v1, 7 dòng, tổng audio 66.88s)
  thành công qua Apps Script proxy. Tổng thời lượng vượt trần cứng 60s của kênh nên thử lồng tiếng
  lại với script rút gọn (v2) — proxy trả lỗi `quota_exceeded` (còn 255/544 credit cần cho 1 lần gọi
  lại nguyên văn cả script). Không gọi lại được. Xử lý: giữ nguyên nội dung script v1 (khớp đúng
  audio đã có, SCRIPT.md ghi rõ ghi chú kỹ thuật này), tăng tốc độ phát đều 10% toàn bộ 7 đoạn bằng
  `ffmpeg atempo=1.10` (giữ nguyên cao độ giọng đọc, không đổi nội dung) → tổng thời lượng còn
  58.78s, dưới trần cứng 60s. Gate D 8.5c (Gemini phiên âm ngược) xác nhận giọng đọc sau khi tăng
  tốc vẫn tự nhiên, đúng nội dung, không lỗi phát âm.
- BGM sinh mới qua Lyria (Google RealTime), bản gốc 50.0s (timeout kết nối ở 69s, ngắn hơn yêu cầu
  61s) được loop-extend bằng ffmpeg (acrossfade 1.2s) lên 98.8s rồi cắt còn đúng 58.8s + fade-out 3s
  cuối — không phải fallback catalog. Kiểm tra bằng spectrogram (showspectrumpic): chỉ thấy dải tần
  percussion/bass đều đặn theo nhịp + pad tần trung, không có cấu trúc formant/vibrato đặc trưng
  giọng hát.
- SFX cho frame CTA: catalog bundled offline chỉ có 1 file gốc duy nhất cho "UI pop click" (không có
  HeyGen auth cho SFX đa dạng, xác nhận lại đúng gotcha đã ghi từ lần chạy trước) — tự tạo 3 biến thể
  phân biệt bằng ffmpeg (pitch-shift qua asetrate/atempo): thẻ trái giữ nguyên cao độ gốc, thẻ phải
  hạ cao độ (asetrate×0.82), nút CTA nâng cao độ + tăng âm lượng (asetrate×1.18, volume×1.6). Đặt
  đúng thời điểm khớp với các mốc pop-in, đồng thời paced theo đúng nội dung voiceover đang đọc tới
  (4.6s/6.6s/8.6s trong frame, khi giọng đọc lần lượt nhắc tới "bước tiến đáng mừng" / "mối lo... lao
  động phổ thông" / "để lại quan điểm... bình luận") thay vì dồn hết vào đầu frame — tránh front-load.
- Font: Inter + Space Grotesk tải qua Google Fonts API dạng static per-weight (css1 legacy endpoint,
  không phải variable font css2 — vốn trả cùng 1 file cho mọi weight khi test), xác nhận phủ đủ dấu
  tiếng Việt bằng fontTools trước khi dùng (test glyph ệ, ầ, ố, ủ, ọ, ế, ữ, ộ, ỹ, ấ, Đ, đ — đều có).
  Đóng gói cục bộ vào assets/fonts/ (.ttf), @font-face khai trong từng frame theo đúng path
  root-relative `assets/fonts/...` (xác nhận qua cách build-frame.mjs/captions.mjs của chính skill
  tự phát @font-face).
- gsap tải qua npm rồi vendor cục bộ vào vendor/gsap.min.js (cdn.jsdelivr.net bị chặn egress, đúng
  gotcha đã ghi trong ROUTINE.md).
- `npx hyperframes check`: 0 lỗi. Có vài warning info-level (content_overlap giữa masthead/tiêu đề
  của 2 frame liền kề, chỉ xuất hiện đúng tại các mốc thời gian crossfade 0.5s giữa 2 frame — do
  masthead cố định vị trí xuyên suốt theo đúng thiết kế, đây là hiện tượng dự kiến khi 2 frame chồng
  hình trong lúc crossfade, không phải lỗi bố cục thật; đã xác nhận lại bằng soát khung hình 8.5d ở
  các mốc giữa từng frame — không frame nào lỗi khi đứng yên). 1 warning contrast tại đúng mốc
  crossfade 04→05 (t=42.449s, giữa lúc 2 frame đang blend) — không phải trạng thái tĩnh thật của
  frame, khung hình soát riêng ở giữa frame 4 (t=38.75s) cho contrast bình thường.
- Video + thumbnail đã commit riêng ở Bước 10 (video.mp4, thumbnail.jpg) để đăng trực tiếp lên
  4 kênh qua GitHub raw URL — không dùng Google Drive.
