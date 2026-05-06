# Day 17 Submission

**Student:** Nguyễn Thành Đại Khánh — A202600404
**Date:** 24/04/2026
**Product idea:** Công cụ AI tự động phát hiện Top 3 gaps giữa CV và JD Backend/Frontend của sinh viên IT năm cuối, kèm gợi ý rewrite và câu hỏi phỏng vấn kỹ thuật sát thực tế.

---

## 1. MVP Boundary Sheet

**Riskiest Assumption:**
> Sinh viên IT sẵn sàng trả 49K–99K/lượt cho một bản report CV gap analysis — thay vì tự dùng ChatGPT miễn phí — vì output đủ "Wow" và tiết kiệm thời gian hơn hẳn.

**In-Scope** (tối đa 3):
- [ ] Upload CV (PDF) + Paste JD → tự động extract keyword và match — test giả định: parser có thể đọc CV sinh viên đủ chính xác để phân tích
- [ ] Xuất ra Top 3 skill/keyword gaps ưu tiên kèm gợi ý rewrite before/after — test giả định: LLM có thể ưu tiên đúng gaps quan trọng nhất theo chuẩn HR/Tech Lead, và gợi ý rewrite đủ cụ thể để user dùng ngay

**Out-of-Scope:**
- Generate câu hỏi phỏng vấn kỹ thuật — lý do bỏ: phục vụ user state khác (đã pass CV screening), value proposition riêng biệt, giữ trong MVP khiến data bị nhiễu — không biết user trả tiền vì Gap Analysis hay vì interview questions
- Auto-apply hoặc kết nối thẳng với ITviec/TopCV — lý do bỏ: không cần thiết để test core value, tăng complexity lớn
- Mock interview realtime (voice/video) — lý do bỏ: tốn infrastructure, chưa validate nhu cầu ở giai đoạn này
- Dashboard theo dõi nhiều JD / account history — lý do bỏ: distraction khỏi loop chính Upload → Gap → Fix

**Non-Goals:**
- Cam kết hoặc đảm bảo user sẽ pass vòng CV / đậu phỏng vấn
- Mở rộng sang ngành nghề khác ngoài IT Software Engineering (Marketing, Design, v.v.)

---

## 2. PRD Skeleton

### Problem Statement
> Sinh viên IT năm cuối tại HN & HCM đang nộp CV hàng loạt lên ITviec mà không biết CV mình thiếu tech keyword/metric gì so với JD cụ thể — điều này khiến họ rớt ngay vòng hồ sơ, mất cơ hội phỏng vấn và kéo dài thời gian tìm việc lên nhiều tháng.

### Target User
> Sinh viên IT năm cuối hoặc Fresher 0–1 năm kinh nghiệm tại HN & HCM, đang apply vị trí Backend/Frontend Intern/Fresher trên ITviec. Thường có CV nặng về liệt kê project môn học (clone app), thiếu business metrics thực tế, và gửi 1 bản CV duy nhất cho 10 công ty khác nhau mà không customize theo JD.

### User Stories

**Story 1:**
> As a **final-year IT student applying for a Backend Intern role**, I want to **automatically see the top 3 skill and keyword gaps between my uploaded CV and a specific JD, along with rewrite suggestions for each gap** so that **I increase my chance of getting an interview callback before I hit the apply button**.

**Story 2:**
> As a **fresher who just passed the CV screening round**, I want to **practice interview questions generated specifically from the gaps and tech stack listed in my own CV against the target JD** so that **I can answer technical deep-dives from the hiring manager without blanking out on my own project details**.

### AI-Specific

**Model Selection:**
- Model: Claude Sonnet (claude-sonnet-4-20250514 via Anthropic API) — hoặc GPT-4o tùy cost per session
- Lý do chọn: Cần khả năng reasoning sâu để phân tích văn bản CV + JD dài, extract implicit requirements, và generate rewrite suggestions mạch lạc. Sonnet cân bằng tốt giữa chất lượng output và chi phí per-session.
- Trade-offs chấp nhận: Latency ~3–8s/request — acceptable vì user chờ report, không cần realtime
- Trade-offs không chấp nhận: Hallucinate tech keywords không có trong JD vào report → làm mất trust ngay lần đầu

