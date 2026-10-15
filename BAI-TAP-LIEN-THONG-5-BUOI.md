# Bài tập liên thông — Vibe Code 5 buổi

## Mục tiêu chung

Mỗi bài tập yêu cầu người học hoàn thành **một lát cắt MVP khép kín**, theo đúng thứ tự:

```text
Vấn đề thật
-> Brief 6 dòng + landing page
-> User stories + cắt scope + page map + wireframe
-> Frontend mock responsive
-> ERD + data dictionary + migration + seed
-> Auth + RBAC + RLS + business command
-> Test allow/deny bằng tài khoản thật
```

Không được nhảy thẳng vào code, database hoặc Supabase khi artefact của bước trước chưa được duyệt. Chỉ dùng dữ liệu giả; không nhập secret hoặc dữ liệu khách hàng thật vào prompt, repository hay database demo.

## Quy trình bắt buộc cho mọi bài

| Chặng | Việc phải làm | Deliverable bắt buộc |
|---|---|---|
| 1. Product brief | Xác định người dùng, vấn đề, giá trị, CTA và giới hạn MVP | Brief 6 dòng, landing page đã deploy, README |
| 2. Product design | Viết 5-8 user stories; chỉ chọn tối đa 3 Must-have | Bảng Must/Should/Later, page map 5-7 trang, UX brief, wireframe, implementation plan |
| 3. Frontend | Xây theo lát cắt: routes, types, mock data, components và states | Frontend bấm được ở 375 px, 768 px, 1440 px; có loading/empty/error/not-found/validation |
| 4. Data design | Truy ngược từ story và wireframe ra dữ liệu cần lưu | Traceability canvas, architecture decision, ERD, dictionary, migration, seed, query plan |
| 5. Backend & security | Chọn đúng một business command có luật nghiệp vụ | Auth flow, role-permission matrix, backend command, RLS/grants, audit event |
| 6. Verification | Chứng minh backend/database thực sự chặn sai phạm | Test matrix và bằng chứng cho anonymous, staff, manager, admin |

### Quy tắc dùng AI

- Mỗi prompt giao đúng **một lát cắt nhỏ**, nêu rõ input, file có thể đổi, ràng buộc và cách test.
- Trước khi AI viết code, yêu cầu AI chỉ ra dữ liệu còn thiếu và kế hoạch thay đổi.
- Sau mỗi thay đổi: chạy app, kiểm tra TypeScript/console, luồng chính, mobile và regression cơ bản.
- UI chỉ ẩn nút để cải thiện trải nghiệm; quyền phải được kiểm ở backend và database.

---

## Bài 1 — SwiftCart: Vận hành đơn hàng e-commerce

### Bối cảnh

SwiftCart là shop e-commerce bán đồ gia dụng. Đơn từ website đổ về nhưng nhân viên đóng gói hay bỏ sót đơn, còn kho không biết đơn nào đã được xác nhận để chuẩn bị hàng.

### Mục tiêu MVP

Một hệ thống nội bộ để xem đơn, tạo đơn demo và xác nhận đơn trước khi đóng gói. Không làm cổng thanh toán, vận chuyển thời gian thực, voucher hay hoàn tiền.

### Ba Must-have user stories

1. Là nhân viên CSKH, tôi muốn tạo đơn demo để không sót thông tin người mua và sản phẩm.
2. Là nhân viên kho, tôi muốn xem/lọc đơn theo trạng thái để biết đơn nào cần chuẩn bị.
3. Là quản lý vận hành, tôi muốn xác nhận đơn đang chờ để kho chỉ đóng gói đơn hợp lệ.

### Ràng buộc thiết kế

- Thực thể chính: `order`.
- Tối đa 6 route: dashboard, orders list, order detail, order new, order edit, profile/sign-in.
- Chỉ thêm `customers`, `products`, `orders`, `order_items`, `order_events`, `profiles` khi ERD chứng minh cần.
- State hợp lệ: `pending -> confirmed -> packed -> shipped` hoặc `pending -> cancelled`; không được confirm đơn đã hủy hoặc hết hàng.

### Business command bắt buộc

`confirmOrder(orderId)`

- Chỉ `manager` hoặc `admin` được gọi.
- Lấy actor từ session; client chỉ gửi `orderId`.
- Cập nhật trạng thái, người xác nhận, thời điểm xác nhận và audit event trong cùng transaction.

### Kiểm thử quyền tối thiểu

