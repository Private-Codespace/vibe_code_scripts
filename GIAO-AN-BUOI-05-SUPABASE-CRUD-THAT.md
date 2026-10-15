# Giáo án chi tiết - Buổi 5: Vibe code backend nghiệp vụ, Auth, RBAC và security

**Thời lượng:** 180 phút  
**Tiền đề:** Học viên đã có user story từ Buổi 2, UI từ Buổi 3 và schema/CRUD thật từ Buổi 4.  
**Đầu ra:** Một business command có luật nghiệp vụ, Auth, RBAC ba role, RLS và bằng chứng test quyền bằng ba tài khoản.

> Buổi 4 trả lời “dữ liệu được lưu thế nào?”. Buổi 5 trả lời “ai được làm gì với dữ liệu, theo quy tắc nào, và backend/database chặn vi phạm ra sao?”. Chỉ dùng dữ liệu giả.

## 1. Mục tiêu và phạm vi

Học viên có thể:

1. Từ một user story, xác định business command, validation, state transition và side effect backend.
2. Vibe code sign up, sign in, sign out, forgot password, session và protected route.
3. Thiết lập security boundary: input không tin cậy, secrets, server boundary, audit event.
4. Thiết kế RBAC tối giản `staff`, `manager`, `admin` và không để user tự leo thang quyền.
5. Viết RLS/grants để database enforce ownership và role.
6. Test luồng allow/deny bằng `staff`, `manager`, `admin`.

| Làm trong Buổi 5 | Không làm trong Buổi 5 |
|---|---|
| Email/password, session, reset password | OAuth, MFA, SSO |
| Một business command có state transition | Toàn bộ backend của sản phẩm |
| RBAC 3 role cho một tổ chức demo | Multi-tenant, invitation, role phức tạp |
| RLS, grants, audit event tối thiểu | Dữ liệu khách thật, payment |

## 2. Khung tư duy

```text
User story + schema Buổi 4
  -> business command và luật nghiệp vụ
  -> Auth biết actor hiện tại
  -> security boundary bảo vệ input/secret
  -> RBAC quyết định quyền theo role
  -> RLS/grants enforce tại database
  -> test allow/deny bằng account thật
```

Phân biệt ba lớp:

| Lớp | Câu hỏi | Ví dụ Hana Desk |
|---|---|---|
| Authentication | Bạn là ai? | Đăng nhập, có session |
| Authorization | Bạn được làm gì? | Manager được xác nhận lịch |
| Database enforcement | Request có bị chặn thật không? | RLS/grant/RPC từ chối staff tự confirm |

Ẩn nút trên UI chỉ tốt cho trải nghiệm; nó không phải bảo mật.

## 3. Chuẩn bị

- App CRUD Buổi 4 có entity chính, ví dụ `appointments`.
- Project Supabase demo; `.env.local` dùng publishable/anon key và bị Git ignore.
- Ba account test: staff, manager, admin; mở browser profile hoặc cửa sổ ẩn danh.
- SQL Editor, Table Editor, DevTools Network/Console.
- Không có `service_role`, token, password hoặc dữ liệu khách thật trong repo/chat/bài nộp.

## 4. Timeline 180 phút

| Thời gian | Bước | Checkpoint |
|---|---|---|
| 00:00-00:25 | Story -> backend command | Có command spec được duyệt |
| 00:25-00:55 | Auth | Có session, logout, reset flow, protected route |
| 00:55-01:15 | Security boundary | Biết input nào không tin cậy, secret ở đâu |
| 01:15-01:30 | Nghỉ + clinic | Không tắt security để chữa lỗi |
| 01:30-01:55 | RBAC | Có role-permission matrix và profile role read-only |
| 01:55-02:25 | RLS/grants | Database chặn direct request trái quyền |
| 02:25-02:50 | Test 3 role | Allow/deny khớp matrix |
| 02:50-03:00 | Review | Pitch bằng evidence, ghi backlog |

## 5. Lab theo quy trình: Hana Desk

