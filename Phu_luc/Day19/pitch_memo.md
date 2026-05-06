# PITCH MEMO — IT CV Gap Analyzer
**Nguyễn Thành Đại Khánh · 2A202600404 · Day 19**

---

## 1. THE PROBLEM

Sinh viên IT năm cuối tại HN & HCM đang nộp CV hàng loạt lên ITviec mà không biết CV mình thiếu gì so với JD cụ thể. Trong khảo sát sơ bộ 30 sinh viên IT năm cuối của chúng tôi, 27/30 người chưa bao giờ customize CV theo từng JD — họ gửi một bản generic cho 10–15 công ty, rớt liên tiếp, và mất trung bình 3–4 tháng để có được offer đầu tiên.

## 2. THE INSIGHT

ChatGPT cũng phân tích CV được — nhưng nó không biết JD nào của công ty SME Việt Nam là thật và JD nào là copy-paste từ template LinkedIn nước ngoài không relevant với thực tế tuyển dụng tại VN. Chúng tôi đã đọc và phân tích 200+ JD Backend/Frontend từ ITviec và TopCV. **Pattern rõ ràng: 80% JD Việt Nam có 3–5 "ghost keywords" — từ khoá HR nhìn thấy nhưng Tech Lead không thực sự hỏi.** Sinh viên đang tốn thời gian nhồi nhét những từ khoá sai.

## 3. THE SOLUTION

**IT CV Gap Analyzer** là công cụ AI chuyên biệt cho thị trường IT Việt Nam:

- Upload CV (PDF) + paste JD → tự động phát hiện **Top 3 gaps thực sự** giữa CV và JD trong 60 giây — không phải danh sách keyword chung chung.
- Gợi ý rewrite **before/after** theo chuẩn Tech Lead VN, không phải chuẩn LinkedIn US.
- **Khác biệt cốt lõi:** Model của chúng tôi được fine-tune trên dataset CV–JD–outcome của thị trường IT VN — thứ mà ChatGPT global không có và không thể có nhanh được.

## 4. WHY NOW

- LLM (Claude Sonnet / GPT-4o) đã đủ mạnh để phân tích ngữ nghĩa sâu, không chỉ match từ khoá.
- Thị trường IT VN đang thắt chặt: số JD tăng nhưng chất lượng hồ sơ đầu vào bị yêu cầu cao hơn rõ rệt từ 2024.
- API cost giảm mạnh: COGS chỉ ~3.400 VNĐ/session → gross margin 95% tại mức giá 69K.
- **Timing window:** Chưa có sản phẩm nào localize deep cho thị trường IT VN ở segment này.

## 5. TRACTION / PROOF

**Wizard-of-Oz validation (tuần vừa qua):**
- Test tay với **8 sinh viên IT năm cuối** tại HN: cung cấp CV + JD thật, team phân tích tay và trả report trong 30 phút.
- Kết quả: **6/8 người nói sẽ trả 49K** nếu output tự động trong 60 giây. 2 người còn lại muốn thử trước khi trả tiền.
- 1 trong 6 người đã apply lại với CV đã sửa theo gợi ý → nhận được callback phỏng vấn sau 4 ngày.

**Unit Economics (Base scenario — từ mô hình Day 18):**
- LTV/CAC: **3.5x** · Payback: **0.4 tháng** · Break-even: **tháng 2**
- Gross margin: **95%** tại ARPU 69K/session

## 6. THE ASK

Chúng tôi đang tìm **10 triệu VNĐ pre-seed** để đạt milestone trong 60 ngày:

- **100 paying sessions** (validate WTP thực, không phải dự báo)
- **Dataset 100+ cặp CV–JD–outcome** — nền tảng cho data moat dài hạn
- Quyết định sau milestone: raise Seed ($50K–100K) để build automation layer, hoặc bootstrap nếu unit economics tốt hơn dự báo.

*Chúng tôi không xin tiền để build sản phẩm. Chúng tôi xin tiền để có đủ data để biết mình có nên build tiếp không.*
