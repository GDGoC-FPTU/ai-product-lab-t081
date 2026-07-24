# 03-ai-log.md

# AI Log & Reflection

**Họ và tên:** Nguyễn Bá Khánh Huy

**MSSV:** 2A202601591

**Nhóm:** T081

---

## AI giúp gì?

Trong buổi lab này, tôi đã sử dụng mô hình LLM như một **thought-partner** để phân tích và đánh giá các điểm nghẽn (bottleneck) trong vận hành của Vin Smart Future. Ban đầu, tôi yêu cầu AI đề xuất các bài toán có thể áp dụng dữ liệu lớn và AI tạo sinh. AI đã gợi ý rất chi tiết các case study thuộc hệ sinh thái Vingroup (từ Computer Vision cho VinFast đến RecSys cho Xanh SM). 

Sau khi cân nhắc, tôi quyết định chọn đào sâu vào bài toán **Hệ thống Multi-Agent RAG hỗ trợ tra cứu pháp lý và giải quyết khiếu nại tại Vinhomes**. AI đã hỗ trợ tôi rà soát các bước trong Current Workflow (quy trình thủ công hiện tại của ban quản lý) và định hình Future-State Flow. Đặc biệt, trong khâu lập trình Prompt Prototype, AI đã gợi ý cấu trúc System Prompt để giới hạn hành vi của mô hình và cung cấp các kịch bản kiểm thử (adversarial test cases) khá hóc búa để thử thách các ranh giới an toàn.

---

## AI sai gì?

Trong quá trình thiết kế giải pháp cho bài toán pháp lý của Vinhomes, AI ban đầu đề xuất một luồng tự động hóa hoàn toàn: hệ thống đọc ticket khiếu nại, tổng hợp thông tin, ra quyết định và **gửi thẳng email phản hồi** cho cư dân. Đây là một rủi ro cực kỳ lớn vì văn bản pháp lý đòi hỏi độ chính xác tuyệt đối; nếu LLM bị ảo giác (hallucination) hoặc trích dẫn sai điều khoản hợp đồng, công ty sẽ đối mặt với rủi ro pháp lý nghiêm trọng.

Bên cạnh đó, AI đề xuất chỉ dùng một mô hình LLM cơ bản (Single-prompt) để đọc toàn bộ tài liệu. Tôi nhận thấy điều này không khả thi vì khối lượng hợp đồng và nội quy là quá lớn, dẫn đến tràn context window và dễ râu ông nọ cắm cằm bà kia (nhầm lẫn giữa hợp đồng của căn hộ A với nội quy chung).

---

## Tôi đã sửa như thế nào?

Để khắc phục những lỗ hổng trên, tôi đã tinh chỉnh lại cấu trúc kiến trúc và ranh giới vận hành (Operational Boundary) như sau:

**1. Thay đổi Kiến trúc kỹ thuật:**
Tôi từ chối phương pháp LLM thông thường và chuyển sang thiết kế **Multi-Agent RAG**. Hệ thống chia làm nhiều tác tử nhỏ: một tác tử chuyên trích xuất hợp đồng riêng của căn hộ, một tác tử tra cứu nội quy chung, và một tác tử tổng hợp câu trả lời. Điều này giúp kiểm soát luồng thông tin chặt chẽ hơn.

**2. Thiết lập Ranh giới vận hành (Operational Boundary):**
Tôi đã viết lại System Prompt bắt buộc hệ thống phải tuân thủ:
* **Chỉ được tạo bản nháp (Draft):** Mọi đầu ra phải gắn tag `[DRAFT_ONLY]` và tuyệt đối không được tự động gửi cho cư dân.
* **Bắt buộc trích dẫn:** AI phải trích dẫn chính xác số trang, số điều khoản từ cơ sở dữ liệu. Nếu không tìm thấy, AI phải kích hoạt Fallback: *"Không tìm thấy quy định rõ ràng, yêu cầu chuyên viên pháp chế rà soát thủ công"*.

**3. Áp dụng Human-in-the-loop (HITL):**
Ban quản lý và chuyên viên pháp chế bắt buộc phải là người đọc lại bản nháp, đối chiếu trích dẫn và nhấn nút phê duyệt cuối cùng. 

Quá trình này giúp tôi củng cố tư duy của một AI Product Engineer: Không phải cứ phó mặc mọi thứ cho AI là tốt, mà giá trị thực sự nằm ở việc chúng ta thiết kế được các **guardrails (ranh giới an toàn)** để AI hoạt động ổn định và kiểm soát được rủi ro trong môi trường doanh nghiệp thực tế.