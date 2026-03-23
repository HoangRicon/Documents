# AI_CODING_WORKFLOW_MASTER

## 1. MỤC TIÊU

Tài liệu này định nghĩa quy trình code bằng AI cho dự án Next.js fullstack sau khi đã có:

- bộ quy tắc code,
- cấu trúc thư mục,
- nguyên tắc phân lớp,
- nguyên tắc bảo toàn scope, architecture, contract, auth, permission.

Tài liệu này dùng để:

- phân tích yêu cầu trước khi code,
- tách rõ yêu cầu chức năng và phi chức năng,
- xác định tác nhân chính của hệ thống,
- quyết định thứ tự ưu tiên build theo tác nhân,
- xác định các thành phần bắt buộc cho từng tác nhân,
- tạo prompt theo từng bước triển khai,
- đảm bảo mọi prompt luôn tuân thủ bộ quy tắc code và cấu trúc thư mục đã thống nhất.

---

## 2. NGUYÊN TẮC GỐC

Mọi bước trong quy trình này phải luôn tuân thủ các nguyên tắc sau:

- không thay đổi scope dự án,
- không tự ý thêm feature mới,
- không viết prompt làm lệch kiến trúc đã chốt,
- không bỏ qua boundary giữa `app`, `components`, `features`, `server`, `lib`, `prisma`,
- không bỏ qua auth, permission, validation, contract, docs,
- không build UI trước khi hiểu rõ tác nhân và luồng nghiệp vụ,
- không code trước khi phân tích kỹ yêu cầu chức năng và phi chức năng,
- không build lẫn admin và user nếu boundary khác nhau,
- không chỉ làm cho chạy; phải làm đúng kiến trúc và đúng contract.

---

## 3. ĐẦU VÀO BẮT BUỘC TRƯỚC KHI CODE

Trước khi bắt đầu quy trình code bằng AI, phải có tối thiểu:

- file rules tổng,
- file cấu trúc thư mục,
- mô tả scope dự án,
- mô tả bài toán nghiệp vụ,
- danh sách chức năng mong muốn,
- danh sách tác nhân dự kiến,
- nếu có: role, permission, entity chính, flow chính.

Nếu đầu vào chưa đầy đủ, vẫn phải phân tích theo intent gốc và ghi assumption rõ ràng.

---

## 4. QUY TRÌNH TỔNG THỂ

Quy trình chuẩn gồm 6 pha:

1. Phân tích yêu cầu
2. Phân tích tác nhân và ưu tiên tác nhân
3. Phân tích kiến trúc thực thi theo tác nhân
4. Xác định thứ tự build
5. Tạo prompt chi tiết theo từng bước
6. Kiểm tra chéo để đảm bảo prompt không lệch rules

---

## 5. PHA 1 — PHÂN TÍCH YÊU CẦU CHỨC NĂNG

### 5.1. Mục tiêu
Phải hiểu thật rõ hệ thống cần làm gì trước khi viết prompt code.

### 5.2. Cách phân tích
Với mỗi chức năng, phải viết rõ:

- tên chức năng,
- mô tả mục đích,
- tác nhân nào sử dụng,
- dữ liệu đầu vào,
- dữ liệu đầu ra,
- điều kiện tiền đề,
- kết quả mong muốn,
- ràng buộc nghiệp vụ,
- quyền truy cập,
- ảnh hưởng tới entity nào,
- API hoặc action nào cần có,
- page hoặc module nào cần có,
- validation nào cần có,
- trạng thái lỗi nào cần xử lý.

### 5.3. Mẫu phân tích chức năng
Mỗi chức năng nên được ghi theo mẫu:

#### Chức năng: `<Tên chức năng>`

- Mục đích:
- Tác nhân sử dụng:
- Điều kiện trước:
- Input:
- Output:
- Luồng chính:
- Luồng lỗi:
- Quyền truy cập:
- Dữ liệu liên quan:
- Page liên quan:
- API / Server Action liên quan:
- Service liên quan:
- Validation liên quan:
- Điều kiện hoàn thành:

### 5.4. Quy tắc phân tích chức năng
- Không mô tả mơ hồ kiểu “quản lý dữ liệu”.
- Phải chỉ rõ quản lý cái gì, bởi ai, ở đâu, theo quyền nào.
- Không gộp nhiều chức năng khác nhau thành một mục quá lớn.
- Nếu một chức năng có admin và user cùng dùng nhưng khác quyền và khác luồng, phải tách thành hai chức năng riêng.
- Nếu chức năng liên quan bảo mật, ownership, hoặc role, phải đánh dấu là high-risk.

