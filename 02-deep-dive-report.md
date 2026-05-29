# 02 - Báo cáo Phân tích Chuyên sâu (Deep-Dive Report)

## 1. Mô tả bài toán chi tiết
Hệ thống điều phối xe sạc pin cứu hộ lưu động (Mobile EV Charger) cho xe điện gặp sự cố cạn kiệt pin trên đường của Xanh SM. Hệ thống cần phân tích ngôn ngữ tự nhiên từ cuộc gọi/tin nhắn của tài xế và dữ liệu telemetry để ra quyết định điều xe cứu hộ khẩn cấp.

## 2. Quy trình hiện tại (Current-State Workflow Mapping)
1. Tài xế gọi điện lên tổng đài báo xe sắp hết pin khẩn cấp.
2. Tổng đài viên kiểm tra thủ công trạng thái pin qua hệ thống quản lý xe. (Handoff)
3. Tổng đài viên liên hệ bộ phận xe sạc lưu động để kiểm tra lịch trống. (Bottleneck - tốn thời gian liên hệ)
4. Xác nhận lệnh điều xe và gửi thông báo thủ công tới tài xế xe cứu hộ.

*Tổng thời gian vận hành trung bình: 15-20 phút/lượt.*

## 3. Problem Statement & Metrics
- **Actor:** Nhân viên trực điều phối cứu hộ của Xanh SM.
- **Bottleneck:** Việc đánh giá mức độ khẩn cấp dựa trên mô tả cảm tính của tài xế và kiểm tra chéo vị trí xe sạc còn trống tốn nhiều thời gian liên lạc điện thoại.
- **Ranh giới an toàn nghiêm ngặt (Operational Boundaries):**
  - Chỉ kích hoạt lệnh điều xe sạc cứu hộ (`dispatch_mobile_charger`) nếu dung lượng pin thực tế của xe bị sự cố (SoC) dưới 5%.
  - Mọi đề xuất điều xe cứu hộ từ AI phải ở trạng thái nháp (`draft_only`) để con người phê duyệt, không được tự ý gửi lệnh chính thức tới xe cứu hộ.
- **Success Metric:** Rút ngắn thời gian xác nhận và ra quyết định điều phối từ 15 phút xuống dưới 2 phút.

## 4. Quy trình tương lai (Future-State Workflow with AI)
1. Tin nhắn/Yêu cầu từ tài xế được hệ thống tiếp nhận.
2. AI phân tích nội dung cuộc gọi kết hợp kiểm tra dữ liệu Telemetry của xe (nếu SoC < 5%). (AI Step)
3. AI tự động lập lệnh điều xe sạc ở trạng thái `draft_only` và gợi ý lộ trình tối ưu.
4. Tổng đài viên kiểm tra thông tin trên màn hình điều khiển và bấm "Phê duyệt". (Human Step - HITL)
5. *Fallback:* Nếu hệ thống AI không thể xác định chính xác SoC của xe, hệ thống sẽ tự động chuyển cuộc gọi đến điều phối viên để xử lý thủ công theo quy trình truyền thống.