# Day 16 Submission

## Members
- A202600404 - Nguyễn Thành Đại Khánh

---

## 1. Idea reframed

Original idea:
> Xây dựng công cụ AI giúp sinh viên và fresher review CV và luyện phỏng vấn trước khi ứng tuyển việc làm.

Reframed as a product opportunity:
> Xây dựng công cụ AI giúp sinh viên IT năm cuối tối ưu CV và luyện phỏng vấn cho các vai trò Backend/Frontend. Các công cụ hiện tại chủ yếu cung cấp template hoặc feedback chung, chưa gắn trực tiếp CV với JD cụ thể mà người dùng đang apply. Sản phẩm này tập trung extract keyword từ JD, match với CV để sinh ra Top 3 gaps (lỗ hổng) ưu tiên và generate câu hỏi phỏng vấn kỹ thuật sát với chính lỗ hổng đó.

---

## 2. Customer / Segment Card

- **Segment name:** Sinh viên IT năm cuối hoặc Fresher (0-1 năm kinh nghiệm) tại HN & HCM, đang apply vị trí Backend/Frontend Intern/Fresher.
- **Operational context:** Đang rải CV trên ITviec, TopCV. CV thường nặng về liệt kê project môn học (clone app) nhưng thiếu các business metrics thực tế hoặc tech stack không map đúng 100% với yêu cầu của JD.
- **Recurring workflow:** 1. Tìm JD trên ITviec. 2. Gửi 1 bản CV duy nhất cho 10 công ty khác nhau. 3. Rớt từ vòng hồ sơ. 4. Tham gia phỏng vấn bị hỏi vặn về tech stack hoặc architecture chưa nắm vững.
- **Pain moment:** Nộp 15 công ty ITviec mà không nhận được cuộc gọi nào, không biết do CV yếu hay do cạnh tranh. Khi phỏng vấn bị hỏi trúng lỗ hổng tech stack ghi trong CV mà không biết đường đỡ.
- **Why now:** Thị trường tuyển dụng IT đang bão hòa ở level Junior/Fresher, tỷ lệ chọi cực cao. Fresher cần mọi lợi thế nhỏ nhất để nổi bật.
- **Access path:** Cộng đồng sinh viên IT (J2Team, VNOI, các group ITviec), GitHub repos, KOL IT trên TikTok/YouTube.

One-sentence description:
> Sinh viên IT năm cuối đang apply Backend/Frontend roles — họ apply liên tục nhưng rớt CV nhiều, không biết chính xác CV mình đang miss tech keywords hoặc metrics nào so với JD mục tiêu.

---

## 3. Need Map (2–3 needs)

