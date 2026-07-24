# 01-problem-scan.md

# Lab 02 — Problem Scan (Phase 1 & Phase 2)

**Họ và tên:** Nguyễn Bá Khánh Huy

**MSSV:** 2A202601591

**Nhóm:** T081

---

# 🔍 Phase 1 — SCAN

## 📝 Danh sách các bài toán tiềm năng

| # | Subsidiary | Lens             | Mô tả ngắn bài toán                                                                                                          |
| - | ---------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| 1 | VinFast    | Repetitive       | Nhân viên bảo hiểm phải duyệt thủ công hàng ngàn ảnh xước xát xe, dễ bị qua mặt bởi ảnh chỉnh sửa hoặc deepfake.             |
| 2 | Xanh SM    | Stakeholder Pain | Hệ thống gợi ý điểm đón (POI) chưa chuẩn xác theo ngữ cảnh thời gian thực, khiến tài xế và khách hàng khó tìm thấy nhau.    |
| 3 | Vinhomes   | Time-consuming   | Ban quản lý mất nhiều thời gian tra cứu thủ công hàng đống văn bản pháp lý, quy định và hợp đồng cư dân để giải quyết tranh chấp. |
| 4 | Vinmec     | AI-upgrade       | Dữ liệu khám chữa bệnh rải rác chưa được tổng hợp và phân tích chuỗi thời gian để gợi ý phác đồ điều trị cá nhân hóa.       |
| 5 | Vinpearl   | Repetitive       | Tổng hợp và phân tích lượng lớn log dữ liệu đánh giá của khách hàng (Reviews) để trích xuất từ khóa phàn nàn/khen ngợi thủ công. |

---

# 🃏 Phase 2 — QUICK-ASSESS

# QUICK PROBLEM CARD #1

### Bài toán

AI tự động phát hiện ảnh giả mạo/chỉnh sửa (Digital Forensics) trong quy trình duyệt hồ sơ bồi thường bảo hiểm vật chất xe.

### Công ty thành viên

* [x] VinFast

### Ai đang đau (Actor)?

* Nhân viên thẩm định bồi thường bảo hiểm.
* Chuyên viên quản trị rủi ro.

### Workflow thủ công hiện tại

1. Khách hàng chụp ảnh xe bị va quẹt/xước xát và tải lên app.
2. Nhân viên thẩm định tải ảnh về, phóng to và kiểm tra bằng mắt thường.
3. So sánh với lịch sử hư hỏng trước đó của xe.
4. Quyết định duyệt bồi thường hoặc đánh dấu nghi ngờ gian lận.

### Bước nào tốn thời gian/lỗi nhất?

**Kiểm tra tính nguyên bản của bức ảnh bằng mắt thường (rất khó phát hiện ảnh AI generate hoặc photoshop tinh vi).**

⏱ Khoảng **10–15 phút/hồ sơ**.

### AI có thể nhảy vào hỗ trợ ở bước nào?

Ngay khi khách hàng tải ảnh lên, hệ thống AI (Computer Vision/Vision Transformers) sẽ:

* Quét cấu trúc pixel và phân tích độ nhiễu để phát hiện dấu vết chỉnh sửa/deepfake.
* Khoanh vùng vùng bị hư hỏng và đối chiếu với cơ sở dữ liệu 3D của xe.
* Trả về điểm số tin cậy (Confidence Score) của bức ảnh.
* Tự động duyệt các ca rõ ràng, chỉ chuyển các ca nghi ngờ (cờ đỏ) cho con người.

### Metric thành công

* Cắt giảm **80%** thời gian duyệt hồ sơ tiêu chuẩn.
* Tỷ lệ phát hiện ảnh giả mạo đạt Hit Rate **> 95%**.
* Tiết kiệm chi phí rò rỉ do gian lận bảo hiểm ít nhất **15%** mỗi quý.

### Quick Architecture

- [ ] No AI
- [ ] Rule
- [x] LLM (Vision-Language Model / CNN)
- [ ] Agent

---

# QUICK PROBLEM CARD #2

### Bài toán

Hệ thống gợi ý điểm đón (POI Recommendation System) theo thời gian thực để tối ưu khoảng cách đi bộ cho khách và lộ trình cho tài xế.

