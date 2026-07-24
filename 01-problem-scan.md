
# Lab 02 - AI Product Scoping: Problem Scan

**Họ và tên:** Nguyễn Đình Liêm  
**Mã HV:** 2A202601421  
**Nhóm:** T081

## Bối cảnh và cách tiếp cận

Tôi đóng vai AI Product Engineer tại Vin Smart Future. Tôi quét các quy trình vận hành có nhiều thao tác lặp lại, mất thời gian hoặc gây khó khăn cho khách hàng và nhân viên. Các số liệu thời gian trong phần này là ước tính dùng để tạo baseline ban đầu; cần được xác minh bằng log vận hành trước khi xây dựng sản phẩm thật.

## Phase 1 - SCAN

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | Xanh SM (GSM) | Tốn thời gian | Khi tài xế báo xe sắp hết pin, điều phối viên phải kiểm tra vị trí, tìm trạm sạc phù hợp và soạn hướng dẫn thủ công. Việc này có thể mất khoảng 10-15 phút mỗi ca. |
| 2 | Xanh SM (GSM) | Stakeholder Pain | Điểm đón do khách nhập đôi khi khó xác định hoặc nằm trong khu vực cấm dừng; tài xế phải gọi lại cho điều phối viên để xác nhận, làm tăng thời gian chờ và tỷ lệ hủy chuyến. |
| 3 | VinFast | Lặp lại | Nhân viên hậu mãi phải đọc nội dung phiếu bảo hành và đối chiếu triệu chứng, mã lỗi, phụ tùng với lịch sử sửa chữa trước khi chuyển tới kỹ thuật viên. Nhiều trường hợp có thể được chuẩn hóa bằng phân loại tự động. |
| 4 | Vinhomes | AI-upgrade | Phản ánh của cư dân trên ứng dụng có thể được phân loại và chuyển đúng bộ phận bằng tay; các câu trả lời về tiện ích, phí dịch vụ hoặc bảo trì dễ bị rập khuôn và chậm. |
| 5 | Vinmec | Tốn thời gian | Bác sĩ phải đọc nhiều ghi chú trong hồ sơ để soạn bản tóm tắt xuất viện. Một bản nháp tóm tắt có thể mất khoảng 20-30 phút, dù quyết định cuối cùng vẫn phải do bác sĩ duyệt. |
| 6 | Vinpearl / VinWonders | Stakeholder Pain | Nhân viên chăm sóc khách hàng phải xử lý câu hỏi lặp lại về giờ mở cửa, vé, khu vực và chính sách đổi vé qua nhiều kênh. Khách khó nhận được câu trả lời nhất quán vào giờ cao điểm. |

## Phase 2 - QUICK-ASSESS

Tôi chọn ba bài toán có tần suất cao, có thể đo hiệu quả trong pilot và vẫn giữ được bước kiểm duyệt của con người: xử lý sự cố pin của Xanh SM, phân loại phản ánh cư dân Vinhomes và hỗ trợ soạn tóm tắt xuất viện tại Vinmec.

### Quick Problem Card #1 - Xanh SM: Xử lý sự cố pin thực địa

**Bài toán:** Khi tài xế báo pin thấp hoặc xe không thể tiếp tục hành trình, điều phối viên mất nhiều thời gian tra cứu dữ liệu và soạn hướng dẫn cứu hộ.

**Công ty thành viên:** Xanh SM (GSM)

**Ai đang đau (Actor/Operator):** Tài xế phải chờ hỗ trợ; điều phối viên phải xử lý nhiều cuộc gọi cùng lúc; khách trên chuyến có thể phải chờ hoặc hủy chuyến.

**Workflow thủ công hiện tại:**

1. Tài xế gọi hoặc nhắn cho trung tâm điều vận.
2. Điều phối viên hỏi lại biển số, mức pin và vị trí hiện tại.
3. Điều phối viên mở bản đồ và hệ thống trạm sạc để tìm lựa chọn phù hợp.
4. Điều phối viên viết hướng dẫn hoặc liên hệ đội cứu hộ.
5. Điều phối viên gửi thông tin sau khi tự kiểm tra.

**Bước tốn thời gian/lỗi nhất:** Bước 2-4, khoảng 10-15 phút/lượt; dễ nhầm vị trí, mức pin hoặc chọn trạm quá xa.

**AI có thể hỗ trợ:** Trích xuất biển số, mức pin và vị trí từ nội dung báo cáo; tóm tắt lựa chọn trạm sạc trong phạm vi an toàn; soạn tin nhắn hướng dẫn dạng nháp. Nếu pin dưới 5% hoặc dữ liệu thiếu, hệ thống phải chuyển sang quy trình cứu hộ và yêu cầu người điều phối xử lý.

**Metric thành công:** Giảm thời gian tạo phương án hỗ trợ từ 15 phút xuống dưới 3 phút/lượt; ít nhất 98% draft không chứa trạm ngoài giới hạn an toàn; 100% tin nhắn phải được điều phối viên duyệt trước khi gửi.

