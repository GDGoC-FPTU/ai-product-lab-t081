# 01-problem-scan.md

# Lab 02 — Problem Scan (Phase 1 & Phase 2)

**Họ và tên:** ....................................

**MSSV:** ....................................

**Nhóm:** ....................................

---

# 🔍 Phase 1 — SCAN

## 📝 Danh sách các bài toán tiềm năng

| # | Subsidiary | Lens             | Mô tả ngắn bài toán                                                                                                          |
| - | ---------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| 1 | Vinmec     | Time-consuming   | Bác sĩ phải đọc toàn bộ bệnh án, kết quả xét nghiệm và lịch sử điều trị trước khi khám, gây mất nhiều thời gian.             |
| 2 | Vinhomes   | AI-upgrade       | Nhân viên CSKH phải đọc và soạn phản hồi thủ công cho các đánh giá và khiếu nại của cư dân trên nhiều kênh.                  |
| 3 | VinFast    | Repetitive       | Nhân viên hỗ trợ phải đọc mô tả lỗi xe và phân loại ticket bảo hành đến đúng bộ phận kỹ thuật.                               |
| 4 | Xanh SM    | Stakeholder Pain | Khiếu nại của khách hàng (đi đường vòng, xe bẩn, thái độ tài xế...) được phân loại thủ công nên xử lý chậm và dễ sai.        |
| 5 | Vinpearl   | AI-upgrade       | Chatbot đặt phòng và tư vấn dịch vụ chưa hiểu tốt các yêu cầu phức tạp của khách hàng nên thường phải chuyển sang nhân viên. |

---

# 🃏 Phase 2 — QUICK-ASSESS

# QUICK PROBLEM CARD #1

### Bài toán

AI hỗ trợ tóm tắt bệnh án để bác sĩ nhanh chóng nắm được tình trạng bệnh nhân trước khi khám.

### Công ty thành viên

* ☑ Vinmec

### Ai đang đau (Actor)?

* Bác sĩ
* Điều dưỡng

### Workflow thủ công hiện tại

1. Điều dưỡng tải hồ sơ bệnh án lên hệ thống.
2. Bác sĩ mở toàn bộ hồ sơ bệnh án.
3. Đọc lịch sử khám, toa thuốc và kết quả xét nghiệm.
4. Tổng hợp thông tin trước khi bắt đầu khám bệnh.

### Bước nào tốn thời gian/lỗi nhất?

**Đọc và tổng hợp bệnh án**

⏱ Khoảng **10–15 phút/lượt khám**.

### AI có thể nhảy vào hỗ trợ ở bước nào?

Sau khi hồ sơ được tải lên, AI sẽ:

* Tóm tắt tiền sử bệnh.
* Liệt kê thuốc đang sử dụng.
* Nêu các kết quả xét nghiệm bất thường.
* Tóm tắt các lần khám gần đây.
* Highlight những thông tin bác sĩ cần lưu ý.

### Metric thành công

* Giảm thời gian đọc hồ sơ từ **15 phút xuống dưới 3 phút**.
* **90%** bác sĩ đánh giá bản tóm tắt hữu ích.
* Không bỏ sót thông tin quan trọng trong **95%** hồ sơ kiểm thử.

### Quick Architecture

☐ No AI

☐ Rule

☑ LLM

☐ Agent

---

# QUICK PROBLEM CARD #2

### Bài toán

AI hỗ trợ soạn phản hồi cho các đánh giá và khiếu nại của cư dân.

### Công ty thành viên

* ☑ Vinhomes

### Ai đang đau (Actor)?

* Nhân viên Chăm sóc khách hàng

### Workflow thủ công hiện tại

1. Nhận đánh giá hoặc khiếu nại.
2. Đọc nội dung.
3. Xác định vấn đề.
4. Soạn phản hồi.
5. Gửi phản hồi cho cư dân.

### Bước nào tốn thời gian/lỗi nhất?

**Soạn phản hồi phù hợp với từng tình huống.**

⏱ Khoảng **8–10 phút/ticket**.

### AI có thể nhảy vào hỗ trợ ở bước nào?

Sau khi đọc nội dung, AI sẽ:

* Phân loại chủ đề.
* Tóm tắt nội dung.
* Soạn phản hồi lịch sự.
* Gợi ý mức độ ưu tiên.

Nhân viên chỉ cần kiểm tra và gửi.

### Metric thành công

* Giảm thời gian phản hồi từ **10 phút xuống dưới 2 phút**.
* **80%** phản hồi được sử dụng sau khi chỉ chỉnh sửa nhỏ.
* Giảm thời gian xử lý trung bình **70%**.

### Quick Architecture

☐ No AI

☐ Rule

☑ LLM

☐ Agent

---

# QUICK PROBLEM CARD #3

### Bài toán

AI hỗ trợ phân loại ticket bảo hành xe và chuyển đến đúng nhóm kỹ thuật.

### Công ty thành viên

* ☑ VinFast

### Ai đang đau (Actor)?

* Nhân viên hỗ trợ bảo hành.
* Điều phối kỹ thuật.

### Workflow thủ công hiện tại

1. Khách hàng gửi mô tả lỗi.
2. Nhân viên đọc nội dung.
3. Xác định nhóm lỗi.
4. Chuyển ticket đến bộ phận kỹ thuật phù hợp.

### Bước nào tốn thời gian/lỗi nhất?

**Đọc mô tả và phân loại lỗi.**

⏱ Khoảng **5–7 phút/ticket**.

### AI có thể nhảy vào hỗ trợ ở bước nào?

AI sẽ:

* Đọc mô tả lỗi.
* Phân loại lỗi (Pin, Động cơ, Phanh, Phần mềm...).
* Tóm tắt nội dung.
* Gợi ý mức độ ưu tiên.
* Đề xuất bộ phận tiếp nhận.

### Metric thành công

* **90%** ticket được phân loại đúng ngay lần đầu.
* Giảm thời gian phân loại từ **7 phút xuống dưới 1 phút**.
* Giảm tỷ lệ chuyển nhầm ticket xuống dưới **5%**.

### Quick Architecture

☐ No AI

☐ Rule

☑ LLM

☐ Agent
