## Bài Học

- Bài lớn mà sơ sài ko giải quyết dc việc không bao giờ bằng Bài nhỏ mà giải quyết được việc, 
- Code thật nhanh làm sao giải quyết được việc ngay (Hoàn thành trước hoàn hảo sau) 
- Prompt phải thật sát mô tả chi tiết và đưa ngữ cảnh tốt cho AI đọc (như code mẫu, file md hướng dẫn)

## Các bước thực hiện để sử dụng AI code theo bài học trên

### 1. Phân tích chi tiết yêu cầu chức năng
- Nghiên cứu thật kỹ các chức năng chính của dự án.
- Viết riêng file markdown yêu cầu chức năng.
- File này phải bám đúng nguyên tắc viết yêu cầu chức năng:
  - gắn từng chức năng với actor cụ thể,
  - làm rõ entity liên quan,
  - làm rõ permission và ownership,
  - tách chức năng lõi và chức năng phụ,
  - ghi rõ actor nào nên build trước,
  - không mô tả mơ hồ,
  - không tự ý thêm feature.
- Mục tiêu là tạo ngữ cảnh thật chính xác cho AI.

### 2. Xác định ngôn ngữ và tech stack phù hợp
- Chọn ngôn ngữ, framework, database, auth, UI library, state management, validation, ORM và công cụ test.
- Tech stack phải phù hợp với:
  - loại dự án,
  - tốc độ phát triển,
  - khả năng tái sử dụng,
  - hệ sinh thái có sẵn,
  - mức độ dễ sinh code bằng AI.
- Ưu tiên stack phổ biến, có nhiều tài liệu và nhiều code mẫu.

### 3. Xây dựng cấu trúc thư mục và quy tắc code
- Tạo cấu trúc thư mục rõ ràng ngay từ đầu.
- Tạo file rules code để giữ đúng scope, architecture, contracts, testing, documentation và collaboration.
- Phải xác định rõ:
  - route layer,
  - UI layer,
  - business layer,
  - data access layer,
  - auth và permission layer.
- Không code tự phát khi chưa có nền cấu trúc.

### 4. Xây dựng UI hệ thống và layout cho từng tác nhân
- Xác định các tác nhân chính như guest, user, admin hoặc actor khác.
- Tạo layout riêng cho từng tác nhân nếu khác quyền hoặc khác luồng nghiệp vụ.
- Ưu tiên dùng UI có sẵn:
  - template có sẵn,
  - component library có sẵn,
  - design blocks có sẵn,
  - admin dashboard UI có sẵn,
  - form và table có sẵn.
- Mục tiêu là làm nhanh, đồng bộ và giảm lỗi UI.

### 5. Xây dựng từng chức năng, làm từng trang một
- Không build toàn bộ dự án trong một lần.
- Làm từng trang, từng flow, từng chức năng một.
- Với mỗi chức năng:
  - copy đúng phần yêu cầu chức năng của chức năng đó,
  - đưa vào AI khác như GPT, Gemini hoặc prompt engine riêng,
  - có thể dùng super prompt đã chuẩn hóa,
  - yêu cầu AI chỉ code đúng phạm vi chức năng đó.
- Ngữ cảnh càng chính xác, code càng ít lỗi.

### 6. Check lại ngay sau khi xong từng trang
- Sau khi build xong một trang hoặc một chức năng:
  - kiểm tra UI,
  - kiểm tra logic,
  - kiểm tra validation,
  - kiểm tra auth và permission,
  - sửa lỗi ngay nếu có.
- Không dồn quá nhiều trang rồi mới check.

### 7. Đẩy Git ngay khi từng phần đã ổn
- Làm đến đâu ổn đến đó thì commit và push Git luôn.
- Không để quá nhiều thay đổi dồn cục.
- Mỗi commit nên bám đúng một phần rõ ràng như:
  - auth foundation,
  - admin layout,
  - user profile page,
  - order list page.
- Cách này giúp rollback nhanh và dễ kiểm soát tiến độ.

## Nguyên tắc tăng tốc quan trọng

### Tận dụng tối đa thứ có sẵn
- Mục tiêu là xây dự án nhanh nhất có thể.
- Nếu trên mạng đã có code giao diện phù hợp thì ưu tiên lấy về dùng.
- Nếu trên mạng đã có code chức năng gần giống thì ưu tiên tham khảo, tái sử dụng hoặc đưa cho AI đọc để code lại đúng theo dự án.
- Nếu đã có UI kit, dashboard template, form mẫu, auth mẫu, table mẫu, CRUD mẫu thì phải tận dụng tối đa.
- Không nên tự viết mọi thứ từ đầu nếu đã có thứ đủ tốt để dùng.

### Nhắc lại nguyên tắc cốt lõi
- Nếu đã có sẵn trên mạng thì nên bê về dùng luôn hoặc cho AI đọc để code lại.
- Làm đến dâu chắc đến đấy phải đẩy lên git luôn để lưu trữ code ngon và discard nếu code rối và lỗi.
- Luôn phải đáp ứng đủ ngữ cảnh cho AI ko được mơ hồ (cái này làm nhiều sẽ ngộ ra)

### Ngữ cảnh là yếu tố quan trọng nhất
- Điều quan trọng nhất là ngữ cảnh đưa vào AI phải thật chính xác.
- AI càng có đúng context về:
  - actor,
  - chức năng,
  - permission,
  - entity,
  - layout,
  - contract,
  - stack,
  - cấu trúc thư mục,
  - rules code,
thì code sinh ra càng ít lỗi, ít lệch scope và ít phải sửa lại.

## Tóm tắt 1 dòng

Phân tích yêu cầu thật kỹ -> chọn tech stack phù hợp -> dựng cấu trúc và rules -> dựng UI và layout theo actor -> build từng chức năng, từng trang một bằng prompt đúng ngữ cảnh -> check xong trang nào thì sửa ngay và push Git luôn -> tận dụng tối đa code có sẵn trên mạng để làm nhanh nhất.