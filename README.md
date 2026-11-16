Trích xuất và định dạng thông tin tuyển dụng
1. Mục tiêu
Hiểu được cách áp dụng khung cấu trúc CREATE để viết prompt rõ ràng, chuyên nghiệp.
Biết cách sử dụng kỹ thuật Few-shot Prompting để hướng dẫn AI định dạng dữ liệu đầu ra chính xác.
Biết cách ép AI trả về dữ liệu thô dạng JSON và bảng biểu Markdown phục vụ cho việc tích hợp hệ thống.
2. Bối cảnh bài toán
Bộ phận Tuyển dụng (HR) của công ty công nghệ nhận được rất nhiều email ứng tuyển của sinh viên. Mỗi email viết theo một phong cách tự do khác nhau, khiến việc nhập liệu và tổng hợp thông tin ứng viên thủ công mất rất nhiều thời gian.
Bạn cần thiết kế một Prompt hệ thống giúp AI tự động trích xuất thông tin quan trọng từ các email thô này thành các cấu trúc dữ liệu chuẩn hóa để nạp vào hệ thống quản lý tuyển dụng.
3. Yêu cầu
Xây dựng một Prompt duy nhất có khả năng xử lý một đoạn email ứng tuyển thô và trả về đồng thời:
•	Một bảng Markdown tóm tắt thông tin ứng viên.
•	Một mảng JSON thô (raw JSON) chứa thông tin ứng viên để nạp vào database.
4. Quy tắc xử lý
Sử dụng khung cấu trúc CREATE:
•	C - Context: Đóng vai trò là Chuyên viên xử lý dữ liệu nhân sự cấp cao.
•	R - Request: Trích xuất các trường thông tin: Họ tên, Số điện thoại, Email, Vị trí ứng tuyển, Năm kinh nghiệm, Kỹ năng chính.
•	E - Explanation: Dữ liệu này sẽ dùng để import trực tiếp vào database tuyển dụng nên cần độ chính xác 100%.
•	A - Action: Quét qua email thô, xác định đúng các trường thông tin, phân loại kỹ năng thành một danh sách.
•	T - Tone: Chính xác, khách quan, không thêm thắt văn phong cảm xúc.
•	E - Extras: Cung cấp ít nhất 2 ví dụ Few-shot mẫu (cặp dữ liệu Input email thô -> Output mong muốn) để định hình kết quả của AI.
5. Yêu cầu xử lý dữ liệu
Trích xuất và chuẩn hóa các thông tin sau:
•	Họ tên: Chuyển về dạng viết hoa chữ cái đầu (Ví dụ: Nguyễn Văn A).
•	Số điện thoại: Chuẩn hóa về định dạng đầu số quốc gia hoặc loại bỏ dấu cách dư thừa.
•	Kỹ năng chính: Gom nhóm thành một danh sách không trùng lặp (ví dụ: ['Python', 'SQL', 'FastAPI']).
•	Trả về định dạng JSON thuần túy theo cấu trúc:
  { "name": "...", "phone": "...", "email": "...", "position": "...", "experience_years": 2, "skills": [...] }
6. Ràng buộc kỹ thuật
Không được tự ý thêm thắt hoặc ảo giác các thông tin không có trong email của ứng viên.
Nếu thông tin nào bị thiếu (ví dụ không ghi số điện thoại), phải để giá trị là null trong JSON, không được tự điền bừa.
Chỉ trả về dữ liệu kết quả, nghiêm cấm AI viết thêm các đoạn văn giải thích ở đầu hoặc cuối câu trả lời.
Kiểm tra tính đúng đắn của cấu trúc JSON bằng các công cụ Validate JSON.


Prompt:
Bạn là HR của một công ty công nghệ lớn,nay bạn sẽ phải xử lý email ứng tuyển sinh viên một cách chuyên nghiệp.
Xử lý email và chuyển thành một bảng Markdown tóm tắt thông tin ứng viên,ột mảng JSON thô (raw JSON) chứa thông tin ứng viên để nạp vào database đối với mỗi email ứng tuyển bằng cách trích xuất Họ tên, Số điện thoại, Email, Vị trí ứng tuyển, Năm kinh nghiệm, Kỹ năng chính.Dữ liệu này sẽ dùng để nhập trực tiếp vào database tuyển dụng nên cần độ chính xác cao và đạt chuẩn.
Hãy quét qua dữ liệu của từng email xác định và đọc thông tin,phân loại các kỹ năng theo mức độ thực tiễn và khả năng xử lý dự án thành 1 danh sách.
Dùng tone giọng khách quan,rõ ràng và mang tính chuyên nghiệp,không chèn các câu từ cảm xúc.
Input:email ứng tuyển thô
Output mong muốn:1 bảng markdown rõ ràng dễ nhận biết và 1 mảng json thô được nhập và database cho từng ứng viên ứng tuyển.
Ràng buộc:
Họ tên buộc viết hoa chữ cái đầu (vd:Nguyễn Văn A)
Chuẩn hóa đầu số quốc gia hoặc loại bỏ các dấu cách dư thừa.
Các kỹ năng sẽ gom thành các nhóm phân loại không trùng lặp.
Trả thành các mảnh json thuần với định dạng: { "name": "...", "phone": "...", "email": "...", "position": "...", "experience_years": 2, "skills": [...] }
Không tự ý thêm hoặc bớt bất kỳ thông tin nào trong email,khi dữ liệu trống thì hãy để giá trị null cho mảng json và tuyệt đối không điền bừa.
Chỉ yêu cầu kết quả dữ liệu thuần túy,không được phép thêm các phần giải thích ở đầu và cuối
Kiểm tra tính đúng đắn của mảng json bằng các công cụ Validate JSON.