---

## 6. PHA 2 — PHÂN TÍCH YÊU CẦU PHI CHỨC NĂNG

### 6.1. Mục tiêu
Xác định hệ thống phải đạt chất lượng gì ngoài chuyện “có chức năng”.

### 6.2. Các nhóm phi chức năng bắt buộc phải phân tích

#### 6.2.1. Kiến trúc
- phải dùng đúng cấu trúc thư mục đã chốt,
- phải tách layer rõ ràng,
- phải tách admin và user nếu khác boundary,
- phải có service và repository khi logic đủ phức tạp,
- phải giữ contract ổn định.

#### 6.2.2. Bảo mật
- route protected nào cần auth,
- route nào cần role admin,
- action nào cần ownership check,
- API nào không được public,
- dữ liệu nào không được expose ở client.

#### 6.2.3. Hiệu năng
- trang nào có thể là server component,
- trang nào bắt buộc là client component,
- dữ liệu nào nên fetch ở server,
- dữ liệu nào có thể cache,
- khu vực nào cần tránh fetch lặp.

#### 6.2.4. Khả năng mở rộng
- feature nào sẽ còn mở rộng sau,
- module nào phải tách domain sớm,
- entity nào là lõi hệ thống nên phải thiết kế ổn định.

#### 6.2.5. Tính dễ bảo trì
- code phải dễ đọc,
- tên module phải rõ,
- boundary phải rõ,
- docs phải cập nhật,
- test phải đủ theo mức độ rủi ro.

#### 6.2.6. UX và giao diện
- mỗi tác nhân có layout riêng hay không,
- navigation riêng hay không,
- loading, error, empty state có cần hay không,
- form validation hiển thị như thế nào.

#### 6.2.7. Dữ liệu và contract
- schema validation nào là nguồn thật,
- API shape chuẩn là gì,
- DB entity nào là core,
- enum hoặc status nào cần thống nhất toàn hệ thống.

### 6.3. Quy tắc phân tích phi chức năng
- Không ghi chung chung kiểu “hệ thống phải bảo mật”.
- Phải chỉ rõ nơi nào cần bảo mật và bảo vệ bằng cách nào.
- Không chỉ nói “cần performance tốt”.
- Phải chỉ rõ trang, luồng, hoặc loại dữ liệu nào cần tối ưu.

---

## 7. PHA 3 — XÁC ĐỊNH TÁC NHÂN CHÍNH

### 7.1. Tác nhân là gì
Tác nhân là nhóm người dùng hoặc hệ thống có quyền và hành vi riêng trong sản phẩm.

### 7.2. Phải xác định rõ cho từng tác nhân
- vai trò của tác nhân,
- mục tiêu chính,
- quyền truy cập,
- màn hình họ dùng,
- dữ liệu họ được phép thấy,
- dữ liệu họ được phép sửa,
- feature cốt lõi họ cần,
- rủi ro nếu làm sai luồng của họ.

### 7.3. Ví dụ tác nhân phổ biến trong Next.js fullstack
- Guest
- User
- Admin
- Moderator
- Staff
- System / Cron / Internal process

### 7.4. Mẫu phân tích tác nhân

#### Tác nhân: `<Tên tác nhân>`

- Vai trò:
- Mục tiêu:
- Quyền chính:
- Route sử dụng:
- Layout riêng:
- Navigation riêng:
- Dữ liệu được xem:
- Dữ liệu được sửa:
- Chức năng chính:
- Mức độ ưu tiên:
- Rủi ro nếu chưa build:

---

## 8. PHA 4 — LÊN KẾ HOẠCH TRIỂN KHAI

### 8.1. Nguyên tắc ưu tiên
Không build theo cảm tính. Phải ưu tiên theo các tiêu chí sau:

#### 8.1.1. Tác nhân quyết định dữ liệu lõi
Nếu một tác nhân tạo, quản lý, hoặc kiểm soát dữ liệu lõi của hệ thống, tác nhân đó thường cần được build trước.

#### 8.1.2. Tác nhân quyết định permission boundary
Nếu toàn bộ role và auth phụ thuộc tác nhân đó, phải build sớm để chốt boundary.

#### 8.1.3. Tác nhân quyết định luồng nghiệp vụ chính
Nếu sản phẩm tồn tại chủ yếu để phục vụ một tác nhân chính, tác nhân đó được ưu tiên trước.