**Data Requirements:**
- Nguồn: CV của user (PDF upload) + JD text (paste hoặc URL crawl từ ITviec/TopCV)
- Owner: User sở hữu CV. JD là public data của employer. Session data lưu tạm, không dùng để train lại model.
- Update frequency: Prompt templates review mỗi ~4 tuần theo phản hồi user và thay đổi xu hướng tuyển dụng IT

**Fallback UX:**
- Chiến lược: Human-in-the-loop + Graceful Handover (ưu tiên)
- Trigger: CV extract được <5 bullet points / JD <80 từ / confidence score thấp từ parser
- Hành động: Hiển thị "CV của bạn có vẻ ngắn hoặc JD chưa đủ chi tiết — kết quả có thể chưa đầy đủ. Bạn có thể thêm mô tả project để cải thiện độ chính xác."
- User options: User có thể edit từng gap suggestion trực tiếp trước khi download report (inline text editor trên mỗi gap card)

**Fallback UX — Missing Edge Cases (bổ sung sau stress test):**

- **Edge case 1 — CV tiếng Việt, JD tiếng Anh (cross-language mismatch):** LLM match kém hơn hẳn khi 2 văn bản khác ngôn ngữ. Fallback: hiển thị danh sách keyword đã extract từ CV cho user xem và confirm trước khi chạy gap analysis — tránh false gaps do language normalization fail.
- **Edge case 2 — JD do HR non-tech viết, toàn soft skills:** JD 200 từ nhưng không có technical signal, vượt qua trigger <80 từ nhưng output sẽ generic/hallucinated. Fallback cần thêm: detect tỷ lệ technical keyword trong JD — nếu < 20% là technical terms, cảnh báo "JD này có vẻ thiếu yêu cầu kỹ thuật cụ thể — kết quả phân tích có thể không sát thực tế."
- **Edge case 3 — CV quá mỏng (sinh viên năm 2–3, chưa có project thực):** Parser extract đủ text nhưng nội dung không đủ để tạo actionable gaps. Output đúng về mặt kỹ thuật nhưng depressing và unusable. Fallback: nếu CV < 150 từ technical content, hiển thị "CV của bạn đang còn khá mỏng — hãy bổ sung mô tả chi tiết cho ít nhất 1–2 project trước khi phân tích để kết quả có ý nghĩa hơn."
- **Edge case 4 — CV dạng table/column layout hoặc scan bằng điện thoại:** PyMuPDF / pdf.js extract ra garbage text hoặc mất hoàn toàn content trong table cells. Fallback: sau khi extract, hiển thị preview text đã extract cho user review — nếu user thấy sai lệch có thể paste plain text thủ công thay vì upload PDF.

### Success Metrics
- Primary metric: % users nhận được ít nhất 1 interview callback sau khi dùng tool (CV → Interview Rate), đo qua self-report sau 2 tuần
- Ngưỡng thành công: ≥ 30% user báo có callback (baseline ước tính hiện tại ~15% khi dùng ChatGPT tự prompt); Retention D30 > 25%; ≥ 70% user nói "đáng 49K" trong exit survey
- Timeframe đo lường: Tuần 6–8 sau launch, cần tối thiểu 100 sessions có trả phí

### Dependencies & Constraints
- API/Service: Anthropic API (Claude Sonnet), PDF parser (PyMuPDF / pdf.js), URL scraper cho ITviec JD, Payment gateway (MoMo hoặc Stripe cho 49K gate)
- Timeline: MVP Wizard-of-Oz: 1 tuần / Auto-MVP end-to-end: 3–4 tuần
- Budget: API cost ~500–1.500 VNĐ/session → margin ~97% trước CAC. Tổng budget test: < 2 triệu VNĐ (bao gồm ads + API)
- Legal/Compliance: Không lưu CV sau session (chứa thông tin cá nhân). Ghi rõ ToS: output là gợi ý, không cam kết kết quả tuyển dụng.

---

## 3. Hypothesis Table

### Hypothesis 1 (cho tính năng In-Scope #1 — CV + JD Keyword Extraction)
> "Chúng tôi tin rằng tính năng tự động extract keyword từ CV và JD sẽ giúp sinh viên IT năm cuối đang apply Backend/Frontend roles đạt được kết quả: hiểu rõ CV mình đang thiếu gì trong vòng dưới 60 giây. Chúng tôi sẽ biết mình đúng khi thấy ≥ 70% user hoàn thành bước upload + paste JD mà không bỏ giữa chừng trong vòng 7 ngày đầu launch."

