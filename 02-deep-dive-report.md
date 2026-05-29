# 02 — Deep-Dive Report & Evaluation (Nhóm)

**Tên nhóm:** Doraemon No u ta 

|   Họ và tên           |  MSSV 
|  
|   Nguyễn Đức Kiên Trung |  2A202600
|   Vũ Quang Bảo -        |  2A202600610
|   Mai Đức Vinh          |  2A202600587
|

---

## Bài toán được chọn để Deep-Dive

**Vinmec — AI tự động xác nhận lịch hẹn bệnh nhân để giảm no-show**

### Lý do chọn bài toán này:
- **Metric rõ ràng và đo được:** Tỉ lệ no-show hiện tại (~18%) là con số có thể theo dõi trực tiếp — dễ đánh giá kết quả sau khi triển khai.
- **Ranh giới an toàn kiểm soát được:** AI chỉ gửi tin nhắn xác nhận và đọc phản hồi, không chạm vào quyết định y tế hay hủy lịch tự động.
- **Tác động ngay lập tức:** Giải phóng 3 lễ tân khỏi 20+ giờ gọi điện/ngày — ROI rõ ràng ngay từ tuần đầu triển khai.

### Lý do loại bỏ các card khác:
- **Card #1 (VinFast Warranty):** Cần tích hợp sâu với 3 hệ thống backend (CRM, ERP, service history) — dependency kỹ thuật phức tạp, timeline dài hơn.
- **Card #2 (Xanh SM Driver Coaching):** Nội dung coaching liên quan đến đánh giá hành vi người — cần cẩn thận về bias và tranh chấp lao động, cần gom thêm policy HR trước khi triển khai AI.

---

# 🏗️ Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow

Quy trình xác nhận lịch hẹn thủ công hiện tại tại Vinmec:

```
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│ Bước 1           │     │ Bước 2           │     │ Bước 3           │     │ Bước 4           │
│ Xuất danh sách   │     │ Lễ tân gọi điện  │     │ Ghi nhận phản    │     │ Cập nhật lịch    │
│ lịch hẹn từ HIS  │ ──→ │ từng bệnh nhân   │ ──→ │ hồi: xác nhận /  │ ──→ │ bác sĩ nếu có   │
│                  │     │ để nhắc nhở      │     │ hủy / đổi giờ   │     │ thay đổi         │
│ Ai: Lễ tân       │     │ Ai: Lễ tân       │     │ Ai: Lễ tân       │     │ Ai: Lễ tân       │
│ ⏱ 10 phút/ngày   │     │ ⏱ 4 phút/cuộc 🔴 │     │ ⏱ 1 phút/ca 🔴   │     │ ⏱ 2 phút/ca      │
│ Tool: HIS        │     │ Tool: Điện thoại │     │ Tool: Excel      │     │ Tool: HIS        │
│ Out: File danh   │     │ Out: Trạng thái  │     │ Out: Ghi chú thủ │     │ Out: Lịch cập    │
│ sách 300 bệnh    │     │ xác nhận miệng   │     │ công             │     │ nhật             │
│ nhân             │     │                  │     │                  │     │                  │
└──────────────────┘     └──────────────────┘     └──────────────────┘     └──────────────────┘

🔴 = Bottleneck (chiếm >90% tổng thời gian)
🔄 Handoff: Bước 1→2 (lễ tân nhận file từ HIS), Bước 3→4 (ghi chú Excel → cập nhật HIS thủ công)

⏱ Tổng thời gian: ~(4 + 1) phút × 300 bệnh nhân = 25 giờ/ngày → cần 3 lễ tân làm full-time chỉ để gọi điện xác nhận.
```

---