### Công ty thành viên

* [x] Xanh SM

### Ai đang đau (Actor)?

* Tài xế Xanh SM.
* Khách hàng đặt xe.

### Workflow thủ công hiện tại

1. Khách hàng nhập địa chỉ (thường rất chung chung, ví dụ "Landmark 81").
2. Hệ thống thả ghim ở vị trí mặc định.
3. Tài xế đến vị trí ghim nhưng khách lại đang đứng ở cổng khác (cách hàng trăm mét).
4. Tài xế phải gọi điện thoại hỏi khách đang ở đâu, sau đó vòng xe lại, gây tắc đường và tốn pin.

### Bước nào tốn thời gian/lỗi nhất?

**Liên lạc qua lại và điều hướng xe vòng vèo trong khu vực đông đúc để tìm đúng cổng đón.**

⏱ Khoảng **3–7 phút lãng phí/chuyến**.

### AI có thể nhảy vào hỗ trợ ở bước nào?

Sử dụng luồng dữ liệu thời gian thực (Kafka/Spark) kết hợp mô hình Sequential Recommendation, AI sẽ:

* Phân tích thói quen đặt xe trong quá khứ của User và dữ liệu vị trí GPS hiện tại.
* Tự động tính toán điểm đón (POI) khả thi nhất cho xe ô tô dừng đỗ hợp lệ.
* Gợi ý lên màn hình của khách: *"Tài xế sẽ đón bạn tại Cửa East - Landmark 81 (cách 50m). Bạn đồng ý chứ?"* trước khi chốt cuốc.

### Metric thành công

* Tăng Hit Rate@5 cho việc gợi ý đúng điểm đón lên **> 85%**.
* Giảm số lượng cuộc gọi giữa tài xế và khách hàng **40%**.
* Giảm thời gian chờ khách trung bình (Waiting Time) xuống dưới **2 phút**.

### Quick Architecture

- [ ] No AI
- [ ] Rule
- [x] LLM / RecSys Model
- [ ] Agent

---

# QUICK PROBLEM CARD #3

### Bài toán

Hệ thống Multi-Agent RAG hỗ trợ ban quản lý tra cứu tự động luật, quy định nội khu và chi tiết hợp đồng để giải quyết khiếu nại cư dân tức thì.

### Công ty thành viên

* [x] Vinhomes

### Ai đang đau (Actor)?

* Ban quản lý tòa nhà.
* Chuyên viên pháp chế.

### Workflow thủ công hiện tại

1. Cư dân gửi ticket khiếu nại (ví dụ: phí gửi xe, quy định sửa chữa nhà, nuôi thú cưng).
2. Ban quản lý tiếp nhận, xác định loại khiếu nại.
3. Lục tìm lại hợp đồng mua bán của căn hộ đó và sổ tay cư dân.
4. Đọc, đối chiếu chéo các điều khoản.
5. Soạn văn bản phản hồi và trích dẫn quy định.

### Bước nào tốn thời gian/lỗi nhất?

**Tìm kiếm và đối chiếu chéo thông tin giữa các tài liệu pháp lý khác nhau.**

⏱ Khoảng **30–45 phút/ticket** phức tạp.

### AI có thể nhảy vào hỗ trợ ở bước nào?

Xây dựng hệ thống Multi-Agent RAG, trong đó:

* Agent 1: Phân tích intent của khiếu nại từ cư dân.
* Agent 2: Truy xuất (Retrieval) chính xác điều khoản trong hợp đồng của riêng căn hộ đó.
* Agent 3: Truy xuất quy định chung của ban quản lý.
* Agent 4: Tổng hợp và sinh câu trả lời (Generation) có trích dẫn nguồn văn bản pháp lý chính xác.

### Metric thành công

* Giảm thời gian xử lý khiếu nại phức tạp từ **45 phút xuống dưới 5 phút**.
* Đảm bảo **100%** không bị ảo giác (Hallucination) khi trích dẫn số liệu pháp lý.
* Tăng chỉ số hài lòng của cư dân (CSAT) lên **15%**.

### Quick Architecture

- [ ] No AI
- [ ] Rule
- [ ] LLM
- [x] Agent (Multi-Agent RAG)