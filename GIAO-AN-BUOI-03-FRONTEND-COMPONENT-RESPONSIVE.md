# Giáo án chi tiết - Buổi 3: Frontend chuyên nghiệp với Component, State và Responsive

**Thời lượng:** 180 phút  
**Đối tượng:** Người mới đã hoàn thành Buổi 1-2: có product brief, tối đa 3 luồng MVP, page map, wireframe và project Next.js + TypeScript + Tailwind chạy được.  
**Sản phẩm cuối buổi:** Một frontend MVP bấm được với 5-7 màn hình, dùng dữ liệu mock có type; có dashboard, list, detail, create/edit; có loading, empty, error, not-found và validation state; dùng tốt tại 375 px, 768 px, 1440 px.

## 1. Vị trí của Buổi 3 trong lộ trình

Buổi 2 trả lời: **cần làm màn hình nào, cho ai, để hoàn thành việc gì?** Buổi 3 trả lời: **tổ chức giao diện đó thế nào để dễ dùng, dễ sửa và không vỡ trên điện thoại?**

```text
Buổi 2: user story -> MVP -> page map -> wireframe
Buổi 3: wireframe -> routes -> typed mock data -> components -> states -> responsive QA
Buổi 4: mock data -> Supabase + CRUD thật
Buổi 5: auth + RLS
```

Giới hạn chủ động của buổi này:

- Có thể mô phỏng tạo, sửa, đổi trạng thái trong bộ nhớ trình duyệt.
- Chưa có database, API, Supabase, đăng nhập, phân quyền, gửi email/SMS/thanh toán.
- Không nói “đã lưu thành công” nếu tải lại trang sẽ mất dữ liệu; phải dùng chữ **“Đã cập nhật dữ liệu minh họa trong phiên này”**.

Nguyên tắc quan trọng: giao diện không phải ảnh chụp đẹp. Nó là một hệ thống gồm dữ liệu, hành động, trạng thái và quy tắc hiển thị ở nhiều kích thước màn hình.

## 2. Mục tiêu đầu ra có thể quan sát

Kết thúc buổi, học viên có thể:

1. Giải thích khi nào nên đặt code ở route/page, component dùng lại, type và mock data.
2. Chuyển page map Buổi 2 thành routes App Router không thừa trang.
3. Tạo type cho thực thể chính và ít nhất 8 bản ghi mock bao phủ các tình huống bình thường/lỗi biên.
4. Xây component nhỏ có trách nhiệm rõ: app shell, navigation, status badge, empty/error/loading state, list/card, form và toast.
5. Dựng một luồng bấm được từ danh sách -> chi tiết -> sửa/tạo -> phản hồi trạng thái, dù chưa lưu thật.
6. Kiểm tra code AI theo TypeScript, console, semantic HTML, keyboard, mobile và hard-code/lặp code.
7. Viết prompt giao việc nhỏ cho AI, có phạm vi file, ràng buộc và cách test.

| Hạng mục | Điều kiện đạt tối thiểu |
|---|---|
| Phạm vi | 5-7 routes bám 3 Must stories, không thêm database/Auth/API |
| Data | Một type chính; >= 8 record mock; có trạng thái đa dạng và record lỗi biên |
| UI | Dashboard, list, detail, create hoặc edit; shared states loading/empty/error/not-found |
| Tương tác | Filter/search/form validation hoặc đổi status chạy được ở client và ghi rõ là mock |
| Responsive | Không scroll ngang tại 375 px; table có phương án card/scroll có chủ đích; CTA dùng được |
| Chất lượng | Không lỗi TypeScript/console nghiêm trọng; thao tác chính bằng bàn phím; status không chỉ phân biệt bằng màu |

## 3. Chuẩn bị và quy tắc lớp học

### Giảng viên chuẩn bị

- Starter Next.js App Router + TypeScript + Tailwind; đã chạy bằng `npm run dev`.
- Case xuyên suốt **Hana Desk**: dashboard nội bộ để lễ tân spa quản lý lịch hẹn.
- Một branch/commit có chủ đích các lỗi để demo: key theo index, dùng `any`, table vỡ mobile, nút icon không có nhãn, form báo lưu thành công giả.
- DevTools mở sẵn với Device Toolbar; thiết bị kiểm tra 375 px, 768 px, 1440 px.
- Bộ mock data không có số điện thoại, tên, bệnh án hay doanh thu thật của khách hàng.

### Học viên chuẩn bị

- Project Buổi 2, page map, wireframe và danh sách Must/Should/Later đã duyệt.
- Một thực thể chính: `appointment`, `lead`, `task`, `order` hoặc `transaction`.
- Đủ dữ liệu giả để tạo tình huống thật: mới tạo, cần xử lý, hoàn thành, hủy, tên dài, ghi chú dài, không có kết quả.

