# QUY TẮC CODE NEXT.JS FULLSTACK

## 1. PURPOSE

- Bộ rules này tồn tại để giữ cho dự án Next.js fullstack phát triển đúng scope, đúng kiến trúc, đúng contract, và không trôi thành một codebase chắp vá.
- Bộ rules này bảo vệ các boundary quan trọng: route layer, UI layer, business layer, data access layer, auth/permission layer, validation layer, và documentation layer.
- Bộ rules này áp dụng cho mọi thay đổi trong codebase: tạo file mới, sửa file cũ, refactor nhỏ, thêm API, thêm page, thêm server action, thêm service, thêm test, cập nhật docs.
- Mục tiêu không phải tạo nhiều quy trình, mà là ép mọi thay đổi đi theo một cách làm nhất quán, an toàn, có thể review, có thể test, và có thể mở rộng.

## 2. GENERAL OPERATING RULES

- Luôn giữ nguyên intent nghiệp vụ gốc của task; không tự ý mở rộng tính năng.
- Không thêm feature mới chỉ vì “tiện tay”.
- Không refactor lan rộng nếu task không yêu cầu hoặc không bắt buộc để sửa lỗi.
- Luôn làm theo bước nhỏ, thay đổi nhỏ, kiểm tra sớm.
- Không sửa nhiều vùng hệ thống cùng lúc khi chưa xác định rõ tác động.
- Không tuyên bố hoàn thành chỉ vì UI render được hoặc build pass cục bộ.
- Mọi assumption phải được nêu rõ trong note hoặc sync log trước khi dựa vào assumption đó để code.
- Mọi thay đổi có ảnh hưởng auth, role, contract, schema, shared component, hoặc shared service đều được xem là thay đổi nhạy cảm.
- Ưu tiên sửa đúng chỗ hơn là tạo workaround tạm.
- Không để code “tạm dùng”, “mock cứng”, “hardcode để chạy trước” mà không đánh dấu rõ và không được phép merge nếu nó làm sai intent gốc.
- Không dùng logic client để thay thế kiểm soát bảo mật server.
- Không dùng UI state để giả lập source of truth cho dữ liệu nghiệp vụ.
- Không duplicate business logic giữa page, api route, server action, service, và component.
- Không import server-only code vào client component.
- Không import client-only hook vào server component.
- Mọi thay đổi phải cố gắng giữ codebase dễ đọc hơn hoặc ít nhất không khó đọc hơn.

## 3. CONTEXT READING RULES

- Trước khi sửa code, luôn đọc:
  - mục tiêu task hiện tại,
  - file route hoặc entry point liên quan,
  - service/repository/guard/schema liên quan,
  - component cha/con liên quan,
  - type và contract liên quan,
  - docs hoặc note trạng thái nếu có.
- Nếu sửa page trong `app/`, phải đọc layout, component chính, data flow, và nơi dữ liệu được lấy.
- Nếu sửa API route hoặc server action, phải đọc validation, auth guard, service, repository, và response shape.
- Nếu sửa feature admin hoặc user, phải đọc boundary quyền truy cập của feature đó.
- Nếu sửa shared component hoặc shared utility, phải rà các điểm dùng chính trước khi sửa.
- Nếu sửa `middleware`, `auth`, `permissions`, `guards`, `schema`, `types`, `lib`, `server/services`, `server/db/repositories`, phải xem đây là vùng high-impact và đọc context rộng hơn vùng đang sửa.
- Không sửa file khi mới nhìn tên file mà chưa đọc implementation hiện tại.
- Không đổi cấu trúc thư mục, layer, hoặc import path khi chưa hiểu vai trò hiện tại của module đó.
- Nếu task mơ hồ, giữ nguyên intent gốc và ghi assumption thay vì tự diễn giải sang scope mới.

## 4. PLANNING RULES

