# PRD — AI Decision Copilot for E-commerce Ads

**Product Name:** AI Decision Copilot  
**Version:** 1.0 (MVP)  
**Author:** Nguyễn Thị Tuyết — 2A202600215  
**Date:** 24/04/2026
**Status:** Draft for Review
**Assignment Day17**
---

## 1. Problem Statement

Chủ shop thời trang nữ online đang lãng phí ngân sách quảng cáo vì không xác định được nhóm khách có ý định mua cao và không tối ưu chiến dịch đủ nhanh khi chỉ số xấu đi — mỗi ngày trễ 12-24h là mất 5-15% ngân sách ads vào nhóm kém chất lượng.

---

## 2. Target User

Chủ shop hoặc marketer vận hành ads cho shop thời trang nữ online có team marketing 1-2 người, doanh thu 200M-1B VND/tháng, chạy ads liên tục trên Facebook/Instagram + website để duy trì đơn hàng.

**User Profile:**
- Thường xuyên theo dõi CPL/CPA/ROAS hàng ngày
- Không có analyst riêng, tự xử lý dữ liệu trong Ads Manager + Google Sheets
- Gặp khó khăn từ ngày 3-7 của chiến dịch khi CPA tăng mà không biết tắt ad set nào

---

## 3. Product Positioning

**What we are:**
> Một AI decision copilot giúp chủ shop online ra quyết định ads nhanh, đúng và có thể đo lường tác động lên CPA/ROAS.

**What we are not / not yet:**
> Chúng tôi không phải full-suite martech thay thế toàn bộ stack marketing, và chưa hướng tới doanh nghiệp enterprise đã có team data riêng.

---

## 4. User Stories

### User Story 1
> As a chủ shop thời trang nữ online, I want hệ thống xếp hạng nhóm khách có khả năng mua cao trong 7 ngày tới, so that tôi ưu tiên ngân sách đúng ad set và giảm CPA 10-20% trong 4-8 tuần.

### User Story 2
> As a marketer team nhỏ (1-2 người), I want nhận khuyến nghị hành động tối ưu ngân sách theo mức độ ưu tiên mỗi ngày với giải thích lý do rõ ràng, so that tôi phản ứng kịp trước biến động và bảo vệ ROAS trong 24-48h đầu của chiến dịch.

---

## 5. MVP Scope

### 5.1 In-Scope (tối đa 3 tính năng)

| # | Tính năng | Mô tả | Test giả định |
|---|---|---|---|
| 1 | Chấm điểm khả năng mua | Xếp hạng nhóm khách theo 3 mức ưu tiên (cao/trung bình/thấp) | Khách hàng có thực sự hành động theo xếp hạng này không? |
| 2 | Khuyến nghị tối ưu ngân sách | Đề xuất 3 hành động mỗi ngày (tắt/giảm/tăng ad set) có giải thích lý do | Người dùng có áp dụng khuyến nghị và tạo ra cải thiện CPA/ROAS không? |
| 3 | Kết nối dữ liệu | Tích hợp dữ liệu từ Meta/Google Ads và dữ liệu đơn hàng | Dữ liệu đủ để model dự đoán ý định mua có ích ngay từ tháng đầu? |

### 5.2 Out-of-Scope

| Tính năng | Lý do bỏ |
|---|---|
| Tự động chạy ads 100% không cần người duyệt | Quá phức tạp, rủi ro cao, chưa cần thiết cho MVP |
| Tích hợp toàn bộ kênh marketing (TikTok, email, CRM sâu) | Tập trung vào Meta/Google ads trước để validate core hypothesis |
| Mô hình AI tự huấn luyện nâng cao theo thời gian thực | Cần dữ liệu pilot trước, giai đoạn MVP dùng model ổn định |
| Ứng dụng mobile riêng | Ưu tiên web dashboard cho team marketing làm việc trên máy tính |
| Phân tích full-funnel đa chạm phức tạp (attribution nâng cao) | Tập trung vào 24-48h đầu của chiến dịch, không cần attribution phức tạp |

### 5.3 Non-Goals