### Quy tắc làm việc với AI

1. Một prompt chỉ nhận **một lát cắt**: một route hoặc một component và state liên quan.
2. Trước khi sửa, AI phải nêu file sẽ thay đổi, điều không thay đổi và kế hoạch ngắn.
3. Không cho AI cài library, đổi toàn bộ design system hay thay routes nếu không có lý do từ User Story.
4. Sau mỗi lần sửa: chạy app -> đọc terminal -> kiểm tra màn vừa đổi -> kiểm tra 375/768/1440 -> console -> commit nhỏ nếu ổn.
5. Không copy lỗi hoặc code lớn vào AI mà thiếu mục tiêu, bước tái hiện, file liên quan và hành vi mong đợi.

## 4. Lý thuyết áp dụng ngay vào sản phẩm

### 4.1 Từ User Story đến “lát cắt dọc” của frontend

Một lát cắt dọc là phần nhỏ người dùng có thể nhìn, bấm và nhận phản hồi từ đầu đến cuối. Nó tốt hơn việc làm hết sidebar, rồi hết màu sắc, rồi mới có màn hình dùng được.

Ví dụ story: “Là lễ tân, tôi muốn lọc lịch chờ xác nhận để không bỏ sót cuộc gọi lại.”

```text
Route /appointments
  -> mock appointments có status = pending
  -> status filter
  -> AppointmentList desktop/mobile
  -> Empty state khi không còn kết quả
  -> link đến /appointments/[id]
  -> test ở 375 px và keyboard
```

Đây là lát cắt hoàn chỉnh. Nó **không** đòi hỏi Supabase, API, lịch nhân viên, SMS hay biểu đồ doanh thu.

### 4.2 Chia trách nhiệm: route, component, type, data

| Nơi đặt | Câu hỏi quyết định | Ví dụ Hana Desk |
|---|---|---|
| `app/.../page.tsx` | Đây có phải màn hình gắn URL không? | `/appointments`, `/appointments/[id]` |
| `components/...` | Có dùng lại hoặc giúp page đọc được hơn không? | `AppointmentList`, `StatusBadge`, `EmptyState` |
| `types/...` | Đây có mô tả dữ liệu dùng ở nhiều nơi không? | `Appointment`, `AppointmentStatus` |
| `data/...` | Đây là dữ liệu giả, không phải UI không? | `mockAppointments` |
| component client | Có click, input, modal hoặc state trình duyệt không? | filter, form, toast |

Đừng tách component chỉ vì một khối có 5 dòng. Tách khi nó có trách nhiệm rõ, lặp lại, hoặc khiến page khó đọc. Ngược lại, đừng nhét type và mảng mock data dài vào `page.tsx`: khi sang Buổi 4, sẽ khó thay bằng nguồn dữ liệu thật.

Gợi ý cấu trúc tối giản:

```text
app/
  (dashboard)/layout.tsx
  (dashboard)/dashboard/page.tsx
  (dashboard)/appointments/page.tsx
  (dashboard)/appointments/new/page.tsx
  (dashboard)/appointments/[id]/page.tsx
  (dashboard)/appointments/[id]/edit/page.tsx
components/
  layout/app-sidebar.tsx
  layout/app-header.tsx
  appointments/appointment-list.tsx
  appointments/appointment-card.tsx
  appointments/appointment-form.tsx
  appointments/appointment-status-badge.tsx
  ui/empty-state.tsx
  ui/error-state.tsx
  ui/loading-skeleton.tsx
  ui/toast.tsx
data/mock-appointments.ts
types/appointment.ts
```

Nhóm `(dashboard)` chỉ giúp tổ chức file; nó không xuất hiện trong URL. Chỉ tạo `ui/` cho primitive thật sự dùng lại; không cần sao chép nguyên một design system vào MVP.

### 4.3 Type không phải thủ tục - nó là hợp đồng dữ liệu

```ts
export type AppointmentStatus = "pending" | "confirmed" | "completed" | "cancelled";

export type Appointment = {
  id: string;
  customerName: string;
  serviceName: string;
  startsAt: string;
  status: AppointmentStatus;
  note?: string;
};
```

Type buộc UI xử lý trường hợp `note` không có, ngăn gõ nhầm status như `"comfirmed"`, và báo lỗi sớm khi mock data thiếu trường. Nó cũng là bản nháp tốt cho schema Buổi 4 - nhưng chưa phải schema database.

Mock data phải có chủ đích:

| Record | Mục đích test |
|---|---|
| `pending` hôm nay | filter và CTA xác nhận |
| `confirmed` | badge/luồng bình thường |
| `completed` | action không còn phù hợp |
| `cancelled` | style vẫn đọc được, không chỉ dùng màu xám |
| tên hoặc dịch vụ dài | text wrap/truncation không phá layout |
| `note` trống | tránh hiển thị `undefined` |
| dữ liệu 8-12 record | list có mật độ thật, không chỉ một card đẹp |
| mảng rỗng và lỗi giả lập | empty/error state |

### 4.4 Component trạng thái làm cho app trung thực

Mỗi màn hình đọc dữ liệu cần trả lời: nếu đang chờ thì sao, không có gì thì sao, có lỗi thì sao, không tìm thấy record thì sao?

| State | Khi nào hiển thị | Nội dung tối thiểu | Hành động |
|---|---|---|---|
| Loading | đang đổi filter/chờ data giả | skeleton có hình dáng gần nội dung | không cần CTA |
| Empty toàn cục | chưa có lịch nào | lý do dễ hiểu + minh hoạ nhẹ | “Tạo lịch hẹn” |
| Empty sau filter | có dữ liệu nhưng filter không khớp | nêu filter hiện tại | “Xóa bộ lọc” |
| Error | không tải được mock/API mô phỏng | việc gì xảy ra, không đổ lỗi người dùng | “Thử lại” |
| Not found | URL id không tồn tại | record không tồn tại hoặc đã bị xóa | “Quay lại danh sách” |
| Validation | nhập thiếu/sai | lỗi ngay cạnh field, bằng chữ | sửa field đó |
| Success mock | action client xong | nêu rõ dữ liệu chỉ trong phiên | “Xem chi tiết” hoặc tự chuyển trang |

Không dùng một màn hình trống cho cả loading/error/empty. Ba tình huống này đòi hỏi hành động người dùng khác nhau.

### 4.5 Responsive là ưu tiên nội dung, không phải co nhỏ desktop

Quy tắc ra quyết định:

1. Trên 375 px: để một cột; CTA chính dễ thấy; vùng bấm khoảng 44 px; label không biến mất.
2. Trên 768 px: tăng không gian và nhóm thông tin, không ép table nhiều cột.
3. Trên 1440 px: tận dụng không gian để quét dữ liệu, nhưng giới hạn độ dài dòng và giữ hierarchy.
4. Nếu một table không thể đọc trên mobile, đổi thành card có nhãn field hoặc giữ 2-3 field quan trọng; không chỉ thu nhỏ chữ đến mức không dùng được.
5. Màu chỉ là tín hiệu phụ. Badge phải có text như “Chờ xác nhận”; icon-only button phải có nhãn truy cập hoặc tooltip.

Ví dụ list lịch hẹn:

| Desktop | Mobile |
|---|---|
| Table: giờ, khách, dịch vụ, status, hành động | Card: giờ + status ở đầu; khách/dịch vụ bên dưới; link “Xem chi tiết” |
| Search và filter cùng hàng | Search full width, filter xuống hàng hoặc popover đơn giản |
| Sidebar cố định | Sidebar thành menu đóng/mở hoặc thanh điều hướng gọn |

### 4.6 Form: cảm giác “làm được việc” trước khi có backend

Form mock vẫn cần hành vi đáng tin:

```text
Chưa nhập -> bấm Lưu -> validation cạnh field
Nhập hợp lệ -> trạng thái đang xử lý ngắn -> success nói rõ “minh họa”
Hủy -> quay lại/giữ lại dữ liệu theo quy tắc đã thông báo
```

Validation tối thiểu cho lịch hẹn: tên khách không rỗng, dịch vụ được chọn, ngày giờ hợp lệ, status thuộc danh sách cho phép. Không dùng `alert()` làm phản hồi chính. Chưa cần thư viện form nếu state đơn giản; tránh đưa form library vào chỉ vì AI đề xuất.

### 4.7 Dùng AI như reviewer thay vì máy phát code

Prompt review hiệu quả:

> Bạn là senior frontend reviewer. Review riêng component `AppointmentList` trong Next.js + TypeScript + Tailwind. Mục tiêu: lễ tân lọc và mở lịch hẹn. Kiểm tra TypeScript, key khi render list, dữ liệu thiếu, semantic HTML, keyboard/focus, responsive 375/768/1440 px, hard-code và trạng thái empty/error. Không viết lại cả component. Trả về bảng P0/P1/P2 gồm: vấn đề, bằng chứng ở dòng/đoạn nào, thay đổi nhỏ nhất, cách test lại. Không đề xuất database, Auth hay library mới.

AI không được thay thế việc mở trình duyệt. Nó có thể không biết font tràn, focus biến mất, hay breakpoint thật sự hỏng.

## 5. Kịch bản đứng lớp 180 phút