#### 8.1.4. Tác nhân quản trị hệ thống
Admin không phải lúc nào cũng build trước. Chỉ build trước nếu:
- admin là nơi tạo dữ liệu lõi,
- admin là nơi quản lý lifecycle dữ liệu,
- user side phụ thuộc dữ liệu do admin tạo.

#### 8.1.5. Tác nhân người dùng cuối
User nên build trước nếu:
- giá trị chính của sản phẩm nằm ở trải nghiệm người dùng cuối,
- admin chỉ là hỗ trợ quản trị phụ,
- business flow chủ yếu nằm ở user flow.

### 8.2. Cách chọn thứ tự
Phải trả lời các câu hỏi sau:

- ai tạo dữ liệu lõi đầu tiên,
- ai là người dùng quan trọng nhất của sản phẩm,
- ai có flow quan trọng nhất cần validate sớm,
- side nào quyết định contract trước,
- side nào nếu build sai sẽ làm lệch toàn bộ kiến trúc.

### 8.3. Quy tắc thực tế
- Auth foundation thường build trước.
- Role và permission foundation build trước.
- Layout theo tác nhân build trước phần feature sâu.
- Entity lõi và contract build trước dashboard phụ.
- Luồng CRUD lõi build trước báo cáo.
- Feature phục vụ ownership và security build trước tiện ích phụ.

---

## 9. PHA 5 — XÁC ĐỊNH NHỮNG THỨ BẮT BUỘC CẦN CÓ CHO MỖI TÁC NHÂN

Với mỗi tác nhân, trước khi code feature sâu, phải xác định đầy đủ các thành phần nền.

### 9.1. Foundation bắt buộc
- role definition,
- permission rules,
- auth access rule,
- route group,
- layout riêng nếu cần,
- navigation riêng nếu cần,
- dashboard entry nếu cần,
- guards cần thiết,
- schema validation,
- service layer,
- repository layer nếu chạm DB,
- docs ngắn cho boundary của tác nhân đó.

### 9.2. Các câu hỏi bắt buộc cho từng tác nhân
- tác nhân này có route group riêng không,
- có layout riêng không,
- có sidebar/header riêng không,
- có dashboard riêng không,
- có entity riêng không,
- có permission riêng không,
- có guard riêng không,
- có API riêng không,
- có action riêng không,
- có feature folder riêng không,
- có component riêng không,
- có form riêng không,
- có error state riêng không.

### 9.3. Ví dụ suy nghĩ đúng
Nếu build `Admin` trước, thường phải có:
- `/admin` route group,
- `admin/layout.tsx`,
- admin navigation,
- `requireAdmin`,
- admin service boundary,
- module quản lý entity lõi.

Nếu build `User` trước, thường phải có:
- `(user)` route group,
- `(user)/layout.tsx`,
- user dashboard entry,
- `requireAuth`,
- user ownership rule,
- user-facing feature modules.

---

## 10. PHA 6 — THỨ TỰ BUILD KHUYẾN NGHỊ

Thứ tự tổng quát nên đi như sau:

### Bước 1. Phân tích yêu cầu
- phân tích chức năng,
- phân tích phi chức năng,
- phân tích tác nhân,
- xác định entity lõi,
- xác định boundary auth và permission.

### Bước 2. Dựng nền kiến trúc
- xác nhận cấu trúc thư mục,
- xác nhận route groups,
- xác nhận shared layers,
- xác nhận naming conventions.

### Bước 3. Dựng nền dữ liệu và contract
- model hoặc schema lõi,
- enum lõi,
- shared types,
- validation schema,
- response conventions.

### Bước 4. Dựng auth và permission
- auth config,
- session contract,
- role contract,
- middleware,
- guards.

### Bước 5. Dựng tác nhân ưu tiên nhất
- layout riêng,
- navigation riêng,
- dashboard entry,
- feature lõi,
- API hoặc action lõi,
- service và repository liên quan.

### Bước 6. Dựng tác nhân tiếp theo
- chỉ sau khi boundary tác nhân đầu đã rõ,
- tái sử dụng shared layer đúng chỗ,
- không phá role boundary.

### Bước 7. Dựng tính năng phụ trợ
- settings,
- reports,
- notifications,
- tiện ích phụ,
- docs hoàn thiện.

### Bước 8. Kiểm thử và reconcile
- rà lại rules,
- rà lại docs,
- rà lại contract,
- rà lại role,
- rà lại layout và data flow.

---

