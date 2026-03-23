# QUY TẮC VIẾT FILE KẾ HOẠCH TRIỂN KHAI CHI TIẾT TỪ YÊU CẦU CHỨC NĂNG

## 1. MỤC ĐÍCH CỦA FILE KẾ HOẠCH TRIỂN KHAI CHI TIẾT

File kế hoạch triển khai chi tiết tồn tại để chuyển file phân tích yêu cầu chức năng thành thứ tự thực thi rõ ràng, có dependency, có mức ưu tiên, có checkpoint kiểm tra và có điều kiện hoàn thành cụ thể.

File này không phải nơi để:

* viết code,
* mô tả kỹ thuật quá sâu ở mức implementation syntax,
* tự ý mở rộng scope,
* bịa thêm feature,
* bỏ qua actor, entity, permission, ownership hoặc business rule đã có,
* biến luôn thành tài liệu chia task vi mô cho từng người hoặc từng agent.

File này phải giúp trả lời rõ:

* nên triển khai theo phase nào,
* nên làm phần nào trước,
* phần nào phụ thuộc phần nào,
* foundation nào phải có trước khi build feature,
* actor nào được ưu tiên trước,
* entity nào cần được dựng trước,
* checkpoint nào cần review,
* điều kiện nào mới được coi là hoàn thành từng phase.

---

## 2. NGUYÊN TẮC QUAN TRỌNG NHẤT

* Phải bám tuyệt đối vào file phân tích yêu cầu chức năng đã được chốt.
* Không được tự ý thêm feature mới chỉ vì thấy hợp lý ở góc nhìn triển khai.
* Không được thay đổi intent nghiệp vụ gốc khi chuyển sang kế hoạch triển khai.
* Không được làm mờ ranh giới giữa chức năng lõi, chức năng hỗ trợ và chức năng phụ.
* Không được bỏ qua actor priority, build order, ownership và permission đã được phân tích trước đó.
* Mỗi hạng mục trong kế hoạch phải truy nguyên được về actor, entity, chức năng hoặc ràng buộc cụ thể trong file phân tích.
* Nếu đầu vào còn mơ hồ, phải giữ nguyên intent gần nhất và ghi assumption rõ ràng.
* Không được biến assumption thành fact.
* Không được dùng từ ngữ chung chung như “xây hệ thống”, “làm backend”, “xử lý dữ liệu”, “làm admin”, “làm user flow” mà không chỉ ra phạm vi cụ thể.
* Kế hoạch phải đủ chi tiết để triển khai đúng thứ tự, nhưng chưa đi xuống mức code-level design quá sớm.

---

## 3. FILE KẾ HOẠCH PHẢI VIẾT DỰA TRÊN FILE PHÂN TÍCH YÊU CẦU CHỨC NĂNG

File kế hoạch triển khai chi tiết không được viết từ suy đoán cảm tính.

Phải đọc và bám theo các phần đã có trong file phân tích:

* tổng quan dự án,
* scope,
* actor chính,
* core entities,
* yêu cầu chức năng theo actor,
* yêu cầu phi chức năng,
* ownership và permission,
* actor priority,
* build order đề xuất,
* risk areas,
* assumptions,
* completion criteria.

Kế hoạch triển khai chỉ là bước chuyển từ:

* “hệ thống phải làm gì”

sang:

* “nên triển khai cái gì trước, cái gì sau, theo dependency nào, với checkpoint nào”.

Không được dùng file kế hoạch để viết lại toàn bộ business analysis theo cách khác.

---

## 4. MỌI HẠNG MỤC TRIỂN KHAI PHẢI GẮN VỚI ACTOR CỤ THỂ

Không được lập kế hoạch triển khai theo kiểu không biết ai dùng tính năng đó.

Sai:

* Triển khai module đơn hàng.
* Triển khai quản lý hồ sơ.
* Triển khai khu vực nội dung.

Đúng hơn:

* Triển khai flow user xem đơn hàng của chính mình.
* Triển khai flow user cập nhật hồ sơ cá nhân.
* Triển khai flow admin duyệt nội dung do user gửi lên.
* Triển khai flow admin khóa hoặc mở khóa tài khoản người dùng.

Mỗi hạng mục triển khai phải trả lời được:

* actor nào dùng,
* actor đó thao tác trên entity nào,
* actor đó cần route, layout, guard hay module gì,
* actor đó bị giới hạn bởi permission và ownership nào,
* flow đó phụ thuộc dữ liệu từ actor nào khác.

---

## 5. MỌI HẠNG MỤC TRIỂN KHAI PHẢI GẮN VỚI ENTITY CỤ THỂ

Không được viết kế hoạch theo kiểu chỉ có actor mà không có entity.

Mỗi phase hoặc mỗi hạng mục phải làm rõ:

* đang triển khai cho entity nào,
* entity đó được tạo từ đâu,
* ai sở hữu entity đó,
* ai được xem,
* ai được sửa,
* ai được đổi trạng thái,
* entity đó có lifecycle nào,
* trạng thái nào ảnh hưởng đến flow khác.

Ví dụ:

* nếu entity là product, phải biết admin tạo và cập nhật, user chỉ xem sản phẩm đang hoạt động.
* nếu entity là order, phải biết user tạo order của chính mình, admin xem toàn bộ order và cập nhật trạng thái theo scope.
* nếu entity là post, phải biết ai tạo, ai duyệt, ai xuất bản, ai chỉ xem.

Nếu không gắn entity rõ, kế hoạch rất dễ sai dependency và sai build order.

---

## 6. PHẢI TRIỂN KHAI THEO DEPENDENCY THAY VÌ THEO CẢM TÍNH

File kế hoạch phải phản ánh dependency thực tế của hệ thống.

Thứ tự triển khai phải xét ít nhất:

* foundation auth,
* role và permission foundation,
* ownership rules,
* entity foundation,
* actor route/layout/guard foundation,
* core flow,
* support flow,
* validation và hardening,
* docs và handoff.

Ví dụ nguyên tắc:

* không triển khai admin protected area trước khi có auth và role guard đủ dùng,
* không triển khai user ownership flow trước khi ownership rules được xác định,
* không triển khai order flow trước khi product hoặc entity đầu vào liên quan đã sẵn sàng nếu flow đó phụ thuộc,
* không triển khai dashboard hoặc báo cáo trước khi dữ liệu nguồn và flow lõi đã tồn tại.

Không được sắp thứ tự theo kiểu:

* phần nào dễ làm thì làm trước,
* phần nào UI đẹp thì làm trước,
* phần nào nghĩ ra trước thì ghi trước.

---

## 7. PHẢI TÁCH FOUNDATION, CORE FLOW VÀ SUPPORT FLOW

Một file kế hoạch tốt phải tách rõ ít nhất ba lớp:

### Foundation

Là các phần bắt buộc phải có để các flow chính chạy đúng.
Ví dụ:

* auth foundation,
* role/permission checks,
* ownership enforcement,
* entity/schema baseline,
* route protection,
* layout hoặc navigation nền tảng.

### Core flow

Là các flow tạo ra giá trị chính của sản phẩm.
Ví dụ:

* admin tạo sản phẩm,
* user xem sản phẩm,
* user đặt đơn,
* admin duyệt bài,
* user học bài,
* user cập nhật hồ sơ chính của mình.

### Support flow

Là các flow hỗ trợ vận hành hoặc tối ưu trải nghiệm nhưng không nên chen lên trước phần lõi nếu chưa cần.
Ví dụ:

* export CSV,
* dashboard thống kê nâng cao,
* notifications nâng cao,
* filter/search nâng cao,
* reporting phụ,
* setting không ảnh hưởng core flow.

Không được trộn ba lớp này vào cùng một phase lớn mơ hồ.

---