*(Note: We prioritize Need #1 over Need #2 because nó xảy ra trước trong user journey (phải qua vòng CV mới tới phỏng vấn), frequency cao hơn (apply 10 JDs cần 10 lần check), và measurable hơn bằng tỷ lệ CV -> Interview).*

### Need #1 (priority) — Tìm ra gap giữa CV và JD cụ thể
- **Statement (JTBD):** Khi tôi nộp CV cho vị trí Backend Intern, tôi muốn biết chính xác CV của mình đang thiếu tech stack/metric gì so với JD, để tôi có thể sửa và bổ sung đúng chỗ trước khi bấm nút apply.
- **Current workaround:** Tự đọc JD và sửa CV bằng tay, nhờ Senior Dev/Mentor review giùm, hoặc quăng vào ChatGPT tự prompt.
- **Pain signal:** Sửa CV 5 lần, apply 15 job vẫn không ai gọi.
- **Evidence / proxy evidence:** (Fake interview data) Phỏng vấn 5 sinh viên IT: 4/5 cho biết "Em nộp 10 chỗ không ai gọi, em không biết CV sai ở đâu". Quote thực tế: *"Em nộp 15 công ty mà không ai gọi, em không biết sửa gì thêm nữa vì project trên trường em để hết vào rồi."*
- **Why underserved:** Senior Dev không rảnh để review chi tiết 1-1 liên tục. ChatGPT cần prompt phức tạp mới ra được output chuẩn xác. TopCV chỉ check keyword sơ sài, không chỉ ra cách rewrite project metrics.

### Need #2 — Luyện phỏng vấn technical đúng trọng tâm
- **Statement (JTBD):** Khi tôi qua vòng CV, tôi muốn được mô phỏng phỏng vấn tập trung vào chính những project và tech stack tôi ghi trong CV, để tôi không bị "khớp" trước những câu vặn vẹo của Technical Manager.
- **Current workaround:** Đọc list "Top 100 Node.js Interview Questions" trên mạng, tự lẩm nhẩm trả lời.
- **Pain signal:** Bị hỏi sâu vào cách tối ưu database trong project cá nhân nhưng trả lời lan man, mất tự tin.
- **Evidence / proxy evidence:** (Quote) *"Em pass vòng CV rồi nhưng vào phỏng vấn bị vặn về database optimization mà em chỉ biết cơ bản, em bị khớp và tạch luôn."*
- **Why underserved:** List câu hỏi trên mạng là generic. Không có công cụ nào generate câu hỏi dựa trên TỪNG DÒNG của CV user kết hợp với yêu cầu thực tế của JD đó.

---

## 4. Strategy Statement

For **sinh viên IT năm cuối applying Backend/Frontend roles**,
who struggle with **tỷ lệ rớt CV cao vì không biết CV miss tech keyword/metrics nào so với JD**,
our product helps them **tăng tỷ lệ được gọi phỏng vấn (interview callbacks)**
through **hệ thống tự động extract keyword từ JD, so khớp với CV, identify missing signals và xuất ra Top 3 gaps kèm gợi ý rewrite (before/after)**,
unlike **TopCV templates hay dùng ChatGPT chay**,
because we can leverage **luồng UX tối ưu 1-click và system logic (prompt chains) được thiết kế riêng cho việc đánh giá Software Engineering CVs.**

---

## 5. Moat Hypothesis

**Moat mechanism:** Speed of iteration + Niche Focus (Software Engineering) + Workflow UX

If we deploy 50 times in the IT Fresher vertical, the following improve:
1. **Scoring Logic & Dataset:** Xây dựng được dataset gồm [CV + JD + Outcome (Call / No call)], từ đó train scoring logic riêng biệt dự đoán chính xác hơn tỷ lệ pass CV ngành IT.
2. **Technical Interview DB:** Bộ câu hỏi technical interview generate ra ngày càng sắc bén và giống HR/Tech Lead thật hơn nhờ thu thập feedback "câu này có sát thực tế không" từ user.
3. **UX Workflow:** Tối ưu hóa luồng trải nghiệm cực mượt (Paste JD link + Upload CV -> ra % match) mà các tool general (ChatGPT) đòi hỏi user phải tự làm prompt engineering cực khổ.

Why competitors cannot easily replicate this:
> Các nền tảng lớn như TopCV quá bự và general để focus sâu vào một niche (IT) với luồng phân tích sâu như vậy. Họ thiên về distribution. Ngược lại, chúng ta không compete bằng Foundation Model (LLM ai cũng tiếp cận được), mà compete bằng **UX, workflow integration và dữ liệu đặc thù (CV-JD-Outcome) của ngành IT.**

---

## 6. Initial TAM / SAM / SOM view

| Layer | Estimate | Key assumptions | Confidence |
|---|---|---|---|
| TAM | ~$1.5M / year | ~50K SV IT tốt nghiệp/năm. ARPU ~1M/năm (100k/tháng x 10 tháng). Giả định ai cũng có nhu cầu tối ưu CV. | low |
| SAM | ~$75K - $375K / year | Tập trung SV IT tại HN & HCM. ~15K SV active tìm việc. Tỷ lệ sẵn sàng trả phí (Conversion rate) thực tế: 1% - 5%. ARPU: 500K/chu kỳ. | med |
| SOM | ~$5K - $15K / year | 12-24mo target: Đạt 500 - 1000 paying users. ARPU test ở mức 49K - 99K/chu kỳ tìm việc. | med |

*Bảng kịch bản doanh thu SAM (ARPU = 100K/tháng x 3 tháng = 300K):*
| Case | Paying rate (Conversion) | Revenue (từ 15,000 active users) |
|------|-------------------------|--------------------------------|
| Low | 1% (150 users) | 45.000.000 VNĐ |
| Mid | 3% (450 users) | 135.000.000 VNĐ |
| High | 5% (750 users) | 225.000.000 VNĐ |

**Top 3 unknowns requiring further research:**
1. **Willingness to Pay (WTP):** Sinh viên IT có thực sự trả 49K-99K cho một bản report CV không, hay họ sẽ quay về dùng ChatGPT free?
2. **Output Quality Threshold:** Output sinh ra có đủ sự "Wow" và perceived value cao hơn hẳn ChatGPT chay để giữ user (retention) sau lần dùng đầu tiên không?
3. **Unit Economics:** Chi phí chạy API (đặc biệt khi dùng LLM xịn xích chuỗi prompt dài) là bao nhiêu cho 1 session? Biên lợi nhuận có bù đắp được chi phí acquisition (CAC) không?

**Judgment:**
- [x] Worth pursuing now (cần triển khai ngay Experiment Plan để validate)
- [ ] Worth pursuing but not now 
- [ ] Not worth pursuing as currently framed

---

## 7. Competitor Analysis (Bonus Section)

| Player | Strength | Weakness |
|---|---|---|
| **TopCV / JDs platform** | Distribution cực lớn, data phong phú, user base sẵn có. | Phân tích CV còn chung chung, chưa cá nhân hóa sâu theo từng JD cụ thể, chủ yếu dựa vào template và keyword matching cơ bản. |
| **Glints / VietnamWorks** | Nắm rõ job data và xu hướng tuyển dụng. | Chưa có focus mạnh vào CV improvement, chủ yếu làm platform kết nối. |
| **ChatGPT / Claude (Free)** | Đa dụng, miễn phí, có khả năng suy luận tốt. | Đòi hỏi user phải có kỹ năng Prompt Engineering tốt. Context window dễ bị nhiễu. UX không thiết kế riêng cho việc apply job (phải copy/paste nhiều lần). |

---

## 8. Positioning Note

**What we are:**
> For IT Interns applying Backend/Frontend roles, we help increase interview callbacks by automatically identifying and fixing the top 3 critical gaps between their CV and specific Job Descriptions.

**What we are not / not yet:**
> Chúng tôi không phải là một công cụ viết CV tự động 100% thay cho bạn, cũng không phải là nền tảng cam kết đậu phỏng vấn.

---

## 9. Self-assessment before Day 17

Trong 6 mắt xích (Idea → Customer → Need → Strategy → Moat → Market Size), mắt xích nào team đang yếu nhất?

> **Moat & Viability (Willingness to Pay):** Lợi thế cạnh tranh (Moat) hiện tại vẫn dựa nhiều vào giả định rằng chúng tôi có thể build một UX tốt hơn và tạo ra dataset độc quyền. Tuy nhiên, rủi ro lớn nhất (Biggest Risk) là **User không chịu trả tiền** (vì quen xài tool AI chùa) và **Output không đủ xuất sắc** để giữ chân họ sau lần đầu tiên.

Open questions chúng tôi muốn khám phá thêm ở Day 17:
1. Chi phí API thực tế cho 1 luồng xử lý CV + JD hoàn chỉnh là bao nhiêu VND?
2. Bỏ ra 49K/lượt, sinh viên kỳ vọng report phải trông như thế nào mới đáng tiền?
3. Làm sao để thu thập được data "Passed/Failed vòng CV" từ user để feed ngược lại model học?

---

## 10. Experiment Plan (Next 2-week validation plan)

Để chứng minh tính khả thi, chúng tôi sẽ thực thi 3 experiments sau:

- **Experiment 1 (Fake Door / Landing Page):** Tạo 1 landing page đơn giản giới thiệu tính năng "Chẩn đoán bệnh CV IT theo chuẩn JD", đặt nút "Scan CV ngay (49.000đ)". Chạy 500k tiền ads FB/TikTok vào tệp sinh viên IT để đo lường *Conversion Rate* click vào nút thanh toán.
- **Experiment 2 (Wizard of Oz CV Review):** Cho user upload CV + JD miễn phí. Thay vì dùng AI, team sẽ chạy bằng cơm (dùng ChatGPT + sửa tay) để trả về 1 bản report cực xịn (đạt chuẩn Top 3 gaps, Rewrite suggestion before/after). Đưa cho user xem và hỏi: *"Bạn có sẵn sàng trả 49k cho 1 bản report như thế này ở lần sau không?"* để test *Perceived Value*.
- **Experiment 3 (Mock Interview Prototype):** Setup 1 luồng chatbot đơn giản trên Coze/Dify, nạp sẵn context của 1 CV và 1 JD thực tế, cho user test thử chat 5 lượt để đánh giá xem câu hỏi phỏng vấn có thật sự "sát" và tạo ra value thực tiễn không.
