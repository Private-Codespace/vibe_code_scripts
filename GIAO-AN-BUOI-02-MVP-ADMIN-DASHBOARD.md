# Giáo án chi tiết - Buổi 2: Từ User Story đến UI và kế hoạch triển khai với AI

**Thời lượng:** 180 phút  
**Sản phẩm cuối buổi:** Một implementation pack cho MVP dashboard: User Stories đã ưu tiên, page map, project structure, wireframe, UI mock, lựa chọn công nghệ và kế hoạch triển khai.

## 1. Mục tiêu và quy trình

Buổi 1 tạo landing page để giới thiệu giá trị. Buổi 2 biến một nhu cầu vận hành thành thiết kế có thể triển khai có kiểm soát.

~~~text
User Story
-> AI phân tích Must Have / Should Have / Later
-> Page map
-> Project structure
-> UX brief từng page
-> Wireframe
-> UI mock
-> Công nghệ + implementation plan
~~~

Trong Buổi 2 dùng mock data cục bộ. Chưa làm database, Auth, API, thanh toán, SMS hay CRUD server thật; các phần đó thuộc Buổi 4-6.

| Buổi | Kết quả |
|---|---|
| 1 | Landing page, brief, prompt cơ bản |
| 2 | Implementation pack và UI mock |
| 3 | Frontend nhiều màn hình, component, responsive |
| 4 | Database/Supabase/CRUD thật |
| 5 | Auth, phân quyền và RLS |

### Đầu ra có thể quan sát

1. Viết 5-8 User Stories có vai trò, hành động, lợi ích.
2. Nhờ AI phân loại Must/Should/Later, rồi tự phản biện scope.
3. Tạo page map tối đa 5-7 pages và ba luồng chính.
4. Nhờ AI đề xuất routes, components, types, mock data trước khi code.
5. Viết UX brief, tạo wireframe, rồi tạo UI theo từng page.
6. Chốt stack và yêu cầu AI lập kế hoạch code/test theo lát cắt.

| Hạng mục | Điều kiện đạt |
|---|---|
| Scope | Tối đa 3 Must stories; một thực thể dữ liệu chính |
| Page map | Tối đa 7 pages; mỗi page gắn với một User Story |
| Structure | Có routes, components, types, mock data; không over-engineer |
| Thiết kế | Wireframe + UI cho dashboard, list, detail, create/edit; có states quan trọng |
| Kế hoạch | Phân biệt công nghệ dùng ngay và để sau |

## 2. Chuẩn bị và quy tắc

### Giảng viên

- Starter Next.js App Router + TypeScript + Tailwind chạy sẵn.
- Case demo xuyên suốt: **Hana Desk**, dashboard lễ tân spa quản lý lịch hẹn.
- Mẫu User Story canvas, bảng Must/Should/Later, giấy hoặc Figma/FigJam để wireframe.
- Ví dụ AI làm sai: tạo quá nhiều pages, tự cài library, tạo UI trước luồng, hoặc nói mock data đã lưu thật.

### Học viên

- Project Buổi 1 hoặc starter, một bài toán vận hành thật.
- Không dùng dữ liệu khách hàng, doanh thu hay secret thật.

### Quy tắc làm việc với AI

1. Prompt phải nêu đầu vào, đầu ra, phạm vi, điều chưa làm và tiêu chí kiểm tra.
2. AI nêu kế hoạch/files trước khi sửa nhiều code.
3. Làm một page hoặc component mỗi lần; không yêu cầu làm cả dashboard.
4. Phản biện wireframe trước UI; kiểm tra UI trước khi triển khai chức năng.
5. Mock state phải được ghi rõ; không nói đã lưu/đã gửi/đã thanh toán khi chưa có backend.

## 3. Lý thuyết áp dụng

### 3.1 User Story

Mẫu: “Là **[vai trò]**, tôi muốn **[hành động]** để **[lợi ích]**.”

Ví dụ Hana Desk:

