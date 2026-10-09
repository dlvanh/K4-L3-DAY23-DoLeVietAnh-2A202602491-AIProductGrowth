# OPERATING DASHBOARD — Styfit

**Loại mô hình:** B2C · **Cập nhật:** 10/10/2026 (Day 0) · Đỗ Lê Việt Anh – 2A202602491
**NORTH STAR:** Retention D30 phẳng (R30 và R60/R30 của người dùng đã kích hoạt) — hiện tại **chưa có số** (0 đợt tester) — mục tiêu R30 ≥ 21,1% và R60/R30 ≥ 0,89
*Số ARPU 49.000đ, GM 65%, CAC 100.000đ, Cost/Job 250đ là **giả định** (v1 chưa có doanh thu, chưa có AI); phép tính ở trang 2.*

### Đèn báo sớm (Leading — nhìn hằng ngày/tuần)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| **1. Retention D30 phẳng** (R30; R60/R30) | ⚪ chưa đo (số đầu ≈ 09/12/2026) | 🟢 R60/R30 ≥ 0,89 và R30 ≥ 21,1% · 🟡 0,84–0,89 · 🔴 < 0,84 hoặc R30 < 21,1% | [MH] + [BM] RevenueCat, kiểm tra 10/10/2026 | LTV, CAC payback |
| **2. Activation** (lưu ≥1 look ≤ 24h đầu) | ⚪ chưa đo (≈ 09/11/2026) | 🟢 ≥ baseline × 1,3 · 🟡 ±10% baseline · 🔴 < baseline | [TB] | Worn-loop → R30 |
| **3. Worn-loop** ("Wore it" ≥1 lần ≤ 7 ngày) | ⚪ chưa đo (≈ 09/11/2026) | 🟢 ≥ 30% · 🟡 21,1–30% · 🔴 < 21,1% | [TB] tạm | R30 → LTV |
| **4. p95 chi phí AI/user ÷ ARPU** (*đèn chi phí AI*) | ⚪ chưa đo (không có AI trước v3) | 🟢 < 0,30 · 🟡 0,30–0,60 · 🔴 > 0,60 | [MH] | Gross margin |

### Đèn vận hành (Operating — nhìn hằng tuần/tháng)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| **5. Chi phí free tier ÷ tổng COGS** | ⚪ chưa đo | 🟢 < 20% · 🟡 20–40% · 🔴 > 40% (quota free = 3 job/tháng) | [MH] | Gross margin |
| **6. Trial → paid** | ⚪ chưa đo (chưa có paywall) | 🟢 ≥ 8,5% · 🟡 5–8,5% · 🔴 < 5% | [BM] RevenueCat, kiểm tra 10/10/2026 | CAC payback, LTV |

### Đèn kết quả (Lagging — nhìn hằng quý)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---|---|---|
| **7. CAC payback** | ⚪ chưa đo | 🟢 ≤ 9 tháng · 🟡 9–12 · 🔴 > 12 (CAC tối đa 382.200đ) | [MH] |
| **8. Gross margin (all-in)** | ⚪ chưa đo | 🟢 ≥ 56% · 🟡 45–56% · 🔴 < 45% | [MH] + [BM] ICONIQ, kiểm tra 10/10/2026 |

### 5 luật quyết định (⏹ = luật dừng)