| Thời gian | Mục tiêu | Hoạt động giảng viên | Hoạt động học viên | Checkpoint |
|---|---|---|---|---|
| 00:00-00:10 | Nối Buổi 2 với Buổi 3 | Cho xem cùng một dashboard: một bản ảnh đẹp, một bản có states/luồng bấm; hỏi bản nào dùng được | Mở page map, chọn 3 Must stories | Xác nhận thực thể chính và 5-7 routes |
| 00:10-00:28 | Routes + cấu trúc | Vẽ route/page/component/type/data; demo cấu trúc Hana Desk | So structure với page map, gạch file dư | Không có API/Auth/store/library chưa cần |
| 00:28-00:48 | Typed mock data | Live-code `Appointment` và 8 record có edge case | Viết type + mock cho case riêng | TypeScript báo được dữ liệu thiếu/sai status |
| 00:48-01:10 | Component + app shell | Demo sidebar/header/status badge/empty state theo từng component | Dựng app shell và 2 component dùng lại | Navigation/active route rõ, badge có text |
| 01:10-01:25 | Nghỉ + clinic | Kiểm tra dự án không chạy, route sai, scope phình | Sửa blocker hoặc phản biện scope | App chạy, không để bạn nào kẹt setup |
| 01:25-01:45 | List responsive | Demo list table -> mobile card, search/filter mock | Làm list của mình bằng mock data | Filter thay đổi kết quả; mobile không tràn |
| 01:45-02:05 | Detail + form | Demo URL dynamic, not-found, form validation/success mock | Làm detail và create/edit của một record | Bấm được list -> detail -> form; data mock minh bạch |
| 02:05-02:23 | State + accessibility | Cố ý tạo loading/empty/error; demo Tab/Enter và DevTools | Thêm ít nhất 3 states, chạy keyboard check | Không dùng blank screen/màu-only state |
| 02:23-02:42 | QA & AI review | Đưa prompt review và lỗi mẫu; hướng dẫn ưu tiên P0/P1/P2 | Test 3 viewport, console, nhờ AI review hẹp | Danh sách lỗi có bằng chứng và cách retest |
| 02:42-03:00 | Demo chéo/chốt bài | Điều phối peer review theo rubric | Demo 1 luồng trong 90 giây, ghi backlog | Có video hoặc kế hoạch quay demo, backlog ưu tiên |

## 6. Demo xuyên suốt: Hana Desk

### 6.1 Bối cảnh và scope

Hana Spa nhận lịch qua điện thoại, Facebook và Zalo. Lễ tân cần một nơi để thấy lịch hôm nay, lọc lịch chờ xác nhận, mở chi tiết và tạo/cập nhật lịch minh họa.

| Có trong Buổi 3 | Không có trong Buổi 3 |
|---|---|
| Dashboard, list, filter, detail, create/edit mock, đổi status trong phiên | Database, login, nhiều chi nhánh, lịch nhân viên, booking công khai, SMS/Zalo, thanh toán, báo cáo |

Routes:

```text
/dashboard
/appointments
/appointments/new
/appointments/[id]
/appointments/[id]/edit
```

### 6.2 Bước 1 - Hợp đồng dữ liệu trước UI

> Bạn là TypeScript developer. Với MVP Hana Desk chỉ dùng mock data, hãy đề xuất type `Appointment` và union type cho status. Các field phải đủ phục vụ list, detail và create/edit: id, tên khách giả, dịch vụ, thời gian, trạng thái, ghi chú tùy chọn. Trả về type và 8-10 record mock bao phủ pending/confirmed/completed/cancelled, tên dài, ghi chú trống. Không tạo schema SQL, API, Supabase hay dữ liệu liên hệ thật. Giải thích record nào dùng để test edge case.

Sau khi AI trả lời, học viên kiểm tra: field nào không dùng ở page map thì xóa; field nào thiếu cho detail/form thì bổ sung. Không thêm `customerPhone`, `price`, `staffId` nếu scope chưa cần.

### 6.3 Bước 2 - Tạo list theo lát cắt

> Bạn là senior frontend developer. Triển khai riêng route `/appointments` cho Next.js App Router + TypeScript + Tailwind hiện có. Dùng `Appointment` và mock data đã có. Yêu cầu: tiêu đề, CTA “Tạo lịch hẹn”, filter status chạy bằng client state, list desktop dễ quét và card mobile tại 375 px, status badge có text, link đến detail. Có `loading`, `empty all`, `empty after filter` và `error` state mô phỏng rõ ràng. Không tạo API/database/Auth, không sửa route khác, không cài thư viện. Trước khi code: nêu tối đa 5 file đổi. Sau khi code: nêu cách test 375/768/1440, keyboard và từng state.