1. Là lễ tân, tôi muốn xem lịch hẹn hôm nay để chuẩn bị đón khách.
2. Là lễ tân, tôi muốn lọc lịch chờ xác nhận để không bỏ sót cuộc gọi lại.
3. Là lễ tân, tôi muốn xem chi tiết lịch để biết dịch vụ và ghi chú.
4. Là lễ tân, tôi muốn tạo lịch từ cuộc gọi để ghi nhận khách mới.
5. Là lễ tân, tôi muốn đổi trạng thái lịch để danh sách phản ánh việc đã xử lý.

“Tôi muốn CRM/dashboard đẹp/quản lý mọi thứ” không phải User Story vì không xác định hành động và tiêu chí hoàn thành.

### 3.2 Must Have / Should Have / Later

| Nhóm | Câu hỏi | Ví dụ Hana Desk |
|---|---|---|
| Must | Bỏ đi thì luồng cốt lõi có hỏng không? | Xem list, filter status, detail, tạo/cập nhật mock |
| Should | Có giá trị nhưng MVP vẫn dùng được nếu thiếu? | Tìm theo tên, confirm trước khi hủy |
| Later | Có phụ thuộc ngoài phạm vi/chưa kiểm chứng? | SMS/Zalo, thanh toán, nhiều chi nhánh, Auth |

Một case chỉ chọn một thực thể chính: appointment, lead, task, order hoặc transaction.

### 3.3 Page map truy ngược từ story

| Must story | Page cần thiết | Không cần trong MVP |
|---|---|---|
| Xem lịch hôm nay | Dashboard | Report riêng |
| Lọc lịch | Appointment list | Bộ lọc nâng cao |
| Xem lịch | Appointment detail | CRM khách hàng độc lập |
| Tạo lịch | Create form | Booking công khai |
| Đổi trạng thái | Detail action | Automation |

### 3.4 Project structure

~~~text
app/
  dashboard/page.tsx
  appointments/page.tsx
  appointments/new/page.tsx
  appointments/[id]/page.tsx
components/
  layout/app-sidebar.tsx
  dashboard/kpi-card.tsx
  appointments/appointment-list.tsx
  appointments/appointment-form.tsx
  ui/empty-state.tsx
  ui/status-badge.tsx
data/mock-appointments.ts
types/appointment.ts
~~~

Structure là bản đồ trách nhiệm, không phải danh sách folder chuyên nghiệp. Không tạo api, supabase, auth, store hay library nếu scope chưa cần.

### 3.5 UX brief trước wireframe

~~~text
Tên page:
Ai dùng / job cần hoàn thành:
Thông tin phải thấy trong 5 giây đầu:
CTA chính và action phụ:
Data/fields cần hiển thị:
Loading, empty, error, validation, success:
Quy tắc mobile và accessibility:
~~~

Wireframe quyết định hierarchy và luồng. UI quyết định typography, màu, spacing, component. Không bỏ wireframe để AI không làm UI đẹp nhưng thiếu CTA/state.

### 3.6 Công nghệ theo scope

| Nhu cầu | Dùng ngay | Để sau |
|---|---|---|
| Route/UI | Next.js App Router + React | - |
| Giảm lỗi AI code | TypeScript | - |
| Style nhanh | Tailwind CSS | - |
| Prototype data | TypeScript mock data | - |
| Form | React state + validation cơ bản | - |
| Data/auth thật | - | Supabase, Auth, RLS ở Buổi 4-5 |

Không thêm ORM, state manager, chart library, OpenAI API, Stripe hay Docker chỉ vì AI đề xuất.

## 4. Kịch bản đứng lớp 180 phút

| Thời gian | Bước | Giảng viên | Học viên | Checkpoint |
|---|---|---|---|---|
| 00:00-00:15 | Đặt bài toán | So sánh landing/dashboard, giới thiệu quy trình | Chọn case | Nêu được người vận hành và việc họ cần làm |
| 00:15-00:40 | User Story | Demo viết/sửa story | Viết 5-8 stories | Có vai trò, hành động, lợi ích |
| 00:40-01:00 | AI cắt scope | Demo prompt Must/Should/Later | Phân loại, chốt 3 Must | Có danh sách không làm |
| 01:00-01:20 | Page map | Demo story -> page -> flow | Tạo 3 flows, map 5-7 pages | Mọi page phục vụ Must |
| 01:20-01:35 | Nghỉ + clinic | Giảm scope case quá lớn | Sửa scope | Một thực thể chính |
| 01:35-01:52 | Project structure | Demo routes/components/types | Review structure | Khớp page map, chưa code lan man |
| 01:52-02:12 | UX/wireframe | Demo UX brief/list page | Wireframe 2 pages | CTA, hierarchy, states rõ |
| 02:12-02:35 | UI từ wireframe | Demo code một page | Tạo dashboard + list/form UI | UI bám wireframe |
| 02:35-02:50 | Tech/plan | Demo technology proposal | Chốt stack/plan | Không nhảy database/Auth |
| 02:50-03:00 | Review | Điều phối review chéo | Demo một flow, backlog | Có P0/P1/P2 |

