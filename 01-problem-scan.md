# 01 — Problem Scan & Quick Problem Cards (Cá nhân)
 Vũ Quang Bảo      |  2A202600610
---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội

Sử dụng **4 Lenses** để quét qua vận hành của các công ty thành viên Vingroup.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **VinFast** | Lặp lại | Nhân viên phòng Warranty xử lý thủ công 200+ yêu cầu bảo hành/ngày — mỗi yêu cầu phải kiểm tra đủ 4 điều kiện (còn hạn, đúng mã lỗi, chưa sửa ngoài, đúng đại lý ủy quyền), mất ~8 phút/yêu cầu. |
| 2 | **Xanh SM** | Tốn thời gian | Đội vận hành phải nghe lại recording và gọi điện nhắc thủ công 150+ tài xế/tuần có hành vi lái nguy hiểm (phanh gấp, tăng tốc đột ngột), mỗi cuộc gọi tốn 5–7 phút. |
| 3 | **Vinhomes** | AI-upgrade | Nhân viên kinh doanh phải tư vấn thủ công 50+ khách hàng tiềm năng/ngày qua Zalo/điện thoại về giá, chính sách thanh toán, tiến độ dự án — trả lời lặp đi lặp lại những câu hỏi giống nhau. |
| 4 | **Vinmec** | Lặp lại | Lễ tân bệnh viện gọi điện xác nhận thủ công lịch hẹn của 300+ bệnh nhân/ngày để giảm no-show, mỗi cuộc gọi mất 3–5 phút, chiếm toàn bộ thời gian của 3 lễ tân. |
| 5 | **Vinpearl** | Pain từ người khác | Nhân viên phục vụ ghi nhận feedback khách hàng thủ công vào cuối buổi tham quan — dữ liệu lộn xộn, không đồng nhất, ban quản lý không thể phân tích được pattern khiếu nại theo khu vực hay mùa. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3: **#1 (VinFast Warranty), #2 (Xanh SM Driver Coaching), #4 (Vinmec Appointment)**

---

## Card #1 — VinFast: AI kiểm tra điều kiện bảo hành tự động

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Nhân viên Warranty VinFast kiểm tra thủ công      │
│ 200+ yêu cầu bảo hành/ngày — 4 điều kiện/yêu cầu.          │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau? Nhân viên phòng Warranty (quá tải, dễ bỏ sót   │
│ điều kiện); khách hàng chờ duyệt 2–3 ngày.                  │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Khách gửi yêu cầu bảo hành qua App/Hotline            │
│   ──> 2. Nhân viên mở hệ thống tra cứu ngày mua + VIN       │
│   ──> 3. Đối chiếu mã lỗi với danh sách lỗi được bảo hành  │
│   ──> 4. Kiểm tra lịch sử sửa chữa ngoài hệ thống          │
│   ──> 5. Phê duyệt/từ chối + soạn email thông báo           │
│                                                             │
│ Bước nào tốn nhất? Bước 2–4 (⏱ 6 phút/yêu cầu)             │
│ AI có thể nhảy vào ở bước nào? Bước 2–4:                    │
│ AI tra cứu song song 3 hệ thống → trả về kết quả            │
│ pass/fail từng điều kiện → nhân viên chỉ cần click duyệt    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   Giảm thời gian xử lý từ 8 phút ──> dưới 90 giây/yêu cầu  │
│   Tỉ lệ duyệt đúng điều kiện đạt ≥ 99% (không bỏ sót)      │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule + LLM  [ ] Agent   │
└─────────────────────────────────────────────────────────────┘
```

---

## Card #2 — Xanh SM: AI phân tích hành vi lái và soạn thông báo coaching

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Đội vận hành nghe recording + gọi điện nhắc thủ   │
│ công 150+ tài xế/tuần có hành vi lái nguy hiểm.             │
│ Công ty thành viên: [x] Xanh SM                             │
│                                                             │
│ Ai đang đau? Đội vận hành (mất cả ngày chỉ để gọi điện);   │
│ tài xế nhận phản hồi chậm, không kịp điều chỉnh.            │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Hệ thống telematics xuất báo cáo lái xe hằng ngày      │
│   ──> 2. Nhân viên đọc dữ liệu, xác định tài xế vi phạm    │
│   ──> 3. Nghe recording để xác nhận tình huống cụ thể       │
│   ──> 4. Gọi điện/nhắn tin giải thích và nhắc nhở tài xế   │
│                                                             │
│ Bước nào tốn nhất? Bước 3–4 (⏱ 6 phút/tài xế × 150 người)  │
│ AI có thể nhảy vào ở bước nào? Bước 2–4:                    │
│ AI phân loại mức độ vi phạm → soạn tin nhắn coaching cá     │
│ nhân hóa → nhân viên duyệt và gửi                           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   Giảm thời gian xử lý từ 15 giờ/tuần ──> dưới 2 giờ/tuần  │
│   Tỉ lệ tài xế nhận phản hồi trong ngày tăng từ 40% → 95%  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

## Card #3 — Vinmec: AI gọi xác nhận lịch hẹn tự động

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Lễ tân Vinmec gọi điện xác nhận thủ công lịch    │
│ hẹn của 300+ bệnh nhân/ngày để giảm tỉ lệ no-show.         │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Lễ tân (3 người chiếm toàn bộ thời gian chỉ    │
│ để gọi điện); bác sĩ bị lãng phí slot khi bệnh nhân no-show.│
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Hệ thống HIS xuất danh sách lịch hẹn ngày mai          │
│   ──> 2. Lễ tân gọi điện từng bệnh nhân xác nhận            │
│   ──> 3. Ghi nhận: xác nhận / hủy / cần đổi giờ            │
│   ──> 4. Cập nhật lại lịch bác sĩ nếu có thay đổi          │
│                                                             │
│ Bước nào tốn nhất? Bước 2–3 (⏱ 4 phút/cuộc gọi × 300 ca)   │
│ AI có thể nhảy vào ở bước nào? Bước 2–3:                    │
│ AI nhắn tin/gọi tự động xác nhận → bệnh nhân reply →        │
│ hệ thống tự cập nhật, chỉ escalate khi cần đổi lịch         │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   Giảm tỉ lệ no-show từ 18% ──> dưới 6%                    │
│   Giải phóng 18 giờ/ngày của lễ tân sang tác vụ có giá trị  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```