Case xuyên suốt: lễ tân (`staff`) tạo lịch; quản lý (`manager`) xác nhận lịch; chủ spa (`admin`) quản trị. Business command trọng tâm là `confirmAppointment`.

### Bước 1 - Từ user story đến backend command

**1. Mục đích**

Biến một user story thành hợp đồng backend trước khi AI viết code. Mục tiêu không phải tạo endpoint CRUD chung chung, mà xác định một lệnh nghiệp vụ có điều kiện đúng/sai.

Ví dụ story: “Là quản lý, tôi muốn xác nhận lịch hẹn đang chờ để lễ tân biết khách đã được chốt.”

**2. Prompt**

> Bạn là backend product engineer. Tôi sẽ dán một user story từ Buổi 2 và schema hiện có từ Buổi 4. Hãy thiết kế đúng một business command phục vụ story đó. Trả về: actor, tên command, input và output, validation, ownership/role phải kiểm, state transition, side effect, lỗi nghiệp vụ, các bảng/cột cần thêm và test allow/deny. Trước khi viết code, hãy nêu các thông tin còn thiếu. Không thêm feature ngoài MVP.

**Đầu vào cần dán:** user story Buổi 2, data dictionary/ERD và enum status từ Buổi 4.

**3. Kết quả mong đợi**

Một command spec như sau:

| Thành phần | Kết quả Hana Desk |
|---|---|
| Command | `confirmAppointment(appointmentId)` |
| Actor | `manager` hoặc `admin` |
| Input | Chỉ UUID của appointment |
| Điều kiện | Có session; lịch `pending`; `starts_at` còn ở tương lai |
| Thay đổi | `pending -> confirmed` |
| Side effect | Ghi `confirmed_by`, `confirmed_at`, audit event |
| Từ chối | Anonymous, staff, lịch đã hủy/đã confirm/quá khứ |

Giảng viên chỉ duyệt khi học viên trả lời được: “Nếu ai đó gọi API trực tiếp, backend sẽ kiểm gì và data nào được đổi?”

### Bước 2 - Vibe code Auth và session

**1. Mục đích**

Tạo actor đáng tin cậy để backend biết ai đang gọi command. Không có session thì RBAC và RLS không có cơ sở kiểm quyền.

**2. Prompt**

> Tôi đã có business command spec từ Bước 1. App Next.js hiện có các route dữ liệu là danh sách, chi tiết và tạo lịch. Hãy triển khai lát cắt Supabase Auth gồm sign up, sign in, sign out, forgot password, update password và session guard. Giữ UI hiện có; liệt kê file sẽ đổi; có loading, error, redirect về trang ban đầu sau login. Không log password, token hoặc secret. Sau code, nêu cách test success, sai password, logout và session hết hạn.

**Đầu vào cần dán:** command spec Bước 1, danh sách routes UI và cấu trúc thư mục hiện tại.

**3. Kết quả mong đợi**

- Có `/sign-up`, `/sign-in`, `/forgot-password`, `/update-password`, `/profile`.
- User chưa login vào route dữ liệu bị chuyển đến sign-in; logout rồi refresh không còn vào lại được.
- UI có loading/error; không báo đăng nhập hay lưu dữ liệu thành công trước response.
- Session là nguồn xác định actor, không lấy email/user ID từ form.

### Bước 3 - Thiết kế security boundary cho backend

**1. Mục đích**

Xác định chính xác dữ liệu nào client được gửi, dữ liệu nào backend tự tính và secret nằm ở đâu. Bước này ngăn frontend tự gửi `role`, `status` hoặc `confirmed_by` để vượt quy trình.

| Thành phần | Quy tắc Hana Desk |
|---|---|
| `appointmentId` | Client được gửi nhưng backend phải validate UUID, tồn tại và quyền |
| `status`, `role`, `confirmed_by` | Client không được gửi; backend tự quyết theo rule/session |
| Actor audit | Lấy từ `auth.uid()`, không lấy từ form |
| Publishable key | Có thể ở browser khi RLS đúng |
| Service-role/API secret | Chỉ ở server/Edge Function env; không trả về browser |

**2. Prompt**