- Không thay thế hoàn toàn đội marketing
- Không xây thành bộ martech all-in-one trong giai đoạn MVP
- Không cam kết tối ưu cho mọi ngành; chỉ tập trung segment shop thời trang nữ
- Không giải quyết bài toán branding dài hạn; chỉ tập trung hiệu quả chuyển đổi ngắn hạn
- Không theo đuổi độ chính xác dự đoán tuyệt đối ngay từ phiên bản đầu

---

## 6. AI-Specific Design

### 6.1 Model Selection

**Model:** Mô hình phân loại/xếp hạng ý định mua (purchase propensity scoring) kết hợp mô hình gợi ý nội dung ngắn

**Lý do chọn:**
- Cần ưu tiên tính ổn định, dễ giải thích, cập nhật nhanh theo chu kỳ chiến dịch
- Không cần mô hình quá nặng ở giai đoạn MVP
- Dùng rule-based + ML nhẹ để đảm bảo explainability cho người dùng

**Trade-offs chấp nhận:**
- Độ chính xác dự đoán không tuyệt đối
- Cần human-in-loop để validate

**Trade-offs không chấp nhận:**
- Model quá phức tạp không giải thích được lý do khuyến nghị

### 6.2 Data Requirements

| Yếu tố | Chi tiết |
|---|---|
| **Nguồn dữ liệu** | Dữ liệu hiệu suất ads (impressions, clicks, CTR, CPC, CPA, ROAS, frequency) từ Meta/Google Ads API; Dữ liệu đơn hàng (đơn, giá trị đơn, thời điểm mua) từ website/shop; Dữ liệu nội dung (creative/message đang chạy) |
| **Owner** | Shop owner/customer tự kết nối qua OAuth hoặc API key |
| **Update frequency** | Daily refresh cho metrics; Real-time cho alert |
| **Rủi ro về data quality** | Dữ liệu có thể không đầy đủ ở shop mới; cần xử lý missing data gracefully |

### 6.3 Fallback UX

**Chiến lược:** Human-in-the-loop

**Trigger:**
- Khi confidence score < 80%
- Khi CPA vượt ngưỡng rủi ro cao (>150% target)

**Hành động:**
- Hiển thị khuyến nghị trong chế độ "review thủ công" với 2-3 phương án an toàn
- Không auto execute bất kỳ hành động nào
- Luôn hiển thị mức độ tự tin (high/medium/low) cho từng khuyến nghị

**User options:**
- Accept / Reject / Modify mỗi khuyến nghị
- Cho phép ghi nhận phản hồi để cải thiện vòng lặp học
- Thông báo: "Đây là gợi ý, không thay thế quyết định cuối cùng của người vận hành."

---

## 7. Success Metrics

### 7.1 Primary Metrics

| Metric | Ngưỡng thành công | Timeframe |
|---|---|---|
| Tỉ lệ người dùng áp dụng ít nhất 2 khuyến nghị AI/tuần | >=60% tài khoản pilot | 3 tuần liên tiếp |
| Tỉ lệ tài khoản quay lại tuần 4 | >=50% vẫn còn áp dụng >=1 khuyến nghị/tuần | Tuần 4 |
| Median CPA giảm | >=10% ở nhóm áp dụng khuyến nghị đều đặn | 4 tuần |

### 7.2 PMF Signals

| Signal | Ngưỡng |
|---|---|
| Sean Ellis Test | >=40% trả lời "rất thất vọng" nếu không còn sản phẩm |
| D30 Retention | >=20% |
| Aha Moment | >=60% user đạt trong 7 ngày đầu |

### 7.3 Vanity Metrics (Tránh)

- Số lượt đăng nhập (vì có thể login mà không làm gì)
- Tổng số khuyến nghị được hiển thị (vì có thể xem mà không áp dụng)
- Số shop đăng ký pilot (vì đăng ký không có nghĩa là sử dụng)

---

## 8. Hypotheses

### Hypothesis 1 — Chấm điểm khả năng mua

> "Chúng tôi tin rằng hệ thống xếp hạng khả năng mua theo 3 mức (cao/trung bình/thấp) sẽ giúp chủ shop thời trang nữ online ưu tiên ngân sách đúng ad set và giảm CPA 10-20% trong 4-8 tuần.
> Chúng tôi sẽ biết mình đúng khi thấy tỉ lệ áp dụng khuyến nghị đạt >=60% và median CPA giảm >=10% ở nhóm dùng đều đặn."