## 3.2. Problem Statement (6-field)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Lễ tân bệnh viện (3 nhân viên) tại bộ phận Đặt lịch & Tiếp nhận của Vinmec. Làm việc 7:00–17:00, phụ trách đồng thời cả tiếp đón tại quầy và gọi xác nhận lịch. |
| **2. Current Workflow** | Mỗi chiều, lễ tân xuất danh sách lịch hẹn ngày hôm sau từ HIS (~300 ca). Từ 14:00–17:00, 3 lễ tân thay nhau gọi điện thoại từng bệnh nhân để nhắc lịch. Ghi nhận kết quả vào Excel (xác nhận / hủy / cần đổi giờ). Cuối ca cập nhật thay đổi lên HIS và thông báo cho bác sĩ liên quan. Toàn bộ 5 bước thủ công, không có hệ thống tự động. |
| **3. Bottleneck** | Bước gọi điện (Bước 2): 4 phút/cuộc × 300 bệnh nhân = 1.200 phút = 20 giờ/ngày. Ngoài ra, ghi chép vào Excel rời rạc dẫn đến sai sót khi cập nhật ngược lên HIS (~5 lỗi cập nhật/ngày). Bác sĩ không được thông báo kịp thời khi có hủy lịch đột xuất → lãng phí slot khám. |
| **4. Business Impact** | Tỉ lệ no-show hiện tại ~18% (~54 ca/ngày). Mỗi slot khám trung bình 500.000 VND → lãng phí ~27 triệu VND doanh thu tiềm năng/ngày. 3 lễ tân full-time bị chiếm toàn bộ thời gian chiều → không thể hỗ trợ tiếp đón tại quầy, gây ùn ứ 15:00–17:00. |
| **5. Success Metric** | 1. Giảm tỉ lệ no-show từ 18% → dưới 6% trong 60 ngày sau triển khai. 2. Giải phóng ≥ 18 giờ lễ tân/ngày (từ gọi điện sang tác vụ tiếp đón). 3. Tỉ lệ phản hồi xác nhận lịch của bệnh nhân qua tin nhắn đạt ≥ 80%. |
| **6. Operational Boundary** | **AI được phép:** Gửi tin nhắn Zalo/SMS xác nhận lịch hẹn dạng [DRAFT_ONLY], đọc và phân loại phản hồi của bệnh nhân (xác nhận / cần hỗ trợ), tự động cập nhật trạng thái "đã xác nhận" vào HIS. **TUYỆT ĐỐI CẤM:** AI không được tự hủy lịch hẹn; không được tự đổi giờ/bác sĩ; không được trả lời câu hỏi y tế (triệu chứng, thuốc, chẩn đoán). **Bắt buộc HITL:** Mọi yêu cầu hủy lịch hoặc đổi lịch → escalate ngay cho lễ tân xử lý. |

---

## 3.3. Future-State Flow & AI Fit

**AI Fit:** `[x] LLM Feature`

**Lý do KHÔNG chọn Agentic Loop:** Quy trình có điểm dừng bắt buộc (lễ tân duyệt trước khi gửi, lễ tân xử lý hủy lịch). Nếu để AI tự chạy vòng lặp không giám sát, rủi ro hủy nhầm lịch hoặc cập nhật sai HIS ảnh hưởng trực tiếp đến lịch khám của bác sĩ và bệnh nhân.

**Quy trình tương lai (Future-State):**

```
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│ Bước 1           │     │ Bước 2           │     │ Bước 3           │     │ Bước 4           │
│ HIS tự động      │     │ 🔵 AI soạn &     │     │ 🟢 Lễ tân xem    │     │ 🔵 AI đọc phản   │
│ xuất danh sách   │ ──→ │ gửi tin nhắn     │ ──→ │ lướt qua batch   │ ──→ │ hồi bệnh nhân    │
│ lịch hẹn ngày    │     │ xác nhận hàng    │     │ & click duyệt    │     │ & phân loại tự   │
│ mai lúc 14:00    │     │ loạt qua Zalo    │     │ gửi (< 5 phút)   │     │ động             │
└──────────────────┘     └──────────────────┘     └──────────────────┘     └──────────────────┘
                                                                                     │
                          ┌──────────────────────────────────────────────────────────┤
                          │                                                          │
                          ▼                                                          ▼
               ┌──────────────────┐                                       ┌──────────────────┐
               │ Bước 5a          │                                       │ Bước 5b          │
               │ 🔵 Phản hồi      │                                       │ 🟢 Phản hồi      │
               │ "Xác nhận" →     │                                       │ "Hủy/Đổi lịch"  │
               │ AI tự cập nhật   │                                       │ → Escalate lễ   │
               │ HIS tự động      │                                       │ tân xử lý trực  │
               └──────────────────┘                                       │ tiếp            │
                                                                          └──────────────────┘
                                                                                     │
                                                                                     ▼
                                                                          ↩️ Fallback:
                                                                          Nếu bệnh nhân không
                                                                          trả lời sau 4 tiếng
                                                                          → Lễ tân gọi điện
                                                                          như quy trình cũ
```

