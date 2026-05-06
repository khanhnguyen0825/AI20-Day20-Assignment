# 📊 Bảng RICE — 5 Tính năng Cốt lõi

| # | Tính năng | Reach (user/quý) | Impact | Confidence | Effort (person-month) | RICE Score |
|---|---|---|---|---|---|---|
| 1 | **PDF CV Upload + Keyword Extraction** — parser tự động đọc CV, extract tech keywords & bullet points | 800 | 2 | 80% | 0.5 | (800 × 2 × 0.8) / 0.5 = **2.560** |
| 2 | **JD Paste / URL Scrape + Keyword Extraction** — user paste hoặc link JD từ ITviec, hệ thống extract requirements | 800 | 2 | 80% | 0.5 | (800 × 2 × 0.8) / 0.5 = **2.560** |
| 3 | **Top 3 Gap Analysis** — LLM ưu tiên 3 skill/keyword gaps quan trọng nhất theo logic HR/Tech Lead | 700 | 3 | 80% | 1 | (700 × 3 × 0.8) / 1 = **1.680** |
| 4 | **Before/After Rewrite Suggestions** — mỗi gap đi kèm gợi ý rewrite inline, user có thể edit trực tiếp | 600 | 2 | 80% | 1 | (600 × 2 × 0.8) / 1 = **960** |
| 5 | **Interview Question Generator** — tạo câu hỏi kỹ thuật theo gaps giữa CV và JD | 400 | 1 | 50% | 2 | (400 × 1 × 0.5) / 2 = **100** |

---

# 🟦 2×2 Value–Effort Matrix

```text
                    LOW EFFORT          HIGH EFFORT
                ┌───────────────────┬────────────────────┐
   HIGH VALUE   │  QUICK WIN        │  STRATEGIC BET     │
                │  CV/JD Extraction │  Gap Analysis      │
                │                   │  Before/After Rw.  │
                ├───────────────────┼────────────────────┤
   LOW VALUE    │   (không có)      │  NON-STARTER       │ 
                │                   │  Interview Q. Gen  │
                └───────────────────┴────────────────────┘
```

| Quadrant | Tính năng | Quyết định |
|---|---|---|
| ⚡ **Quick Win** — làm trước | CV Upload + Keyword Extraction & JD Paste + Extraction | Làm ngay trong Sprint 1 |
| 🎯 **Strategic Bet** — đầu tư dài hạn | Top 3 Gap Analysis + Before/After Rewrite | Làm sau khi extraction ổn định |
| 🚫 **Non-starter** — bỏ thẳng | Interview Question Generator | Out-of-Scope khỏi MVP |

> **Lý do loại Interview Question Generator khỏi MVP:** Phục vụ user state khác (đã pass CV screening), value proposition riêng biệt, và giữ trong MVP sẽ làm data bị nhiễu — không biết user trả 49K vì Gap Analysis hay vì interview questions.
