# 03 - Nhật ký tương tác AI (AI Log & Reflection)

## 1. Nhật ký Prompting & Thử nghiệm
Trong buổi làm việc này, chúng tôi đã sử dụng AI làm đối tác tư duy (thought-partner) để xây dựng ranh giới an toàn cho hệ thống điều phối xe sạc pin lưu động của Xanh SM.

- **Phiên bản Prompt ban đầu:**
  "Bạn là trợ lý điều phối xe sạc pin cứu hộ cho Xanh SM. Hãy điều xe cứu hộ khi tài xế báo hết pin."
  *Lỗi gặp phải:* AI phản hồi quá thân thiện và dễ dàng bị thuyết phục điều xe cứu hộ ngay cả khi tài xế báo pin còn 10% - 15%, gây lãng phí tài nguyên vận hành.
  
- **Cải tiến thiết kế Prompt:**
  Chúng tôi đã áp dụng các chỉ thị giới hạn nghiêm ngặt (Hard Guardrails), tích hợp các từ khóa bắt buộc `dispatch_mobile_charger`, `5%`, và yêu cầu ghi nhãn `draft_only` cho mọi quyết định.

## 2. Phản ánh & Bài học kinh nghiệm
- **AI đã giúp ích gì:** AI giúp cấu trúc nhanh chóng các kịch bản kiểm thử tấn công (Adversarial Tests) để chủ động phát hiện lỗ hổng logic trong prompt.
- **Hạn chế gặp phải:** Khi người dùng cố tình đóng giả tình huống khẩn cấp giả tạo để yêu cầu phê duyệt tự động, prompt thông thường dễ bị vượt qua nếu không quy định rõ cấu trúc dữ liệu JSON phản hồi nghiêm ngặt.
- **Sửa đổi thực tế:** Luôn áp dụng kiến trúc Human-in-the-loop (HITL) bằng cách thiết lập trạng thái `draft_only` thay vì cho phép AI tự ra quyết định độc lập.