1. ⏹ **NẾU** đèn 1 đỏ (R60/R30 < 0,84 hoặc R30 < 21,1%) **TRONG** 2 đợt liên tiếp **VÀ** mỗi đợt ≥ 40 user activated **THÌ** đóng băng mọi chi tiêu và công sức acquisition 3 tuần, cả đội làm lại vòng Style It → Wore it → For you **KHÔNG THÌ** không tăng ngân sách/seeding để bù user rời đi.
2. ⏹ **NẾU** Activation < baseline **TRÊN** 2 đợt tester liên tiếp **VÀ** mỗi đợt ≥ 20 tester **THÌ** dừng thêm tính năng mới 2 sprint, làm lại luồng Style It để lưu look đầu tiên trong ≤ 3 thao tác **KHÔNG THÌ** không thêm tính năng với lý do "thiếu tính năng".
3. **NẾU** p95 chi phí AI/ARPU > 0,60 **TRONG** 1 tháng **VÀ** ≥ 200 job **THÌ** đặt trần 100 job/tháng cho gói trả phí, tách Outfit Check ảnh thành gói riêng sprint sau **KHÔNG THÌ** không tăng giá 49.000đ đại trà.
4. ⏹ **NẾU** free tier > 40% tổng COGS **TRONG** 2 tháng **THÌ** hạ quota free xuống 3 job AI/tháng và dừng mời user free mới vào luồng AI ngay sprint tới **KHÔNG THÌ** không mở rộng free tier để kéo tăng trưởng.
5. **NẾU** Worn-loop < 21,1% **TRÊN** 2 đợt **VÀ** mỗi đợt ≥ 40 user activated **THÌ** đưa nút "Wore it" lên màn kết quả Style It (1 chạm), chạy 2 tuần, phỏng vấn 8 user không bấm **KHÔNG THÌ** không gửi thông báo hằng ngày, không bật sẵn "For you" khi chưa có đồng ý.

### Cổng gác 90 ngày (Day 0 = 10/10/2026)

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---|---|---|---|---|
| 30 (09/11/2026) — cổng **học** | Số tester 18–30 thật (ngoài đội) có log activation đầy đủ | ≥ 40 tester, kèm một con số activation baseline | `evidence/d30-activation-log.csv` (1 dòng/tester ẩn danh) + `evidence/aha-definition.md` | **FIX** (1 lần): ở lại 30 ngày, chỉ để tuyển đủ tester |
| 60 (09/12/2026) | R30 của đợt tester đầu (≥ 40 user activated) | ≥ 21,1% | `evidence/cohort-1.csv` + ảnh đường cong D1/D7/D30 | **FIX** nếu biết rõ vì sao (sửa đúng 1 thứ); **PIVOT** nếu giả định "Wore it → For you tạo thói quen" đã sai |
| 90 (08/01/2027) | R60/R30 của đợt đầu | ≥ 0,89 | `evidence/cohort-1.csv` cập nhật tới D60 + `evidence/gate90-review.md` có người ngoài đội ký | 0,84–0,89: **FIX** nếu chưa dùng ở D60, nếu không thì **PIVOT**; < 0,84: **KILL** |

**KILL CRITERIA:** đến hết 08/01/2027, nếu R60/R30 của đợt tester đầu (≥ 40 user activated) vẫn < 0,84 sau đúng một lần FIX ở cổng ngày 60, tôi dừng hẳn hướng B2C cá nhân của Styfit: không xây v2/v3 AI, không chi thêm tiền hay công sức.

**CHƯA ĐO ĐƯỢC:**
- **Retention (đèn 1; M12):** cần bản cho tester dùng trên điện thoại + event log cohort; R30 ≈ 09/12/2026, R60/R30 ≈ 08/01/2027, M12 ≈ 11/2027. Hôm nay v1 chạy local nên không có user nào quá 30 ngày.
- **Activation, Worn-loop (đèn 2, 3):** cần định nghĩa aha moment, event log trong trình duyệt và ≥ 40 tester; có số ≈ 09/11/2026.
- **Chi phí AI (đèn 4, 5):** AI mới có ở v3. Cần spike Gemini ≥ 200 job để đo Cost/Job (hạn 24/10/2026; nếu > 294đ/job thì mô hình đổi trần); p95 theo user và free tier cần beta có AI, chưa có ngày.
- **Trial → paid, refund, CAC payback, LTV/CAC, GM (đèn 6, 7, 8):** chưa có paywall, doanh thu, chi phí thật; cần v2 có thanh toán, sớm nhất Q1/2027. Hiện chỉ có mô hình giả định.
- **Chưa kiểm chứng:** ARPU 49.000đ, CAC 100.000đ, Cost/Job 250đ, 30% user free còn dùng AI, người trả phí rời đi giống tester hoạt động.