## 8. MỖI PHASE PHẢI CÓ MỤC TIÊU, ĐẦU VÀO, ĐẦU RA VÀ TIÊU CHÍ XONG

Mỗi phase trong file kế hoạch triển khai chi tiết phải có ít nhất:

* tên phase,
* mục tiêu,
* phạm vi,
* actor liên quan,
* entity liên quan,
* dependency đầu vào,
* đầu ra mong muốn,
* rủi ro chính,
* tiêu chí xác nhận phase hoàn thành.

Không được viết phase theo kiểu:

* Làm admin.
* Làm user.
* Hoàn thiện hệ thống.

Đúng hơn nên là:

* Phase 1: Dựng auth và access foundation.
* Phase 2: Dựng admin product management foundation.
* Phase 3: Dựng user product consumption flow.
* Phase 4: Dựng order creation và order ownership flow.
* Phase 5: Validation, hardening, docs sync.

Mỗi phase phải có điểm dừng rõ để review được.

---

## 9. PHẢI BẢO TOÀN OWNERSHIP, PERMISSION VÀ AUTH TRONG KẾ HOẠCH

Kế hoạch triển khai chi tiết không được chỉ nói “làm auth” hoặc “làm phân quyền”.

Phải làm rõ:

* phase nào dựng authentication,
* phase nào dựng role guard,
* phase nào dựng ownership check,
* flow nào là public,
* flow nào chỉ dành cho admin,
* flow nào dành cho user đã đăng nhập,
* flow nào yêu cầu kiểm tra dữ liệu thuộc về chính actor đó.

Ví dụ:

* guest xem landing page là public flow,
* user xem hồ sơ cá nhân phải phụ thuộc login,
* admin vào khu vực /admin phải phụ thuộc role admin,
* user xem order của chính mình phải phụ thuộc ownership,
* admin cập nhật trạng thái order phải phụ thuộc admin permission nhưng vẫn phải bám scope nghiệp vụ.

Không được để ownership và permission chỉ xuất hiện ở phần phân tích mà biến mất trong file kế hoạch.

---

## 10. PHẢI TÁCH RÕ KẾ HOẠCH NGHIỆP VỤ VÀ GIẢI PHÁP KỸ THUẬT CỤ THỂ

Trong file kế hoạch triển khai chi tiết, được phép nói đến các lớp triển khai như:

* foundation,
* module,
* route group,
* layout,
* API surface,
* validation layer,
* service layer,
* repository layer,
* test layer,
* documentation sync.

Nhưng không nên nhảy quá sớm sang chi tiết kiểu:

* dùng server action cho bước này,
* dùng thư viện form cụ thể cho bước kia,
* dùng ORM pattern cụ thể cho entity nọ,
* viết endpoint cụ thể theo framework syntax.

File kế hoạch phải giữ ở mức:

* triển khai phần gì,
* theo dependency nào,
* để đạt business outcome nào,
* có gate kiểm tra gì.

Chi tiết kỹ thuật sâu hơn nên nằm ở tài liệu thiết kế kỹ thuật hoặc prompt thực thi sau đó.

---

## 11. PHẢI VIẾT ĐỦ CHECKPOINT VALIDATION CHO TỪNG GIAI ĐOẠN

Không được viết kế hoạch mà không có checkpoint kiểm tra.

Mỗi phase phải chỉ ra tối thiểu:

* cần review điều gì,
* cần đối chiếu lại actor/entity/permission nào,
* cần kiểm tra luồng chính nào,
* cần kiểm tra luồng lỗi nào,
* cần xác nhận contract nào không bị lệch,
* cần xác nhận docs nào phải sync.

Ví dụ checkpoint tốt:

* xác nhận admin không thể truy cập route ngoài quyền đã định,
* xác nhận user chỉ xem được dữ liệu của chính mình,
* xác nhận entity mới được tạo đúng lifecycle mong đợi,
* xác nhận flow lỗi trả kết quả nhất quán với contract,
* xác nhận phase sau không dựa vào assumption chưa được đánh dấu.