- Luôn viết plan ngắn trước khi sửa code.
- Plan phải xác định:
  - mục tiêu thay đổi,
  - các file sẽ ảnh hưởng,
  - boundary cần giữ nguyên,
  - contract nào cần bảo vệ,
  - check nào phải chạy sau khi sửa.
- Chia thay đổi thành atomic steps đủ nhỏ để review và rollback dễ.
- Ưu tiên thứ tự: đọc context -> xác định boundary -> sửa contract-safe -> test -> cập nhật docs.
- Trước khi tạo file mới, phải xác nhận file đó thực sự cần thiết và không trùng trách nhiệm với file đã có.
- Trước khi xóa file hoặc di chuyển file, phải xác nhận không làm gãy import, route, alias, hoặc documentation.
- Nếu task chạm nhiều layer, plan phải nêu rõ luồng đi qua các layer.
- Không bắt đầu bằng refactor nếu bug hoặc feature có thể xử lý bằng thay đổi nhỏ hơn.
- Không mở rộng plan thành kế hoạch triển khai chi tiết theo agent; chỉ mô tả thay đổi kỹ thuật cần thiết.

## 5. ARCHITECTURE PRESERVATION RULES

- Giữ rõ boundary giữa các lớp:
  - `src/app/` cho routing, layout, page composition, route handlers,
  - `src/components/ui/` cho shadcn/ui components,
  - `src/components/shared/` cho shared business components,
  - `src/features/` cho code theo domain (components + actions + schemas),
  - `src/server/services/` cho business logic,
  - `src/server/db/repositories/` cho data access,
  - `src/lib/` cho utility/config dùng chung,
  - `prisma/` cho schema và migration.
- Không đặt business logic nặng trực tiếp trong `page.tsx`, `layout.tsx`, hoặc component UI.
- Không để route handler hoặc server action tự xử lý toàn bộ nghiệp vụ; phải gọi service layer.
- Không để repository chứa logic nghiệp vụ; repository chỉ truy cập dữ liệu.
- Không để service thao tác UI.
- Không để component truy cập DB trực tiếp.
- Không trộn code admin và user trong cùng module nếu khác boundary nghiệp vụ hoặc khác permission.
- Không để logic phân quyền rải rác ở nhiều chỗ; dùng `guards/` hoặc helper chung.
- Giữ tách biệt rõ:
  - authn (xác thực),
  - authz (phân quyền),
  - validation,
  - business logic,
  - persistence.
- Chỉ refactor cấu trúc module khi có lý do rõ: trùng trách nhiệm, import vòng, vi phạm layer, hoặc gây khó test/bảo trì.
- Với Next.js App Router:
  - server component là mặc định nếu không cần client interactivity,
  - chỉ dùng client component khi thật sự cần state, hook, hoặc browser API,
  - không đẩy component sang client chỉ để “cho nhanh”.
- Server action, route handler, service đều phải có boundary rõ; không chọn ngẫu nhiên.
- Middleware chỉ dùng cho gate hoặc check sớm; không nhồi business logic phức tạp vào middleware.
- Mọi logic nhạy cảm phải được kiểm tra lại ở server ngay cả khi middleware đã chặn.
- **Features organization:**
  - Mỗi feature trong `src/features/` chỉ chứa: `components/`, `actions.ts`, `schemas.ts`
  - KHÔNG tạo `services/` trong features - business logic phải ở `src/server/services/`
  - KHÔNG tạo `types.ts` riêng trong features - dùng `src/types/` global hoặc inline
  - Components trong `src/features/[name]/components/` chỉ dùng cho feature đó
  - Shared components phải ở `src/components/shared/`
- **Import rules:**
  - `src/app/` có thể import từ `src/components/`, `src/features/`, `src/lib/`
  - `src/features/actions.ts` có thể import từ `src/server/services/`, `src/server/guards/`, `src/lib/`
  - `src/features/components/` KHÔNG được import từ `src/server/` (trừ qua actions)
  - `src/server/services/` có thể import từ `src/server/repositories/`, `src/lib/`
  - `src/server/repositories/` chỉ import từ `src/server/db/prisma.ts` và `@prisma/client`
  - Client components KHÔNG được import server-only code
