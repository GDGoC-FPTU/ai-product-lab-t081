# 02-deep-dive-report.md

# Lab 02 – Deep Dive Report

## Nhóm

**Tên nhóm:** : T081

### Thành viên

| Họ và tên           | MSSV |
| ------------------- | ---- |
| Đinh Xuân Huy       | 1894 |
| Nguyễn Bá Khánh Huy | 1591 |
| Nguyễn Đình Liêm    | 1421 |

---

# Bài toán được lựa chọn

**AI hỗ trợ tóm tắt bệnh án (Medical Record Summarizer) cho bác sĩ tại Vinmec trước khi khám bệnh.**

---

# Phase 3.1 – Current-State Workflow Mapping

## Current Workflow

```text
Bệnh nhân đến khám
        │
        ▼
Điều dưỡng tiếp nhận và cập nhật hồ sơ
        │
        │ 🔄 Handoff
        ▼
Bác sĩ mở toàn bộ bệnh án
        │
        ▼
🔴 Đọc lịch sử khám, toa thuốc, kết quả xét nghiệm
        │
        ▼
Tự tổng hợp thông tin quan trọng
        │
        ▼
Khám và tư vấn bệnh nhân
```

### Thời gian trung bình

| Bước                | Thời gian         |
| ------------------- | ----------------- |
| Tiếp nhận bệnh nhân | 3 phút            |
| Cập nhật hồ sơ      | 2 phút            |
| Đọc bệnh án         | **10–15 phút** 🔴 |
| Tổng hợp thông tin  | 3 phút            |
| Khám bệnh           | 10 phút           |

**Tổng thời gian:** khoảng **28–33 phút/lượt khám**

### Bottleneck

🔴 Bác sĩ phải đọc lượng lớn hồ sơ bệnh án và kết quả xét nghiệm trước khi khám.

---

# Phase 3.2 – Problem Statement (6-field)

| Field                       | Nội dung                                                                                                                                                                                                                                                                                    |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Actor / Operator**     | Bác sĩ và điều dưỡng tại Vinmec.                                                                                                                                                                                                                                                            |
| **2. Current Workflow**     | Điều dưỡng cập nhật hồ sơ bệnh án. Bác sĩ đọc toàn bộ lịch sử khám, kết quả xét nghiệm, toa thuốc và tự tổng hợp trước khi bắt đầu khám.                                                                                                                                                    |
| **3. Bottleneck**           | Việc đọc và tổng hợp bệnh án mất nhiều thời gian, đặc biệt với bệnh nhân có lịch sử điều trị dài.                                                                                                                                                                                           |
| **4. Business Impact**      | Mỗi lượt khám mất thêm khoảng 10–15 phút chỉ để đọc hồ sơ. Điều này làm giảm số lượng bệnh nhân có thể khám mỗi ngày, tăng thời gian chờ và ảnh hưởng trải nghiệm khách hàng.                                                                                                               |
| **5. Success Metric**       | - Giảm thời gian đọc bệnh án từ **15 phút xuống dưới 3 phút**.<br>- Ít nhất **90%** bác sĩ đánh giá bản tóm tắt hữu ích.<br>- Không bỏ sót thông tin quan trọng trong **95%** hồ sơ kiểm thử.                                                                                               |
| **6. Operational Boundary** | AI **chỉ được phép** tóm tắt bệnh án, liệt kê thông tin quan trọng và đánh dấu dữ liệu còn thiếu. AI **không được phép** chẩn đoán bệnh, kê đơn thuốc, đề xuất điều trị hoặc tự thay thế quyết định của bác sĩ. Mọi kết quả phải được bác sĩ xem xét trước khi sử dụng (Human-in-the-loop). |

---

# Phase 3.3 – Future-State Flow & AI Fit

## AI Fit Matrix

**☑ LLM Feature**

### Lý do

* Dữ liệu đầu vào là văn bản y khoa dài và không có cấu trúc.
* Rule-based khó xử lý các cách diễn đạt khác nhau.
* LLM có khả năng tóm tắt và trích xuất thông tin quan trọng hiệu quả hơn.

---

## Future-State Workflow

```text
Bệnh nhân đến khám
        │
        ▼
Điều dưỡng cập nhật hồ sơ
        │
        ▼
🔵 AI đọc toàn bộ bệnh án
        │
        ▼
🔵 AI tạo bản tóm tắt:
    • Tiền sử bệnh
    • Thuốc đang dùng
    • Kết quả xét nghiệm bất thường
    • Các điểm cần lưu ý
        │
        ▼
🟢 Bác sĩ xem lại bản tóm tắt (Human-in-the-loop)
        │
        ├───────────────┐
        │               │
        ▼               ▼
Đúng               Không đúng
        │               │
        ▼               ▼
Khám bệnh      ↩️ Đọc lại hồ sơ gốc
```

