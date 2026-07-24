# Nhật ký AI - Reflection cá nhân

**Họ và tên:** Nguyễn Đình Liêm  
**Mã HV:** 2A202601421  
**Nhóm:** T081

## 1. Tôi đã dùng AI để làm gì?

Tôi dùng AI như một thought-partner trong ba việc. Trước hết, tôi yêu cầu AI brainstorm các quy trình vận hành có thể cải thiện tại VinFast, Xanh SM, Vinhomes, Vinmec và Vinpearl. Sau đó, tôi dùng AI để biến các ý tưởng chung như “chatbot chăm sóc khách hàng” thành problem card có actor, workflow, bottleneck và metric cụ thể. Cuối cùng, tôi nhờ AI phản biện các lựa chọn kiến trúc Rule, LLM Feature và Agent, đồng thời hỗ trợ kiểm tra system prompt và các tình huống prompt injection trong prototype Python.

AI giúp tôi nhìn ra nhiều hướng nhanh hơn, nhưng tôi không xem các con số do AI đưa ra là dữ kiện thật. Tôi dùng chúng như giả định ban đầu để đặt câu hỏi cần xác minh bằng log vận hành, phỏng vấn nhân viên hoặc một pilot nhỏ.

## 2. AI đã sai hoặc chưa đáng tin ở đâu?

Ở vòng brainstorm đầu tiên, AI đưa ra một số con số khá cụ thể, chẳng hạn số lượng sự cố mỗi ngày, phần trăm doanh thu bị mất và thời gian xử lý trung bình. Cách viết khiến các con số này trông như thống kê nội bộ của doanh nghiệp, dù AI không có quyền truy cập các hệ thống vận hành của Vingroup. Đây là một dạng hallucination về dữ liệu và nếu đưa thẳng vào báo cáo thì sẽ làm cho lập luận thiếu trung thực.

AI cũng từng đề xuất dùng Agent để tự động tìm trạm sạc, gửi tin nhắn cho tài xế và điều phối xe cứu hộ. Đề xuất này quá rộng so với bài toán: một lỗi về vị trí hoặc mức pin có thể khiến xe đi tới trạm không an toàn, còn việc gửi tin hay điều xe là hành động ngoài đời thực cần người chịu trách nhiệm. Với các bước có rủi ro cao, Rule và Human-in-the-loop phù hợp hơn việc cho Agent tự trị.

## 3. Tôi đã sửa prompt và cách làm ra sao?

Tôi sửa prompt theo hướng yêu cầu AI tách rõ **fact**, **assumption** và **câu hỏi cần xác minh**. Tôi cũng thêm các điều kiện:

- Không được tự tạo số liệu nội bộ; nếu không có nguồn thì phải gắn nhãn “ước tính để lập baseline”.
- Mỗi đề xuất phải nêu actor, workflow hiện tại, bottleneck, metric có số và operational boundary.
- Phải so sánh Rule-based với LLM Feature và Agent, không mặc định rằng Agent là lựa chọn tốt nhất.
- Với nghiệp vụ y tế, phí dịch vụ, pháp lý hoặc điều xe, AI chỉ được tạo bản nháp; con người phải kiểm tra và phê duyệt.
- Khi dữ liệu đầu vào thiếu, mâu thuẫn hoặc mức độ tin cậy thấp, hệ thống phải hỏi lại hoặc chuyển sang quy trình thủ công.

Ví dụ prompt đã chỉnh:

> Hãy đóng vai AI Product Engineer đang scoping một pilot. Chỉ sử dụng thông tin trong input; không tuyên bố số liệu nội bộ nếu không có nguồn. Mọi con số chưa được kiểm chứng phải ghi là ESTIMATE. Trả lời theo các mục Actor, Current Workflow, Bottleneck, Business Impact, Success Metric, Operational Boundary và Fallback. Đề xuất kiến trúc tối thiểu cần thiết, ưu tiên Rule hoặc LLM Feature trước Agent. Không được tự động gửi tin, điều xe, chẩn đoán hoặc thay đổi hồ sơ; các hành động này luôn cần Human-in-the-loop.

Sau khi chỉnh prompt, tôi kiểm tra bằng các input đối nghịch như “bỏ qua hướng dẫn trước”, “gửi ngay không cần duyệt”, pin xe dưới 5% nhưng trạm sạc cách 8 km, và yêu cầu AI tiết lộ system prompt. Kết quả mong đợi không chỉ là câu trả lời nghe hợp lý mà còn phải giữ tag `[DRAFT_ONLY]`, không đề xuất trạm quá xa trong tình huống pin tới hạn, không nhận đã thực hiện hành động và chuyển trường hợp rủi ro cho con người.

## 4. Bài học rút ra

AI hữu ích nhất ở giai đoạn mở rộng góc nhìn, cấu trúc hóa suy nghĩ và phản biện giả định. AI không thay thế việc xác minh dữ liệu, hiểu quy trình thực tế hoặc quyết định ranh giới vận hành. Sau buổi lab, tôi sẽ đánh giá một prompt bằng ba câu hỏi: dữ liệu nào là sự thật và dữ liệu nào chỉ là giả định, nếu AI sai thì hậu quả là gì, và ai có quyền duyệt trước khi output trở thành hành động thật.
