# Day 17 Submission
**Student:** Nguyễn Thị Tuyết - 2A202600215

**Date:** 24/04/2026

**Product idea:** AI decision copilot giúp chủ shop thời trang nữ online ra quyết định ads nhanh và đúng để giảm CPA 10-20%

---

## 1. MVP Boundary Sheet

**Riskiest Assumption:**
> Chủ shop/marketer team nhỏ sẽ tin tưởng và hành động theo khuyến nghị AI đủ thường xuyên (không chỉ xem cho biết), vì nếu không có hành vi áp dụng thực tế thì sản phẩm không tạo ra kết quả kinh doanh.

**In-Scope** (tối đa 3):
- [ ] Chấm điểm khả năng mua của các nhóm khách theo 3 mức ưu tiên (cao/trung bình/thấp) — test giả định: Khách hàng có thực sự hành động theo xếp hạng này không?
- [ ] Đề xuất 3 hành động tối ưu ngân sách mỗi ngày (tắt/giảm/tăng ad set) có giải thích lý do — test giả định: Người dùng có áp dụng khuyến nghị và tạo ra cải thiện CPA/ROAS không?
- [ ] Kết nối dữ liệu cơ bản từ kênh ads (Meta/Google) và dữ liệu đơn hàng — test giả định: Dữ liệu đủ để model dự đoán ý định mua có ích ngay từ tháng đầu?

**Out-of-Scope:**
- [Tự động chạy ads 100% không cần người duyệt] — lý do bỏ: Quá phức tạp, rủi ro cao, chưa cần thiết cho MVP
- [Tích hợp toàn bộ kênh marketing (TikTok, email, CRM sâu)] — lý do bỏ: Tập trung vào Meta/Google ads trước để validate core hypothesis
- [Mô hình AI tự huấn luyện nâng cao theo thời gian thực] — lý do bỏ: Cần dữ liệu pilot trước, giai đoạn MVP dùng model ổn định
- [Ứng dụng mobile riêng] — lý do bỏ: Ưu tiên web dashboard cho team marketing làm việc trên máy tính
- [Phân tích full-funnel đa chạm phức tạp (attribution nâng cao)] — lý do bỏ: Tập trung vào 24-48h đầu của chiến dịch, không cần attribution phức tạp

**Non-Goals:**
- Không thay thế hoàn toàn đội marketing
- Không xây thành bộ martech all-in-one trong giai đoạn MVP
- Không cam kết tối ưu cho mọi ngành; chỉ tập trung segment shop thời trang nữ
- Không giải quyết bài toán branding dài hạn; chỉ tập trung hiệu quả chuyển đổi ngắn hạn

---

## 2. PRD Skeleton

### Problem Statement
> Chủ shop thời trang nữ online đang lãng phí ngân sách quảng cáo vì không xác định được nhóm khách có ý định mua cao và không tối ưu chiến dịch đủ nhanh khi chỉ số xấu đi — mỗi ngày trễ 12-24h là mất 5-15% ngân sách ads vào nhóm kém chất lượng.

### Target User
> Chủ shop hoặc marketer vận hành ads cho shop thời trang nữ online có team marketing 1-2 người, doanh thu 200M-1B VND/tháng, chạy ads liên tục trên Facebook/Instagram + website để duy trì đơn hàng.

### User Stories
**Story 1:**
> As a chủ shop thời trang nữ online, I want hệ thống xếp hạng nhóm khách có khả năng mua cao trong 7 ngày tới, so that tôi ưu tiên ngân sách đúng ad set và giảm CPA 10-20% trong 4-8 tuần.

**Story 2:**
> As a marketer team nhỏ (1-2 người), I want nhận khuyến nghị hành động tối ưu ngân sách theo mức độ ưu tiên mỗi ngày với giải thích lý do rõ ràng, so that tôi phản ứng kịp trước biến động và bảo vệ ROAS trong 24-48h đầu của chiến dịch.

### AI-Specific

**Model Selection:**
- Model: Mô hình phân loại/xếp hạng ý định mua (purchase propensity scoring) kết hợp mô hình gợi ý nội dung ngắn
- Lý do chọn: Cần ưu tiên tính ổn định, dễ giải thích, cập nhật nhanh theo chu kỳ chiến dịch; không cần mô hình quá nặng ở giai đoạn MVP. Dùng rule-based + ML nhẹ để đảm bảo explainability cho người dùng.
- Trade-offs chấp nhận: Độ chính xác dự đoán không tuyệt đối; cần human-in-loop để validate
- Trade-offs không chấp nhận: Model quá phức tạp không giải thích được lý do khuyến nghị