Riskiest assumption: PDF CV của sinh viên đủ chuẩn để parser extract text chính xác (không bị ảnh hóa hoặc format lạ)
Cách test cheapest: Thu thập 20 CV thật từ bạn bè, chạy tay qua PyMuPDF, đếm tỷ lệ extract thành công trước khi build UI

⚠️ **Stress test note — Hypothesis yếu nhất:** Metric "70% completion rate của upload form" đo onboarding UX, không đo accuracy của extraction hay quality của output. Hypothesis có thể "pass" về completion rate trong khi extraction bị sai keyword với 50% CV do table/column layout. **Fix:** bổ sung metric thứ 2 — ≥ 80% keyword được extract có trong CV thật (đo bằng manual spot-check 20 sessions đầu). Invalidate nhanh nhất: lấy 10 CV Canva template + 3 CV scan điện thoại, chạy PyMuPDF — nhiều khả năng 3–4 cái ra garbage text trong < 2 giờ, không cần build gì.

### Hypothesis 2 (cho tính năng In-Scope #2 — Top 3 Gap Identification + Rewrite)
> "Chúng tôi tin rằng tính năng xuất ra Top 3 gaps ưu tiên kèm gợi ý rewrite before/after sẽ giúp sinh viên IT năm cuối đạt được kết quả: biết chính xác cần sửa gì và sửa được ngay trong buổi đó mà không cần tự nghĩ từ đầu. Chúng tôi sẽ biết mình đúng khi thấy ≥ 65% user đánh giá Top 3 gaps là 'đúng và hữu ích' VÀ ≥ 50% user copy ít nhất 1 suggestion vào CV thật trong vòng 48 giờ sau session."

Riskiest assumption: LLM có thể ưu tiên đúng 3 gaps quan trọng nhất theo logic HR/Tech Lead thực tế, và gợi ý rewrite đủ tự nhiên — không bị detect là AI-written
Cách test cheapest: Wizard-of-Oz — team tự chọn Top 3 gaps + viết before/after bằng tay cho 10 cặp CV-JD, nhờ 2 Senior Dev đánh giá trước khi build bất kỳ thứ gì

---

## 4. PMF Scorecard

**Aha Moment:**
> User scroll hết 3 gap cards VÀ click mở ít nhất 1 rewrite suggestion trong cùng 1 session — tức là họ từ passive reader chuyển sang active engager, chứng tỏ đã "thấy" gaps là đúng và muốn fix ngay.
>
> ⚠️ **Stress test note:** `scroll_past_gap3 + rewrite_expand_click` đo engagement với UI, không đo perceived value. User có thể đạt cả 2 event trong 30 giây rồi đóng tab vì thấy output generic. **Leading indicator thực sự:** user quay lại trong vòng 48 giờ với một CV đã được sửa (upload CV mới sau session đầu tiên) — đây mới là signal output đủ chất lượng để tạo ra hành động thực.

**Actionable Metric:**
> **Primary (đo được ngay):** % sessions kích hoạt cả 2 event: `scroll_past_gap3` + `rewrite_expand_click` trong cùng session. Ngưỡng: ≥ 60% sessions trong 30 ngày đầu.
>
> **True leading indicator (ưu tiên hơn):** % users upload CV lần 2 trong vòng 48 giờ sau session đầu tiên. Ngưỡng: ≥ 25%. Đây mới phản ánh output đủ tốt để drive hành động sửa CV thực sự.

**PMF Method:**
> Sean Ellis Test — ngưỡng: > 40% trả lời "Very disappointed" nếu sản phẩm biến mất
> Retention Curve — ngưỡng: D30 > 25% (quay lại scan CV mới trong 30 ngày)
> Ngưỡng thành công tổng hợp: đạt cả 2 phương pháp trên vào tuần 6–8 sau launch

**Vanity Metrics tôi sẽ không dùng:**
- Tổng số lượt truy cập website / page views
- Số lượng sign-ups (chưa upload CV)
- Số lượt download report (chưa biết có dùng không)
- Rating sao trung bình (dễ bị bias bởi bạn bè)

---

## 5. AI Critique Log