---

## Human-in-the-loop (HITL)

Bác sĩ luôn xem và xác nhận bản tóm tắt trước khi sử dụng trong quá trình khám.

---

## Fallback

Nếu:

* AI không đủ tự tin.
* Hồ sơ quá thiếu dữ liệu.
* AI không thể tóm tắt.

Hệ thống sẽ:

* Hiển thị cảnh báo.
* Hiển thị hồ sơ gốc.
* Bác sĩ đọc thủ công như quy trình hiện tại.

---

# Phase 5 – Evaluate

## AI Readiness Checklist

* ☑ Có dữ liệu mẫu/logs sạch để test.
* ☑ Rủi ro khi AI sai được kiểm soát bằng Human-in-the-loop và Fallback.
* ☑ Stakeholders (bác sĩ, điều dưỡng) có thể tích hợp AI vào quy trình hiện tại mà không thay đổi toàn bộ hệ thống.

---

# Decision

## ☑ GO (Bắt đầu xây dựng Prototype)

### Justification

Đây là bài toán có mức độ phù hợp cao với LLM vì dữ liệu đầu vào chủ yếu là văn bản y khoa dài và phi cấu trúc. AI không đưa ra quyết định chuyên môn mà chỉ hỗ trợ tóm tắt thông tin để giảm thời gian đọc hồ sơ của bác sĩ.

Giải pháp có rủi ro thấp vì mọi kết quả đều được kiểm tra bởi bác sĩ trước khi sử dụng (Human-in-the-loop). Khi AI không đủ tự tin hoặc phát hiện dữ liệu thiếu, hệ thống sẽ chuyển sang quy trình đọc hồ sơ thủ công (Fallback), đảm bảo không ảnh hưởng đến chất lượng khám chữa bệnh.

Prototype ban đầu chỉ cần tích hợp chức năng tóm tắt hồ sơ bệnh án nên chi phí triển khai thấp, phạm vi hẹp và dễ đánh giá hiệu quả thông qua các chỉ số như thời gian đọc hồ sơ, mức độ hài lòng của bác sĩ và tỷ lệ bỏ sót thông tin quan trọng.
# 02-deep-dive-report.md

# Lab 02 – Deep Dive Report

## Nhóm

**Tên nhóm:** : T081

### Thành viên

| Họ và tên           | MSSV |
| ------------------- | ---- |
| Đinh Xuân Huy       | 1894 |
| Nguyễn Bá Khánh Huy | 1591 |
| Nguyễn Đình Liêm    | 1421 |

---

# Bài toán được lựa chọn

**AI hỗ trợ tóm tắt bệnh án (Medical Record Summarizer) cho bác sĩ tại Vinmec trước khi khám bệnh.**

---

# Phase 3.1 – Current-State Workflow Mapping

## Current Workflow

```text
Bệnh nhân đến khám
        │
        ▼
Điều dưỡng tiếp nhận và cập nhật hồ sơ
        │
        │ 🔄 Handoff
        ▼
Bác sĩ mở toàn bộ bệnh án
        │
        ▼
🔴 Đọc lịch sử khám, toa thuốc, kết quả xét nghiệm
        │
        ▼
Tự tổng hợp thông tin quan trọng
        │
        ▼
Khám và tư vấn bệnh nhân
```

### Thời gian trung bình

| Bước                | Thời gian         |
| ------------------- | ----------------- |
| Tiếp nhận bệnh nhân | 3 phút            |
| Cập nhật hồ sơ      | 2 phút            |
| Đọc bệnh án         | **10–15 phút** 🔴 |
| Tổng hợp thông tin  | 3 phút            |
| Khám bệnh           | 10 phút           |

**Tổng thời gian:** khoảng **28–33 phút/lượt khám**

### Bottleneck

🔴 Bác sĩ phải đọc lượng lớn hồ sơ bệnh án và kết quả xét nghiệm trước khi khám.

---

# Phase 3.2 – Problem Statement (6-field)