**Data Requirements:**
- Nguồn: Dữ liệu hiệu suất ads (impressions, clicks, CTR, CPC, CPA, ROAS, frequency) từ Meta/Google Ads API; Dữ liệu đơn hàng (đơn, giá trị đơn, thời điểm mua) từ website/shop; Dữ liệu nội dung (creative/message đang chạy)
- Owner: Shop owner/customer tự kết nối qua OAuth hoặc API key
- Update frequency: Daily refresh cho metrics; Real-time cho alert

**Fallback UX:**
- Chiến lược: Human-in-the-loop
- Trigger: Khi confidence score < 80% hoặc khi CPA vượt ngưỡng rủi ro cao (>150% target)
- Hành động: Hiển thị khuyến nghị trong chế độ "review thủ công" với 2-3 phương án an toàn; không auto execute bất kỳ hành động nào
- User options: User có thể "Accept / Reject / Modify" mỗi khuyến nghị; luôn hiển thị mức độ tự tin (high/medium/low) cho từng khuyến nghị; cho phép ghi nhận phản hồi để cải thiện vòng lặp học

### Success Metrics
- Primary metric: Tỉ lệ người dùng áp dụng ít nhất 2 khuyến nghị AI/tuần trong 3 tuần liên tiếp
- Ngưỡng thành công: >=60% tài khoản pilot đạt primary metric và >=50% đạt outcome signal (CPA giảm >=10%)
- Timeframe đo lường: 4-8 tuần sau launch pilot

### Dependencies & Constraints
- Tích hợp Meta Ads API (cần Meta Developer account)
- Tích hợp Google Ads API (cần Google Cloud project)
- Timeline: MVP trong 2 tuần đầu
- Budget constraint: Không có budget cho giai đoạn này

---

## 3. Hypothesis Table

### Hypothesis 1 (cho tính năng In-Scope #1: Chấm điểm khả năng mua)
> "Chúng tôi tin rằng hệ thống xếp hạng khả năng mua theo 3 mức (cao/trung bình/thấp) sẽ giúp chủ shop thời trang nữ online ưu tiên ngân sách đúng ad set và giảm CPA 10-20% trong 4-8 tuần.
> Chúng tôi sẽ biết mình đúng khi thấy tỉ lệ áp dụng khuyến nghị đạt >=60% và median CPA giảm >=10% ở nhóm dùng đều đặn."

Riskiest assumption: Chủ shop/marketer sẽ hành động theo xếp hạng khả năng mua thay vì dựa hoàn toàn vào kinh nghiệm cá nhân.
Cách test cheapest: A/B test giữa nhóm shop dùng xếp hạng vs nhóm không dùng, đo CPA sau 4 tuần.

### Hypothesis 2 (cho tính năng In-Scope #2: Khuyến nghị tối ưu ngân sách)
> "Chúng tôi tin rằng khuyến nghị tối ưu ngân sách theo mức ưu tiên + giải thích lý do rõ ràng sẽ giúp chủ shop thời trang nữ online giảm tốc độ burn rate ngân sách trong các đợt biến động chiến dịch.
> Chúng tôi sẽ biết mình đúng khi thấy tỉ lệ chấp nhận khuyến nghị đạt >=50% và không có trường hợp burn rate tăng >30% trong 48h mà không có cảnh báo."

Riskiest assumption: Người dùng sẽ tin tưởng vào khuyến nghị có giải thích lý do rõ ràng hơn là khuyến nghị không có giải thích.
Cách test cheapest: So sánh tỉ lệ chấp nhận giữa nhóm nhận khuyến nghị có giải thích vs không có giải thích.

### Hypothesis 3 (cho tính năng In-Scope #3: Kết nối dữ liệu)
> "Chúng tôi tin rằng dữ liệu cơ bản từ Meta/Google Ads + dữ liệu đơn hàng đủ để model dự đoán ý định mua có ích ngay từ tháng đầu.
> Chúng tôi sẽ biết mình đúng khi thấy >=80% shop pilot kết nối thành công dữ liệu trong 48h và model có thể đưa ra xếp hạng cho >=90% ad sets."

