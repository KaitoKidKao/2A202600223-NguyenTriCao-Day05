# AI Product Canvas — template

Điền Canvas cho product AI của nhóm. Mỗi ô có câu hỏi guide — trả lời trực tiếp, xóa phần in nghiêng khi điền.

---

## Canvas

|   | Value | Trust | Feasibility |
|---|-------|-------|-------------|
| **Câu hỏi guide** | User nào? Pain gì? AI giải quyết gì mà cách hiện tại không giải được? | Khi AI sai thì user bị ảnh hưởng thế nào? User biết AI sai bằng cách nào? User sửa bằng cách nào? | Cost bao nhiêu/request? Latency bao lâu? Risk chính là gì? |
| **Trả lời** | **User:** Hành khách (nội địa/quốc tế), Hội viên Lotusmiles.<br><br>**Pain:** Chờ đợi tổng đài viên lâu, khó tra cứu policy vé/hành lý phức tạp trên web, cần hỗ trợ gấp ngoài giờ hành chính.<br><br>**AI giải quyết gì:** Cung cấp câu trả lời/hướng dẫn tức thì (mili-giây) 24/7. Trích xuất đúng policy thay vì user phải tự đọc một văn bản dài. | **Ảnh hưởng khi AI sai:** User lỡ/trễ chuyến bay, bị phạt tiền (mang lố hành lý, sai tên vé), bực bội.<br><br>**User biết sai bằng cách:** Câu trả lời lạc đề, thông tin mâu thuẫn với vé đã mua (Đại loại hỏi A trả lời B).<br><br>**Sửa bằng cách:** Nhấn ngay nút "Gặp nhân viên hỗ trợ" (Hand-off) để thoát bot, hoặc buộc phải gõ lại từ khoá cực kỳ đơn giản. | **Cost/request:** Rất rẻ (~$0.001 - $0.002). Vi NEO nghiêng về NLP/Rule-based intent matching nhiều hơn là Generative LLM lớn, ít tốn compute cost.<br><br>**Latency:** Rất thấp, phản hồi gần như ngay lập tức (<1s) để tạo cảm giác tự nhiên.<br><br>**Risk chính:** Khủng hoảng truyền thông/pháp lý nếu AI tư vấn sai luật hàng không hoặc thủ tục Visa; User bỏ đi vì bot bị "loop" (lặp lại câu "Neo chưa hiểu"). |

---

## Automation hay augmentation?

☐ Automation — AI làm thay, user không can thiệp

☑ Augmentation — AI gợi ý, user quyết định cuối cùng

**Justify:** Mặc dù với tác vụ "Tra cứu" (FAQ), NEO thiên về automation. Nhưng đối với các tương tác cốt lõi sinh ra doanh thu hoặc nhạy cảm rủi ro (đổi ngày vé, mua thêm hành lý, thanh toán), NEO đóng vai trò là "Người hướng dẫn lộ trình" (Augmentation). AI sẽ cung cấp luồng thông tin, link trỏ đến màn hình thao tác hoặc báo giá, nhưng **quyết định thao tác và xác nhận (confirm) cuối cùng thuộc về user**. Nếu AI làm sai (Automation) trong giao dịch vé máy bay có thể dẫn đến thiệt hại tài chính rất lớn và rủi ro pháp lý.

Gợi ý: nếu AI sai mà user không biết → automation nguy hiểm, cân nhắc augmentation.

---

## Learning signal

| # | Câu hỏi | Trả lời |
|---|---------|---------|
| 1 | User correction đi vào đâu? | Đi vào Chat logs. Hệ thống sẽ bắt các sự kiện (events) như "User liên tục gõ lại nội dung", hoặc sự kiện "Bấm chuyển nhân viên" (Hand-off trigger) để chuyển cho team Data/Product review lại luồng hội thoại gãy và tái đào tạo (re-label/re-train) Intent cho bot. |
| 2 | Product thu signal gì để biết tốt lên hay tệ đi? | **Tín hiệu tường minh (Explicit):** Đánh giá sao (CSAT 1-5 sao) hoặc Thumbs Up/Down ở cuối phiên chat.<br>**Tín hiệu ngầm định (Implicit):** Tỷ lệ tự động hoá thành công (Deflection Rate) - tức tỷ lệ user giải quyết xong vấn đề mà không cần gặp tổng đài viên; Tỷ lệ bỏ ngang (Drop-off rate). |
| 3 | Data thuộc loại nào? ☐ User-specific · ☐ Domain-specific · ☐ Real-time · ☐ Human-judgment · ☐ Khác: ___ | ☑ User-specific <br>☑ Domain-specific <br>☑ Human-judgment |

**Có marginal value không?** (Model đã biết cái này chưa? Ai khác cũng thu được data này không?)
Khẳng định là **CÓ RẤT NHIỀU giá trị biên (Marginal value)**. Các foundation models cơ bản (như ChatGPT hoặc Gemini) có thể biết được kiến thức chung (VD: Sân bay Nội Bài ở đâu), nhưng chúng không thể biết được "Bảng giá/Chính sách thay đổi theo block/theo mùa" nội bộ của VNA, cũng như không thể truy xuất được "PNR (mã đặt chỗ) xyz" này thuộc về khách hàng VIP Lotusmiles nào. Việc thu thập data lịch sử hội thoại hỗ trợ nghiệp vụ bay (Domain+User specific data) tạo ra rào cản phòng thủ dữ liệu (data moat) rất lớn, giúp VNA xây dựng các luồng hỗ trợ trúng đích và cá nhân hoá tốt hơn đối thủ. Trải nghiệm càng tốt thì user càng dùng nhiều -> data càng sinh ra nhiều.
___

---

## Cách dùng

1. Điền Value trước — chưa rõ pain thì chưa điền Trust/Feasibility
2. Trust: trả lời 4 câu UX (đúng → sai → không chắc → user sửa)
3. Feasibility: ước lượng cost, không cần chính xác — order of magnitude đủ
4. Learning signal: nghĩ về vòng lặp dài hạn, không chỉ demo ngày mai
5. Đánh [?] cho chỗ chưa biết — Canvas là hypothesis, không phải đáp án

---

*AI Product Canvas — Ngày 5 — VinUni A20 — AI Thực Chiến · 2026*