> Tôi đang triển khai backend command. Sau prompt này, tôi sẽ dán: tên command và input từ client, schema/bảng liên quan, side effect cần thực hiện. Hãy phân loại dữ liệu tin cậy và không tin cậy; nêu validation bắt buộc ở backend, secret phải đặt ở đâu, role/ownership cần kiểm ở đâu và những field client tuyệt đối không được gửi. Không đề xuất service-role ở browser.

**Đầu vào cần dán:** command spec Bước 1 và Auth/session flow Bước 2.

**3. Kết quả mong đợi**

Một bảng security decision. Với `confirmAppointment`, frontend chỉ gửi `appointmentId`; backend đọc actor từ session, tự gán status/confirmed fields và tạo audit event. Read/list đơn giản có thể dùng client + RLS; command có state transition, nhiều bảng, secret hoặc side effect phải qua server action, route handler, Edge Function hoặc database function.

### Bước 4 - Vibe code RBAC

**1. Mục đích**

Chuyển business command thành ma trận quyền rõ ràng. RBAC quyết định role nào được gọi command; ownership quyết định record nào họ được thao tác.

| Role | Quyền trong lab |
|---|---|
| `staff` | Tạo và đọc lịch mình tạo; không confirm/xóa |
| `manager` | Đọc lịch, gọi `confirmAppointment` theo rule; không đổi role |
| `admin` | Quyền quản trị đã định, ví dụ xóa record test; gán role qua admin-only flow |

**2. Prompt**

> Tôi đã có command spec ở Bước 1 và security decision ở Bước 3. Hãy thiết kế RBAC tối giản gồm staff, manager, admin cho một tổ chức demo. Trả về role-permission matrix, schema profile role, quyền thuộc ownership và quyền thuộc role, route/UI nào cần ẩn cho UX, check backend nào là bắt buộc và các test allow/deny. User không được tự chọn hoặc tự update role.

**Đầu vào cần dán:** command spec Bước 1, security decision Bước 3 và schema Buổi 4.

**3. Kết quả mong đợi**

- Có `profiles(id, display_name, role)` với `role` là enum `staff`, `manager`, `admin`.
- Role được seed/migration hoặc admin-only flow gán; client không có form/API tự update role.
- Có role-permission matrix được giảng viên duyệt trước khi viết policy.

### Bước 5 - Vibe code backend command

**1. Mục đích**

Đặt business rule vào backend thay vì chỉ ẩn nút ở UI. Command phải atomically kiểm quyền, kiểm state, update appointment và ghi audit event.

**2. Prompt**

> Tôi đã có command spec, Auth flow, security decision và role-permission matrix ở các Bước 1-4. Hãy triển khai command xác nhận lịch bằng một database function RPC hoặc Next.js Server Action; chọn một cách và giải thích lý do. Command chỉ nhận appointment ID, lấy actor từ session, kiểm tra manager/admin, chỉ cho pending chuyển confirmed khi lịch ở tương lai, ghi actor/time và audit event trong cùng transaction. Không nhận status, role hoặc actor ID từ client. Nêu grants cần thiết, lỗi nghiệp vụ và test staff/manager/admin.

**Đầu vào cần dán:** toàn bộ artefact Bước 1-4; không dán key/secret.

**3. Kết quả mong đợi**

- Có một command thực sự gọi được từ UI, không dùng generic `.update({ status })` ở client.
- Staff gọi trực tiếp command vẫn bị từ chối.
- Manager xác nhận lịch hợp lệ: status, người xác nhận, thời gian và event được tạo cùng lúc.
- Xác nhận lại, lịch quá khứ hoặc lịch đã hủy bị từ chối, không để lại event sai.

### Bước 6 - Thiết kế RLS và grants ở database

**1. Mục đích**

Đặt enforcement cuối cùng tại database. Kể cả khi ai đó sửa UI hay gửi request thủ công, direct query không được vượt ownership/RBAC hoặc bypass backend command.

**2. Prompt**