- **Route groups:**
  - `(public)` dùng cho tất cả user (đã đăng nhập hoặc chưa)
  - `(auth)` chỉ cho auth pages
  - `admin/` là route thật, không dùng route group, bắt buộc có guard

## 6. CONTRACT SAFETY RULES

- Không thay đổi request shape, response shape, params, shared type, DB schema, hoặc validation schema một cách im lặng.
- Mọi thay đổi contract phải được cập nhật đồng thời ở:
  - type,
  - validation schema,
  - service xử lý,
  - API hoặc server action,
  - nơi tiêu thụ dữ liệu,
  - docs liên quan.
- Không đổi tên field public hoặc shared nếu chưa rà toàn bộ nơi dùng.
- Không đổi enum role, status, hoặc các giá trị nghiệp vụ cốt lõi nếu chưa rà downstream impact.
- Không để UI đoán dữ liệu bằng fallback sai contract.
- Không dùng `any` để né contract.
- Không dùng ép kiểu bừa bãi để che lỗi type.
- Nếu response có dạng chuẩn của dự án, phải giữ nguyên wrapper, status, hoặc error conventions.
- Với form:
  - schema validate là nguồn thật cho input shape,
  - không để field UI và field backend lệch tên mà không có mapping rõ.
- Với auth hoặc session:
  - không thêm field session, token, hoặc user payload mà không cập nhật type augmentation và nơi đọc field đó.
- Với Prisma hoặc schema:
  - mọi đổi model, field, hoặc enum phải đi kèm migration, type impact review, seed impact review nếu có.
- Mọi thay đổi ở shared component props phải được xem là contract change nếu component đó dùng nhiều nơi.
- Khi không chắc một interface có shared hay không, xử lý như shared cho đến khi chứng minh ngược lại.

## 7. CODING RULES

- Ưu tiên code rõ ràng, đơn giản, dễ debug, dễ test.
- Ưu tiên tên biến, hàm, component, service, file phản ánh đúng trách nhiệm.
- Một file chỉ nên có một trách nhiệm chính rõ ràng.
- Tránh function quá dài; tách khi logic bắt đầu có nhiều nhánh hoặc nhiều trách nhiệm.
- Tránh nesting sâu; ưu tiên early return.
- Không duplicate logic; nếu lặp từ lần thứ hai và có giá trị dùng lại rõ ràng, trích helper hoặc module phù hợp.
- Không tạo abstraction sớm khi mới chỉ có một use case đơn lẻ.
- Không thêm dependency mới nếu tiện ích có thể giải quyết bằng code hiện có hoặc dependency sẵn có.
- Giữ style nhất quán với codebase hiện tại trước khi áp style cá nhân.
- Không trộn tiếng Anh và tiếng Việt trong identifier; dùng một chuẩn nhất quán, ưu tiên tiếng Anh cho code.
- Comment chỉ viết khi nó bổ sung ngữ cảnh mà code khó tự diễn đạt; không comment lại điều hiển nhiên.
- Comment phải giải thích “vì sao” hoặc boundary quan trọng, không chỉ “đang làm gì”.
- Không để `console.log`, debug code, dead code, hoặc commented-out code trong phần hoàn thành.
- Mọi `TODO`, `FIXME`, `HACK` phải có ngữ cảnh rõ và chỉ tồn tại khi task cho phép giữ lại.
- Dùng type hẹp nhất hợp lý; tránh type quá rộng.
- Không export tràn lan; chỉ export phần thực sự cần dùng.
- Không dùng default export cho utility trừ khi codebase đã theo chuẩn đó.
- Không tạo helper chung trong `lib/` nếu helper đó thực tế chỉ thuộc một feature.
- Mọi parser, formatter, mapper nên có tên phản ánh đầu vào và đầu ra.
- Với async code:
  - luôn xử lý lỗi có chủ đích,
  - không bỏ promise lơ lửng,
  - không giả định request luôn thành công.