Kịch bản test giảng viên thực hiện trực tiếp:

1. Chọn “Chờ xác nhận” -> chỉ record pending hiện ra.
2. Chọn một filter không có record -> thông báo phải nói đang không có kết quả, có nút “Xóa bộ lọc”.
3. Thu về 375 px -> không còn table 6 cột bị ép; card vẫn cho biết giờ, khách, dịch vụ, trạng thái.
4. Dùng phím Tab rồi Enter -> focus thấy rõ và mở được detail.

### 6.4 Bước 3 - Detail và not-found

> Chỉ triển khai route `/appointments/[id]`. Đọc record từ mock data bằng id của URL. Nếu không tìm thấy, render not-found state có link quay về `/appointments`; không để lỗi runtime. Nếu tìm thấy, hiển thị thông tin đủ cho lễ tân: khách, dịch vụ, thời gian, status, ghi chú và CTA “Chỉnh sửa”. Trên mobile, thứ tự thông tin phải dễ quét. Không thêm fetch/API/Auth. Nêu file sửa và cách test id hợp lệ, id sai, note trống, tên dài.

Học viên thử URL như `/appointments/khong-ton-tai`; không chấp nhận trang trắng hay console error.

### 6.5 Bước 4 - Form tạo/sửa trung thực

> Chỉ tạo component `AppointmentForm` tái dùng cho create/edit mock. Fields: tên khách, dịch vụ, ngày giờ, status, ghi chú. Validate khi submit; lỗi phải xuất hiện cạnh field bằng text và input có liên kết hỗ trợ truy cập phù hợp. Khi hợp lệ, hiển thị toast “Đã cập nhật dữ liệu minh họa trong phiên này”; không khẳng định đã lưu database. Có nút Hủy quay về list/detail. Không cài form library, không gọi API. Nêu trạng thái component và cách test validation/keyboard/mobile.

### 6.6 Bước 5 - Review sửa nhỏ, không viết lại

> Review frontend của Hana Desk theo thứ tự P0/P1/P2. Phạm vi: `/appointments`, `AppointmentList`, `AppointmentForm`, `StatusBadge`. Tiêu chí: route/link đúng, TypeScript, state loading/empty/error/not-found, form validation, semantic HTML, focus/keyboard, mobile 375 px, hard-code/lặp code. Không thay UI đã duyệt, không thêm library/backend/Auth. Với mỗi lỗi: nêu dấu hiệu, mức độ, thay đổi nhỏ nhất, cách retest.

## 7. Hướng dẫn thực hành cho học viên

### Chặng A - Kiểm tra đầu vào từ Buổi 2 (8 phút)

Điền trước khi code:

```text
Thực thể chính:
3 Must stories:
5-7 routes thật sự cần:
Thông tin phải thấy trong list:
CTA chính của mỗi page:
State có thể xảy ra ở mỗi page:
Điều chưa làm đến Buổi 4-5:
```

Nếu chưa trả lời được, quay lại page map thay vì mở AI yêu cầu “làm dashboard đẹp”.

### Chặng B - Data-first (12 phút)

1. Tạo union type cho những giá trị hữu hạn: status, priority, category.
2. Tạo interface/type cho thực thể chính; không đặt mọi field là `string` nếu có thể hẹp hơn.
3. Thêm 8-12 record giả và 2-3 edge case.
4. Xem TypeScript có bắt được field sai hay status sai không.

Checklist:

- [ ] Không dùng `any` để dập lỗi.
- [ ] Có `id` ổn định, không dùng index làm key list.
- [ ] Dữ liệu không chứa khách hàng/số điện thoại/tài chính thật.
- [ ] Có dữ liệu đủ dài để thử vỡ layout.

### Chặng C - App shell và components dùng lại (15 phút)

Thứ tự gợi ý: `AppSidebar`/header -> `StatusBadge` -> `EmptyState` -> `ErrorState` -> `LoadingSkeleton`. Hãy tạo một contract đơn giản, ví dụ `StatusBadge` chỉ nhận một status hợp lệ và tự map label/style; page không tự viết bốn màu khác nhau.

Không tạo `Button`, `Card`, `Stack`, `Grid`, `Container`, `Typography` riêng nếu chỉ có một page và Tailwind class hiện tại vẫn đọc được. Mục tiêu là giảm lỗi khi thay đổi, không mô phỏng một thư viện UI.

### Chặng D - Xây luồng chính, không xây từng màn hình cô lập (35 phút)

Chọn một luồng Must và làm tới khi bấm được:

```text
Dashboard -> Appointment list -> Filter pending -> Detail -> Edit mock -> Success feedback
```

Sau đó mới làm luồng thứ hai. Mỗi đoạn chạy được mới commit. Nếu form chưa lưu thật, tuyệt đối không dùng copy “Lịch hẹn đã được lưu vào hệ thống”.