| Field                       | Nội dung                                                                                                                                                                                                                                                                                    |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Actor / Operator**     | Bác sĩ và điều dưỡng tại Vinmec.                                                                                                                                                                                                                                                            |
| **2. Current Workflow**     | Điều dưỡng cập nhật hồ sơ bệnh án. Bác sĩ đọc toàn bộ lịch sử khám, kết quả xét nghiệm, toa thuốc và tự tổng hợp trước khi bắt đầu khám.                                                                                                                                                    |
| **3. Bottleneck**           | Việc đọc và tổng hợp bệnh án mất nhiều thời gian, đặc biệt với bệnh nhân có lịch sử điều trị dài.                                                                                                                                                                                           |
| **4. Business Impact**      | Mỗi lượt khám mất thêm khoảng 10–15 phút chỉ để đọc hồ sơ. Điều này làm giảm số lượng bệnh nhân có thể khám mỗi ngày, tăng thời gian chờ và ảnh hưởng trải nghiệm khách hàng.                                                                                                               |
| **5. Success Metric**       | - Giảm thời gian đọc bệnh án từ **15 phút xuống dưới 3 phút**.<br>- Ít nhất **90%** bác sĩ đánh giá bản tóm tắt hữu ích.<br>- Không bỏ sót thông tin quan trọng trong **95%** hồ sơ kiểm thử.                                                                                               |
| **6. Operational Boundary** | AI **chỉ được phép** tóm tắt bệnh án, liệt kê thông tin quan trọng và đánh dấu dữ liệu còn thiếu. AI **không được phép** chẩn đoán bệnh, kê đơn thuốc, đề xuất điều trị hoặc tự thay thế quyết định của bác sĩ. Mọi kết quả phải được bác sĩ xem xét trước khi sử dụng (Human-in-the-loop). |

---

# Phase 3.3 – Future-State Flow & AI Fit

## AI Fit Matrix

**☑ LLM Feature**

### Lý do

* Dữ liệu đầu vào là văn bản y khoa dài và không có cấu trúc.
* Rule-based khó xử lý các cách diễn đạt khác nhau.
* LLM có khả năng tóm tắt và trích xuất thông tin quan trọng hiệu quả hơn.

---

## Future-State Workflow

```text
Bệnh nhân đến khám
        │
        ▼
Điều dưỡng cập nhật hồ sơ
        │
        ▼
🔵 AI đọc toàn bộ bệnh án
        │
        ▼
🔵 AI tạo bản tóm tắt:
    • Tiền sử bệnh
    • Thuốc đang dùng
    • Kết quả xét nghiệm bất thường
    • Các điểm cần lưu ý
        │
        ▼
🟢 Bác sĩ xem lại bản tóm tắt (Human-in-the-loop)
        │
        ├───────────────┐
        │               │
        ▼               ▼
Đúng               Không đúng
        │               │
        ▼               ▼
Khám bệnh      ↩️ Đọc lại hồ sơ gốc
```

---

## Human-in-the-loop (HITL)

Bác sĩ luôn xem và xác nhận bản tóm tắt trước khi sử dụng trong quá trình khám.

---

## Fallback

Nếu:

* AI không đủ tự tin.
* Hồ sơ quá thiếu dữ liệu.
* AI không thể tóm tắt.

Hệ thống sẽ:

* Hiển thị cảnh báo.
* Hiển thị hồ sơ gốc.
* Bác sĩ đọc thủ công như quy trình hiện tại.

---

# Phase 5 – Evaluate

## AI Readiness Checklist

* ☑ Có dữ liệu mẫu/logs sạch để test.
* ☑ Rủi ro khi AI sai được kiểm soát bằng Human-in-the-loop và Fallback.
* ☑ Stakeholders (bác sĩ, điều dưỡng) có thể tích hợp AI vào quy trình hiện tại mà không thay đổi toàn bộ hệ thống.

---

# Decision

## ☑ GO (Bắt đầu xây dựng Prototype)

### Justification

Đây là bài toán có mức độ phù hợp cao với LLM vì dữ liệu đầu vào chủ yếu là văn bản y khoa dài và phi cấu trúc. AI không đưa ra quyết định chuyên môn mà chỉ hỗ trợ tóm tắt thông tin để giảm thời gian đọc hồ sơ của bác sĩ.

Giải pháp có rủi ro thấp vì mọi kết quả đều được kiểm tra bởi bác sĩ trước khi sử dụng (Human-in-the-loop). Khi AI không đủ tự tin hoặc phát hiện dữ liệu thiếu, hệ thống sẽ chuyển sang quy trình đọc hồ sơ thủ công (Fallback), đảm bảo không ảnh hưởng đến chất lượng khám chữa bệnh.

Prototype ban đầu chỉ cần tích hợp chức năng tóm tắt hồ sơ bệnh án nên chi phí triển khai thấp, phạm vi hẹp và dễ đánh giá hiệu quả thông qua các chỉ số như thời gian đọc hồ sơ, mức độ hài lòng của bác sĩ và tỷ lệ bỏ sót thông tin quan trọng.