Không được chỉ ghi chung chung như:

* test lại hệ thống,
* kiểm tra mọi thứ,
* đảm bảo chạy ổn.

---

## 12. PHẢI BAO GỒM LUỒNG LỖI VÀ RỦI RO TRIỂN KHAI

File kế hoạch không chỉ lập cho happy path.

Với mỗi phase, phải xem xét:

* dữ liệu thiếu hoặc chưa sẵn sàng,
* permission sai hoặc ownership check thiếu,
* dependency phase trước chưa ổn,
* contract bị hiểu khác giữa các actor,
* core entity lifecycle chưa được khóa rõ,
* flow chính có thể chạy nhưng flow lỗi chưa được xử lý.

Phải có mục risk areas hoặc rủi ro theo phase, ví dụ:

* rủi ro build sai actor trước,
* rủi ro trộn admin logic và user logic,
* rủi ro sai ownership,
* rủi ro API shape lệch giữa các flow,
* rủi ro docs không theo kịp implementation.

Kế hoạch tốt phải chỉ ra nơi nào cần thận trọng hơn, không giả vờ mọi thứ đều tuyến tính.

---

## 13. PHẢI TÁCH RÕ CHỨC NĂNG LÕI VÀ PHẦN CÓ THỂ LÀM SAU

File kế hoạch triển khai phải phản ánh mức độ ưu tiên thật sự.

Phải phân loại rõ:

* foundation bắt buộc,
* core flow cần triển khai sớm,
* support flow có thể triển khai sau,
* auxiliary hoặc enhancement chưa nên chen vào sớm.

Không được để các phần như:

* theme,
* export,
* dashboard phức tạp,
* notification nâng cao,
* reporting phụ

đứng trước các flow cốt lõi nếu file phân tích không yêu cầu.

Mọi quyết định ưu tiên phải bám theo:

* giá trị chính của sản phẩm,
* actor priority,
* dependency thực tế,
* boundary permission,
* dữ liệu lõi cần có trước.

---

## 14. PHẢI CHỈ RA BUILD ORDER Ở MỨC THỰC THI, NHƯNG KHÔNG CHIA VIỆC VI MÔ

File kế hoạch triển khai chi tiết phải nói rõ:

* build phần nào trước,
* build phần nào sau,
* vì sao,
* phần nào phụ thuộc phần nào,
* phase nào mở đường cho phase nào.

Nhưng không được đi sang các dạng:

* agent A làm file nào,
* agent B làm file nào,
* sáng làm gì chiều làm gì,
* commit 1 sửa gì commit 2 sửa gì.

Đây là kế hoạch triển khai ở mức dự án hoặc module, không phải task board vi mô.

---

## 15. PHẢI CÓ CẤU TRÚC TRÌNH BÀY NHẤT QUÁN

Một file kế hoạch triển khai chi tiết nên có cấu trúc ổn định như sau:

1. Tổng quan mục tiêu triển khai
2. Nguồn đầu vào và phạm vi áp dụng
3. Scope được bảo toàn
4. Actor priority cho triển khai
5. Core entities và dependency chính
6. Nguyên tắc build order
7. Danh sách phase triển khai
8. Checkpoint validation theo phase
9. Risk areas
10. Assumptions
11. Completion criteria
12. Handoff notes nếu có

Không nên viết lộn xộn kiểu nghĩ đến đâu ghi đến đó.

---

## 16. PHẢI GHI ASSUMPTION RÕ RÀNG KHI FILE PHÂN TÍCH CHƯA ĐỦ

Nếu file phân tích yêu cầu chức năng chưa đủ rõ để lập kế hoạch, phải tạo mục assumptions riêng.

Ví dụ:

