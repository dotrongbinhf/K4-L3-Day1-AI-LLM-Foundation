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

> Mức độ sáng tạo trong cách diễn đạt nội dung tăng dần theo temparature tăng từ 0.0 lên 1.5. Temparature thấp hơn cũng cho ra kết quả ổn định, tương tự nhau hơn giữa các lần thử nghiệm. Nội dung của câu trả lời thì vẫn giống nhau qua các lần chạy với temparature khác nhau, vẫn nói về hang Sơn Đoòng.

### Câu 1.2 — Chọn temperature cho sản phẩm

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**

> Temparature sẽ đặt trong khoảng 0.2-0.4 hoặc hẹp hơn, sẽ là thiên về thấp nhưng không hẳn là thấp hẳn về 0.0 để Chatbot trả lời với độ chính xác cao, ổn định nhưng vẫn đủ linh hoạt để diễn đạt tự nhiên và thân thiện.

### Câu 1.3 — Đánh đổi chi phí

Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**

> GPT-4o đắt hơn GPT-4o-mini khoảng 16,67 lần do chi phí input và output của GPT-4o đều đắt hơn GPT-4o-mini 16,67 lần. GPT-4o sẽ phù hợp hơn khi xử lý các yêu cầu phức tạp, cần suy luận và hiểu ngữ cảnh như giải quyết khiếu nại của khách hàng có nhiều bước, còn với GPT-4o-mini sẽ phù hợp cho các tác vụ đơn giản, lặp lại không cần suy luận nhiều như việc FAQ, tra cứu chính sách.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona

Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:

- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)

> Phản hồi dành cho trẻ 8 tuổi thì ngắn gọn hơn, từ ngữ đơn giản và gần gũi hơn. Phản hồi với vai trò là chuyên gia tài chính thì dài hơn, chi tiết hơn, kỹ thuật hơn. System Prompt định hướng vai trò, cách diễn đạt, mức độ chuyên môn trong câu trả lời để phù hợp với đối tượng được yêu cầu trả lời.

### Câu 2.2 — tiktoken vs đếm từ

Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**

> Ước lượng nhiều hơn khoảng 10% so với tính theo count_tokens. Tiếng Việt thường tốn nhiều token hơn tiếng Anh cùng đồ dài do tiếng Việt có những ký tự unicode, có dấu, một từ có nghĩa cũng có thể được ghép từ hai chữ ngăn cách nhau bằng khoảng trắng nên tokenizer thường tách thành nhiều token nhỏ hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming

**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)

> Streaming phù hợp nhất trong trường hợp câu trả lời dài, điều đấy sẽ làm tăng trải nghiệm người dùng, họ sẽ thấy câu trả lời từ từ xuất hiện và biết rằng đang được xử lý thay vì đợi một lúc mới hiện ra kết quả, điều đó có thể khiến họ thấy khó chịu hoặc thấy rằng yêu cầu trò chuyện của họ đang không được xử lý,... Non-streaming phù hợp hơn trong những trường hợp cần nhận toàn bộ kết quả một lần để xử lý tiếp như trả về đối tượng JSON để phân tích dữ liệu.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?

**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**

> Exponential backoff sẽ giúp tăng dần thời gian chờ sau mỗi lần retry, giảm áp lực cho server đang quá tải và tăng thời gian để có thể hồi phục trở lại. Nếu hàng nghìn client cùng retry và delay cố định giống nhau thì đến khi retry được gửi lại, cũng sẽ là hàng nghìn request gửi đến server có thể sẽ tiếp tục gây ra tình trạng quá tải cho server.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona

**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**

> Persona được chọn là chuyên gia hướng dẫn viên du lịch thân thiện. System Prompt: "Bạn là chuyên gia hướng dẫn viên du lịch thân thiện, trả lời ngắn gọn bằng tiếng Việt. Hãy gợi ý lịch trình, địa điểm, món ăn và lưu ý thực tế theo nhu cầu của người dùng." Cụm từ "chuyên gia hướng dẫn viên du lịch" để trợ lý tập trung vào việc tư vấn chuyến đi, đề xuất địa điểm, lịch trình, chi phí, trải nghiệm phù hợp. Cụm từ "ngắn gọn bằng tiếng Việt" cũng giúp câu trả lời dễ đọc, đúng ngữ cảnh hơn.

### Câu 4.2 — Hạn chế & cải thiện

**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**

> Hạn chế lớn nhất là trợ lý chỉ giữ 3 lượt hội thoại gần nhất trong history, nên khi cuộc trò chuyện dài thì trợ lý sẽ quên đi các nội dung cũ mục tiêu là để giảm input token. Cải tiến có thể là thêm bộ nhớ tóm tắt cho trợ lý, trước khi cắt lấy history = history[-6:] thì nếu như history quá dài thì sẽ gọi model để tóm tắt thành một đoạn ngắn và lưu vào một biến summary, mỗi lần gọi tiếp theo sẽ đi kèm với summary để trợ lý vẫn có thể nhớ được tổng quát nội dung cuộc trò chuyện đang được diễn ra, giảm tình trạng quên đi các nội dung quá xa.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
