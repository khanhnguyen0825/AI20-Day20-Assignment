# 🔗 Dependency Map — CV Gap Analyzer

## 3 External Dependencies Có Thể Giết Dự Án Trong 30 Ngày Tới

**Dependency 1 — Anthropic API Rate Limit / Cost Spike**
- **Worst-case:** Anthropic thay đổi pricing hoặc giới hạn rate limit vào đúng tuần launch — mỗi session phân tích CV+JD dài tốn 2–3x token dự kiến, cost/session vọt từ 1.500đ lên 6.000đ, margin âm ngay từ đầu.
- **Plan B:** Pre-cache prompt template + giới hạn CV input tối đa 800 từ và JD tối đa 600 từ trước khi gửi API — giảm token ~40%. Song song test thêm GPT-4o Mini làm fallback model nếu Anthropic cost vượt ngưỡng.
- **Cost của Plan B:** ~3 ngày engineering để implement token trimming + 1 ngày test GPT-4o Mini. Chi phí thêm ~0đ nếu dùng free tier để test.

**Dependency 2 — PDF Parser Không Đọc Được CV Của Target User**
- **Worst-case:** 40–50% CV sinh viên dùng Canva template hoặc chụp ảnh điện thoại → PyMuPDF extract ra garbage text hoặc trống hoàn toàn → parser "thành công" về mặt kỹ thuật nhưng output vô nghĩa → user mất tiền, trust về 0.
- **Plan B:** Nếu extracted text < 100 từ hoặc < 5 bullet points, tự động fallback sang plain text paste — hiển thị box "CV của bạn có vẻ khó đọc — paste nội dung CV vào đây để tiếp tục." Không block user, không mất session.
- **Cost của Plan B:** ~2 ngày để build fallback UI + text paste input. Cần test với 20 CV thật (Canva, Word, scan) trước launch — 0đ chi phí nếu thu từ bạn bè.

**Dependency 3 — ITviec / TopCV Block URL Scraper**
- **Worst-case:** ITviec phát hiện và block IP scraper trong tuần đầu — tính năng "paste URL để lấy JD tự động" bị chết hoàn toàn, user phải copy-paste tay mà không được thông báo trước → UX gãy giữa chừng, trust drop.
- **Plan B:** Loại bỏ URL scraping khỏi MVP, chỉ giữ plain text paste cho JD. Thêm hướng dẫn 2 bước ngay trong UI: "Copy toàn bộ nội dung JD từ ITviec → Paste vào đây." Mất 1 click của user nhưng không có dependency nào vào bên ngoài.
- **Cost của Plan B:** 0đ, tiết kiệm ~3 ngày không phải build scraper. Triển khai được ngay trong 1 ngày.

---

## 🔴 Critical Path — Cột NOW

```text
[1] Thu thập 20 CV thật → test PyMuPDF
        ↓ (blocking)
[2] Build PDF parser + fallback plain text paste
        ↓ (blocking)
[3] Build JD text paste input (bỏ URL scraper)
        ↓ (blocking)
[4] Viết & test prompt template Gap Analysis
        ↓ (blocking)
[5] Integrate Anthropic API + token trimming
        ↓ (blocking)
[6] Build Gap Report UI (3 gap cards)
        ↓
[7] Soft launch — 10 users thật, manual monitor
```

- **Critical Path** = toàn bộ chuỗi `[1] → [7]` — không task nào có thể bỏ qua hoặc chạy song song nếu task trước chưa xong.
- **Task blocking nguy hiểm nhất:** `[1]` và `[2]` — nếu parser fail với phần lớn CV thật, toàn bộ chuỗi phía sau vô nghĩa. Phải invalidate trong 2 ngày đầu tiên trước khi build bất cứ thứ gì khác.