## 5. Demo xuyên suốt: Hana Desk

### Bối cảnh

Hana Spa nhận lịch qua điện thoại, Facebook và Zalo. Lễ tân ghi chú ở nhiều nơi nên bỏ sót khách chờ xác nhận.

- Người dùng: lễ tân một chi nhánh.
- Thực thể: appointment.
- MVP: xem, lọc, mở chi tiết, tạo/cập nhật lịch bằng mock data.
- Không làm: khách tự booking, thanh toán, SMS/Zalo, lịch nhân viên, nhiều chi nhánh, database/Auth.

### Bước 1 - Prompt AI phân tích User Stories

> Bạn là product manager cho MVP nội bộ. Phân loại các User Stories dưới đây của Hana Desk vào Must Have, Should Have hoặc Later. Với từng quyết định, giải thích theo ba tiêu chí: có cần cho luồng cốt lõi không, có phụ thuộc hệ thống ngoài không, có kiểm chứng bằng prototype mock không. Sau đó đề xuất tối đa 3 luồng MVP và nêu phần không làm ở phiên bản 1. Không tự thêm tính năng.

Học viên dán 5-8 stories vào cuối prompt, sau đó tự chốt:

| Story | AI xếp | Tôi chốt | Lý do |
|---|---|---|---|
| Xem lịch hôm nay | Must | Must | Điểm bắt đầu vận hành |
| Tìm theo tên | Should | Should | Có thể làm sau list cơ bản |
| Gửi SMS | Later | Later | Cần nhà cung cấp ngoài |

### Bước 2 - Prompt AI tạo page map

> Dựa trên Must Have stories và 3 luồng MVP, hãy tạo page map tối đa 5 pages. Mỗi page gồm: route, mục tiêu, story phục vụ, CTA chính, data hiển thị và states. Không thêm Settings, Reports, Customers, Auth hay page không phục vụ Must. Mô tả 3 luồng bằng mũi tên.

| Page | Route | CTA | State |
|---|---|---|---|
| Dashboard | /dashboard | Xem tất cả lịch | loading, summary rỗng |
| Appointment list | /appointments | Tạo lịch hẹn | loading, empty, error |
| Detail | /appointments/[id] | Cập nhật status | not found, confirm |
| Create | /appointments/new | Lưu lịch demo | validation, success mock |
| Edit | /appointments/[id]/edit | Lưu thay đổi demo | validation, cancel |

### Bước 3 - Prompt AI tạo structure

> Bạn là senior Next.js architect. Từ page map này, đề xuất project structure tối giản cho Next.js App Router + TypeScript + Tailwind. Dùng mock data cục bộ; chưa có database, Auth hay API. Trả về cây thư mục, mục đích từng file/folder, type Appointment tối thiểu và file mock data. Tách component chỉ khi dùng lại hoặc giúp page dễ đọc. Không cài library, không viết code.

Review: route phải khớp page map; type/mock data nằm ngoài component; không tạo layer giả tạo.

### Bước 4 - UX brief và wireframe

~~~text
Người dùng: lễ tân.
Job: tìm lịch cần gọi xác nhận hoặc tạo lịch mới.
5 giây đầu: ngày đang xem, CTA Tạo lịch, danh sách, filter status.
Record: giờ, tên khách giả, dịch vụ, status, ghi chú ngắn.
Empty: chưa có lịch + CTA tạo lịch.
Error: không tải được dữ liệu minh họa + Thử lại.
Mobile: chuyển table thành cards.
~~~