### Chặng E - Responsive và accessibility QA (18 phút)

| Bài test | 375 px | 768 px | 1440 px |
|---|---|---|---|
| Điều hướng | mở/đóng được, không che CTA | gọn, dễ hiểu | không lãng phí không gian |
| List | card hoặc strategy rõ, không tràn | nhóm cột hợp lý | scan nhanh nhiều record |
| Form | field full width, label/lỗi đọc được | grid khi có lợi | giới hạn chiều rộng đọc tốt |
| Action | bấm được, không sát mép | primary/secondary rõ | không rơi rớt giữa khoảng trắng |

Keyboard check: Tab từ đầu page, focus phải nhìn thấy; Enter/Space kích hoạt button; không dùng `div` có `onClick` thay button/link; icon button có `aria-label` hoặc text hiển thị.

### Chặng F - Sửa theo ưu tiên (12 phút)

| Mức | Ý nghĩa | Ví dụ |
|---|---|---|
| P0 | Chặn luồng hoặc gây hiểu nhầm dữ liệu | nút Lưu báo thành công nhưng không lưu; detail URL lỗi crash |
| P1 | Dùng được nhưng UX/accessibility sai đáng kể | table không đọc được mobile; focus không thấy; error không có Retry |
| P2 | Polish sau khi luồng ổn | spacing chưa đều; câu copy dài; animation không cần thiết |

Sửa một P0/P1 mỗi lần, retest đúng hành vi gây lỗi. Không “dọn code” cả project ở cuối giờ.

## 8. Bài toán thực tế để thực hành xử lý

Các bài dưới đây cố ý có ràng buộc, trạng thái lỗi biên và scope cám dỗ. Học viên chọn một case hoặc áp dụng cấu trúc cho dự án riêng.

### Bài 1 - CRM mini cho agency: không bỏ sót lead cần gọi lại (35 phút)

**Bối cảnh:** Sales agency đang nhận lead từ form, Facebook và giới thiệu. Mỗi ngày họ cần biết lead nào phải liên hệ trước, nhưng chưa cần gửi email tự động.

| Yêu cầu phải xử lý | Gợi ý UI/data |
|---|---|
| Xem lead cần follow-up hôm nay | dashboard KPI + danh sách ưu tiên |
| Lọc theo status và nguồn | filter client-side |
| Mở chi tiết, ghi note mock, đổi status | detail + edit mock |
| Lead không có số điện thoại hoặc note | trạng thái dữ liệu thiếu, không crash |
| Không tìm thấy theo filter | empty-after-filter + clear filter |

**Không làm:** gửi email, gọi điện, import CSV, chấm điểm AI, phân quyền sales, database.

**Yêu cầu nộp:** type `Lead`, 10 mock records, routes list/detail/create-edit, mobile card, form validation cho name/company/status, hai empty state.

**Câu hỏi phản biện:** Nếu filter “Cần gọi hôm nay” đang rỗng, CTA nên là “Tạo lead” hay “Xóa bộ lọc”? Khi nào dùng mỗi loại?  
**Hướng xử lý:** nếu toàn bộ hệ thống chưa có lead, dùng “Tạo lead”; nếu chỉ query hiện tại rỗng, dùng “Xóa bộ lọc”.

### Bài 2 - Điều phối đội thi công: task bị chặn (35 phút)

**Bối cảnh:** Điều phối viên cần biết các việc hôm nay, quá hạn và bị chặn để sắp người xử lý. Mỗi task có tiêu đề, công trình giả, hạn, priority, status, blocker note tùy chọn.

**Bài toán UX:** Một task `blocked` không được chỉ đỏ; người xem phải hiểu bị chặn bởi gì và action tiếp theo là gì.

Yêu cầu:

1. Tạo `TaskStatus = "todo" | "in_progress" | "blocked" | "done"` và type `Task`.
2. List desktop có thể scan hạn/priority/status; mobile chuyển card.
3. Detail chỉ hiện `blockerNote` khi status `blocked`; nếu không có note, hiển thị lời nhắc “Chưa ghi lý do bị chặn”.
4. Form không cho status `blocked` mà blocker note trống.
5. Tạo state “Không có task quá hạn” khác với “Chưa có task nào”.

**Lỗi thường gặp cần sửa:** AI dùng màu đỏ duy nhất cho blocked, đặt logic validation ở mỗi page khác nhau, hoặc nhét `blockerNote!` khiến runtime lỗi.

### Bài 3 - Shop đóng gói đơn: đơn thiếu địa chỉ (30 phút)