**Quick Architecture:** LLM Feature kết hợp Rule-based safety checks và Human-in-the-loop. Không dùng Agent tự động gửi lệnh điều xe.

### Quick Problem Card #2 - Vinhomes: Phân loại và soạn phản hồi cho phản ánh cư dân

**Bài toán:** Phản ánh của cư dân thường được chuyển thủ công tới bộ phận vận hành, khiến các yêu cầu bảo trì hoặc câu hỏi dịch vụ bị chậm phản hồi.

**Công ty thành viên:** Vinhomes

**Ai đang đau (Actor/Operator):** Nhân viên chăm sóc cư dân phải đọc, gắn nhãn và chuyển từng yêu cầu; cư dân phải chờ lâu khi phản ánh được gửi nhầm bộ phận.

**Workflow thủ công hiện tại:**

1. Cư dân gửi nội dung và ảnh qua ứng dụng hoặc hotline.
2. Nhân viên đọc nội dung, xác định tòa/khu vực và loại yêu cầu.
3. Nhân viên chuyển ticket cho bảo trì, an ninh, vệ sinh hoặc kế toán.
4. Bộ phận phụ trách kiểm tra và phản hồi lại.
5. Nhân viên CSKH rà soát rồi trả lời cư dân.

**Bước tốn thời gian/lỗi nhất:** Bước 2-3, khoảng 6-10 phút/ticket; các nội dung mơ hồ hoặc có nhiều vấn đề dễ bị gắn sai nhóm.

**AI có thể hỗ trợ:** Phân loại ticket theo taxonomy đã được phê duyệt, trích xuất địa điểm và mức độ khẩn cấp, đồng thời soạn phản hồi nháp dựa trên kho tri thức đã kiểm duyệt. Các vấn đề về phí, tranh chấp, an toàn hoặc pháp lý phải chuyển người phụ trách.

**Metric thành công:** 90% ticket được gợi ý đúng bộ phận với thời gian dưới 30 giây; giảm thời gian phân loại từ 8 phút xuống dưới 1 phút; không tự động cam kết thời hạn hoặc số tiền bồi hoàn.

**Quick Architecture:** Rule-based router + LLM Feature có trích dẫn nguồn nội bộ và bước Human-in-the-loop.

### Quick Problem Card #3 - Vinmec: Soạn nháp tóm tắt xuất viện

**Bài toán:** Bác sĩ phải tổng hợp thủ công diễn biến điều trị, thuốc và hướng dẫn theo dõi từ nhiều phần của hồ sơ bệnh án để tạo tóm tắt xuất viện.

**Công ty thành viên:** Vinmec

**Ai đang đau (Actor/Operator):** Bác sĩ điều trị và điều dưỡng mất thời gian nhập liệu; bệnh nhân phải chờ hoàn tất thủ tục xuất viện.

**Workflow thủ công hiện tại:**

1. Bác sĩ mở hồ sơ bệnh án và xem các ghi chú, kết quả xét nghiệm, chẩn đoán hình ảnh.
2. Bác sĩ chọn thông tin cần đưa vào bản tóm tắt.
3. Bác sĩ tự viết diễn biến, thuốc và hướng dẫn theo dõi.
4. Điều dưỡng kiểm tra các mục hành chính và in/gửi tài liệu.
5. Bác sĩ ký duyệt bản cuối cùng.

**Bước tốn thời gian/lỗi nhất:** Bước 1-3, khoảng 20-30 phút/bệnh nhân; nguy cơ bỏ sót thông tin hoặc viết nhầm liều dùng nếu sao chép thủ công.

**AI có thể hỗ trợ:** Tạo bản nháp có cấu trúc từ các trường dữ liệu đã được phép truy cập, kèm liên kết tới nguồn trong hồ sơ để bác sĩ đối chiếu. AI không được tự chẩn đoán, tự thay đổi liều thuốc hoặc phát hành tài liệu.

**Metric thành công:** Giảm thời gian tạo bản nháp từ 25 phút xuống dưới 8 phút/bệnh nhân; 100% bản nháp được bác sĩ duyệt; đạt ít nhất 95% độ đầy đủ trên checklist các trường bắt buộc trong pilot.

**Quick Architecture:** LLM Feature với retrieval từ hồ sơ được phân quyền, kiểm tra schema và Human-in-the-loop. Không dùng Agent tự cập nhật hồ sơ.

## Kết luận cá nhân

Bài toán Xanh SM có ưu tiên thử nghiệm cao vì dữ liệu đầu vào tương đối có cấu trúc, metric thời gian rõ và có thể đặt ranh giới an toàn bằng rule. Bài toán Vinmec có giá trị lớn nhưng rủi ro cao hơn, nên chỉ nên bắt đầu bằng bản nháp không tự động gửi và một tập dữ liệu đã ẩn thông tin định danh. Bài toán Vinhomes phù hợp với pilot phân loại ticket trước khi mở rộng sang sinh câu trả lời.