> Bạn là UX designer. Từ UX brief này, tạo wireframe low-fidelity bằng ASCII/Markdown cho Appointment List desktop và mobile. Thể hiện hierarchy, search/filter, CTA, list, empty và error state. Chưa chọn màu/font/ảnh. Giải thích ngắn vị trí CTA và thông tin ưu tiên. Không thêm tính năng ngoài brief.

### Bước 5 - Tạo UI từ wireframe

> Bạn là senior frontend developer. Dựa đúng wireframe đã duyệt, triển khai page /appointments trong project Next.js + TypeScript + Tailwind hiện có. Dùng type và mock data hiện có. Desktop hiển thị list/table dễ scan; ở 375 px chuyển card; status có text; CTA Tạo lịch hẹn rõ; có empty/error state. Không thêm thư viện, backend/API/Auth, không sửa dashboard/sidebar/route khác. Trước khi code, nêu files đổi. Sau khi code, nêu cách test 375/768/1440 px và giới hạn mock.

### Bước 6 - Tech proposal và implementation plan

> Bạn là technical lead. Với page map, structure, wireframe và UI prototype này, hãy chia đề xuất thành: (A) công nghệ dùng ngay để hoàn thiện frontend mock, (B) công nghệ dùng sau khi UI được duyệt. Mỗi lựa chọn phải gắn với Must story, nêu trade-off. Sau đó lập plan 6-8 bước; mỗi bước có mục tiêu, files/routes tác động, rủi ro và cách test. Không đề xuất công nghệ không phục vụ Must và không triển khai database/Auth/API trong plan hiện tại.

Plan mong đợi: type/mock data -> app shell -> dashboard -> list/filter/states -> detail/status mock -> form/validation -> QA -> Supabase ở Buổi 4, Auth/RLS ở Buổi 5.

## 6. Hướng dẫn thực hành theo quy trình

### A. User Stories (25 phút)

| Vai trò | Tôi muốn | Để | Thực thể |
|---|---|---|---|
| Sales | lọc lead cần follow-up | gọi lại đúng lúc | lead |

Checklist: vai trò cụ thể? hành động quan sát được? lợi ích thực tế? có gộp hai việc? có thể demo/test?

### B. AI cắt scope (20 phút)

Dùng prompt Bước 1. Giữ tối đa ba Must stories. Nếu AI xếp dark mode, analytics, Auth, notifications là Must mà không chứng minh luồng cốt lõi, học viên phải loại ra.

### C. Story -> page map (20 phút)

~~~text
Luồng: điểm bắt đầu -> hành động -> kết quả -> edge state
Page | Route | Story | CTA | Data | States
~~~

Không giữ page chỉ vì dashboard nào cũng cần có.

### D. Structure trước code (17 phút)

Review: routes khớp map? type/mock data tách khỏi UI? có file/folder/library nào chưa phục vụ Must không? Chỉ khi đạt mới tạo UI.

### E. UX brief -> wireframe (20 phút)

Ưu tiên dashboard/list hoặc list/create-edit. Mỗi wireframe có CTA, hierarchy, empty/error/validation.

> Review wireframe theo Must stories. Nêu tối đa 3 vấn đề P0/P1 về hierarchy, CTA, thông tin thiếu, states và mobile. Không nhận xét màu sắc. Với mỗi vấn đề, đề xuất thay đổi nhỏ nhất.

### F. Wireframe -> UI (23 phút)

Thứ tự: app shell -> dashboard/list -> shared status/empty/error -> detail -> create/edit form. Sau từng lát: chạy app, kiểm tra 375/768/1440 px, console/type và so UI với wireframe.

### G. Tech + plan (15 phút)

Hỏi AI theo scope cụ thể, không hỏi công nghệ tốt nhất là gì. Công nghệ chỉ được giữ khi phục vụ Must story ngay lúc này.

## 7. Case thực tế