| Account | Thao tác | Kết quả mong đợi |
|---|---|---|
| Anonymous | Mở danh sách đơn | Bị chuyển về sign-in |
| Staff | Tạo đơn demo | Thành công; `created_by` đúng user |
| Staff | Gọi trực tiếp command xác nhận | Bị từ chối; không có event mới |
| Manager | Xác nhận đơn pending còn hàng | Thành công; status và audit event cùng xuất hiện |
| Admin | Xóa bản ghi demo nếu matrix cho phép | Thành công |

---

## Bài 2 — StoreFlow: Điều chuyển và kiểm đếm tồn kho retail

### Bối cảnh

StoreFlow vận hành chuỗi cửa hàng retail. Nhân viên tạo yêu cầu điều chuyển hàng giữa kho và cửa hàng, nhưng quản lý kho cần duyệt đúng yêu cầu trước khi hàng rời kho.

### Mục tiêu MVP

Quản lý yêu cầu điều chuyển: tạo yêu cầu, xem danh sách theo trạng thái và duyệt xuất kho. Không làm POS, dự báo nhu cầu, barcode scanner hay đồng bộ ERP.

### Ba Must-have user stories

1. Là nhân viên cửa hàng, tôi muốn tạo yêu cầu điều chuyển theo sản phẩm và số lượng.
2. Là nhân viên kho, tôi muốn lọc yêu cầu theo kho nguồn, cửa hàng nhận và trạng thái.
3. Là quản lý kho, tôi muốn duyệt yêu cầu khi tồn kho khả dụng để không xuất quá số lượng.

### Ràng buộc thiết kế

- Thực thể chính: `stock_transfer`.
- Form validate kho nguồn, cửa hàng nhận, sản phẩm, số lượng nguyên dương và ngày yêu cầu.
- Có loading, empty state, error state và not-found.
- State: `pending -> approved -> dispatched -> received` hoặc `pending -> rejected`.
- Query plan có list theo location + status, detail theo id và filter theo product nếu UI có filter.

### Business command bắt buộc

`approveStockTransfer(transferId)`

- Chỉ `manager` hoặc `admin` duyệt yêu cầu `pending` khi tồn kho khả dụng đủ.
- Backend tự kiểm quantity và stock available; client không gửi `status`, `approved_by` hay role.
- Lưu audit event ghi ai duyệt và số lượng đã được chấp thuận.

### Kiểm thử quyền tối thiểu

Ngoài ma trận chung, cần chứng minh manager không thể duyệt yêu cầu lớn hơn tồn kho; request bị từ chối và không tạo audit event.

---

## Bài 3 — BankBranch: Tiếp nhận yêu cầu mở tài khoản tại ngân hàng

### Bối cảnh

BankBranch là hệ thống demo nội bộ cho chi nhánh ngân hàng. Giao dịch viên tiếp nhận yêu cầu mở tài khoản; kiểm soát viên cần duyệt hồ sơ hợp lệ trước khi kích hoạt. Chỉ dùng dữ liệu giả, không dùng dữ liệu định danh thật.

### Mục tiêu MVP

Quản lý hồ sơ mở tài khoản giả lập: tạo hồ sơ, xem trạng thái và phê duyệt sau khi đủ trường bắt buộc. Không làm eKYC thật, chuyển tiền, thẻ, tính lãi hay tích hợp hệ thống ngân hàng.

### Ba Must-have user stories

1. Là giao dịch viên, tôi muốn tạo hồ sơ khách hàng giả lập với thông tin tối thiểu để tiếp nhận yêu cầu.
2. Là giao dịch viên, tôi muốn xem/lọc hồ sơ theo trạng thái để biết hồ sơ nào cần bổ sung.
3. Là kiểm soát viên, tôi muốn duyệt hồ sơ đủ điều kiện để kích hoạt tài khoản demo đúng quy trình.

### Ràng buộc thiết kế

- Thực thể chính: `account_application`.
- Mock data có ít nhất 8 hồ sơ gồm pending, approved, rejected và một edge case thiếu trường bắt buộc.
- Không lưu số CCCD, tài khoản hay PII thật; sử dụng dữ liệu giả/đã che.
- Responsive: table có phương án card hoặc scroll ngang có chủ đích ở 375 px.

### Business command bắt buộc

`approveAccountApplication(applicationId)`

- Chỉ `manager` hoặc `admin` được gọi.
- Transaction phải kiểm: hồ sơ còn `pending`, đủ trường bắt buộc và chưa có customer reference trùng trong dữ liệu demo.
- Khi thành công, đổi trạng thái, tạo account demo reference và ghi event; khi thất bại, database không thay đổi.

