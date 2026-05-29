# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1  | VinFast                         | Lặp lại (Repetitive)                  | Tự động hóa kiểm tra ngoại quan sản phẩm: Sử dụng thị giác máy tính (Computer Vision) để phát hiện các lỗi bề mặt (sơn, mối hàn, lắp ráp) trên dây chuyền sản xuất thay vì để công nhân kiểm tra bằng mắt thường liên tục trong nhiều giờ.                           |
| 2  | Xanh SM                         | Pain từ người khác (Stakeholder Pain) | Tối ưu hóa điểm đón khách thực tế: Sử dụng thuật toán học máy phân tích dữ liệu lịch sử để gợi ý điểm đón/trả khách tối ưu cho tài xế và khách hàng, khắc phục tình trạng sai lệch định vị GPS khiến tài xế khó tìm khách.                                           |
| 3  | Vinhomes                        | Tốn thời gian (Time-consuming)        | Xử lý và phân loại phản ánh của cư dân: Ứng dụng xử lý ngôn ngữ tự nhiên (NLP/LLM) để tự động đọc, phân nhóm mức độ khẩn cấp và dự thảo câu trả lời cho hàng trăm khiếu nại gửi về Ban Quản lý hằng ngày trên app cư dân.                                            |
| 4  | Vinmec                          | AI có thể tốt hơn (AI-upgrade)        | Phân luồng và điều phối bệnh nhân thông minh: Áp dụng mô hình AI phân tích triệu chứng ban đầu để gợi ý chuyên khoa phù hợp và hỗ trợ bác sĩ chuẩn bị trước hồ sơ bệnh án, rút ngắn thời gian chờ đợi và nâng cao trải nghiệm khám chữa bệnh.                        |
| 5  | Vinpearl                        | AI có thể tốt hơn (AI-upgrade)        | Cá nhân hóa lịch trình trải nghiệm nghỉ dưỡng: Sử dụng hệ thống khuyến nghị (Recommendation System) dựa trên dữ liệu hành vi lịch sử để tự động thiết kế gói dịch vụ (phòng, spa, khu vui chơi) tối ưu riêng cho từng nhóm khách hàng thay vì các gói combo cố định. |



---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #01                                      │
│                                                             │
│ Bài toán (1 câu): Tối ưu hóa điểm đón khách thực tế để giảm  │
│ khoảng cách và thời gian tài xế tìm khách do định vị GPS lệch.│
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                              │
│ Ai đang đau (Actor)? Tài xế Xanh SM và Khách hàng đặt xe.   │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                         │
│   1. Khách đặt xe ──> 2. GPS app lấy vị trí lệch ──>         │
│   3. Tài xế di chuyển theo app ──> 4. Khách/Tài xế gọi điện  │
│   tìm nhau ──> 5. Tài xế quay đầu/di chuyển lại để đón.      │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 4 & 5 (⏱ 5-7 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3 (Gợi ý các │
│ "điểm đón thông minh" thuận tiện gần khách hàng nhất).      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   - Giảm thời gian tài xế tìm gặp khách: 5-7 min ──> under 2 min│
│   - Giảm tỷ lệ hủy chuyến do không tìm thấy nhau: 8% ──> under 3%│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Machine│
│ Learning (Clustering/GPS Routing)                           │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #02                                      │
│                                                             │
│ Bài toán (1 câu): Tự động phân loại, gán nhãn khẩn cấp và   │
│ dự thảo phản hồi cho ý kiến cư dân gửi về Ban quản lý (BQL). │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên điều phối thuộc BQL Vinhomes.│
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                         │
│   1. Nhận phản ánh trên app ──> 2. Đọc & Phân loại thủ công │
│   ──> 3. Chuyển tiếp phòng ban xử lý ──> 4. Soạn thư trả lời │
│   gửi lại cư dân.                                           │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 4 (⏱ 12 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (Phân loại tự  │
│ động bằng NLP) và Bước 4 (Dự thảo phản hồi tự động bằng LLM)│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   - Giảm thời gian xử lý và soạn phản hồi: 12 min ──> under 2 min│
│   - Tăng tỷ lệ phản hồi đúng hạn trong 1 giờ: 65% ──> over 95%  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #03                                      │
│                                                             │
│ Bài toán (1 câu): Nhận diện lỗi ngoại quan bề mặt sơn và sai │
│ lệch mối nối vỏ xe bằng AI Camera tại xưởng lắp ráp VinFast. │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Kỹ thuật viên kiểm định chất lượng (QA).│
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                         │
│   1. Xe vào trạm kiểm tra ──> 2. Công nhân đi vòng quanh quét│
│   mắt tìm vết trầy ──> 3. Đo khe hở bằng thước ──> 4. Ghi log│
│   lỗi thủ công vào máy tính.                                │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 8-10 phút/xe)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3 (Chụp quét │
│ đa góc độ bằng Camera và tự động phát hiện, đo sai lệch).   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   - Thời gian kiểm tra ngoại quan mỗi xe: 10 min ──> under 1 min│
│   - Tỷ lệ bỏ sót lỗi ngoại quan lọt xưởng: 5% ──> under 0.5%    │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Computer│
│ Vision (Anomaly Detection/Object Detection)                 │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---