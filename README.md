# CNTT1-K-N-ng-Promting-v-i-AI-Session01_Ex09
## Phần 1: Phân tích nguyên nhân AI bị "ảo giác" (Hallucination)
Lý do ChatGPT trả lời một cách tự tin nhưng hoàn toàn sai lệch (phạt 20% và đình chỉ 30 ngày) xuất phát từ các điểm sau:

Thiếu dữ liệu nguồn (Grounding): Do nhân viên hỏi cộc lốc mà không đưa kèm hợp đồng thực tế, AI hoàn toàn "mù tịt" về nội quy riêng của MartX.

Cơ chế đoán mò theo xác suất: Vì bản chất của AI là sinh văn bản dựa trên xác suất từ kho dữ liệu khổng lồ trên Internet (nơi có hàng nghìn quy chế phạt khác nhau của các công ty ngoài thị trường), nó tự động bốc đại những con số phổ biến nhất để lấp đầy khoảng trống thông tin.

Thái độ "nói dối như thật": Mô hình được thiết kế để luôn đưa ra câu trả lời trôi chảy, mạch lạc nên dù không biết chính xác, nó vẫn phỏng đoán với một giọng điệu cực kỳ chắc nịch, khiến người dùng dễ bị lừa và gây ra hậu quả nghiêm trọng trong công việc.

## Phần 2: Cấu trúc RAG Prompt chuẩn Grounding
Để ép AI phải bám sát tài liệu gốc và chặn đứng tình trạng ảo giác, đây là khung prompt chuẩn được xây dựng gồm 3 phần rõ ràng:

Markdown
[1. Văn bản hợp đồng chuẩn]
Trích lục quy định MartX: "Điều 8. Khoản 2: Trường hợp nhà cung cấp giao hàng rau củ bị dập nát, hư hỏng vượt quá tỉ lệ cho phép, MartX áp dụng mức phạt 5% giá trị lô bị dập nát và yêu cầu nhà cung cấp thực hiện đổi hàng mới trong vòng 24 giờ kể từ thời điểm lập biên bản."

[2. Câu hỏi nghiệp vụ]
Theo quy định chính thức của MartX, nhà cung cấp giao rau bị dập nát trên 10% thì bị phạt bao nhiêu và xử lý thế nào?

[3. Negative Constraint - Khóa an toàn]
BẮT BUỘC: Chỉ được phép dùng đúng thông tin có trong mục [1. Văn bản hợp đồng chuẩn] ở trên để trả lời. Tuyệt đối không tự suy đoán, không dùng kiến thức bên ngoài hay bịa thêm bất kỳ con số nào khác. Nếu thông tin không có trong văn bản, phải ghi đúng câu: "Không có dữ liệu trong tài liệu".
