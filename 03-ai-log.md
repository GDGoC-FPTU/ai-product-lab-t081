# 03-ai-log.md

# AI Log & Reflection

**Họ và tên:** Đinh Xuân Huy

**MSSV:** 1894

---

## AI giúp gì?

Trong buổi lab này, tôi sử dụng ChatGPT như một **thought-partner** để hỗ trợ brainstorming và hoàn thiện ý tưởng. Ban đầu nhóm chưa thống nhất nên chọn bài toán nào trong hệ sinh thái Vin Smart Future. AI đã gợi ý nhiều bài toán thuộc VinFast, Vinmec, Vinhomes và Xanh SM, đồng thời phân tích ưu nhược điểm của từng bài theo tiêu chí AI Fit, Business Impact và khả năng xây dựng prototype.

Sau khi thảo luận, nhóm lựa chọn bài toán **AI hỗ trợ tóm tắt bệnh án tại Vinmec**. AI tiếp tục hỗ trợ xây dựng Current Workflow, Future Workflow, Problem Statement, Success Metric, Human-in-the-loop và Operational Boundary. Ngoài ra, AI còn hỗ trợ viết System Prompt, thiết kế JSON Output và đề xuất các adversarial test cases để kiểm thử prompt prototype.

---

## AI sai gì?

Ban đầu AI đề xuất để mô hình đưa ra chẩn đoán hoặc gợi ý hướng điều trị sau khi đọc bệnh án. Sau khi xem xét, nhóm nhận thấy đây là phạm vi quá rộng và không phù hợp với vai trò của một hệ thống hỗ trợ. Nếu AI đưa ra chẩn đoán hoặc kê đơn thì rủi ro rất cao và không đáp ứng yêu cầu an toàn của bài toán.

Ngoài ra, AI cũng từng đề xuất sử dụng Agent cho bài toán này. Tuy nhiên, sau khi phân tích workflow, nhóm nhận thấy hệ thống chỉ cần thực hiện một tác vụ tóm tắt văn bản nên sử dụng **LLM Feature** sẽ đơn giản, phù hợp và tiết kiệm chi phí hơn.

---

## Tôi đã sửa như thế nào?

Nhóm đã điều chỉnh phạm vi hoạt động của AI bằng cách bổ sung **Operational Boundary** rõ ràng trong System Prompt.

AI chỉ được phép:

* Tóm tắt bệnh án.
* Trích xuất tiền sử bệnh.
* Liệt kê thuốc đang sử dụng.
* Tóm tắt kết quả xét nghiệm.
* Đánh dấu thông tin còn thiếu.

AI tuyệt đối **không được**:

* Chẩn đoán bệnh.
* Kê đơn thuốc.
* Đề xuất phác đồ điều trị.
* Tự thay thế quyết định của bác sĩ.

Bên cạnh đó, nhóm bổ sung **Human-in-the-loop**, yêu cầu bác sĩ luôn xem lại bản tóm tắt trước khi sử dụng, đồng thời thiết kế **Fallback** để hệ thống quay về quy trình đọc hồ sơ thủ công nếu AI không đủ tự tin hoặc dữ liệu đầu vào không đầy đủ.

Thông qua quá trình này, tôi nhận thấy AI là một công cụ hỗ trợ rất tốt trong việc brainstorming và xây dựng ý tưởng, nhưng con người vẫn cần đánh giá tính khả thi, xác định ranh giới vận hành và kiểm soát rủi ro trước khi đưa giải pháp vào thực tế.
