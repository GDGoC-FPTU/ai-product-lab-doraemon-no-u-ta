# 03 — AI Log & Reflection (Cá nhân)

 Vũ Quang Bảo      |  2A202600610

---

## AI giúp gì trong buổi lab hôm nay?

Trong Phase 1 (SCAN), tôi dùng AI để brainstorm pain point bằng prompt từ worksheet:

> *"Tôi là AI Engineer tại Vin Smart Future. Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng Vinmec. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

AI giúp tôi nhanh chóng có danh sách ban đầu với các con số ước tính (ví dụ: tỉ lệ no-show, thời gian/cuộc gọi). Từ đó tôi lọc ra 5 bài toán thực tế để điền vào bảng SCAN.

Trong Phase 2, tôi dùng AI để stress-test Card #3 (Vinmec Appointment):

> *"Đây là thẻ bài toán: [dán Card #3]. Hãy đóng vai CFO và Trưởng phòng Vận hành, chỉ ra 3 điểm yếu về logic, metric và lý do rule-based code thông thường có thể giải quyết tốt hơn AI."*

AI phản biện rằng hệ thống nhắn tin tự động xác nhận lịch có thể làm bằng rule-based (gửi SMS template cố định), không cần LLM — điều này giúp tôi làm rõ hơn phần AI thực sự cần thiết là xử lý các phản hồi tự do của bệnh nhân (hủy, đổi giờ, hỏi thêm), không phải chỉ gửi tin cố định.

---

## AI sai gì?

Khi tôi hỏi số liệu tỉ lệ no-show thực tế của Vinmec, AI trả lời tự tin: **"Tỉ lệ no-show trung bình của bệnh viện tư tại Việt Nam là 18–22%"** và **"Vinmec có thể đang có ~300 lịch hẹn/ngày tại mỗi cơ sở"**.

Đây là **hallucination** — AI không có số liệu nội bộ của Vinmec, những con số này chỉ là ước tính suy diễn từ benchmark ngành y tế quốc tế. Nếu đưa vào báo cáo mà không ghi nguồn, dễ bị phản biện ngay.

---

## Sửa đổi ra sao?

Tôi thêm chỉ thị vào prompt:

> *"Mỗi khi đưa ra con số thống kê, hãy ghi rõ: đây là số liệu từ nguồn cụ thể nào, hay chỉ là ước tính suy diễn. Nếu là ước tính, gắn nhãn [ƯỚC TÍNH] trước con số."*

Sau điều chỉnh, AI bắt đầu phân biệt rõ ràng giữa số liệu có nguồn và ước tính, giúp tôi tự tin hơn khi biết phần nào cần kiểm chứng thêm.

---