- Với form:
  - validate đầu vào rõ ràng bằng Zod schemas,
  - hiển thị lỗi nhất quán,
  - không hardcode error message ở nhiều nơi nếu có thể chuẩn hóa.
- Với data fetching:
  - đặt logic fetch ở layer phù hợp,
  - không fetch lặp vô ích,
  - không cache hoặc `no-store` một cách ngẫu nhiên.
- Với role-based UI:
  - UI có thể ẩn chức năng theo role,
  - nhưng quyền thật phải được chặn ở server.
- Với file hoặc folder:
  - đặt đúng layer theo cấu trúc thư mục,
  - không nhét tạm vào `utils` hoặc `shared` nếu trách nhiệm chưa rõ.
- **Naming conventions:**
  - Files: `kebab-case.tsx`, `kebab-case.ts`
  - Components: `PascalCase` (UserList, ProductCard)
  - Functions/variables: `camelCase` (getUserById, isAdmin)
  - Constants: `UPPER_SNAKE_CASE` (API_URL, MAX_ITEMS)
  - Types/Interfaces: `PascalCase` (User, OrderStatus)
  - Server actions: `camelCase` với prefix động từ (createUser, updateProduct)
  - Repositories: `[entity].repository.ts` (user.repository.ts)
  - Services: `[entity].service.ts` (user.service.ts)
  - Schemas: `[entity].schemas.ts` hoặc `schemas.ts` trong feature folder
- **File organization trong features:**
  - `features/[name]/components/` - Components đặc thù
  - `features/[name]/actions.ts` - Server actions
  - `features/[name]/schemas.ts` - Zod schemas
  - KHÔNG tạo thêm subfolder khác trừ khi thực sự cần thiết

## 8. TESTING AND VALIDATION RULES

- Mọi thay đổi phải xác định rõ loại kiểm thử cần thiết:
  - type check,
  - lint,
  - unit test,
  - integration test,
  - e2e test,
  - manual verification.
- Tối thiểu phải chạy các check liên quan trực tiếp đến vùng đã sửa.
- Nếu sửa contract, auth, permission, schema, middleware, route handler, service, repository, hoặc shared component, phải tăng mức kiểm tra.
- Không bỏ qua test liên quan chỉ vì thay đổi “nhỏ”.
- Với thay đổi auth hoặc role:
  - kiểm tra case hợp lệ,
  - kiểm tra case không đủ quyền,
  - kiểm tra case chưa đăng nhập.
- Với thay đổi form:
  - kiểm tra happy path,
  - validation fail,
  - message lỗi,
  - mapping dữ liệu submit.
- Với thay đổi API hoặc server action:
  - kiểm tra input invalid,
  - unauthorized hoặc forbidden nếu có,
  - success shape,
  - failure shape.
- Với thay đổi UI quan trọng:
  - kiểm tra loading,
  - empty,
  - error,
  - success.
- Với thay đổi DB hoặc schema:
  - kiểm tra migration chạy được,
  - logic đọc và ghi không gãy,
  - dữ liệu cũ có bị ảnh hưởng không.
- Sau mỗi task, rà lại tác động toàn luồng chứ không chỉ file vừa sửa.
- Nếu không thể chạy đủ full checks, phải ghi rõ:
  - đã chạy gì,
  - chưa chạy gì,
  - rủi ro còn lại là gì.
- Build pass không thay thế cho validation nghiệp vụ.
- Type pass không thay thế cho permission check.
- UI render được không đồng nghĩa data flow đúng.
- “Không thấy lỗi” không được coi là test.

## 9. DOCUMENTATION RULES