**Bối cảnh:** Nhân viên đóng gói cần lọc đơn mới, xem thông tin giao hàng, đánh dấu đang đóng gói/đã đóng gói. Chưa tích hợp thanh toán, kho hay đơn vị vận chuyển.

Tình huống cần giải quyết:

```text
Order #A103: status new, thiếu địa chỉ giao hàng
Order #A104: status packing, tên sản phẩm rất dài
Order #A105: cancelled, không được hiện CTA “Đánh dấu đã đóng gói”
```

**Đề:** Thiết kế action matrix trước khi code.

| Status | CTA chính | Cảnh báo/giới hạn |
|---|---|---|
| new | Bắt đầu đóng gói | chặn khi thiếu địa chỉ |
| packing | Hoàn tất đóng gói | hiển thị thông tin đơn |
| packed | Xem chi tiết | không lặp action |
| cancelled | Không có action xử lý | giải thích đơn đã hủy |

Học viên viết type, component `OrderStatusBadge`, function/logic chọn CTA và test đủ bốn status. Đây là cách tránh UI hiển thị các nút trái với trạng thái dữ liệu.

### Bài 4 - Quản lý thu chi cá nhân: nhập số tiền sai (25 phút)

**Bối cảnh:** Chủ quán muốn ghi thu/chi tuần bằng dữ liệu demo. Không kết nối ngân hàng, không tính thuế, không dùng dữ liệu thật.

**Yêu cầu form:** amount phải là số dương; type chỉ là `income`/`expense`; category phải chọn; ngày hợp lệ. Khi nhập `-50000`, `abc`, hoặc bỏ category, lỗi phải nêu rõ cạnh field, không chỉ disable nút mà không giải thích.

**Thử thách responsive:** Desktop có summary card + list; mobile ưu tiên số dư, nút “Thêm giao dịch”, rồi list card. Không ép biểu đồ vào màn nhỏ nếu chart không phục vụ Must story.

### Bài 5 - Triage lỗi AI (15 phút, làm nhóm)

Cho đoạn/pseudo-code sau để nhóm đọc và không chạy mù:

```tsx
{appointments.map((item, index) => (
  <div key={index} onClick={() => router.push(`/appointments/${item.id}`)}>
    <span className="text-green-500">{item.status}</span>
    <button onClick={saveAppointment}>Lưu</button>
  </div>
))}
```

Nêu tối thiểu 5 vấn đề và sửa nhỏ nhất:

- `key={index}` sai khi list thay đổi/filter: dùng `item.id` ổn định.
- `div onClick` không phải control có semantics/keyboard: dùng link hoặc button phù hợp.
- status chỉ đổi màu và hiển thị raw enum: map sang label dễ hiểu, thêm style phụ.
- Bấm Lưu có thể trigger click của parent: tách cấu trúc hoặc xử lý event có chủ đích.
- `saveAppointment` chưa biết record nào được lưu và chưa phản hồi trạng thái/validation.
- Chưa rõ `button` nằm trong form có phải submit không: đặt `type` phù hợp.

## 9. Prompt library dùng ngay

### 9.1 Từ page map đến file plan

> Dự án dùng Next.js App Router, TypeScript, Tailwind. Đây là 5 routes và 3 Must stories: [DÁN]. Thực thể chính là [ENTITY]. Chưa có database, API, Auth hay library mới. Hãy đề xuất structure tối giản gồm routes, components, `types/` và `data/`. Với mỗi file, nêu trách nhiệm. Chỉ tách component khi dùng lại hoặc làm page dễ đọc. Không viết code, không thêm folder/service không phục vụ Must story.

### 9.2 Tạo component state tái dùng

> Hãy thiết kế component `EmptyState` cho frontend MVP hiện có. Nó cần title, description, CTA tùy chọn và icon/minh họa không bắt buộc; dùng semantic HTML, tiếng Việt, mobile-friendly. Đưa API props TypeScript và một ví dụ cho “chưa có lịch hẹn” và “filter không có kết quả”. Không cài thư viện, không tạo design system lớn, không dùng nội dung khẳng định dữ liệu đã lưu thật.

### 9.3 Sửa lỗi mobile có bằng chứng

> Hiện trạng: tại 375 px, bảng lịch hẹn có 6 cột bị tràn ngang; lễ tân không thấy CTA xem chi tiết. Mục tiêu: mobile hiển thị mỗi lịch dưới dạng card gồm giờ, khách, dịch vụ, status text và link xem chi tiết; desktop giữ table hiện có. Phạm vi: chỉ `AppointmentList` và style liên quan. Không đổi route, data model, API hay cài thư viện. Hãy nêu file đổi, phương án breakpoint, thay đổi nhỏ nhất và checklist test ở 375/768/1440 px.

### 9.4 Yêu cầu AI debug có kiểm soát

