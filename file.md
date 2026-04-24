# 1. Vấn đề cốt lõi (The Problem)
- Các shop thời trang online (doanh thu 200M - 1B/tháng) thường lãng phí 20-30% ngân sách quảng cáo do:
- Độ trễ quyết định: Phản ứng chậm trước các biến động của Facebook/Google Ads (thường mất 24-48h mới nhận ra ad set đang lỗ).
- Tối ưu cảm tính: Thiếu năng lực phân tích dữ liệu chuyên sâu để biết nhóm khách hàng nào thực sự có khả năng chốt đơn.

# 2. Giải pháp (The Solution)
- Phát triển một AI Decision Copilot đóng vai trò như một trợ lý ảo ra quyết định, tập trung vào 3 tác vụ: Dự báo khả năng mua - Đề xuất ngân sách - Gợi ý nội dung.
 
# 3. Phạm vi sản phẩm tối thiểu (MVP Scope)

- Mục tiêu của MVP là kiểm chứng xem liệu AI có giúp giảm CPA (chi phí trên mỗi đơn hàng) thông qua các khuyến nghị hay không.

## CÁC TÍNH NĂNG THỰC HIỆN (In-Scope)

- Kết nối dữ liệu: Tích hợp API cơ bản từ Meta Ads và dữ liệu đơn hàng (CRM/Web).

- Chấm điểm khách hàng (Scoring): Phân loại tệp khách theo 3 mức độ ưu tiên mua hàng (High/Medium/Low).

- Khuyến nghị hành động: Đưa ra 3 lệnh tối ưu mỗi ngày (Ví dụ: "Tắt Ad set A vì CPA vượt ngưỡng 20%" hoặc "Tăng 10% ngân sách cho nhóm B").

Gợi ý nội dung: AI đề xuất 2-3 mẫu thông điệp phù hợp với từng phân khúc khách hàng.

## PHẦN TẠM HOÃN (Out-of-Scope)
Chưa triển khai tự động hóa hoàn toàn (AI tự chạy Ads không cần người duyệt).

Chưa tích hợp đa kênh (TikTok, Shopee, Email).

Chưa xây dựng ứng dụng di động (Mobile App).

# 4. Chỉ số đo lường thành công (Success Metrics)
CPA (Cost Per Acquisition): Giảm 10-20% sau 4-8 tuần sử dụng.

Thời gian tối ưu: Giảm từ 2 giờ đọc số xuống còn 15 phút duyệt khuyến nghị.

Tỷ lệ chấp nhận (Acceptance Rate): >60% khuyến nghị của AI được người dùng thực thi.

Thông điệp chính: MVP không thay thế con người, mà cung cấp "não bộ phân tích" để các shop nhỏ ra quyết định nhanh hơn, chính xác hơn trong khung giờ vàng của chiến dịch.

#Fallback UX. Nếu sản phẩm của em là AI tư vấn luật, việc 'xin lỗi và chuyển sang người thật' có làm khách hàng cảm thấy sản phẩm của mình yếu kém không?

 Fallback UX (khi AI không chắc chắn thì xin lỗi và chuyển sang con người). Em đang băn khoăn về khía cạnh perception của người dùng:
Nếu sản phẩm của em là AI tư vấn luật/thuế, việc “xin lỗi + chuyển ngay sang chuyên gia thật” (ví dụ: “SCAI không chắc chắn về trường hợp này, em chuyển anh/chị sang luật sư thuế để hỗ trợ chính xác hơn nhé?”) có làm khách hàng cảm thấy sản phẩm yếu kém, thiếu tin cậy không?