- Mọi thay đổi ảnh hưởng hành vi hệ thống phải được phản ánh vào docs liên quan.
- Mọi thay đổi contract phải cập nhật docs contract hoặc API usage.
- Mọi thay đổi auth, role, hoặc permission phải cập nhật docs phân quyền nếu dự án có docs đó.
- Mọi thay đổi cấu trúc thư mục, flow dữ liệu, hoặc boundary kiến trúc phải cập nhật docs kiến trúc nếu có.
- Không để README, setup guide, env example, hoặc docs sử dụng bị lỗi thời sau khi đổi cách chạy hoặc cấu hình.
- Nếu thêm env mới, phải cập nhật `.env.example` và note ý nghĩa của biến đó.
- Nếu migration hoặc seed thay đổi cách setup local, phải cập nhật hướng dẫn setup.
- Docs phải phản ánh implementation thật, không mô tả aspirational architecture không tồn tại.
- Không cập nhật docs kiểu chung chung; chỉ ghi điều thực sự thay đổi.

## 10. COLLABORATION RULES

- Trong môi trường nhiều agent, mọi agent phải coi sync file là nguồn sự thật vận hành.
- Mỗi microtask chỉ có một owner chính tại một thời điểm.
- Không âm thầm sửa file đang là shared surface hoặc file agent khác vừa claim.
- Trước khi sửa file có khả năng shared, phải đọc sync state mới nhất.
- Mọi thay đổi ảnh hưởng chéo phải được log trước hoặc ngay khi bắt đầu.
- Nếu phát hiện agent khác đang đi theo assumption sai có thể gây breakage, phải phát `CORRECTION` thay vì âm thầm sửa theo hướng khác.
- Không overwrite lịch sử sync; chỉ append.
- Không xóa note blocker cũ; append trạng thái mới để đóng blocker.
- Mọi handoff phải nêu rõ:
  - đã làm gì,
  - file nào đã đụng,
  - boundary nào cần giữ,
  - rủi ro còn lại,
  - check đã chạy.
- Khi review thay đổi của agent khác, tập trung vào contract, boundary, permission, test coverage, và docs impact.

## 11. SHARED FILE RULES

- Shared files là vùng nhạy cảm cao.
- Shared files mặc định bao gồm:
  - `src/middleware.ts`,
  - `src/lib/auth.ts`,
  - `src/lib/permissions.ts`,
  - `src/lib/db.ts`,
  - `src/lib/utils.ts`,
  - `src/lib/constants.ts`,
  - `src/lib/env.ts`,
  - `src/types/*` - global types,
  - `src/components/ui/*` - shadcn/ui components,
  - `src/components/shared/*` - shared business components,
  - `src/server/db/prisma.ts`,
  - `src/server/guards/*`,
  - root layouts (`src/app/layout.tsx`),
  - config files (`next.config.ts`, `tsconfig.json`),
  - database schema (`prisma/schema.prisma`),
  - env example (`.env.example`).
- Trước khi sửa shared file, phải:
  - đọc implementation hiện tại,
  - đọc sync state,
  - xác định downstream impact,
  - ghi log claim hoặc update nếu làm multi-agent.
- Mọi thay đổi shared file phải được log rõ file và mục đích.
- Không đổi shared file theo hướng phục vụ một feature nhưng làm lệch chuẩn toàn app.
- Nếu phát hiện conflict trong shared file, không xóa lịch sử hoặc ghi đè âm thầm; append correction hoặc blocker trong sync và sửa có chủ đích.
- Mọi thay đổi shared file phải đi kèm rà soát các import hoặc usage chính.
- Nếu shared file chứa contract chung, phải rà lại type, schema, và usage đồng bộ.
- **Đặc biệt với features:**
  - `src/features/[name]/schemas.ts` là shared trong feature đó
  - `src/features/[name]/actions.ts` là shared trong feature đó
  - Thay đổi schemas hoặc actions phải review impact đến tất cả components dùng chúng

## 12. SYNC COMMUNICATION RULES