> Mục tiêu: mở `/appointments/[id]` và thấy record mock. Hiện tại: URL id không tồn tại làm trang trắng. Bước tái hiện: mở `/appointments/does-not-exist`. Console: [DÁN LỖI]. File: [DÁN FILE]. Hãy nêu tối đa 3 nguyên nhân có khả năng nhất, cách kiểm tra từng nguyên nhân, thay đổi nhỏ nhất để có not-found state và cách test id hợp lệ/id sai/note trống. Không thay router, không thêm API/Auth.

## 10. QA, peer review và rubric

### Checklist trước khi demo

- [ ] Route khớp page map và mỗi page phục vụ một Must story.
- [ ] Type/mock data ở ngoài UI; không `any`, không index làm key.
- [ ] Có >= 8 records mock và edge cases đã nêu được.
- [ ] List -> detail -> create/edit chạy được bằng link/nút thật.
- [ ] Có loading, empty all, empty after filter, error hoặc not-found ở nơi phù hợp.
- [ ] Form có validation hiểu được; success nói rõ là mock nếu chưa có backend.
- [ ] 375 px không tràn ngang; 768/1440 px không có khoảng trắng/hierarchy bất thường.
- [ ] Có thể thao tác chính bằng Tab/Enter/Space; focus nhìn thấy.
- [ ] Console không có lỗi nghiêm trọng; terminal/type check không lỗi.
- [ ] Không chứa secret hoặc dữ liệu cá nhân/tài chính/khách hàng thật.

### Peer review 6 phút/người

Người review chỉ làm ba việc: (1) thực hiện một luồng chính, (2) chuyển sang 375 px, (3) thử một state lỗi biên. Phản hồi dùng mẫu:

```text
Tôi đã thử: [bước].
Kỳ vọng: [điều người dùng cần].
Tôi thấy: [bằng chứng cụ thể].
Mức: P0/P1/P2.
Đề xuất nhỏ nhất: [một thay đổi].
```

Không nhận xét “trông chưa đẹp” nếu không chỉ ra hierarchy, khả năng đọc, hành động hoặc trạng thái nào gặp vấn đề.

### Rubric 10 điểm

| Hạng mục | Điểm | Dấu hiệu đạt |
|---|---:|---|
| Scope và route | 1.5 | 5-7 màn hình bám Must story, không phình phạm vi |
| Data/type | 1.5 | type rõ, mock data đủ state/edge case |
| Component và cấu trúc | 2.0 | trách nhiệm rõ, dùng lại hợp lý, không over-engineer |
| Luồng và form mock | 2.0 | bấm được end-to-end, validation/phản hồi trung thực |
| Responsive/accessibility | 2.0 | mobile usable, keyboard/focus/label/status text đạt |
| QA và cách làm với AI | 1.0 | có checklist, sửa P0/P1 dựa trên bằng chứng |

## 11. Bài tập về nhà và cầu nối Buổi 4

1. Hoàn thiện tất cả routes Must Have với mock data và quay video demo 1-2 phút: dashboard/list -> detail -> form -> state lỗi biên.
2. Tạo `README` ngắn: bài toán, User Stories, routes, mock data, phần đang mô phỏng và điều sẽ nối database ở Buổi 4.
3. Lập backlog tối đa 5 việc P0/P1/P2. Không ghi tính năng “hay ho” nếu vẫn còn state/validation/mobile lỗi.
4. Chuẩn bị sơ đồ dữ liệu: entity chính, các field thực sự cần lưu, dữ liệu nào bắt buộc/tùy chọn, quan hệ có thể cần. Không tự tạo Supabase trước Buổi 4.

### Tiêu chí sẵn sàng cho Buổi 4

| Đạt | Chưa đạt - cần xử lý trước khi nối database |
|---|---|
| UI đã biết field nào đọc/ghi và states cần hiển thị | Vẫn chưa chốt entity hoặc list/detail/form đang mâu thuẫn |
| Mock data/type có contract rõ | Dùng object tùy ý, `any`, hard-code data trong nhiều component |
| Luồng UI được test bằng dữ liệu thường và dữ liệu xấu | Chỉ có ảnh demo một record “đẹp” |
| Biết phần nào hiện chỉ trong bộ nhớ | Giao diện hứa hẹn dữ liệu đã lưu thật |

**Thông điệp chốt:** Frontend tốt không phải là nhiều hiệu ứng hay nhiều component. Nó giúp người dùng hoàn thành một việc rõ ràng, cho họ biết chuyện gì đang xảy ra khi dữ liệu không lý tưởng, và tạo một contract đủ sạch để Buổi 4 thay mock data bằng dữ liệu thật mà không phải làm lại sản phẩm từ đầu.
