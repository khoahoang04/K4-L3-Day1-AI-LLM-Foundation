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
> 
* Ký hiệu temperature = t
- Khi tăng t từ 0.0 lên 1.5: Độ ngẫu nhiên và tính sáng tạo của phản hồi tăng dần, 
tính nhất quán và độ chính xác giảm đi. 
- Ở mức 0.0 và 0.5: Câu trả lời mang tính chính xác, lặp lại các sự thật phổ biến 
gần gũi với user và lập luận rất chặt chẽ. 
- Ở mức 1.0: Cách diễn đạt đa dạng và phong phú hơn so với mức < 1.0.
- Set ở mức 1.5: Cách diễn đạt trở nên bất thường, lan man, ngữ pháp kém tự nhiên 
và có nguy cơ bị hallucination.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> 
- Mình sẽ đặt temperature ở mức thấp, từ 0.0 đến 0.3 (Càng thấp càng tốt)
- Lý do: Chatbot hỗ trợ khách hàng đòi hỏi tính chính xác, nhất quán và tuân 
thủ dữ liệu ở mức cao nhất, temperature thấp giúp triệt tiêu độ ngẫu nhiên, 
giảm thiểu tối đa hiện tượng hallucination và tránh việc chatbot tự ý suy diễn 
hoặc bịa đặt chính sách của doanh nghiệp hoặc công ty. Vì vậy nên câu trả lời 
sẽ luôn đồng nhất giữa các khách hàng và sẽ bám sát tài liệu được cung cấp

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> 
- Với giá output 0.010 USD/1K token của GPT-4o so với 0.0006 USD/1K token của 
GPT-4o-mini, GPT-4o đắt hơn khoảng 16,7 lần (cho 10,5 triệu token output/ngày, 
chi phí là ~105 USD/ngày đối với 4o so với chỉ ~6,3 USD/ngày đối với mini)
- Nên dùng GPT-4o: Cho các tác vụ suy luận đa bước phức tạp, phân tích tài chính/pháp 
lý hoặc sinh mã nguồn quy mô lớn, nơi đòi hỏi độ chính xác cao và sai sót mang lại 
rủi ro lớn
- Nên dùng GPT-4o-mini: Cho các tác vụ khối lượng lớn như chatbot tra cứu FAQ 
cơ bản, trích xuất dữ liệu JSON, phân loại đơn giản hoặc tóm tắt ngắn để tối 
ưu chi phí và giảm độ trễ cho user

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> 
- Hai phản hồi có sự phân hóa rõ rệt: 
+ Prompt giáo viên dùng câu ngắn, từ ngữ rất giản dị, gần gũi và quen thuộc 
+ Prompt chuyên gia tài chính dùng văn phong học thuật, câu dài với nhiều thuật 
ngữ kỹ thuật chuyên sâu
- Vì vậy ta có thể hiểu: System prompt đóng vai trò như một bộ khung ngữ cảnh, 
định hình trực tiếp không gian từ vựng, mức độ chi tiết và cách nói của model, 
tuy là cùng một câu hỏi và cùng một tri thức nền tảng, model có thể tự động điều 
chỉnh cách truyền tải và lựa chọn ví dụ phù hợp đối với từng nhóm đối tượng mục tiêu

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> 
- Với đoạn văn mẫu 100 từ tiếng Việt, công thức ước lượng thô (số từ / 0.75) 
cho kết quả khoảng 133 token, trong khi count_tokens (tiktoken với model gpt-4o) 
đếm được 138 token (chênh lệch khoảng 3,6%)
- Theo cá nhân mình tiếng Việt tốn nhiều token hơn tiếng Anh vì:
+ Thuật toán tokenizer được tối ưu dựa trên dữ liệu tiếng Anh, khiến các từ 
tiếng Anh thường là 1 token nguyên vẹn
+ Tiếng Việt có các ký tự chứa dấu, khiến bộ mã hóa không nhận diện được trọn 
vẹn từ mà phải phân tách thành nhiều subwords hoặc các byte riêng lẻ

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> 
- Steaming phù hợp nhất trong trường hợp khi các ứng dụng có giao diện tương 
tác trực tiếp với người dùng (như chatbot hội thoại), nơi việc hiển thị tức 
thì từng token giúp tối ưu thời gian phản hồi đầu tiên và loại bỏ cảm giác chờ đợi
- Non-streaming phù hợp hơn cho các tác vụ background/batch jobs, giao tiếp 
machine - machine khi cần trích xuất dữ liệu có cấu trúc hoàn chỉnh (JSON, XML) 
để code phân tích cú pháp, hoặc khi cần kiểm duyệt an toàn toàn bộ nội dung trước 
khinchuyển

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> 
- So với delay cố định, exponential backoff dãn dần khoảng cách giữa các lần 
thử lại theo cấp số nhân, giúp giảm áp lực tần suất request và cho server đủ 
"khoảng thở" để xử lý tắc nghẽn và tự phục hồi tài nguyên.Nếu hàng nghìn client 
cùng retry với một khoảng thời gian cố định giống nhau (ví dụ 1 giây), hiện tượng 
"bão retry" sẽ xảy ra: toàn bộ client sẽ đồng loạt gửi request dồn dập vào cùng 
một thời điểm, tạo thành các đợt sóng tải chu kỳ đánh sập server liên tục khiến 
hệ thống không thể hồi phục

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> 
- Persona lựa chọn: Trợ giảng thân thiện của khóa học AI.
- System prompt: "Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn 
bằng tiếng Việt."
- Giải thích 2 lựa chọn từ ngữ quan trọng:
+ "trả lời ngắn gọn": Giúp kiểm soát độ dài câu trả lời, tiết kiệm token output, 
giảm thời gian chờ (latency) khi stream trên terminal CLI và tránh để model 
giải thích lan man gây quá tải thông tin cho người học 
+ "bằng tiếng Việt": Cố định ngôn ngữ phản hồi thống nhất, ngăn model tự động 
nhảy sang tiếng Anh khi câu hỏi của người dùng có chứa các thuật ngữ lập trình hay 
kỹ thuật (như token, prompt, latency, backoff, streaming)

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> 
- Hạn chế lớn nhất: Trợ lý bị cắt cứng lịch sử hội thoại với n lượt nhỏ 
gần nhất, khi cuộc trò chuyện kéo dài, toàn bộ thông tin bối cảnh ban đầu 
của người dùng sẽ bị quên sạch
- Đề xuất cải thiện: Triển khai cơ chế tóm tắt ngữ cảnh tự động
- Mô tả cách triển khai:
1. Khi lịch sử vượt quá 3 lượt, thay vì xóa bỏ, ta gửi các lượt chat cũ vào 
gpt-4o-mini để tóm tắt các sự kiện/thông tin chính thành 1–2 câu ngắn
2. Chèn bản tóm tắt này vào ngay sau System Prompt để làm ngữ cảnh nền tảng, 
sau đó chỉ nối thêm 2 lượt chat gần nhất cùng tin nhắn mới của người dùng
3. Nhờ đó, trợ lý vừa ghi nhớ được bối cảnh xuyên suốt phiên trò chuyện, 
vừa giữ lượng token input nhỏ gọn để tiết kiệm chi phí

---

## Danh Sách Kiểm Tra Nộp Bài

- [x] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [x] Cả 4 checkpoint pytest đều pass
- [x] Tất cả 9 câu trong file này đã được trả lời
- [x] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