### Kiểm thử quyền tối thiểu

Ngoài ma trận chung, cần chứng minh giao dịch viên không thể tự duyệt hồ sơ hoặc đọc hồ sơ không thuộc phạm vi được cấp quyền.

---

## Bài 4 — LoanDesk: Tiếp nhận và phê duyệt khoản vay demo

### Bối cảnh

LoanDesk là công cụ đào tạo nội bộ cho quy trình khoản vay giả lập. Nhân viên tín dụng tiếp nhận hồ sơ, còn quản lý tín dụng duyệt hoặc từ chối theo checklist rõ ràng. Mọi dữ liệu và quyết định chỉ mang tính mô phỏng giáo dục.

### Mục tiêu MVP

Ghi nhận hồ sơ vay demo, lọc theo trạng thái và ra quyết định phê duyệt theo rule bài tập. Không đưa ra khuyến nghị tín dụng thật, chấm điểm tự động, giải ngân, tính lãi hoặc kết nối hệ thống thật.

### Ba Must-have user stories

1. Là nhân viên tín dụng, tôi muốn tạo hồ sơ vay demo với khoản tiền, mục đích và checklist tài liệu.
2. Là nhân viên tín dụng, tôi muốn xem/lọc hồ sơ theo trạng thái và loại khoản vay để theo dõi.
3. Là quản lý tín dụng, tôi muốn phê duyệt hoặc từ chối hồ sơ đủ điều kiện theo rule bài tập.

### Ràng buộc thiết kế

- Thực thể chính: `loan_application`.
- Validation: requested_amount dương, mục đích vay không rỗng, checklist tài liệu đầy đủ theo rule demo.
- Dữ liệu tiền dùng integer đơn vị nhỏ nhất hoặc numeric; không dùng floating point tùy tiện.
- State: `pending -> approved` hoặc `pending -> rejected`; hồ sơ đã có quyết định không được staff sửa/xóa.
- Seed có edge case amount không hợp lệ hoặc thiếu checklist để constraint/rule chặn.

### Business command bắt buộc

`decideLoanApplication(applicationId, decision)`

- Chỉ `manager` hoặc `admin` ra quyết định cho hồ sơ `pending`.
- Command kiểm actor/session, trạng thái, dữ liệu checklist demo; tự ghi decision_by, decision_at, audit event.
- Chặn direct update status/decision từ client bằng RLS/grants.

### Kiểm thử quyền tối thiểu

Ngoài ma trận chung, phải chứng minh staff không thể thay đổi amount hoặc decision của hồ sơ đã có quyết định, kể cả khi gửi request trực tiếp.

---

## Cấu trúc nộp bài chung

```text
01-brief.md
02-landing-page-url.md
03-user-stories-and-scope.md
04-page-map-and-wireframes.md
05-implementation-plan.md
06-frontend-qa.md
07-traceability.md
08-architecture-decision.md
09-erd.md
10-data-dictionary.md
11-migration.sql
12-seed.sql
13-query-plan.md
14-auth-and-role-matrix.md
15-business-command.md
16-rls-and-grants.sql
17-test-matrix-and-evidence.md
18-review-notes.md
README.md
```

## Tiêu chí đạt chung (10 điểm)

| Tiêu chí | Điểm | Đạt khi |
|---|---:|---|
| Product và scope | 1.5 | Brief rõ; tối đa 3 Must stories; chủ động loại Later |
| UX và frontend | 2.0 | Luồng list → detail → create/edit dùng được; states và responsive đạt |
| Traceability & data design | 2.0 | Story → route → dữ liệu → ERD/dictionary/migration nhất quán |
| Backend nghiệp vụ | 1.5 | Một command có validation, state transition và transaction rõ |
| Security | 2.0 | Auth, role matrix, RLS/grants chặn được hành vi trái quyền |
| QA và phản biện | 1.0 | Có test allow/deny, evidence và ít nhất một thay đổi sau review |

### Điều kiện không đạt dù giao diện đẹp

- Nhảy vào code/database trước khi chốt scope và artefact trước đó.
- Client tự gửi hoặc tự sửa `role`, `status`, `approved_by`/`confirmed_by`.
- Chỉ ẩn nút ở UI nhưng request/RPC trực tiếp vẫn vượt quyền.
- Không có bằng chứng test role hoặc database policy quá rộng để “cho chạy được”.