| Case | Người dùng / thực thể | Must story gợi ý | Later / edge case |
|---|---|---|---|
| CRM agency | Sales / lead | follow-up, filter, detail, create/update mock | Email automation, scoring AI; lead quá hạn/thiếu SĐT |
| Spa/phòng khám | Lễ tân / appointment | xem hôm nay, filter, detail, create/status mock | Thanh toán, SMS, lịch nhân viên; hủy/trùng giờ |
| Đội thi công | Điều phối / task | việc hôm nay/quá hạn, filter, detail, update mock | Gantt, chat, upload; task bị block |
| Shop online | Đóng gói / order | đơn mới, filter đóng gói, detail, status mock | Vận chuyển, thanh toán, kho; thiếu địa chỉ/hủy |
| Quán cà phê | Chủ quán / transaction | thu-chi tuần, filter, thêm/sửa mock | Thuế, ngân hàng; số tiền sai/no data |

Không dùng dữ liệu thật trong các case tài chính, sức khỏe hoặc khách hàng.

## 8. Phản biện thực hành

### Bài 1 - Cắt scope (8 phút)

Đề: Phần mềm spa có đặt lịch, khách hàng, nhân viên, ca làm, kho, doanh thu, khuyến mãi, chatbot và nhắc Zalo. Nhóm viết 5 stories, nhờ AI phân loại, chỉ giữ ba Must. Hướng đáp án: lễ tân + appointment + lịch hôm nay; các phần còn lại là Later.

### Bài 2 - Traceability (10 phút)

Cho map gồm Dashboard, Appointments, Reports, Settings, Customers, Chatbot. Gạch pages không truy ngược được về Must stories, sau đó chỉ ra route/component/type/mock data cần cho pages còn lại.

### Bài 3 - Wireframe triage (10 phút)

Wireframe có table 8 cột ở mobile, CTA chìm dưới chart, thiếu empty/error state, nút Lưu không nói chỉ là mock. Mỗi nhóm nêu P0/P1/P2 và viết prompt: hiện trạng -> mục tiêu -> phạm vi component -> không được đổi -> cách test.

### Bài 4 - Tech proposal challenge (8 phút)

AI đề xuất Supabase, Prisma, Zustand, shadcn/ui, Recharts, OpenAI API, Stripe, Resend, Docker cho prototype 4 pages mock. Học viên giữ/bỏ/để sau từng thứ bằng câu hỏi: nó phục vụ Must story nào ngay bây giờ?

## 9. Deliverables, QA và rubric

### Deliverables

- [ ] 5-8 User Stories và 3 Must stories.
- [ ] Bảng Must/Should/Later có lý do.
- [ ] Page map: route, purpose, story, CTA, states.
- [ ] Project structure AI đề xuất đã review.
- [ ] UX brief + wireframe ít nhất hai pages.
- [ ] UI mock dashboard/list và một detail hoặc form.
- [ ] Tech proposal: dùng ngay/để sau.
- [ ] Implementation plan và backlog P0/P1/P2.

### QA

- [ ] Mỗi page/UI truy ngược được về story.
- [ ] Không có scope/library không phục vụ luồng chính.
- [ ] CTA/hierarchy đúng wireframe.
- [ ] Có empty/loading/error/validation ở nơi cần.
- [ ] 375 px không tràn; status không chỉ dùng màu.
- [ ] Không tuyên bố chức năng server chưa tồn tại.
- [ ] Không dùng dữ liệu nhạy cảm/secret thật.

### Rubric (10 điểm)

| Hạng mục | Điểm |
|---|---:|
| User Story và scope hợp lý | 2 |
| Page map và ba luồng rõ | 2 |
| Structure không over-engineer | 2 |
| Wireframe/UI responsive, trung thực | 2 |
| Technology/implementation plan đúng thứ tự | 2 |

## 10. Bài tập về nhà, cầu nối Buổi 3

1. Hoàn thiện tất cả UI pages Must Have theo map và structure.
2. Tạo mock data tối thiểu 8 records với đủ states.
3. Hoàn thiện một form có validation và success state ghi rõ là demo.
4. Cập nhật README: bài toán, Must stories, page map, stack hiện tại, phần chưa làm.
5. Lập backlog 5 việc: P0 chặn luồng, P1 ảnh hưởng UX, P2 là polish.

**Thông điệp chốt:** Đừng nhờ AI làm cả ứng dụng từ ý tưởng mơ hồ. Dùng User Story để chốt phạm vi; page map, structure và wireframe để biến ý định thành kế hoạch; rồi giao cho AI từng lát cắt để triển khai và kiểm tra.