* Giả định auth foundation là dependency bắt buộc cho cả admin và user flows.
* Giả định admin là actor tạo dữ liệu product trước khi user có thể tiêu thụ dữ liệu đó.
* Giả định ownership check phải hoàn thành trước khi mở các flow chỉnh sửa dữ liệu cá nhân.
* Giả định support flow không được ưu tiên trước core flow nếu không có yêu cầu riêng.

Nguyên tắc:

* assumption phải gần intent gốc nhất,
* assumption không được mở rộng scope,
* assumption phải có thể sửa lại về sau,
* assumption không được ngụy trang thành kết luận chắc chắn.

---

## 17. PHẢI VIẾT SAO CHO CÓ THỂ DÙNG ĐỂ SINH PROMPT TRIỂN KHAI SAU ĐÓ

File kế hoạch triển khai chi tiết phải đủ rõ để chuyển tiếp thành prompt triển khai từng phần.

Ví dụ sau khi có file kế hoạch tốt, có thể sinh ra các prompt kiểu:

* dựng auth foundation theo boundary đã chốt,
* dựng admin route, layout và access guard,
* dựng user profile flow với ownership check,
* dựng product management flow cho admin,
* dựng product listing flow cho user,
* dựng order flow theo lifecycle và permission đã xác định,
* dựng validation và review gates cho các flow chính.

Nếu file kế hoạch vẫn viết kiểu:

* làm admin,
* làm user,
* làm dữ liệu,
* làm kiểm thử,

thì chưa đủ chất lượng để dùng tiếp.

---

## 18. PHẢI ĐẢM BẢO FILE KẾ HOẠCH REVIEW ĐƯỢC VÀ ĐỐI CHIẾU ĐƯỢC

File kế hoạch triển khai chi tiết phải giúp review được:

* có lệch scope không,
* có bỏ sót actor không,
* có build sai thứ tự không,
* có bỏ sót permission hoặc ownership không,
* có đẩy support flow lên trước core flow không,
* có checkpoint validation đủ chưa,
* có assumption nào đang bị dùng như fact không.

Vì vậy:

* thuật ngữ phải nhất quán,
* tên actor phải nhất quán,
* tên entity phải nhất quán,
* phase naming phải rõ,
* dependency phải đọc ra được,
* completion criteria phải kiểm được.

---

## 19. TIÊU CHÍ CỦA MỘT FILE KẾ HOẠCH TRIỂN KHAI CHI TIẾT TỐT

Một file kế hoạch triển khai chi tiết được coi là tốt khi:

* bám đúng file phân tích yêu cầu chức năng,
* không mở rộng scope,
* giữ đúng intent nghiệp vụ,
* phản ánh rõ actor priority,
* phản ánh rõ core entity dependency,
* có build order hợp lý,
* có phase rõ ràng,
* có checkpoint validation rõ ràng,
* có xét ownership, permission và auth,
* tách rõ foundation, core flow và support flow,
* có assumptions rõ ràng nếu đầu vào thiếu,
* đủ cụ thể để sinh prompt triển khai,
* nhưng chưa sa vào code-level detail quá sớm.

---

## 20. KẾT LUẬN

Nguyên tắc viết file kế hoạch triển khai chi tiết từ yêu cầu chức năng là:

* bám chặt file phân tích yêu cầu chức năng,
* không đổi scope,
* không thêm feature,
* triển khai theo actor và entity cụ thể,
* triển khai theo dependency thực tế,
* dựng foundation trước,
* ưu tiên core flow trước support flow,
* giữ chặt ownership, permission và auth,
* có checkpoint validation theo phase,
* ghi assumptions rõ,
* giữ kế hoạch đủ cụ thể để dẫn đến prompt thực thi,
* nhưng chưa nhảy quá sâu sang code và giải pháp kỹ thuật chi tiết.

File này là cầu nối giữa business analysis và thực thi.
Nếu file kế hoạch mơ hồ, prompt triển khai sẽ mơ hồ.
Nếu prompt triển khai mơ hồ, code sẽ dễ lệch scope, lệch quyền, lệch dependency và lệch kiến trúc.