**Tóm tắt thay đổi chính:**

| | Trước | Sau |
|---|---|---|
| Thời gian lễ tân/ngày cho xác nhận | 20 giờ | < 2 giờ (chỉ xử lý exceptions) |
| Hình thức liên hệ | Gọi điện 1-1 | Tin nhắn Zalo tự động hàng loạt |
| Xử lý hủy/đổi lịch | Lễ tân ghi tay Excel | Escalate trực tiếp lên màn hình lễ tân |
| Cập nhật HIS | Thủ công cuối ca | Tự động ngay khi bệnh nhân xác nhận |

---

# 🏁 Phase 5 — EVALUATE

## AI Readiness Checklist

| # | Tiêu chí | Đánh giá |
|---|---------|---------|
| 1 | Có sẵn dữ liệu mẫu/logs sạch để test? | ✅ HIS của Vinmec có lịch sử lịch hẹn + trạng thái no-show đầy đủ, sẵn sàng làm test dataset. |
| 2 | Rủi ro khi AI sai có nằm trong tầm kiểm soát? | ✅ Có HITL (lễ tân duyệt batch trước khi gửi) và Fallback (gọi điện thủ công nếu bệnh nhân không phản hồi). AI không được tự hủy lịch — rủi ro sai sót giới hạn ở mức "tin nhắn sai ngữ pháp", không gây hại lâm sàng. |
| 3 | Stakeholders sẵn sàng thay đổi quy trình? | ✅ Lễ tân được lợi trực tiếp (bớt công việc lặp lại). Bác sĩ được lợi (ít no-show hơn, lịch ổn định hơn). Ban quản lý có thêm dữ liệu phân tích tỉ lệ xác nhận. |

---

## Quyết định cuối cùng

**[x] GO — Bắt đầu xây dựng Prototype với scope hẹp**

**Justification:**

> Ba tiêu chí trong AI Readiness Checklist đều đạt. Dữ liệu lịch sử có sẵn trong HIS để huấn luyện và test. Rủi ro được kiểm soát chặt: AI chỉ soạn tin nhắn và đọc phản hồi — lễ tân vẫn giữ quyền duyệt cuối và xử lý mọi trường hợp phức tạp (hủy, đổi lịch, câu hỏi y tế). Fallback rõ ràng: bệnh nhân không trả lời sau 4 tiếng → gọi điện như cũ.
>
> **Ước tính chi phí triển khai (Scope hẹp — 1 cơ sở Vinmec):**
> - API Gemini 2.5 Flash: ~$0.0004/tin nhắn × 300 tin/ngày × 30 ngày ≈ **$3.6/tháng** (không đáng kể).
> - Tích hợp Zalo Business API: ~$50/tháng (flat fee).
> - Chi phí kỹ sư tích hợp HIS: ước tính 2–3 tuần × 1 kỹ sư.
>
> **Lợi ích đo được:**
> - Giải phóng 18 giờ lễ tân/ngày → tương đương 1.5 FTE lễ tân có thể tái phân bổ sang tiếp đón.
> - Nếu no-show giảm từ 18% → 6%: 36 slot khám/ngày được lấp đầy × 500.000 VND = **18 triệu VND doanh thu/ngày** được phục hồi.
> - ROI dương ngay trong tháng đầu vận hành.
>
> **Đề xuất bước tiếp theo:** Pilot tại 1 khoa (Nội tổng quát) trong 2 tuần để đo tỉ lệ phản hồi tin nhắn thực tế trước khi mở rộng toàn viện.
