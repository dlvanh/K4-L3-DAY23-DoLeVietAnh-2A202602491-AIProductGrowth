# Worksheet — Styfit

Họ tên: Đỗ Lê Việt Anh · MSSV: 2A202602491 · Ngày làm: 10/10/2026 (Day 0 của kế hoạch 90 ngày)

> **Trạng thái bản nháp:** các số đánh dấu **[G#]** bên dưới là **giả định** của chủ sản phẩm. Styfit v1 là prototype chạy local, chưa có doanh thu, chưa có AI, nên chưa có số thật. Mọi ngưỡng [MH] được suy từ các giả định này; đổi giả định thì chạy lại phụ lục.

## Số liệu đầu vào (bảng số của Trạm 3)

| Mã | Biến | Giá trị | Nguồn / cơ sở |
|---|---|---|---|
| G1 | ARPU (người trả phí, gói "Plus") | **49.000đ/tháng** | Giả định. Chưa có giá bán thật; mốc tham chiếu: app subscription tiêu dùng tại VN ở mức vài chục nghìn đ/tháng |
| G2 | Gross margin mục tiêu, tính trên người trả phí | **65%** | Giả định, nằm giữa ICONIQ 2027P = 59% và Duolingo ≈ 73% (HANDBOOK §8.1–8.2) |
| G3 | CAC payback tối đa | **12 tháng** | HANDBOOK §3.1 (hàng LTV/CAC · payback · GM: LTV/CAC ≥ 3, payback < 12 tháng) |
| G4 | COGS không-AI (hosting, lưu ảnh, phí cổng thanh toán) | **15% ARPU = 7.350đ** | Giả định, chưa đo |
| G5 | Cost/Job AI (1 lượt Style It / Outfit Check = 1 lần gắn thẻ ảnh + 1 lần viết giải thích, kiến trúc v3 dùng Gemini) | **250đ/job** | Giả định. **Phải đo bằng spike Gemini trước 24/10/2026**, xem Trạm 5 |
| G6 | CAC mục tiêu trên 1 người trả phí | **100.000đ** | Giả định: tăng trưởng organic/creator, không chạy ads trả phí khi chưa chứng minh retention |
| G7 | Tỷ lệ free → paid | **5%** | Cổng ngày 90 B2C trong HANDBOOK §3.1; thấp hơn 8,5% [BM] app AI và 9% Duolingo, nên không giả định mình giỏi hơn Duolingo |
| G8 | Tỷ lệ user free còn dùng AI trong 1 tháng | **30%** | Giả định, chưa đo |
| G9 | Người trả phí dùng trung bình | **35 job/tháng**, trần đề xuất **100 job/tháng** | Giả định; 35 × 250đ = 8.750đ, nằm trong ngân sách AI 9.800đ (Phụ lục MH-3) |
| G10 | Runway | **6 tháng** (thời gian đội + tiền mặt gần như 0) | Giả định, **cần chủ sản phẩm xác nhận** |

Tài liệu gốc của Styfit: `docs/STYFIT_BRIEF.md` và `docs/ARCHITECTURE_PLAN.md` (repo Styfit). Mốc kế hoạch: v1 prototype chạy local, v2 rule-based (tài khoản thật), v3 AI (Gemini). AI chỉ xuất hiện ở v3, nên chi phí AI trong 90 ngày tới chỉ có thể đo qua một spike riêng (xem Chưa đo được).

---

## Trạm 1 — Loại mô hình

**Câu chốt loại:** Chúng tôi là **B2C** vì tiền (dự kiến gói Plus 49.000đ/tháng) đến từ chính người dùng cá nhân 18–30 tuổi, người dùng thật cũng chính là người trả tiền, và chúng tôi chạm trực tiếp họ qua web app mobile-first (Style It, Outfit Check, Worn → For you), không qua bên trung gian nào (affiliate và shopping links nằm ở mục "Later" của roadmap).

Ba câu hỏi theo thực tế hôm nay:
1. **Ai trả tiền?** Hôm nay không ai trả (v1 chưa có thanh toán). Người trả dự kiến là cá nhân 18–30 tuổi.
2. **Ai dùng?** Chính người trả tiền, nên không phải B2B.
3. **Có bên trung gian không?** Không. Styfit tự có giao diện, tự giữ dữ liệu, tự chạm end-user, nên không phải B2B2C.

**Bảng đèn §3.1 B2C** (đủ mọi đèn trong bảng; thêm 2 đèn đặc thù Styfit ở cuối):

| Đèn | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| Đường cong retention có phẳng không (D1/D7/D30/D60) | ❌ | Biết cách (cohort + event log) nhưng **chưa có user nào dùng ≥30 ngày**. v1 chạy local, dữ liệu chỉ ở localStorage từng trình duyệt. Cần: bản cho tester dùng trên điện thoại + event log. Số đầu tiên (R30) ≈ 09/12/2026 |
| Activation rate | 🔧 | Cần trong 2 tuần: (1) chốt định nghĩa aha moment (Style It → lưu ≥1 look), (2) ghi event ngay trong trình duyệt (không gửi server, phù hợp nguyên tắc v1), (3) tester xuất file log |
| p95 cost/user/tháng ÷ ARPU | ❌ | v1/v2 là rule-based nên chi phí AI = 0; chưa có phân phối usage. Mới có thể đo **Cost/Job** (đầu vào) bằng spike Gemini (🔧, xem Chưa đo được) |
| Trial → paid | ❌ | Chưa có paywall, gói trả phí hay thanh toán. Cần v2 (tài khoản, thanh toán) |
| Retention M12 | ❌ | Cần ≥12 tháng dữ liệu của người trả phí. Không có số trước ≈ 11/2027; hiện chỉ dùng [BM] |
| Chi phí free tier ÷ tổng COGS | ❌ | Chưa có AI, chưa có COGS thật. Cần beta có AI và hoá đơn token |
| Tỷ lệ refund | ❌ | Chưa có giao dịch |
| LTV/CAC | ❌ | Chưa có doanh thu và chi phí thật; hiện chỉ là mô hình giả định (Phụ lục MH-2) |
| CAC payback | ❌ | Như trên; ngưỡng đã suy từ mô hình (Phụ lục MH-1) |
| Gross margin | ❌ | Như trên; cần hoá đơn hosting, token, cổng thanh toán |
| *(thêm)* Worn-loop rate: % user activated bấm "Wore it" ≥1 lần trong 7 ngày đầu | 🔧 | Tính năng "Worn" đã có trong thiết kế v1; cần event log như Activation |
| *(thêm)* Cost/Job AI | 🔧 | Chạy 200 job thật với Gemini trên ảnh mẫu, ghi token và chi phí (1 tuần, ~ 24/10/2026) |

Tổng kết: 2 🔧 đo được trong 2 tuần ngoài Cost/Job (Activation, Worn-loop), còn lại ❌. Tất cả ô ❌/🔧 được chép sang mục **Chưa đo được** của dashboard.

---

## Trạm 2 — Thẻ đèn

**North Star:** Đường cong retention của người dùng đã kích hoạt, đọc bằng **R30** (% còn hoạt động ở ngày 28–34) và **R60/R30** (độ phẳng) — hiện tại **chưa có số** (0 đợt tester) — mục tiêu 🟢: R30 ≥ 21,1% và R60/R30 ≥ 0,89.

**Định nghĩa "hoạt động" dùng chung:** user thực hiện ≥1 hành động giá trị trong tuần: hoàn tất một kết quả Style It/Outfit Check **và** (lưu look **hoặc** bấm "Wore it"). **Không đếm:** mở app, xem trang, đổi role dev switch (Customer/Admin), tài khoản của đội, phiên tester được người của đội cầm tay làm hộ.

| # | Tầng (L/O/G) | Đèn | Định nghĩa (đếm gì · **không** đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---|---|---|---|---|---|---|
| 1 | L | **Retention D30 phẳng** (North Star) | % user activated còn "hoạt động" (định nghĩa chung) ở ngày 28–34 (R30) và ở ngày 58–64 (R60). Chỉ tính đợt ≥40 user activated. **Không** tính user của đội, user dưới 18 hoặc đã xoá | R30 = số còn hoạt động ngày 28–34 ÷ số activated của đợt; độ phẳng = R60 ÷ R30 | Tuần · chủ sản phẩm xuất cohort từ event log | LTV (G) và CAC payback (G) |
| 2 | L | **Activation rate** | % user mới **lưu ≥1 look** từ kết quả Style It trong 24 giờ đầu kể từ lần mở đầu tiên. **Không** tính: xem mà không lưu, lưu bằng cách đội cầm tay, tester đã dùng bản trước đó | Số user lưu ≥1 look ≤24h ÷ số user mới mở lần đầu | Ngày · chủ sản phẩm | Worn-loop rate (L) → R30 (L) |
| 3 | L | **Worn-loop rate** | % user activated bấm **"Wore it" ≥1 lần trong 7 ngày** kể từ lúc activated, ngày mặc nằm trong cửa sổ đó. **Không** tính: bấm rồi hoàn tác, ghi ngày ngoài cửa sổ | Số user có ≥1 "Wore it" hợp lệ ≤7 ngày ÷ số user activated của đợt | Tuần · chủ sản phẩm | R30 (L) → LTV (G) |
| 4 | L | **p95 chi phí AI/user/tháng ÷ ARPU** (*đèn chi phí AI*) | Chi phí token Gemini (gắn thẻ ảnh + viết giải thích) của user ở **phân vị 95**, chia cho ARPU 49.000đ. **Không** lấy trung bình; **không** tính chi phí hosting | (Tổng chi phí AI của user p95 trong tháng) ÷ 49.000đ | Tuần · chủ sản phẩm (từ hoá đơn token + log job theo user) | Gross margin (G) |
| 5 | O | **Chi phí free tier ÷ tổng COGS** | Chi phí AI của user chưa trả phí ÷ tổng COGS trong tháng (AI trả phí + AI free + hosting + phí thanh toán). **Không** tính lương đội | Chi phí AI user free ÷ tổng COGS | Tháng · chủ sản phẩm | Gross margin (G) |
| 6 | O | **Trial → paid** | % user bắt đầu dùng thử Plus mà **trả tiền thật** trong 14 ngày sau khi hết thử. **Không** tính bấm "đăng ký" mà chưa trả, và không tính tài khoản test của đội | Số user trả tiền ÷ số user bắt đầu thử | Tháng · chủ sản phẩm | CAC payback (G), LTV (G) |
| 7 | G | **CAC payback** | Số tháng để lãi gộp của 1 người trả phí bù CAC. CAC gồm chi phí trả creator, ads, voucher; **không** tính công của đội | CAC ÷ (ARPU × GM) | Quý · chủ sản phẩm + 1 người ngoài đội | — |
| 8 | G | **Gross margin (all-in)** | (Doanh thu − COGS) ÷ doanh thu; COGS gồm token AI của cả user free, hosting, lưu ảnh, phí thanh toán. **Không** tính lương | Như định nghĩa | Quý · chủ sản phẩm + 1 người ngoài đội | — |

Thống kê: 8 đèn = **4 Leading** (≥2 ✓) · **2 Operating** · **2 Lagging** (≤3 ✓, 25% < 50% ✓).
Đèn chi phí AI là đèn số: **4** (p95 chi phí AI ÷ ARPU), kèm đèn số 5 (free tier) để thấy phần chi phí cho người chưa trả tiền.

Đã cân nhắc và **bỏ**: Retention M12 (không đo được trong 90 ngày), Tỷ lệ refund (chưa có giao dịch), LTV/CAC (suy được từ #1 và #7, không cần đèn riêng).

---

## Trạm 3 — Ngưỡng

Ngày kiểm tra mọi [BM]: **10/10/2026**. Các số [BM] lấy từ HANDBOOK §8 (chốt 27/08/2026; còn dưới 3 tháng nên không phải cập nhật). Tôi **chưa mở lại trang gốc** của RevenueCat, ICONIQ; nếu trước hạn nộp muốn chắc, mở các link ở HANDBOOK §8.1 mục 1 và §8.2 mục 6–7 và xác nhận lại.

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn | Lý do (1 câu) · ngày kiểm tra nếu [BM] |
|---|---|---|---|---|---|---|
| 1 | Retention D30 phẳng | R60/R30 ≥ 0,89 **và** R30 ≥ 21,1% | R60/R30 từ 0,84 đến 0,89 | R60/R30 < 0,84 **hoặc** R30 < 21,1% | [MH] cho độ phẳng; [BM] cho R30 | 0,89 là mức giữ chân tháng để LTV/CAC ≥ 3 và 0,84 để LTV/CAC ≥ 2 (Phụ lục MH-2); 21,1% là trung vị retention M12 của app AI, vì đường cong phẳng thì R30 ≈ R12 (RevenueCat 03/2026, kiểm tra 10/10/2026) |
| 2 | Activation rate | ≥ baseline × 1,3 | trong ±10% quanh baseline | < baseline | [TB] | Chưa có chuẩn; baseline là trung bình 2 đợt tester đầu, ngày dự kiến có số **09/11/2026** (đợt 1) và **23/11/2026** (đợt 2); hệ số +30% theo quy ước HANDBOOK §3.1 |
| 3 | Worn-loop rate | ≥ 30% | 21,1–30% | < 21,1% | [TB] tạm, có chặn dưới suy từ #1 | Nếu ít hơn 1/5 user activated từng "mặc thử" trong tuần đầu thì vòng lặp lõi không đủ giữ R30 ≥ 21,1%; ngưỡng 30% là đệm ~9 điểm, **thay bằng baseline sau 2 đợt (≈ 23/11/2026)** |
| 4 | p95 chi phí AI ÷ ARPU | < 0,30 | 0,30–0,60 | > 0,60 | [MH] | 0,30 nghĩa là user nặng nhất nhóm p95 vẫn còn GM ≈ 55%, sát mục tiêu; 0,60 nghĩa là GM còn 25% (Phụ lục MH-3) |
| 5 | Chi phí free tier ÷ tổng COGS | < 20% | 20–40% | > 40% | [MH] | Với 5% chuyển đổi, 20% ứng với quota free ≈ 3 job/tháng và 40% ứng với ≈ 8 job/tháng (Phụ lục MH-4); mốc 20/40% giữ từ HANDBOOK §3.1 |
| 6 | Trial → paid | ≥ 8,5% | 5–8,5% | < 5% | [BM] | 8,5% là trung vị app AI (RevenueCat 03/2026, kiểm tra 10/10/2026); mô hình giả định 5% nên 🟡 là vùng "đúng như giả định" |
| 7 | CAC payback | ≤ 9 tháng | 9–12 tháng | > 12 tháng | [MH] | 12 tháng ⇔ CAC tối đa 382.200đ; 9 tháng chừa đệm 25% (Phụ lục MH-1) |
| 8 | Gross margin (all-in) | ≥ 56% | 45–56% | < 45% | [MH] + [BM] | 56% là GM khi GM trả phí 65% và free tier ở mức 20% COGS (Phụ lục MH-4); 45% là GM trung bình công ty AI năm 2025 (ICONIQ, kiểm tra 10/10/2026) và gần khớp mức 41,7% khi free tier chạm 40% COGS |

### Phụ lục [MH] — phép tính

**[MH]-1 — CAC payback → CAC tối đa**

```
Đầu vào: ARPU = 49.000đ/tháng [G1] · GM trả phí = 65% [G2] · payback tối đa = 12 tháng [G3]
Phép tính:
  Lãi gộp mỗi người trả phí mỗi tháng = 49.000 × 65% = 31.850đ
  CAC tối đa (payback 12 tháng)       = 31.850 × 12  = 382.200đ
  CAC tại payback 9 tháng             = 31.850 × 9   = 286.650đ
Kết quả → 🟢 payback ≤ 9 tháng (CAC ≤ 286.650đ) · 🟡 9–12 tháng · 🔴 > 12 tháng (CAC > 382.200đ)
Ghi chú: CAC mục tiêu thật là 100.000đ [G6] ⇒ payback dự kiến 100.000 ÷ 31.850 ≈ 3,1 tháng, còn dư rất nhiều
```

**[MH]-2 — Độ phẳng retention (R60/R30) từ LTV/CAC**

```
Đầu vào: lãi gộp/tháng = 31.850đ · CAC mục tiêu = 100.000đ [G6]
Giả định đơn giản: sau D30 mỗi tháng rời đi cùng một tỷ lệ c, nên thời gian ở lại trung bình = 1/c tháng
Phép tính:
  LTV/CAC = 3 → LTV = 300.000đ → thời gian ở lại = 300.000 ÷ 31.850 = 9,42 tháng → c = 10,6% → R60/R30 = 1 − c = 0,894
  LTV/CAC = 2 → LTV = 200.000đ → thời gian ở lại = 200.000 ÷ 31.850 = 6,28 tháng → c = 15,9% → R60/R30 = 0,841
Kiểm chứng với benchmark: M12 trung vị app AI = 21,1% ⇒ rời bình quân 1 − 0,211^(1/11) = 13,2%/tháng ⇒ R60/R30 ≈ 0,868, ở vùng 🟡
  ⇒ thời gian ở lại ≈ 7,6 tháng ⇒ LTV ≈ 241.500đ ⇒ LTV/CAC ≈ 2,4 tại CAC 100.000đ.
  Tức nếu Styfit chỉ bằng trung vị ngành, CAC tối đa để LTV/CAC ≥ 3 chỉ còn ≈ 80.500đ/người trả phí.
Kết quả → 🟢 R60/R30 ≥ 0,89 · 🟡 0,84–0,89 · 🔴 < 0,84
Hạn chế: coi người trả phí rời đi giống nhóm tester hoạt động; thực tế hai nhóm khác nhau (ghi ở Chưa đo được)
```

**[MH]-3 — p95 chi phí AI ÷ ARPU, trần job và Cost/Job tối đa**

```
Đầu vào: ARPU = 49.000đ · COGS không-AI = 15% = 7.350đ [G4] · GM trả phí 65% ⇒ tổng COGS trả phí ≤ 35% = 17.150đ
         Cost/Job = 250đ [G5]
Ngân sách AI trung bình mỗi người trả phí = 17.150 − 7.350 = 9.800đ ⇒ dùng trung bình ≤ 9.800 ÷ 250 = 39 job/tháng (G9 chọn 35 job = 8.750đ ✓)
Phép tính cho user nặng nhất nhóm p95:
  🟢 p95 < 0,30 ARPU = 14.700đ ≈ 58 job → GM của user đó = 100% − 15% − 30% = 55%
  🔴 p95 > 0,60 ARPU = 29.400đ ≈ 117 job → GM của user đó = 100% − 15% − 60% = 25%
Từ đó suy ra:
  Trần job gói trả phí = 100 job/tháng (100 × 250 = 25.000đ = 0,51 ARPU, dưới ngưỡng đỏ)
  Cost/Job tối đa để chạm trần mà không đỏ = 29.400 ÷ 100 = 294đ ⇒ nếu spike đo được > 294đ/job thì mô hình phải đổi trần hoặc đổi model Gemini
Kết quả → 🟢 < 0,30 · 🟡 0,30–0,60 · 🔴 > 0,60
```

**[MH]-4 — Free tier ÷ COGS → quota free và GM all-in**

```
Đầu vào: COGS trả phí/người = 17.150đ · tỷ lệ free→paid = 5% [G7] ⇒ 19 user free cho mỗi người trả phí
         30% user free còn dùng AI [G8] ⇒ 19 × 30% = 5,7 user free hoạt động/người trả phí · Cost/Job = 250đ
Công thức: chi phí free/người trả phí = s/(1−s) × 17.150, với s = tỷ lệ free trong tổng COGS
  s = 20% → 4.288đ → mỗi user free hoạt động 752đ → 3,0 job/tháng; tổng COGS = 21.438đ → GM all-in = 1 − 21.438/49.000 = 56,3%
  s = 40% → 11.433đ → mỗi user free hoạt động 2.006đ → 8,0 job/tháng; tổng COGS = 28.583đ → GM all-in = 41,7%
Kết quả → quota free = 3 job AI/tháng để ở 🟢; tuyệt đối không quá 8 job/tháng
          🟢 free tier < 20% COGS · 🟡 20–40% · 🔴 > 40%
          GM all-in: 🟢 ≥ 56% · 🔴 < 45% (ICONIQ 2025 = 45%, gần mức 41,7% ở trên)
```

---

## Trạm 4 — 5 luật quyết định

Đánh dấu ⏹ cho luật dừng (có 3). Vế NẾU dùng đúng ngưỡng 🔴 ở Trạm 3. "Tôi" là chủ sản phẩm; sprint là 1 tuần.

1. ⏹ **NẾU** đèn #1 đỏ (R60/R30 < 0,84 hoặc R30 < 21,1%) **TRONG** 2 đợt user liên tiếp **VÀ** mỗi đợt ≥ 40 user activated, **THÌ** tôi đóng băng toàn bộ chi tiêu và công sức acquisition (trả creator, ads, voucher, seeding nhóm) trong 3 tuần kể từ tuần sau, cả đội làm lại vòng Style It → Wore it → For you, **KHÔNG THÌ** không tăng ngân sách hay seeding thêm để bù số user rời đi.
2. ⏹ **NẾU** Activation rate < baseline (đèn #2 đỏ) **TRÊN** 2 đợt tester liên tiếp **VÀ** mỗi đợt ≥ 20 tester thật, **THÌ** tôi dừng thêm tính năng mới (Explore filters mới, Outfit Check mới, Fit Profile mới) trong 2 sprint và làm lại luồng Style It đầu tiên để user lưu được look đầu tiên trong ≤ 3 thao tác, **KHÔNG THÌ** không thêm tính năng với lý do "thiếu tính năng" và không mời thêm tester mới vào luồng cũ.
3. **NẾU** p95 chi phí AI ÷ ARPU > 0,60 (đèn #4 đỏ) **TRONG** 1 tháng **VÀ** có ≥ 200 job trong tháng đó, **THÌ** tôi đặt trần 100 job/tháng cho gói trả phí và tách Outfit Check bằng ảnh thành gói riêng trong sprint tiếp theo, **KHÔNG THÌ** không tăng giá 49.000đ đại trà, vì làm vậy mất user nhẹ để cứu biên do 5% user nặng gây ra.
4. ⏹ **NẾU** chi phí free tier > 40% tổng COGS (đèn #5 đỏ) **TRONG** 2 tháng liên tiếp, **THÌ** tôi hạ quota free xuống 3 job AI/tháng ngay sprint tới và dừng mời user free mới vào luồng AI (họ vẫn dùng bản rule-based), **KHÔNG THÌ** không mở rộng free tier hay tặng thêm job để kéo tăng trưởng nhanh hơn.
5. **NẾU** Worn-loop rate < 21,1% (đèn #3 đỏ) **TRÊN** 2 đợt liên tiếp **VÀ** mỗi đợt ≥ 40 user activated, **THÌ** tôi đưa nút "Wore it" lên thẳng màn kết quả Style It (1 chạm), chạy phiên bản đó 2 tuần và phỏng vấn 8 user không bấm "Wore it", **KHÔNG THÌ** không gửi thông báo hằng ngày, không bật sẵn "For you" mà thiếu đồng ý của user (trái nguyên tắc consent theo mục đích của Styfit).

Kiểm tra: 5 luật, đủ 4 vế bắt buộc (NẾU / TRONG-TRÊN / THÌ / KHÔNG THÌ); vế VÀ có ở luật 1, 2, 3, 5 (metric nhiễu vì mẫu nhỏ); luật dừng ⏹ = 3 (luật 1, 2, 4); không luật nào kết thúc bằng "xem xét lại".

**Phản xạ sai đã chặn (bước 3 của Trạm 4):**

| Luật | Khi đèn đỏ, phản xạ đầu tiên của tôi | Vì sao sai |
|---|---|---|
| 1 | Chạy thêm seeding/creator để "kéo user mới bù user cũ" | Retention không phẳng thì mỗi user mới rời đi như cũ, CAC tăng mà LTV không đổi |
| 2 | Thêm tính năng (thêm bộ lọc, thêm Outfit Check) | User không lưu nổi look đầu tiên thì thêm tính năng chỉ tạo thêm chỗ để họ bỏ đi |
| 3 | Tăng giá toàn bộ gói | 95% user nhẹ trả thêm tiền để bù 5% user nặng |
| 4 | Nới free tier để giữ tốc độ tăng trưởng | Mỗi user free thêm là thêm chi phí token, không có doanh thu |
| 5 | Bắn thông báo hằng ngày để ép thói quen | Là spam, và trái nguyên tắc consent theo mục đích (PDPL) mà Styfit đã cam kết |

---

## Trạm 5 — nháp cổng gác (bản cuối nằm ở `dashboard.md`)

Mốc ngày: Day 0 = 10/10/2026 · Day 30 = 09/11/2026 · Day 60 = 09/12/2026 · Day 90 = 08/01/2027.

Điều kiện nền (cần chủ sản phẩm quyết định): v1 hiện **chạy local, không deploy**. Để có tester dùng ≥30–60 ngày trên điện thoại của họ cần một bản preview không lưu dữ liệu cá nhân lên server, hoặc v2 beta. Nếu không có điều kiện này, cổng ngày 60 và 90 không thể có số, và phải chuyển sang FIX ngay ở ngày 30.
