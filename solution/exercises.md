# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
Khi tăng temperature từ 0 lên 1.5, câu trả lời có xu hướng đa dạng và sáng tạo hơn. Các phản hồi đều cung cấp thông tin về một đối tượng cụ thể, nhưng khi temperature tăng, mô hình bổ sung thêm nhiều chi tiết và thay đổi cách diễn đạt. Đến temperature 1.5, mô hình chuyển sang một chủ đề khác và đưa thêm yếu tố truyền thuyết, cho thấy mức độ sáng tạo và biến thiên trong câu trả lời cao hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
Tôi sẽ đặt temperature ở mức thấp, khoảng 0.0-0.3 vì hỗ trợ khách hàng thì thông tin đưa ra phải chính xác, ổn định và nhất quán giữa các lần hỏi, hạn chế thêm thắt thông tin hoặc diễn đạt quá sáng tạo.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
Theo giá hiện tại, GPT-4o là $10 cho 1 triệu output tokens, còn GPT-4o mini là $0,60 cho 1 triệu output tokens.
Suy ra, GPT-4o đắt hơn GPT-4o-mini khoảng 16.7 lần (10 / 0.6) cho workload này.
GPT-4o phù hợp với các tác vụ phức tạp, cần suy luận hoặc độ chính xác cao, ví dụ như phân tích một bài toán và đưa ra lời giải chi tiết.
GPT-4o-mini phù hợp với các tác vụ đơn giản, cần tốc độ phản hồi nhanh và chi phí thấp, ví dụ như chatbot hỏi đáp thông thường.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
Với system prompt thứ nhất, câu trả lời ngắn, từ vựng đơn giản, dễ hiểu với trẻ 8 tuổi, dùng hình ảnh quen thuộc (cuốn sổ) để minh hoạ blockchain. Với system prompt thứ hai, câu trả lời dài và chuyên sâu hơn, sử dụng các thuật ngữ kỹ thuật mà người có base mới có thể hiểu được. System prompt định hướng đối tượng người đọc, mức độ chi tiết, cách dùng từ và cách trình bày của model, từ đó làm thay đổi cách model xây dựng phản hồi.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
Với đoạn text 101 từ, tiktoken cho kết quả 125, còn ước lượng theo Part 1 cho khoảng 134 tokens, chênh nhau khoảng 7.7%. Tiếng Việt thường tốn nhiều token hơn Tiếng Anh cùng độ dài vì từ có dấu, cách ghép âm tiết và cách tokenizer phân tách từ và ký tự tiếng Việt có thể tạo ra nhiều token hơn.
---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
Streaming quan trọng khi câu trả lời dài hoặc người dùng cần nhận phản hồi ngay, chẳng hạn như chatbot tư vấn, trợ lý AI hoặc tạo nội dung. Người dùng có thể bắt đầu đọc trong khi model vẫn đang sinh phần còn lại, giúp trải nghiệm nhanh hơn, giảm cảm giác chờ đợi. Non-streaming phù hợp hơn khi ứng dụng cần nhận toàn bộ kết quả rồi mới xử lý tiếp, chẳng hạn như các tác vụ backend, lưu kết quả vào cơ sở dữ liệu hoặc xử lý tự động mà người dùng không cần theo dõi quá trình sinh nội dung.


### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
Exponential backoff có lợi thế vì khi API bị quá tải, mỗi lần retry sẽ chờ lâu hơn (ví dụ 1s → 2s → 4s → 8s), giúp giảm áp lực lên server và tăng cơ hội request thành công. Nếu hàng nghìn client cùng retry với delay cố định 1 giây, chúng có thể retry đồng loạt, tạo ra một đợt request mới đúng lúc server đang quá tải, khiến tình trạng quá tải kéo dài hoặc nghiêm trọng hơn.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
Persona tôi chọn là: "Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt." Cụm "trợ giảng thân thiện" định hướng model trả lời gần gũi, phù hợp cho người học, "trả lời ngắn gọn" giúp tiết kiệm thời gian và token.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
Hạn chế lớn nhất của trợ lý là chỉ lưu được ba lượt hội thoại gần nhất, nên model có thể quên những thông tin quan trọng ở các lượt trước. Một cải thiện cụ thể là lưu các thông tin quan trọng vào cơ sở dữ liệu hoặc bộ nhớ dài hạn. Trước mỗi lượt gọi API, hệ thống có thể tìm kiếm những thông tin liên quan rồi thêm chúng vào messages, giúp trợ lý duy trì ngữ cảnh lâu dài mà không cần gửi toàn bộ lịch sử.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