- **Riskiest Assumption:** Chủ shop/marketer sẽ hành động theo xếp hạng khả năng mua thay vì dựa hoàn toàn vào kinh nghiệm cá nhân
- **Cách test cheapest:** A/B test giữa nhóm shop dùng xếp hạng vs nhóm không dùng, đo CPA sau 4 tuần

### Hypothesis 2 — Khuyến nghị tối ưu ngân sách

> "Chúng tôi tin rằng khuyến nghị tối ưu ngân sách theo mức ưu tiên + giải thích lý do rõ ràng sẽ giúp chủ shop thời trang nữ online giảm tốc độ burn rate ngân sách trong các đợt biến động chiến dịch.
> Chúng tôi sẽ biết mình đúng khi thấy tỉ lệ chấp nhận khuyến nghị đạt >=50% và không có trường hợp burn rate tăng >30% trong 48h mà không có cảnh báo."

- **Riskiest Assumption:** Người dùng sẽ tin tưởng vào khuyến nghị có giải thích lý do rõ ràng hơn là khuyến nghị không có giải thích
- **Cách test cheapest:** So sánh tỉ lệ chấp nhận giữa nhóm nhận khuyến nghị có giải thích vs không có giải thích

### Hypothesis 3 — Kết nối dữ liệu

> "Chúng tôi tin rằng dữ liệu cơ bản từ Meta/Google Ads + dữ liệu đơn hàng đủ để model dự đoán ý định mua có ích ngay từ tháng đầu.
> Chúng tôi sẽ biết mình đúng khi thấy >=80% shop pilot kết nối thành công dữ liệu trong 48h và model có thể đưa ra xếp hạng cho >=90% ad sets."

- **Riskiest Assumption:** Dữ liệu từ Meta/Google Ads API đủ chất lượng và đầy đủ để model hoạt động
- **Cách test cheapest:** Pilot với 5-10 shop, đo tỉ lệ kết nối thành công và chất lượng dữ liệu đầu ra

---

## 9. Dependencies & Constraints

| Loại | Chi tiết |
|---|---|
| **API / Service cần tích hợp** | Meta Ads API (cần Meta Developer account); Google Ads API (cần Google Cloud project) |
| **Timeline constraint** | MVP trong 2 tuần đầu |
| **Budget constraint** | Không có budget cho giai đoạn này |
| **Legal / Compliance** | Tuân thủ Meta và Google API terms of service; bảo mật dữ liệu khách hàng |

---

## 10. Aha Moment

Trong 7 ngày đầu, người dùng nhận ra giá trị khi:
- Ít nhất 1 khuyến nghị AI được áp dụng và tạo cải thiện đo được (CPA giảm hoặc ROAS tăng) trong vòng 48 giờ
- Người dùng quay lại để xem và xử lý khuyến nghị mới thay vì tối ưu hoàn toàn thủ công

---

## 11. Kill Question

Nếu cắt thêm 1 tính năng trong In-Scope, khách hàng còn nhận được giá trị cốt lõi không?

- Nếu cắt dashboard (nhưng vẫn giữ scoring + khuyến nghị hành động): **Còn giá trị cốt lõi**
- Nếu cắt chấm điểm khả năng mua hoặc cắt khuyến nghị tối ưu ngân sách: **Mất giá trị cốt lõi**

**Kết luận:** Lõi MVP bắt buộc phải giữ là (1) chấm điểm khả năng mua, (2) khuyến nghị tối ưu ngân sách, (3) dữ liệu đầu vào tối thiểu để hai phần trên hoạt động ổn định.

---

## 12. Open Questions for Next Phase

1. **MVP trong 2 tuần đầu nên đóng gói quanh "predict + recommend" hay kèm luôn "auto-action"?**
   - Đã chọn: predict + recommend (không auto-action để giữ human-in-loop)

2. **Cần bộ event dữ liệu tối thiểu nào để model dự đoán ý định mua có ích ngay từ tháng đầu?**
   - Đã xác định: impressions, clicks, CTR, CPC, CPA, ROAS, frequency từ Meta/Google + dữ liệu đơn hàng

3. **Gói pricing nào tối ưu cho tỉ lệ chốt và tỉ lệ duy trì 3 tháng?**
   - Cần pilot để validate (1.2-3.0 triệu VND/tháng là assumption từ V2)

