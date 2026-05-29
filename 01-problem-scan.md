# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

## 🔍 Phase 1 — SCAN

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | Xanh SM | Pain từ người khác (Stakeholder Pain) | Hệ thống định vị điểm đón khách lệch khiến tài xế tốn thời gian di chuyển tìm kiếm khách hàng thực tế. |
| 2 | Vinhomes | Tốn thời gian (Time-consuming) | Nhân viên Ban Quản lý tốn thời gian đọc, phân loại thủ công và soạn câu trả lời cho hàng trăm khiếu nại của cư dân mỗi ngày. |
| 3 | VinFast | Lặp lại (Repetitive) | Công nhân kiểm soát chất lượng phải dùng mắt thường liên tục dò lỗi ngoại quan (vết trầy, mối hở) trên dây chuyền lắp ráp. |
| 4 | Xanh SM | Lặp lại (Repetitive) | Kiểm soát viên phải nghe ghi âm và hậu kiểm thủ công để phát hiện các trường hợp tài xế gian lận chuyến đi để lạm dụng thưởng. |
| 5 | Vinmec | AI có thể tốt hơn (AI-upgrade) | Quy trình phân luồng và tiếp nhận bệnh nhân ban đầu dựa trên các câu hỏi rập khuôn, chưa tối ưu theo triệu chứng lâm sàng thực tế. |

---

## 🃏 Phase 2 — QUICK-ASSESS

### QUICK PROBLEM CARD #01
- **Bài toán (1 câu):** Tối ưu hóa điểm đón khách thực tế để giảm khoảng cách và thời gian tài xế di chuyển tìm khách do sai lệch định vị GPS.
- **Công ty thành viên:** [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  [ ] Vinmec
- **Ai đang đau (Actor)?** Tài xế Xanh SM và Khách hàng đặt xe.
- **Workflow thủ công hiện tại:**
  1. Khách đặt xe ──> 2. GPS tự động lấy vị trí hiện tại (bị lệch) ──> 3. Tài xế di chuyển đến vị trí định vị ──> 4. Tài xế gọi điện thoại xác nhận lộ trình thực tế ──> 5. Tài xế di chuyển lại để đón khách.
- **Bước nào tốn thời gian/lỗi nhất?** Bước 4 & 5 (⏱ 5-7 phút/lượt)
- **AI có thể nhảy vào hỗ trợ ở bước nào?** Bước 2 & 3 (Gợi ý các điểm đón thông minh dựa trên lịch sử đón khách thành công).
- **Đo thành công bằng gì (Metric có số)?** Giảm thời gian tài xế tiếp cận khách hàng thực tế từ 5-7 phút xuống dưới 2 phút; giảm tỷ lệ hủy chuyến từ 8% xuống dưới 3%.
- **Quick Architecture:** [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent / ML (Routing)

### QUICK PROBLEM CARD #02
- **Bài toán (1 câu):** Tự động phân loại mức độ khẩn cấp và soạn thảo câu trả lời dự thảo cho phản ánh của cư dân gửi về Ban Quản lý.
- **Công ty thành viên:** [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec
- **Ai đang đau (Actor)?** Nhân viên Ban Quản lý (BQL) Vinhomes.
- **Workflow thủ công hiện tại:**
  1. Nhận phản ánh trên ứng dụng cư dân ──> 2. Nhân viên đọc và phân loại thủ công ──> 3. Chuyển thông tin đến bộ phận kỹ thuật/an ninh ──> 4. Soạn thư phản hồi thủ công gửi cư dân.
- **Bước nào tốn thời gian/lỗi nhất?** Bước 2 & 4 (⏱ 12 phút/lượt)
- **AI có thể nhảy vào hỗ trợ ở bước nào?** Bước 2 (Phân loại tự động) và Bước 4 (Dự thảo phản hồi dựa trên kho tài liệu quy chuẩn).
- **Đo thành công bằng gì (Metric có số)?** Giảm thời gian soạn thảo phản hồi sơ bộ từ 12 phút xuống dưới 2 phút.
- **Quick Architecture:** [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent

### QUICK PROBLEM CARD #03
- **Bài toán (1 câu):** Tự động nhận diện lỗi bề mặt sơn và sai lệch mối nối vỏ xe bằng thị giác máy tính tại xưởng lắp ráp ô tô.
- **Công ty thành viên:** [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  [ ] Vinmec
- **Ai đang đau (Actor)?** Kỹ thuật viên kiểm định chất lượng (QA/QC) tại xưởng lắp ráp.
- **Workflow thủ công hiện tại:**
  1. Xe di chuyển vào trạm kiểm soát ngoại quan ──> 2. Công nhân đi quanh xe dò vết xước bằng mắt ──> 3. Đo đạc thủ công khe hở bằng thước đo chuyên dụng ──> 4. Ghi nhận lỗi thủ công vào hệ thống.
- **Bước nào tốn thời gian/lỗi nhất?** Bước 2 & 3 (⏱ 10 phút/lượt)
- **AI có thể nhảy vào hỗ trợ ở bước nào?** Bước 2 & 3 (Quét camera đa góc và tự động phân tích phát hiện lỗi).
- **Đo thành công bằng gì (Metric có số)?** Giảm thời gian kiểm định ngoại quan mỗi xe từ 10 phút xuống dưới 1 phút.
- **Quick Architecture:** [ ] No AI  [ ] Rule  [ ] LLM  [x] Computer Vision (Machine Learning)