**Điểm AI chỉ ra (vòng trước):**

1. [Moat quá phụ thuộc vào UX, thiếu technical differentiation] — Action: **Accept** — Lý do: Đây là rủi ro thực. Tuy nhiên ở giai đoạn MVP, moat từ UX + niche focus là khởi điểm hợp lý. Sẽ bổ sung scoring logic riêng cho IT domain sau khi có đủ data CV-JD-Outcome từ 50+ sessions thực.

2. [Primary metric (interview callback) khó đo vì phụ thuộc self-report của user] — Action: **Accept + Partial fix** — Lý do: Đúng. Đã bổ sung proxy metric thay thế: repeat usage trong 30 ngày (D30 retention) và Aha Moment tracking event trong app để không bị hoàn toàn phụ thuộc vào self-report.

3. [TAM/SAM trong Day 16 tính ARPU theo tháng nhưng sản phẩm là per-session, dễ gây nhầm lẫn trong model doanh thu] — Action: **Accept** — Lý do: Đúng về mặt unit economics. Đã điều chỉnh trong PRD: ARPU = 49K–99K/session, không phải /tháng. Break-even và margin tính trên per-session rõ hơn.

**Stress test (Lead PM + Senior AI Engineer):**

4. [Scope creep — Interview question generator không nên ở In-Scope] — Action: **Accept** — Lý do: Phục vụ user state khác, value proposition riêng, giữ trong MVP làm data bị nhiễu — không biết user trả tiền vì Gap Analysis hay interview questions. Đã chuyển ra Out-of-Scope.

5. [Fallback UX bỏ sót 4 edge cases: cross-language mismatch, JD non-tech HR, CV quá mỏng, CV table/column layout] — Action: **Accept** — Lý do: Đây là những failure mode xảy ra thường xuyên với đúng target user (sinh viên dùng Canva template, JD từ công ty SME). Đã bổ sung 4 fallback triggers cụ thể vào PRD.

6. [Aha Moment metric scroll+click là vanity metric đo UI engagement, không đo perceived value] — Action: **Accept** — Lý do: Đúng — user có thể đạt cả 2 event và vẫn thấy output vô dụng. Đã thêm true leading indicator: % users upload CV lần 2 trong 48 giờ, ngưỡng ≥ 25%.

7. [Hypothesis 1 đo completion rate của form, không đo extraction accuracy] — Action: **Accept + Fix** — Lý do: "70% không bỏ giữa chừng" là UX metric, không phải product hypothesis metric. Đã bổ sung metric thứ 2: ≥ 80% keyword extract đúng (manual spot-check 20 sessions đầu) và invalidation test cụ thể: 10 CV Canva + 3 CV scan điện thoại qua PyMuPDF.

**Thay đổi lớn nhất giữa Version A (Day 16) và Version B (Day 17):**
> Version A (Day 16) focus vào mô tả rộng cả 2 needs (CV review + Mock interview) với experiment plan độc lập. Version B (Day 17) thu hẹp lại: In-Scope chỉ giữ 2 tính năng cốt lõi đủ để test giả thuyết WTP (bỏ interview questions ra), bổ sung 4 fallback edge cases thực tế, và thay Aha Moment metric bằng true leading indicator (return upload trong 48h).

---

## 6. Self-assessment

Mắt xích nào trong [MVP Boundary | PRD | Hypothesis | PMF] bạn đang yếu nhất?
> **PMF:** Phương pháp đo PMF (Sean Ellis Test, Retention Curve) được chọn đúng về lý thuyết, nhưng timeline "6–8 tuần sau launch" giả định đã có đủ paying users để đo. Rủi ro thực: nếu conversion rate thấp hơn dự kiến (< 1%), sẽ không đủ sample size để kết luận bất cứ điều gì từ PMF measurement — và sẽ phải kéo dài hoặc pivot trước khi có signal rõ ràng.

Open questions bạn muốn giải đáp tiếp:
1. Làm thế nào để thiết kế Sean Ellis Survey ngắn gọn nhất mà không bị user bỏ qua (response rate cao)?
2. Nếu D30 retention thấp nhưng Sean Ellis > 40%, sản phẩm có thực sự đạt PMF không — hay đây là dấu hiệu của use case one-time (chỉ dùng khi đang tìm việc)?