> Tôi đã có command implementation và role-permission matrix ở Bước 4-5. Hãy thiết kế migration RLS và grants cho `profiles`, `appointments` và `appointment_events`. Yêu cầu: user chỉ đọc profile mình; staff chỉ tạo/đọc lịch mình tạo; manager/admin đọc theo matrix; direct UPDATE status bị chặn để state transition buộc đi qua command; chỉ admin xóa record test; audit event chỉ người phù hợp đọc. Kiểm tra role escalation, USING, WITH CHECK, function execute privilege, security definer và search path. Trả SQL migration cùng test allow/deny cho staff/manager/admin. Không tắt RLS để sửa lỗi.

**Đầu vào cần dán:** role-permission matrix Bước 4, command Bước 5, ownership fields và schema Buổi 4.

**3. Kết quả mong đợi**

- RLS bật cho bảng exposed; policy/grant khớp role matrix.
- Không có policy rộng kiểu `using (true)` chỉ để UI hoạt động.
- Direct update trạng thái bị chặn; command hợp lệ vẫn chạy với quyền đã giới hạn.
- Nếu dùng `security definer`, hàm kiểm role bên trong, khóa `search_path`, revoke quyền mặc định và chỉ grant execute cho `authenticated`.

### Bước 7 - Test quyền bằng ba tài khoản

**1. Mục đích**

Chứng minh security bằng hành vi thực tế, không bằng việc “đã viết policy”.

**2. Prompt**

> Dựa trên command, role matrix và RLS migration ở các Bước 4-6, hãy tạo test matrix cho staff, manager, admin và unauthenticated. Mỗi test phải nêu: account, thao tác UI/URL/RPC, kết quả mong đợi trong database và bằng chứng cần chụp. Bao gồm owner spoof, direct request, state transition sai, logout và sai password.

**Đầu vào cần dán:** role matrix Bước 4, command Bước 5 và RLS/grants Bước 6.

**3. Kết quả mong đợi**

| Test | Kết quả đạt |
|---|---|
| Staff tạo lịch pending | Có `created_by` đúng |
| Staff gọi confirm hoặc sửa role | Bị từ chối, data không đổi |
| Manager confirm lịch hợp lệ | Status + audit event xuất hiện |
| Manager confirm lại/quá khứ/hủy | Bị từ chối, không event mới |
| Admin xóa record test | Được nếu matrix cho phép |
| Unauthenticated | Bị redirect và không đọc được data riêng |

## 6. Bài toán thực tế mở rộng

| Case | Business command | RBAC/RLS cần chứng minh |
|---|---|---|
| CRM mini | Manager chuyển lead thành qualified | Sales chỉ thấy lead mình; manager duyệt theo team model |
| Đội thi công | Manager giao/đóng task | Assignee cập nhật đúng phạm vi; manager xem toàn bộ |
| Shop đóng gói | Manager chuyển `packing -> shipped` | Staff không tự giao hàng bằng request sửa status |
| Thu chi | Admin duyệt/xóa giao dịch | Nhân viên không tự sửa số tiền đã duyệt |

## 7. Rubric và nộp bài

| Tiêu chí | Điểm |
|---|---:|
| Business command spec từ user story | 15 |
| Auth, session, protected route | 15 |
| Security boundary và secrets | 10 |
| RBAC role-permission matrix | 15 |
| Backend command + audit event | 20 |
| RLS/grants đúng và không leo thang quyền | 15 |
| Test staff/manager/admin có bằng chứng | 10 |

**Nộp bài:** video 2-3 phút: staff tạo lịch -> staff bị từ chối confirm -> manager confirm -> xem audit event -> admin action; migration, command implementation, RLS/grants; README có role matrix, business rule, test matrix và known limitations.

## 8. Handoff Buổi 6

Buổi 6 dùng security boundary đã có để thiết kế AI feature/API: input nào được gửi AI, secret nằm ở server nào, user duyệt output ở đâu và record nào phải qua ownership/RLS check.

Thông điệp chốt: **Vibe code backend tốt bắt đầu từ luật nghiệp vụ, không bắt đầu từ code. Auth nhận diện actor; RBAC xác định quyền; RLS bảo đảm quyền đó vẫn đúng khi client bị can thiệp.**
