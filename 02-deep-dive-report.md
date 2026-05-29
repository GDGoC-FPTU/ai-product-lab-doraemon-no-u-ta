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

Quy trình xác nhận lịch hẹn thủ công hiện tại tại Vinmec (thực hiện mỗi ngày làm việc):

```
[16:00 hôm trước]                               [7:30 – 11:30 hôm sau]
        │                                                  │
        ▼                                                  ▼
┌──────────────────┐     ┌──────────────────┐     ┌────────────────────────────────────────────────────────┐
│ Bước 1           │     │ Bước 2           │     │ Bước 3 — Gọi điện xác nhận  🔴 BOTTLENECK             │
│ Xuất & lọc danh  │     │ Chia danh sách   │     │                                                        │
│ sách lịch hẹn    │ ──→ │ cho 3 lễ tân     │ ──→ │  Lần 1: Gọi từng BN (~3 phút/cuộc)                   │
│ ngày mai từ HIS  │     │ (~100 ca/người)  │     │     ├─ Bắt máy → xác nhận → ghi "XN" vào Excel       │
│                  │ 🔄  │ Ghi tay phân     │ 🔄  │     └─ Không bắt máy → ghi chú, chờ 30 phút          │
│ Ai: Lễ tân       │     │ công ai gọi ai   │     │                                                        │
│ ⏱ 15 phút/ngày   │     │ Ai: Lễ tân       │     │  Lần 2 (nếu không bắt máy lần 1):                    │
│ Tool: HIS        │     │ ⏱ 10 phút/ngày   │     │     ├─ Bắt máy → xác nhận / hủy / đổi giờ            │
│ Out: File Excel  │     │ Tool: Excel      │     │     └─ Vẫn không bắt → ghi "KLL" (không liên lạc)    │
│ 300 bệnh nhân    │     │ Out: 3 danh sách │     │                                                        │
└──────────────────┘     └──────────────────┘     │ Ai: 3 Lễ tân song song                                │
                                                  │ ⏱ ~3–5 phút/BN × 300 ca = 15–25 giờ/ngày              │
                                                  │ Tool: Điện thoại bàn + Excel riêng từng người          │
                                                  │ Out: 3 file Excel rời nhau, chưa tổng hợp              │
                                                  └────────────────────────────────────────────────────────┘
                                                                          │
                                                                          ▼ 🔄 Handoff
                                         ┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
                                         │ Bước 6           │     │ Bước 5           │     │ Bước 4           │
                                         │ Xử lý no-show    │     │ Cập nhật HIS +   │     │ Tổng hợp 3 file  │
                                         │ thực tế trong    │ ←── │ thông báo bác sĩ │ ←── │ Excel thành 1    │
                                         │ ngày: gọi thêm  │     │ nếu có hủy/đổi   │     │ bảng chung       │
                                         │ 1 lần, chờ 15'  │     │                  │ 🔄  │                  │
                                         │ Ai: Lễ tân       │     │ Ai: Lễ tân       │     │ Ai: Lễ tân       │
                                         │ ⏱ ~2 phút/ca 🔴  │     │ ⏱ ~2 phút/thay  │     │ ⏱ 20 phút/ngày   │
                                         │ Tool: Điện thoại │     │ đổi × N ca 🔴    │     │ Tool: Excel      │
                                         │ Out: Log no-show │     │ Tool: HIS        │     │ Out: Báo cáo     │
                                         │ báo bác sĩ chờ  │     │ Out: HIS updated │     │ tổng xác nhận    │
                                         └──────────────────┘     └──────────────────┘     └──────────────────┘

🔴 = Bottleneck  |  🔄 = Handoff (điểm chuyển giao dữ liệu giữa người / hệ thống)

Tổng cộng: 6 bước, hoàn toàn thủ công, phân tán qua 3 nhân viên và 2 hệ thống (điện thoại + Excel + HIS).
⏱ Tổng thời gian xử lý: ~25–30 giờ/ngày → cần 3 lễ tân full-time chỉ để gọi điện, chưa tính tiếp đón quầy.
```

---

## 3.2. Problem Statement (6-field)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Lễ tân bệnh viện (3 nhân viên) tại bộ phận Đặt lịch & Tiếp nhận của Vinmec. Làm việc 7:00–17:00, phụ trách đồng thời cả tiếp đón tại quầy và gọi xác nhận lịch. |
| **2. Current Workflow** | 6 bước thủ công hoàn toàn: (1) Lễ tân xuất danh sách ~300 ca từ HIS lúc 16:00 hôm trước. (2) Chia tay danh sách cho 3 lễ tân (~100 ca/người). (3) Mỗi lễ tân gọi điện từng bệnh nhân 7:30–11:30, gọi đến 2 lần nếu không bắt máy, ghi kết quả vào 3 file Excel riêng. (4) Tổng hợp 3 file thành 1 bảng chung. (5) Cập nhật từng thay đổi (hủy/đổi) vào HIS và thông báo bác sĩ. (6) Trong ngày nếu bệnh nhân no-show vẫn phải gọi thêm 1 lần. Không có bước nào tự động. |
| **3. Bottleneck** | Bước 3 (gọi điện): 3–5 phút/cuộc × 300 ca = tối đa 25 giờ/ngày — chiếm >85% tổng thời gian. Bước 5 (cập nhật HIS): nhập tay từ Excel sang HIS dễ sai sót (~5 lỗi/ngày), dữ liệu trễ khiến bác sĩ không biết slot nào vừa bị hủy. Bước 4 (tổng hợp Excel): 3 file rời nhau dễ gây xung đột, mất 20 phút/ngày để merge. |
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