- Sync file tồn tại để đồng bộ trạng thái thực thi, claim file, nêu assumption, báo blocker, phát correction, handoff, và xác nhận done.
- Sync file phải được dùng theo nguyên tắc append-only.
- Không sửa hoặc xóa log cũ, trừ khi có cơ chế hệ thống riêng cho archive; mặc định không archive trong task.
- Event types bắt buộc hỗ trợ:
  - `CLAIM`
  - `UPDATE`
  - `BLOCKER`
  - `REVIEW`
  - `HANDOFF`
  - `DONE`
  - `CORRECTION`
- Line format chuẩn:
  - `[ISO_TIME] agent=<A|B|C> type=<CLAIM|UPDATE|BLOCKER|REVIEW|HANDOFF|DONE|CORRECTION> task=<TASK_ID> files=<FILE_LIST> note="<MESSAGE>"`
- Quy tắc line format:
  - dùng ISO time đầy đủ,
  - `files` là danh sách ngăn cách bằng dấu phẩy hoặc `none`,
  - `note` ngắn nhưng đủ hành động,
  - không ghi message mơ hồ kiểu “đang sửa vài thứ”.
- Mandatory checkpoints:
  - trước khi bắt đầu microtask quan trọng,
  - trước khi sửa shared file,
  - sau khi hoàn thành thay đổi chính,
  - sau khi chạy validation,
  - khi phát hiện assumption sai,
  - khi gặp blocker,
  - trước khi handoff,
  - khi xác nhận done.
- `CLAIM` dùng khi nhận sửa file hoặc microtask.
- `UPDATE` dùng khi có tiến triển thực chất.
- `BLOCKER` dùng khi không thể tiếp tục an toàn hoặc thiếu thông tin hoặc contract.
- `REVIEW` dùng để ghi phát hiện ảnh hưởng chéo, contract risk, hoặc quality concern.
- `CORRECTION` dùng khi cần sửa lại assumption hoặc trạng thái hoặc log trước đó.
- `HANDOFF` dùng khi chuyển trạng thái cho agent tiếp theo.
- `DONE` chỉ được dùng khi đã qua quality gates liên quan.
- Nếu task không phải multi-agent, tinh thần của sync vẫn áp dụng dưới dạng note nội bộ ngắn: claim -> update -> validate -> done.

## 13. REVIEW AND RECONCILIATION RULES

- Sau mỗi microtask, phải tự review thay đổi trước khi coi là xong.
- Checklist review tối thiểu:
  - có đúng scope không,
  - có vi phạm layer không,
  - có làm đổi contract không,
  - có làm hở quyền truy cập không,
  - có phá shared surface không,
  - có tăng duplicate logic không,
  - có thiếu test hoặc check không,
  - có cần update docs không.
- Nếu sửa code admin hoặc user, phải rà lại boundary role.
- Nếu sửa data flow, phải rà source of truth.
- Nếu sửa validation hoặc schema, phải rà mapping input-output.
- Nếu sửa shared component, phải rà props contract.
- Nếu sửa service hoặc repository, phải rà caller chính.
- Nếu sửa middleware, guard, hoặc permission, phải rà lại các luồng unauthorized hoặc forbidden.
- Nếu phát hiện lệch giữa implementation và docs hoặc sync, phải reconcile ngay bằng docs update hoặc sync correction.
- Không đẩy phần “reconcile sau” nếu biết hiện tại đang lệch contract hoặc lệch docs nghiêm trọng.

## 14. BLOCKER AND ESCALATION RULES

- Phải phát blocker ngay khi phát hiện:
  - thiếu context quan trọng,
  - contract mâu thuẫn,
  - assumption sai,
  - thay đổi yêu cầu chạm shared boundary chưa rõ,
  - nguy cơ làm lệch scope,
  - không thể xác minh an toàn.