Riskiest assumption: Dữ liệu từ Meta/Google Ads API đủ chất lượng và đầy đủ để model hoạt động.
Cách test cheapest: Pilot với 5-10 shop, đo tỉ lệ kết nối thành công và chất lượng dữ liệu đầu ra.

---

## 4. PMF Scorecard

**Aha Moment:**
> Trong 7 ngày đầu, người dùng nhận ra giá trị khi ít nhất 1 khuyến nghị AI được áp dụng và tạo cải thiện đo được (CPA giảm hoặc ROAS tăng) trong vòng 48 giờ — và người dùng quay lại để xem và xử lý khuyến nghị mới thay vì tối ưu hoàn toàn thủ công.

**Actionable Metric:**
> Tỉ lệ người dùng áp dụng ít nhất 2 khuyến nghị AI/tuần trong 3 tuần liên tiếp (không phải: số lượt đăng nhập, số lượt xem dashboard)

**PMF Method:**
> [Sean Ellis Test + Retention Curve + Aha Moment tracking]
> Ngưỡng thành công: >=40% trả lời "rất thất vọng" nếu không còn sản phẩm (Sean Ellis); D30 retention >=20%; >=60% user đạt Aha Moment trong 7 ngày đầu

**Vanity Metrics tôi sẽ không dùng:**
- Số lượt đăng nhập (vì có thể login mà không làm gì)
- Tổng số khuyến nghị được hiển thị (vì có thể xem mà không áp dụng)
- Số shop đăng ký pilot (vì đăng ký không có nghĩa là sử dụng)

**Vanity Metrics tôi sẽ không dùng:**
- [...]

---

## 5. AI Critique Log

**Điểm AI chỉ ra:**
1. [Scope creep] — Action: Accept — Lý do: Dashboard theo dõi KPI không phải là core value, chỉ là nice-to-have. Đã chuyển sang Out-of-Scope.
2. [Fallback UX chưa đủ cụ thể] — Action: Accept — Lý do: Cần xác định rõ confidence threshold và các trigger cụ thể. Đã bổ sung: confidence < 80% hoặc CPA vượt 150% target.
3. [Hypothesis chưa có cách test cheapest rõ ràng] — Action: Accept — Lý do: Cần đưa ra phương pháp test cụ thể cho từng hypothesis. Đã bổ sung A/B test và so sánh tỉ lệ chấp nhận.
4. [PMF method cần rõ ngưỡng] — Action: Accept — Lý do: Cần xác định ngưỡng cụ thể cho từng phương pháp. Đã bổ sung: >=40% Sean Ellis, D30 >=20%, >=60% user đạt Aha Moment.
5. [Market sizing confidence thấp] — Action: Partial — Lý do: Đây là known unknown từ Day 16, cần pilot để validate. Đã note trong Self-assessment.

**Thay đổi lớn nhất giữa Version A và Version B:**
> Từ V2 (Day 16) sang Day 17, đã chuyển đổi từ strategic framework (Market, Needs, Strategy, Moat) sang product execution (MVP, PRD, Hypothesis, PMF). Điểm khác biệt chính: (1) Cắt dashboard khỏi In-Scope vì không phải core value; (2) Bổ sung Fallback UX chi tiết với trigger cụ thể; (3) Xác định Aha Moment bằng hành vi cụ thể thay vì mô tả mơ hồ; (4) Thêm cách test cheapest cho từng hypothesis.

---

## 6. Self-assessment

Mắt xích nào trong [MVP Boundary → PRD → Hypothesis → PMF] bạn đang yếu nhất?
> Market sizing đã rõ logic hơn V1 nhưng vẫn cần dữ liệu pilot để nâng confidence từ low lên medium. Đây là mắt xích yếu nhất vì TAM/SAM/SOM đều dựa trên assumptions chưa được validate.

Open questions bạn muốn giải đáp tiếp:
1. MVP trong 2 tuần đầu nên đóng gói quanh "predict + recommend" hay kèm luôn "auto-action"? → Đã chọn: predict + recommend (không auto-action để giữ human-in-loop)
2. Cần bộ event dữ liệu tối thiểu nào để model dự đoán ý định mua có ích ngay từ tháng đầu? → Đã xác định: impressions, clicks, CTR, CPC, CPA, ROAS, frequency từ Meta/Google + dữ liệu đơn hàng
3. Gói pricing nào tối ưu cho tỉ lệ chốt và tỉ lệ duy trì 3 tháng? → Cần pilot để validate (1.2-3.0 triệu VND/tháng là assumption từ V2)