## 11. TEMPLATE FILE PHÂN TÍCH YÊU CẦU RIÊNG

File này nên lưu riêng, ví dụ: `FUNCTIONAL_REQUIREMENTS_ANALYSIS.md`

Cấu trúc đề xuất:

# FUNCTIONAL_REQUIREMENTS_ANALYSIS

## 1. Project Scope
## 2. Core Entities
## 3. Main Actors
## 4. Functional Requirements
## 5. Non-Functional Requirements
## 6. Actor Priority
## 7. Build Order
## 8. Risk Areas
## 9. Assumptions
## 10. Completion Criteria

### 11.1. Nội dung chi tiết nên có

#### Project Scope
- hệ thống dùng để làm gì,
- không bao gồm gì,
- boundary nghiệp vụ.

#### Core Entities
- các thực thể chính,
- ownership của từng thực thể,
- ai tạo, ai sửa, ai xem.

#### Main Actors
- guest,
- user,
- admin,
- actor khác nếu có.

#### Functional Requirements
- chia theo actor,
- mỗi requirement có luồng chính và quyền.

#### Non-Functional Requirements
- auth,
- security,
- validation,
- performance,
- maintainability,
- architecture.

#### Actor Priority
- actor nào build trước,
- vì sao.

#### Build Order
- foundation,
- actor chính,
- actor phụ,
- tính năng phụ.

#### Risk Areas
- auth,
- role,
- ownership,
- shared contract,
- DB schema,
- shared UI.

#### Assumptions
- ghi rõ assumption hiện tại.

#### Completion Criteria
- điều kiện để coi phân tích đủ tốt cho bước tạo prompt.

---

## 12. QUY TẮC TẠO PROMPT TỪ FILE PHÂN TÍCH

Prompt không được viết trực tiếp từ ý tưởng mơ hồ.
Prompt phải được sinh từ file phân tích yêu cầu đã hoàn chỉnh.

### 12.1. Mỗi prompt phải có đủ
- mục tiêu cụ thể,
- scope cụ thể,
- file hoặc layer được phép đụng,
- boundary không được phá,
- actor đang phục vụ,
- feature đang triển khai,
- contract cần giữ,
- validation cần có,
- auth hoặc permission cần có,
- output mong muốn,
- yêu cầu test,
- yêu cầu docs.

### 12.2. Prompt phải cấm rõ
- không thêm feature ngoài scope,
- không refactor lan rộng,
- không phá cấu trúc thư mục,
- không bỏ qua service hoặc repository nếu đã có boundary,
- không bỏ qua rules tổng.

### 12.3. Prompt phải gắn actor
Ví dụ:
- prompt cho auth foundation,
- prompt cho admin foundation,
- prompt cho user dashboard foundation,
- prompt cho user CRUD entity,
- prompt cho admin management module.

---

## 13. TEMPLATE PROMPT GỐC

Dùng template này để sinh prompt cho từng bước.

### PROMPT TEMPLATE

Mục tiêu:
- Triển khai `<tên bước hoặc feature>` cho tác nhân `<actor>` trong dự án Next.js fullstack.

Bối cảnh bắt buộc:
- Dự án đã có bộ rules code tổng.
- Dự án đã có cấu trúc thư mục chuẩn.
- Phải tuân thủ strict các rules về scope, architecture, contract, validation, auth, permission, testing, documentation.
- Không được thay đổi cấu trúc thư mục đã chốt.
- Không được thêm feature ngoài phạm vi yêu cầu.
- Không được refactor lan rộng ngoài vùng cần thiết.

Phạm vi thực hiện:
- Chỉ xử lý `<scope cụ thể>`.
- Chỉ chạm vào các layer cần thiết.
- Nếu cần thêm file mới, file phải nằm đúng layer và đúng trách nhiệm.

Yêu cầu chức năng:
- `<trích requirement cụ thể từ file phân tích>`

Yêu cầu phi chức năng:
- `<trích các ràng buộc liên quan: auth, permission, validation, performance, maintainability>`

Boundary bắt buộc giữ:
- Không trộn UI với business logic.
- Không để page hoặc route handler ôm nghiệp vụ nặng nếu service layer là phù hợp.
- Không cho phép role check chỉ tồn tại ở client.
- Không phá shared contract hoặc shared component props.

Đầu ra mong muốn:
- Tạo hoặc cập nhật code cần thiết đúng cấu trúc.
- Giữ code rõ ràng, type-safe, dễ test.
- Bổ sung validation cần thiết.
- Bổ sung guard hoặc permission check cần thiết.
- Cập nhật docs liên quan nếu behavior thay đổi.