- Không tiếp tục đào sâu khi biết rõ đang có blocker kiến trúc hoặc contract chưa rõ.
- Khi blocker xuất hiện, phải ghi rõ:
  - cái gì bị chặn,
  - file hoặc boundary nào bị ảnh hưởng,
  - vì sao không thể tiếp tục an toàn,
  - assumption nào đang nghi ngờ,
  - phần nào vẫn có thể tiếp tục độc lập.
- Dùng `CORRECTION` khi assumption cũ sai nhưng hướng xử lý mới đã rõ.
- Dùng `REVIEW` khi chưa block hoàn toàn nhưng có rủi ro đáng chú ý cần agent khác biết.
- Ưu tiên làm rõ boundary hơn là vá tạm cho chạy.
- Không “im lặng bỏ qua” mismatch type, permission, schema, hoặc docs.
- Không merge tư duy “chạy được trước rồi tính sau” vào vùng auth, contract, shared file, migration, hoặc permission.

## 15. COMPLETION RULES

- Một task chỉ được coi là hoàn thành khi:
  - đúng scope,
  - đúng kiến trúc,
  - không phá contract,
  - đã chạy các check liên quan,
  - docs liên quan đã cập nhật nếu cần,
  - sync hoặc state phản ánh đúng,
  - không còn blocker nghiêm trọng mở.
- “Done” không chỉ là code compile.
- “Done” không chỉ là UI nhìn đúng.
- “Done” không chỉ là một API trả `200`.
- Với thay đổi nhạy cảm, completion gate phải gồm:
  - review boundary,
  - review permission,
  - review validation,
  - review downstream impact.
- Nếu còn trade-off hoặc risk tồn tại, chỉ được coi là done khi risk đã được nêu rõ và không vi phạm acceptance hiện tại.
- Không dùng `DONE` nếu chưa xác nhận check tối thiểu.
- Mọi completion note nên nêu:
  - file chính đã đổi,
  - check đã chạy,
  - docs đã cập nhật hay chưa,
  - risk còn lại nếu có.

## 16. ASSUMPTION HANDLING RULES

- Khi đầu vào chưa đủ rõ, giữ nguyên intent gốc và ghi assumption rõ ràng.
- Không dùng assumption để tự mở rộng feature.
- Không biến assumption thành fact trong docs hoặc code comments.
- Assumption phải càng gần implementation càng tốt; tránh assumption mơ hồ kiểu “chắc là thế”.
- Nếu assumption liên quan auth, role, contract, schema, hoặc data ownership, phải xử lý như high-risk.
- Khi assumption bị bác bỏ bởi code hoặc context mới, phải:
  - ngừng dựa vào assumption cũ,
  - phát `CORRECTION`,
  - rà lại phần code đã bị assumption đó chi phối.
- Nếu nhiều assumption cùng tồn tại, ưu tiên assumption ít phá kiến trúc nhất và ít mở rộng scope nhất.
- Trong Next.js fullstack, các assumption phổ biến cần ghi rõ nếu chưa xác minh:
  - route này là public hay protected,
  - page này là server hay client by design,
  - action này có cần auth không,
  - admin có quyền toàn phần hay theo permission granular,
  - API này là internal-only hay public contract,
  - schema field này là optional thật hay chỉ optional trên UI.

## 17. RULE APPLICATION NOTES FOR CURSOR

- Dùng bộ rules này như project-level operating system, không phải checklist hình thức.
- Luôn active trong tinh thần:
  - giữ scope,
  - giữ layer,
  - giữ contract,
  - giữ permission,
  - giữ docs khớp implementation.
- Trong giai đoạn implementation, các rules quan trọng nhất là:
  - Context Reading Rules,
  - Architecture Preservation Rules,
  - Contract Safety Rules,
  - Testing and Validation Rules,
  - Completion Rules.
- Khi sửa nhanh một bug nhỏ, vẫn phải giữ tối thiểu:
  - đọc context đủ dùng,
  - không phá boundary,
  - test phần liên quan,
  - ghi assumption nếu có.
