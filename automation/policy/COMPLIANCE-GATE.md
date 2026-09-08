# COMPLIANCE-GATE — Trạm AI

Checklist rút gọn, dùng được trực tiếp trong 1 routine tự động (không cần đọc lại 2 file gốc mỗi
lần) — rút từ `Meta_Multi_Platform_Content_Policy_Engine_V3_Vietnam.md` (Facebook/Instagram/
Threads) và `Google_YouTube_AI_Content_Policy_Engine_V1_Vietnam.md` (YouTube/Google Ads), cùng thư
mục này. Khi 2 file gốc được cập nhật, cập nhật lại checklist này cho khớp.

Áp dụng theo đúng 3 cổng, không gộp — mỗi cổng có mục đích khác nhau, bỏ cổng nào là mất đúng lớp
bảo vệ đó.

---

## GATE A — Lúc chọn tin (trước khi viết script)

Kiểm tra NGAY sau khi lấy danh sách tin, TRƯỚC khi chọn 1 tin để viết script — tránh phí công viết
script cho 1 tin sẽ bị loại.

**Loại bỏ khỏi danh sách ứng viên** (không chọn làm chủ đề chính của video):
- Tin hình sự / cáo buộc tội phạm cụ thể nhắm vào 1 cá nhân/doanh nghiệp.
- Tin chính trị / bầu cử.
- Tin y tế dạng "thuốc thần", cam kết chữa khỏi bệnh.
- Tin tài chính dạng cam kết lợi nhuận/lãi suất chắc chắn.
- Tin mà nội dung minh hoạ chính là deepfake/dựng lại cảnh người thật nói điều họ chưa từng nói.

**BLACK — không bao giờ dựng, bất kể tin gì**: lừa đảo/phishing, khai thác/tình dục hoá trẻ em,
bạo lực nghiêm trọng, doxxing gây hại.

Tin về AI-marketing/công nghệ nói chung (đúng niche của kênh) gần như luôn qua được Gate A. Nếu
sau khi lọc không còn tin nào hợp lệ, quay lại chờ lần fetch tiếp theo (không cố ép chọn 1 tin
thuộc nhóm bị loại).

---

## GATE B — Trước khi render (sau khi viết xong script)

Tự chấm điểm SCRIPT.md vừa viết, trả lời JSON:

```json
{
  "decision": "APPROVE|REWRITE|HUMAN_REVIEW|REJECT",
  "risk_level": "GREEN|YELLOW|ORANGE|RED|BLACK",
  "violations": [],
  "claims_to_verify": [],
  "ai_disclosure_required": false,
  "reason": ""
}
```

Kiểm tra:
- **Fact/nguồn**: mọi số liệu/sự kiện phải khớp bài báo gốc đã đọc (WebFetch) — không suy diễn
  thêm. Nếu bài không nêu rõ ngày/nguồn xác nhận, dùng framing "theo báo cáo/theo phát biểu..."
  thay vì khẳng định chắc chắn.
- **Tuyên bố tuyệt đối → có điều kiện**: "AI chắc chắn giúp tăng doanh thu X%" → viết lại "AI có
  thể hỗ trợ...; hiệu quả thực tế phụ thuộc vào cách triển khai".
- **Không fabricate**: không bịa quote, số liệu, case study, testimonial, ảnh chụp màn hình.
- **AI disclosure**: giọng ElevenLabs là AI narrator generic — theo YT-AI-001 KHÔNG bắt buộc
  disclosure. Ảnh AI-generated (chỉ dùng khi bài không có ảnh thật) là conceptual illustration,
  KHÔNG được tái dựng cảnh/người thật như thể ảnh chụp thật — nếu phát hiện dựng cảnh/người thật
  giống ảnh chụp, phải REJECT/REWRITE lại cách trình bày ảnh.
- **Bản quyền**: ảnh dùng đúng ảnh thật của bài báo (ghi nguồn) hoặc minh hoạ tự vẽ; nhạc nền qua
  `/media-use` (đã có quyền). Không hướng dẫn xoá watermark, không né Content ID.
- **Chủ đề nhạy cảm mặc định YELLOW/ORANGE**: tài chính, sức khỏe, chính trị/pháp lý, cáo buộc cá
  nhân/doanh nghiệp cụ thể. Tin AI-marketing/công nghệ thông thường là GREEN.
- **Chống mass-produced**: mỗi video phải có ít nhất 1 góc nhìn/tổng hợp riêng (không chỉ dịch
  thẳng bài báo) — đúng yêu cầu gốc của cấu trúc Hook→Context→Nội dung→So what→CTA.

**Quyết định**:
- GREEN/YELLOW + APPROVE/REWRITE → tự sửa đúng phần bị gắn cờ, tiếp tục.
- ORANGE/RED/BLACK hoặc HUMAN_REVIEW/REJECT → DỪNG LẠI, không lồng tiếng, không dựng.

---

## GATE C — Sau khi có thumbnail (trước khi đưa vào hàng đợi đăng bài)

Sau khi render xong + trích thumbnail, ghi lại quyết định cuối cùng thành 1 file audit:

`videos/<slug>/COMPLIANCE.md`:
```markdown
# Compliance — <slug>
- decision: <APPROVE|REWRITE|HUMAN_REVIEW|REJECT>
- risk_level: <GREEN|YELLOW|ORANGE|RED|BLACK>
- gate_a_passed: <true|false>
- gate_b_result: <JSON từ Gate B>
- checked_at: <ISO timestamp>
- reviewer: automated-routine
```

Commit file này vào repo (`git add videos/<slug>/COMPLIANCE.md && git commit -m "compliance: <slug>" && git push`).

**RED/ORANGE/BLACK ở Gate B → KHÔNG chạy tới Gate C, không render, không đăng.** Gate C chỉ ghi
log cho các video đã qua được Gate B (GREEN/YELLOW) — đây là nhật ký audit lâu dài, không phải
vòng kiểm duyệt thứ 3.