Kiểm tra bắt buộc:
- type safety,
- contract consistency,
- auth hoặc permission flow,
- validation behavior,
- error states,
- downstream usage impact.

---

## 14. QUY TRÌNH TẠO PROMPT THEO BƯỚC

### Bước A. Tạo prompt cho foundation
Ví dụ:
- shared types,
- role enum,
- auth config,
- middleware,
- requireAuth,
- requireAdmin,
- base layouts.

### Bước B. Tạo prompt cho actor ưu tiên
Ví dụ:
- layout riêng,
- dashboard entry,
- navigation,
- feature lõi của actor.

### Bước C. Tạo prompt cho entity lõi
Ví dụ:
- schema,
- repository,
- service,
- API,
- page,
- form,
- validation.

### Bước D. Tạo prompt cho actor tiếp theo
Chỉ thực hiện sau khi:
- actor trước đã có boundary rõ,
- auth hoặc role đã ổn định,
- contract lõi đã rõ.

### Bước E. Tạo prompt cho tính năng phụ
Ví dụ:
- settings,
- reports,
- filters,
- exports,
- notification surfaces.

### Bước F. Tạo prompt cho docs và verification
Ví dụ:
- update README,
- update module docs,
- update env example,
- add test coverage notes.

---

## 15. MẪU THỨ TỰ PROMPT THỰC TẾ CHO NEXT.JS FULLSTACK CÓ ADMIN VÀ USER

1. Prompt phân tích hệ thống thành actors, entities, permissions, flows
2. Prompt dựng auth foundation
3. Prompt dựng role và permission foundation
4. Prompt dựng route groups và layouts nền
5. Prompt dựng shared validation và shared types
6. Prompt dựng entity lõi
7. Prompt dựng service và repository cho entity lõi
8. Prompt dựng actor ưu tiên nhất
9. Prompt dựng actor thứ hai
10. Prompt dựng các feature phụ
11. Prompt rà soát docs, tests, contract, permissions

---

## 16. QUY TẮC QUAN TRỌNG KHI AI CODE THEO QUY TRÌNH NÀY

- Luôn bắt đầu từ phân tích, không bắt đầu từ code.
- Luôn xác định actor trước khi dựng UI.
- Luôn xác định entity trước khi dựng CRUD.
- Luôn xác định auth và role trước khi dựng feature protected.
- Luôn xác định layout và navigation trước khi dựng màn hình sâu.
- Luôn xác định service boundary trước khi viết logic route.
- Luôn xác định schema validate trước khi xử lý form.
- Luôn xác định contract trước khi trả response.
- Luôn kiểm tra prompt có ép AI tuân thủ rules hay không.
- Luôn tách prompt thành bước nhỏ đủ kiểm soát.
- Không tạo một prompt quá lớn ôm toàn bộ dự án.
- Không yêu cầu AI “xây full app hoàn chỉnh” trong một bước.
- Mỗi prompt chỉ nên có một mục tiêu kỹ thuật rõ ràng.

---

## 17. TIÊU CHÍ HOÀN THÀNH FILE QUY TRÌNH

File quy trình này được coi là hoàn chỉnh khi:

- đã mô tả rõ cách phân tích chức năng,
- đã mô tả rõ cách phân tích phi chức năng,
- đã có cách xác định tác nhân,
- đã có logic xác định tác nhân nào build trước,
- đã có checklist những thứ bắt buộc cần có cho từng tác nhân,
- đã có thứ tự build rõ ràng,
- đã có template file yêu cầu chức năng riêng,
- đã có template prompt,
- đã có nguyên tắc ép prompt tuân thủ rules tổng.

---

## 18. KẾT LUẬN

Muốn code bằng AI hiệu quả trong dự án Next.js fullstack, không được nhảy thẳng vào prompt code.

Phải đi theo chuỗi đúng:

1. phân tích chức năng thật kỹ,
2. phân tích phi chức năng thật kỹ,
3. xác định tác nhân chính,
4. xác định actor nào build trước,
5. xác định layout, route, guard, contract, validation cho actor đó,
6. chia bước thành prompt nhỏ,
7. ở mỗi prompt đều ép tuân thủ rules tổng và cấu trúc thư mục.

Đây là cách giảm mạnh các lỗi:
- sai scope,
- sai kiến trúc,
- sai permission,
- sai contract,
- sai thứ tự build,
- code được nhưng không dùng được lâu dài.