- Khi thêm feature mới trong scope hiện có, ưu tiên:
  - đặt code đúng domain,
  - không nhồi logic vào route hoặc page,
  - validate input rõ,
  - bảo vệ role ở server,
  - cập nhật docs.
- Khi refactor, ngưỡng cẩn trọng tăng mạnh nếu đụng:
  - auth,
  - middleware,
  - permission,
  - shared types,
  - shared UI,
  - schema hoặc migration,
  - service hoặc repository chung.
- Nếu codebase chưa hoàn hảo, không dùng rules này để ép refactor toàn diện ngoài scope; dùng chúng để tạo thay đổi nhỏ nhưng đúng hướng.
- Với Next.js fullstack, luôn ưu tiên các pattern sau:
  - server component mặc định,
  - client component khi thực sự cần,
  - validation gần input boundary,
  - auth hoặc role check ở server boundary,
  - service layer cho business logic,
  - repository cho data access,
  - shared contracts được kiểm soát tập trung.
- Khi có xung đột giữa “làm nhanh” và “giữ contract hoặc boundary”, ưu tiên giữ contract hoặc boundary.

## 18. HANDOFF_FOR_GPT3

- Protected boundaries:
  - Không trộn route, UI, business, và data access vào cùng một lớp.
  - Không đưa business logic nặng vào `src/app/` pages hoặc layouts.
  - Không để quyền truy cập chỉ được kiểm soát ở client.
  - Không để repository gánh business logic.
  - Không để shared files bị sửa theo nhu cầu cục bộ của một feature.
  - Không tạo `services/` trong `src/features/` - business logic phải ở `src/server/services/`.
  - Không tạo `components/admin/` hoặc `components/user/` - dùng `src/features/` thay thế.
- Shared surfaces:
  - `src/lib/auth.ts`, `src/lib/permissions.ts`, `src/lib/db.ts`,
  - `src/middleware.ts`,
  - `src/server/guards/*`,
  - `src/types/*` - global types,
  - `src/components/ui/*` - shadcn/ui components,
  - `src/components/shared/*` - shared business components,
  - root layouts (`src/app/layout.tsx`),
  - env và config files,
  - `prisma/schema.prisma`,
  - API response conventions.
- Contract-sensitive areas:
  - request hoặc response shapes,
  - `src/features/[name]/schemas.ts` - Zod schemas,
  - session, user, hoặc token types,
  - role hoặc status enums,
  - component props của shared components,
  - service input hoặc output,
  - repository return types,
  - database model fields.
- Validation gates:
  - đúng type,
  - đúng lint,
  - đúng flow auth hoặc permission,
  - đúng schema validation (Zod),
  - đúng success hoặc error states,
  - đúng downstream usages,
  - docs được cập nhật nếu behavior đổi.
- Collaboration obligations:
  - đọc context trước khi sửa,
  - claim trước khi đụng shared surface,
  - append sync logs đúng format,
  - phát blocker hoặc correction ngay khi cần,
  - handoff phải nêu files, checks, và risks.
- Completion gates:
  - đúng scope,
  - không phá kiến trúc,
  - không phá contract,
  - checks liên quan đã chạy,
  - docs hoặc sync đã khớp,
  - không còn blocker nghiêm trọng,
  - trạng thái done có thể giải thích và review được.
- **Feature organization rules:**
  - Mỗi feature có cấu trúc: `components/`, `actions.ts`, `schemas.ts`
  - Business logic ở `src/server/services/[feature].service.ts`
  - Data access ở `src/server/db/repositories/[feature].repository.ts`
  - Không tạo nested structure phức tạp trong features
  - Import flow: `src/app/page` → `src/features/actions` → `src/server/services` → `src/server/repositories` → `database`
- **Route organization rules:**
  - `src/app/(public)/` cho tất cả user
  - `src/app/(auth)/` chỉ cho auth pages
  - `src/app/admin/` là route thật với guard, không dùng route group