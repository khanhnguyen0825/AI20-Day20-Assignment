# 🗺️ Roadmap — CV Gap Analyzer for IT Students
**Sản phẩm:** AI tự động phát hiện Top 3 gaps giữa CV và JD Backend/Frontend của sinh viên IT năm cuối

---

## 🟢 NOW — Vấn đề cần giải ngay
*(Quick Wins từ 2×2 + RICE score cao nhất)*

**Vấn đề 1: Sinh viên không biết CV mình thiếu keyword gì so với JD cụ thể**
> Parser chưa đọc được CV → không có gì để phân tích. Đây là điều kiện tiên quyết để product tồn tại.

**Vấn đề 2: Không có cách nào extract requirements từ JD một cách tự động**
> User phải tự đọc JD và tự so sánh — tốn 30–60 phút mỗi lần apply, không scalable.

**Vấn đề 3: CV sinh viên bị rớt ngay vòng hồ sơ mà không biết lý do**
> Không có feedback loop nào giữa "nộp CV" và "bị loại" — user không biết sửa gì.

---

## 🟡 NEXT — Vấn đề giải sau khi NOW ổn định
*(Strategic Bets từ 2×2)*

**Vấn đề 1: User biết gap nhưng không biết phải viết lại CV như thế nào**
> Biết thiếu "Docker" không đủ — cần gợi ý cụ thể viết lại bullet point ra sao để pass ATS và HR.

**Vấn đề 2: Output gap analysis còn generic, chưa ưu tiên đúng theo logic HR/Tech Lead thực tế**
> LLM có thể liệt kê 10 gaps nhưng không biết cái nào quan trọng nhất với hiring manager — cần ranking logic riêng cho IT domain.

**Vấn đề 3: Fallback UX chưa xử lý được các edge cases phổ biến**
> CV Canva table layout, CV tiếng Việt + JD tiếng Anh, JD do HR non-tech viết — những trường hợp này chiếm ~40% target user thực tế.

**Vấn đề 4: Chưa có signal nào cho thấy user thực sự dùng output để sửa CV**
> Cần tracking: user có quay lại upload CV mới trong 48h không? Đây là true leading indicator của product value.

---

## 🔴 LATER — Vấn đề lớn, giải khi có data thực
*(Vision lớn — có thể chưa technically khả thi)*

**Vấn đề 1: Sinh viên pass CV nhưng blank out khi phỏng vấn kỹ thuật về chính project của mình**
> Interview Question Generator cần user state khác (đã có callback) — validate riêng sau khi Gap Analysis đạt PMF. *(Có thể bỏ)*

**Vấn đề 2: Mỗi lần apply JD mới phải làm lại từ đầu — không có memory giữa các sessions**
> Dashboard lịch sử nhiều JD, track progress theo thời gian — chỉ có giá trị khi user đã tin tưởng core loop. *(Có thể bỏ)*
