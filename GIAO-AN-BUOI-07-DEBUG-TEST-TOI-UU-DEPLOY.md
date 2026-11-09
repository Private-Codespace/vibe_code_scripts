# GIÁO ÁN BUỔI 7 - DEBUG, KIỂM THỬ, TỐI ƯU VÀ DEPLOY PRODUCTION

**Thời lượng:** 180 phút, gồm 15 phút nghỉ.  
**Đối tượng:** người mới đã hoàn thành buổi 1-6; làm cá nhân hoặc theo cặp.  
**Stack tham chiếu:** Next.js + TypeScript + Tailwind CSS + Supabase + Vercel; giữ phiên bản và cấu trúc thực tế của từng project.  
**Sản phẩm cuối buổi:** release candidate (RC, bản ứng viên phát hành) có URL production, QA có bằng chứng, luồng chính dùng được trên mobile, không còn lỗi P0/P1 trong phạm vi phát hành và có kế hoạch khôi phục.

> Căn cứ: `lo-trinh-vibe-code-8-buoi.pdf`, trang 9; nối tiếp giáo án backend buổi 5 và AI feature buổi 6. Đây là giáo án triển khai chi tiết do người soạn đề xuất, không phải phần trích nguyên văn từ PDF. Trong khóa học, production là môi trường triển khai công khai dùng dữ liệu demo; hoàn thành buổi học chưa đồng nghĩa sẵn sàng phục vụ dữ liệu khách hàng thật.

## 1. Cách sử dụng bộ tài liệu

| Tài liệu | Ai dùng | Dùng để làm gì |
|---|---|---|
| Giáo án này | Giảng viên và học viên | Dạy theo timeline; hiểu lý thuyết, làm lab Hana Desk, build và deploy |
| [Bài tập và phiếu thực hành](BAI-TAP-VA-PHIEU-THUC-HANH-BUOI-07.md) | Học viên | Chọn bài toán thực tế; điền bug report, test matrix, release record |
| [Hướng dẫn giảng viên](HUONG-DAN-GIANG-VIEN-VA-GOI-Y-XU-LY.md) | Giảng viên | Chuẩn bị lỗi trên bản sao demo; chẩn đoán, đáp án định hướng và chấm bài |

**Khối lượng trong lớp:** cả lớp làm lab Hana Desk; mỗi cặp xử lý **một lỗi được giao**, chạy bộ smoke test chung, sửa một vấn đề chất lượng nhỏ và deploy. Không yêu cầu hoàn thành tất cả bài toán mở rộng trong 180 phút. Bài tập ngành khác và test mở rộng làm sau giờ học.

## 2. Vai trò của buổi 7 trong toàn khóa

| Buổi | Artefact mang sang | Buổi 7 cần kiểm chứng |
|---|---|---|
| 1 | Brief và landing page | Sản phẩm còn giải quyết đúng vấn đề, CTA không hứa chức năng chưa có |
| 2 | Ba luồng Must-have, user stories, acceptance criteria | Dùng làm chuẩn expected; không dùng code hiện tại làm định nghĩa đúng |
| 3 | UI, route, responsive và states | Người dùng thao tác được, lỗi/trống/chờ hiển thị rõ |
| 4 | Schema, migration, seed, CRUD | Refresh vẫn còn dữ liệu; môi trường deploy có schema đúng |
| 5 | Role matrix, Auth, RLS/grants, command và audit | User trái quyền không vượt được backend/database bằng request trực tiếp |
| 6 | AI endpoint, contract, mock/provider, test và backlog | Thiếu dữ liệu, timeout, sai output và người duyệt được xử lý đúng |
| 7 | Bug log, QA, build, release record | Có bằng chứng để quyết định phát hành hoặc hoãn |
| 8 | RC, giới hạn và backlog | Chuẩn bị pitch, demo, README và kế hoạch cải tiến |

Case nối tiếp là **Hana Desk**, công cụ quản lý lịch spa: staff tạo/xem lịch theo quyền; manager/admin xác nhận lịch `pending` bằng command `confirmAppointment`; AI hỗ trợ soạn nháp cho lịch đã `confirmed`, người dùng xem và sửa trước khi dùng. Tên bảng, route và API dưới đây là tham chiếu từ giáo án, cần kiểm tra repo thật trước khi sửa. Không giả định project học viên đã có đủ chức năng.

## 3. Mục tiêu đo được và điều kiện đạt

Sau buổi học, học viên có thể:

1. Viết bug report có expected, actual, môi trường và bước tái hiện đủ để người khác làm lại.
2. Nêu tối đa ba giả thuyết, dùng Console/Network/server log/database để kiểm tra thay vì yêu cầu AI sửa ngẫu nhiên.
3. Sửa một nguyên nhân đã có bằng chứng; kiểm thử lại lỗi gốc và luồng liên quan.
4. Viết test case có tiền điều kiện, input, expected, actual và evidence; phân biệt PASS, FAIL, BLOCKED, NOT RUN.
5. Kiểm chứng Auth/RLS và AI failure mode; không tắt bảo mật để chữa lỗi.
6. Cải thiện một vấn đề mobile/accessibility/ảnh/metadata có đo hoặc quan sát trước-sau.
7. Build, chạy bản production ở local, deploy đúng cấu hình, smoke test URL mới và lập phương án khôi phục.

### Definition of Done - tiêu chí kết thúc

| Hạng mục | Đạt khi |
|---|---|
| Debug | Có ít nhất 1 bug được tái hiện, có giả thuyết bị loại/xác nhận và evidence trước-sau |
| QA | Hoàn thành 12 smoke test S01-S12; case bổ sung theo lỗi sửa; không đánh PASS khi chưa chạy |
| Quyền | Có ít nhất staff A, staff B và manager; role admin test thêm nếu scope sử dụng; direct request trái quyền bị chặn |
| AI | Có happy path và failure path; ghi rõ mock hoặc provider thật; người dùng phải duyệt |
| Mobile | Luồng chính ở 375 px thao tác được; kiểm thêm 768 px và 1440 px cho màn hình bị thay đổi |
| Chất lượng | Sửa một vấn đề cụ thể; không còn exception chưa xử lý ảnh hưởng luồng chính |
| Build | Các kiểm tra phù hợp project và production build thành công; chạy thử bản build |
| Deploy | URL production mở được từ cửa sổ chưa đăng nhập hosting; dữ liệu riêng vẫn cần Auth |
| Bàn giao | Release record, QA evidence, known limitations, rollback và video 2-3 phút |

Nếu chưa đạt, ghi **HOLD - chưa phát hành/không xác nhận đạt RC** và kế hoạch sửa. Không đổi FAIL thành PASS để kịp giờ. Nếu không thể deploy vì tài khoản/mạng, nộp build local và hồ sơ QA, đánh dấu BLOCKED ở deploy và hoàn thành sau giờ; chưa đạt đầy đủ đầu ra buổi 7.

## 4. Chuẩn bị trước buổi học

### 4.1 Giảng viên

- Chuẩn bị project demo chạy đến buổi 6, dependency lockfile và phiên bản Node phù hợp; build thử từ trước.
- Có một phiên bản tốt để quay về và các bản sao lỗi riêng. Không tạo lỗi trên ứng dụng đang phục vụ người dùng.
- Chuẩn bị account demo staff A, staff B, manager; admin nếu cần. Cấp thông tin đăng nhập qua kênh riêng, không đưa mật khẩu vào tài liệu/repo/video.
- Chuẩn bị Supabase demo, seed, role matrix, migration; biết bảng nào chứa lịch và audit event. SQL Editor thường chạy quyền cao nên không dùng kết quả ở đó để kết luận RLS của user đúng.
- Kết nối GitHub/Vercel trước giờ; kiểm quyền project, root directory, production branch và env. Nếu phải tạo mới tài khoản trong 20 phút cuối sẽ dễ không kịp.
- Chuẩn bị adapter AI mock ở server với các tình huống success/timeout/invalid-output, chỉ bật ở local hoặc preview có kiểm soát. Không để endpoint công khai cho người dùng tùy ý bật lỗi.
- Chạy thử 12 smoke test, lưu ví dụ bằng chứng đã che dữ liệu nhạy cảm; không đánh dấu sẵn PASS trên phiếu học viên.

### 4.2 Học viên

- Mang brief, ba Must-have stories, schema, role matrix, backlog buổi 6; chạy app được và đăng nhập được.
- Mở editor, terminal, DevTools Console/Network; biết xem log deployment và log server.
- Lưu phiên bản đang chạy trước khi sửa; không reset/xóa thay đổi của bạn cùng nhóm.
- Kiểm tra `package.json`, lockfile, `.gitignore`, `.env.example` chỉ có tên biến/placeholder. Không gửi `.env.local`, cookie, bearer token hoặc ảnh log có secret cho AI.
- Mỗi cặp chia vai: **người tái hiện/kiểm thử** và **người chẩn đoán/sửa**; đổi vai sau lab debug.

### 4.3 Quy ước dữ liệu demo

| Mã | Dữ liệu/điều kiện | Mục đích |
|---|---|---|
| A1 | Lịch `pending`, staff A tạo, giờ hiện tại +24 giờ | Manager xác nhận thành công |
| A2 | Lịch `pending`, staff B tạo, +26 giờ | Staff A không đọc được nếu matrix quy định owner-only |
| A3 | Lịch `cancelled`, staff A tạo | Không thể xác nhận |
| A4 | Lịch `pending`, giờ hiện tại -24 giờ | Không thể xác nhận lịch quá khứ |
| A5 | Lịch `confirmed`, dữ liệu dịch vụ/giờ đủ | AI tạo nháp |
| A6 | Lịch mà dữ liệu chọn cho AI thiếu trường bắt buộc | Validation không gọi provider |

Lấy mốc thời gian lúc seed theo UTC, hiển thị với `Asia/Ho_Chi_Minh`; không dùng ngày cố định dễ thành quá khứ. Ghi ID thật vào phiếu, không dùng chuỗi “A1” thay UUID để gọi API. Nếu schema không cho phép tạo A6, mô phỏng dữ liệu thiếu ở adapter test server, không bỏ constraint để tạo dữ liệu sai. Reset seed có kiểm soát sau mỗi vòng test; không chạy seed xóa dữ liệu trên hệ thống thật.

## 5. Kịch bản 180 phút bám lộ trình gốc

| Thời gian | Giảng viên làm gì | Học viên làm gì | Đầu ra/checkpoint |
|---|---|---|---|
| 00:00-00:05 | Demo lỗi “lưu thành công nhưng refresh mất lịch”; nhắc story chuẩn | Nói tác động với lễ tân/khách | Hiểu lỗi nghiệp vụ |
| 00:05-00:15 | Làm mẫu bug report và cách đọc Network | Ghi expected/actual, môi trường, bước tái hiện | Bug có thể lặp lại |
| 00:15-00:30 | Làm mẫu tối đa 3 giả thuyết, loại 1, sửa nhỏ và retest | Theo dõi bằng chứng, không đoán theo thông báo UI | Chu trình debug đầy đủ |
| 00:30-00:40 | Giải thích bảng lỗi env/type/hydration/RLS/CORS/loading/build | Chọn công cụ quan sát cho từng lỗi | Sơ đồ lớp lỗi |
| 00:40-01:00 | Phát 1 lỗi/cặp; hỗ trợ theo câu hỏi chẩn đoán | Tái hiện, lập giả thuyết, sửa, test lại | Bug report + diff + evidence |
| 01:00-01:10 | Chuyển acceptance criteria thành test case; ví dụ quyền và timeout | Viết expected trước khi chạy | Test matrix |
| 01:10-01:25 | Hướng dẫn chạy smoke test theo vai | Chạy test; chia phần manual/automation phù hợp | Actual + case chưa chạy |
| 01:25-01:40 | Nghỉ, hỗ trợ môi trường | Nghỉ; không ép hoàn thành toàn bộ lỗi | Project tiếp tục chạy |
| 01:40-01:50 | Demo một phép đo trước-sau, mobile/a11y/metadata | Ghi một vấn đề chất lượng có tác động | Baseline |
| 01:50-02:10 | Review scope thay đổi nhỏ | Sửa 1 vấn đề, đo/quan sát lại; bổ sung QA | Bằng chứng cải thiện |
| 02:10-02:20 | Ghép cặp khác, ưu tiên P0/P1 | Test chéo không được hướng dẫn từng nút; báo lỗi rõ | Phiếu review |
| 02:20-02:30 | Hỗ trợ sửa blocker, freeze tính năng mới | Sửa P0/P1; chạy regression liên quan | Danh sách lỗi đã chốt |
| 02:30-02:40 | Kiểm tra build log và release gate | Type/lint/test phù hợp, build, start, test nhanh | GO hoặc HOLD |
| 02:40-02:50 | Hướng dẫn env, deploy đúng commit, Auth URL | Deploy bản đã kiểm tra; kiểm cấu hình | Deployment/commit ID |
| 02:50-02:57 | Dẫn smoke test trên URL production | Chạy 6 bước production; đọc runtime log | Release record |
| 02:57-03:00 | Exit ticket, chốt backlog buổi 8 | Nói lỗi gốc, evidence và giới hạn | Handoff |

**Khi thiếu thời gian:** hoãn bài tập mở rộng, tối ưu phụ và automation mới; giữ debug, quyền, test luồng chính, build và smoke production. Nếu P0/P1 chưa sửa xong thì HOLD. Video có thể quay sau giờ; các cấu hình hosting cần chuẩn bị từ trước.

## 6. Lý thuyết cốt lõi và cách dùng ngay

### 6.1 Lỗi phải được định nghĩa bằng hành vi người dùng

“Có lỗi đỏ” chưa đủ để sửa. Một bug cần ba lớp:

1. **Người dùng cần gì:** lễ tân lưu lịch để đồng nghiệp xem sau đó.
2. **Hợp đồng đúng là gì:** lưu hợp lệ thì có row; `created_by` lấy từ session; refresh vẫn còn; lưu lỗi thì không báo thành công.
3. **Thực tế là gì:** toast thành công hiện lên nhưng request bị từ chối và không có row.

Expected lấy từ story, contract và role matrix. Nếu chưa rõ “staff có được xem lịch người khác không”, cần ghi yêu cầu chưa chốt, không tự mở policy cho mọi người.

### 6.2 Vòng debug 7 bước

```text
Định nghĩa expected/actual
  -> Tái hiện ở môi trường xác định
  -> Thu bằng chứng đã che dữ liệu nhạy cảm
  -> Lập tối đa 3 giả thuyết và phép kiểm tra
  -> Xác nhận nguyên nhân, sửa nhỏ
  -> Retest lỗi gốc + regression liên quan
  -> Ghi kết quả và lưu phiên bản
```

| Bước | Câu hỏi cần trả lời | Sản phẩm cần ghi |
|---|---|---|
| Định nghĩa | Lỗi làm người dùng không hoàn thành việc gì? | Story + expected/actual |
| Tái hiện | Account, record, route, lần bấm, môi trường nào? | Steps, tần suất, commit |
| Thu bằng chứng | Request có gửi không, status/body gì, database đổi gì? | Ảnh/log an toàn, request ID |
| Giả thuyết | Dấu hiệu nào sẽ chứng minh hoặc bác bỏ từng nguyên nhân? | H1-H3 và phép kiểm tra |
| Sửa | File nhỏ nhất cần thay là gì? | Diff, giải thích vì sao |
| Retest | Cùng điều kiện cũ còn lỗi không; ai/luồng nào có thể bị ảnh hưởng? | Case cũ + regression |
| Lưu | Có thể quay về bản nào; còn giới hạn gì? | Commit và bug status |

Không đổi đồng thời env, policy, SDK và toàn bộ component. Nếu nhiều thay đổi đều có thể giải thích kết quả thì khó xác định nguyên nhân nào thực sự đúng. Gặp blocker ngoài khả năng: ghi BLOCKED, evidence và người/điều kiện cần để tiếp tục.

### 6.3 Đọc lỗi theo lớp

```text
Browser/UI -> request API -> session/quyền -> command/database
                       -> AI provider -> validate output -> UI

Local dev -> production build local -> preview -> production
```

| Dấu hiệu | Quan sát đầu tiên | Giả thuyết cần kiểm | Sửa theo bằng chứng; tránh |
|---|---|---|---|
| Chỉ production thiếu cấu hình | Env name, scope và deployment mới/cũ | Biến chỉ có ở local/Preview | Thêm đúng scope, redeploy; không log giá trị secret |
| TypeScript báo kiểu không khớp | Dòng lỗi đầu, type và schema thật | Nullable, tên cột đổi, async API khác phiên bản | Sửa type/guard/hợp đồng; không dùng `any` hàng loạt |
| Hydration mismatch | Console stack, HTML đầu server/client | Date/random, locale, browser API khi render, HTML lồng sai | Làm initial render nhất quán; không chỉ che warning |
| Lịch không hiện dù DB có | Session, query filter, project URL, policy, dữ liệu owner | Sai account/project/filter hoặc RLS | Kiểm từng giả thuyết; không tắt RLS |
| CORS trên browser | Origin, URL đích, OPTIONS và response | Gọi nhầm host/cross-origin hoặc preflight lỗi | Sửa ở server đích khi cần; không `mode: no-cors` |
| Loading mãi | Request pending hay đã kết thúc; state transition | Không clear state khi reject, không timeout, request cũ ghi đè | Cleanup/finally, timeout và điều phối request; không chỉ ẩn spinner |
| Vercel build fail, local dev được | Lỗi đầu trong build log, Node, lockfile, casing/import | Dev chưa kiểm hết; thiếu dependency/env; sai chữ hoa-thường | Tái hiện bằng production build; không bỏ type checking |
| Toast thành công, không lưu | Response error, mutation, read lại sau refresh | Bỏ qua lỗi SDK, success/optimistic sai | Chỉ báo success sau kết quả hợp lệ, reconcile UI |

**Những phân biệt quan trọng:** RLS có thể lọc `SELECT` ra mảng rỗng; mảng rỗng tự nó không chứng minh lỗi policy. Mutation có thể bị từ chối hoặc không ảnh hưởng row nào; HTTP 200 cũng chưa chứng minh nghiệp vụ thành công. Với SDK, đọc cả `data` và `error` theo contract thực tế. RLS đánh giá theo danh tính request; kết quả từ client quyền cao không chứng minh user thường được phép hay bị chặn. Tham khảo [Supabase RLS](https://supabase.com/docs/guides/database/postgres/row-level-security).

Client Component cũng có thể được prerender; thêm `"use client"` không tự giải quyết việc đọc `window`/thời gian ngẫu nhiên trong initial render. Giải thích và lựa chọn sửa theo [Next.js hydration errors](https://nextjs.org/docs/messages/react-hydration-error).

Browser gọi route cùng origin của app rồi server gọi AI provider là flow tham chiếu buổi 6. Không mở CORS provider chỉ để đưa AI key về browser. Nếu thật sự dùng Edge Function khác origin, xử lý preflight và CORS tại function, đồng thời giữ Auth/quyền độc lập; xem [Supabase CORS](https://supabase.com/docs/guides/functions/cors).

### 6.4 Mức ưu tiên lỗi

Quy ước dưới đây dùng riêng cho lớp; ưu tiên dựa trên tác động, tần suất và khả năng tránh lỗi.

| Mức | Ví dụ | Hành động |
|---|---|---|
| P0 | Lộ secret, đọc chéo dữ liệu riêng, mất/sai dữ liệu nghiêm trọng | Dừng phát hành; chặn đường lỗi; xử lý và xác minh lại |
| P1 | Luồng Must-have hỏng, login production lỗi, lưu không thành công nhưng báo thành công | Sửa trước phát hành trong scope |
| P2 | Luồng phụ khó dùng nhưng có cách thay thế rõ | Ghi backlog, người chịu trách nhiệm và giới hạn |
| P3 | Khoảng cách/icon/copy nhỏ, không cản tác vụ | Xử lý khi còn thời gian |

Không hạ mức lỗi vì “demo chỉ bấm đường đúng”. Nếu key từng bị lộ, xóa khỏi code chưa đủ: thu hồi/rotate key, cập nhật môi trường và xem phạm vi log/history có liên quan trước khi phát hành.

### 6.5 Test: chọn đúng loại thay vì viết thật nhiều

| Loại | Dùng khi | Ví dụ trong lớp |
|---|---|---|
| Kiểm tra tĩnh/build | Lỗi type/import/config trước runtime | Type check, lint nếu project có, build |
| Unit | Rule thuần, xác định, dễ sai | Hàm kiểm trạng thái/giờ; không cần AI hay DB |
| Integration | Nhiều thành phần phải phối hợp | Command + RLS + audit với account có/không có quyền |
| E2E | Người dùng đi qua luồng chính | Login -> tạo -> manager confirm -> AI nháp -> refresh |
| Exploratory/manual | UI, mobile, keyboard, tình huống chưa dự đoán | Người test chéo tìm lỗi và viết lại steps |

**Retest** chạy lại case từng lỗi. **Regression** kiểm các hành vi liên quan vẫn đúng. **Smoke test** là bộ kiểm tra nhanh các chức năng thiết yếu để biết bản mới có dùng được ở mức cơ bản.

Không cài thêm cả bộ test framework nếu project chưa có và lỗi chỉ cần quan sát UI đơn giản. Ưu tiên một regression tự động cho bug nghiệp vụ/quyền đáng kể nếu hạ tầng đã sẵn; kiểm thử quyền phải đi qua lớp thực tế, không chỉ mock role rồi kết luận RLS đúng. AI có thể sinh test nhưng người học phải kiểm expected độc lập và thực sự chạy.

## 7. Lab xuyên suốt: Hana Desk từ “bấm được” đến bản phát hành

### 7.1 Story và acceptance criteria trước khi sửa

**Story:** “Là lễ tân, tôi muốn lưu lịch và theo dõi trạng thái để không bỏ sót khách; quản lý xác nhận đúng quyền; nháp AI chỉ hỗ trợ viết thông báo.”

| ID | Điều kiện đúng |
|---|---|
| AC1 | Staff A tạo lịch hợp lệ; row được lưu với owner từ session; refresh vẫn thấy theo matrix |
| AC2 | Lưu lỗi: không toast success; dữ liệu nhập được giữ; thông báo có cách thử lại |
| AC3 | Staff A không đọc/sửa lịch staff B nếu matrix owner-only; không tự confirm hoặc đổi role |
| AC4 | Manager confirm lịch pending tương lai: status, actor/time và event nhất quán; không tạo event confirm lần hai |
| AC5 | Confirm lịch cancelled/quá khứ bị từ chối, không đổi dữ liệu và không tạo event thành công |
| AC6 | AI cho lịch confirmed trả nháp xem/sửa được; không tự gửi tin hay thay đổi trạng thái |
| AC7 | AI thiếu dữ liệu/timeout/sai output: thông báo rõ, dừng loading, không hiển thị thành công giả |
| AC8 | Luồng login -> list -> detail -> thao tác chính dùng được trên 375 px và URL production |

### 7.2 Giảng viên làm mẫu: lỗi toast thành công nhưng lịch không được lưu

**Thiết lập:** trên bản sao demo, cố ý cho adapter mutation trả lỗi và kiểm tra UI hiện có bỏ qua lỗi hay không. Đây là lỗi mô phỏng cho lab, không khẳng định app học viên đang có lỗi này.

**Bước tái hiện:** đăng nhập staff A -> mở tạo lịch -> nhập dữ liệu giả hợp lệ -> bật adapter lỗi có kiểm soát -> Lưu -> nhìn toast -> refresh -> đọc lại theo account A. Ghi request/response và trạng thái row. Phân biệt lỗi chủ động trả về với request bị mất kết nối có kết quả chưa biết.

| Giả thuyết | Kiểm tra | Evidence minh họa trong bản lỗi |
|---|---|---|
| H1: form không gửi request | Network, dấu hiệu submit | Có request: loại H1 |
| H2: UI bỏ qua lỗi mutation | Response/SDK error và code nhánh success | Error tồn tại nhưng toast success vẫn chạy: xác nhận |
| H3: lưu sang project khác | So khớp project URL/env không lộ key | Nếu H2 đã đủ giải thích thì chưa cần đổi env; ghi chưa kiểm thay vì tự kết luận |

**Thay đổi nhỏ:** kiểm kết quả mutation trước success; bắt lỗi ngoài dự kiến; clear trạng thái chờ; giữ form khi thất bại. Không sửa RLS nếu bằng chứng đang chỉ vào adapter/UI. Nếu dùng optimistic UI, phải rollback/reconcile khi lỗi.

Ví dụ TypeScript dưới đây chỉ minh họa trình tự, không phải component copy trực tiếp; `createAppointment` phải chuẩn hóa kết quả và thực thi quyền ở server/database:

```ts
type SaveResult =
  | { ok: true; id: string }
  | { ok: false; code: string; message: string };

async function saveAppointment() {
  if (isSubmitting) return; // UX; không thay thế chống trùng ở server
  setIsSubmitting(true);
  setError(null);
  try {
    const result: SaveResult = await createAppointment(validatedInput);
    if (!result.ok) {
      setError(result.message); // thông báo đã được server làm an toàn
      return;
    }
    showSuccess("Đã lưu lịch");
    openAppointment(result.id);
  } catch {
    setError("Chưa xác định kết quả lưu. Hãy kiểm tra danh sách trước khi thử lại.");
  } finally {
    setIsSubmitting(false);
  }
}
```

**Phân biệt quan trọng:** network timeout không chứng minh server chưa lưu. Không tự retry thao tác tạo/ghi nếu có nguy cơ trùng; xác minh kết quả bằng record/request ID hoặc cơ chế idempotency của command trước khi thử lại. State/nút disabled chỉ giảm bấm nhầm, không chống request đồng thời từ nhiều tab.

**Retest:** lần lỗi không báo success, form giữ nguyên; lần success có row và refresh còn; owner đúng; bấm nhanh không tạo bản sao trong tình huống kiểm; staff khác vẫn bị chặn. Ghi giới hạn nếu chống trùng phía server chưa có, không tuyên bố đã bảo đảm exactly-once.

### 7.3 Học viên làm một chu trình debug độc lập

Chọn một: AI timeout/loading; thiếu env production; query sai tên trường; hydration giờ hẹn; lỗi quyền trên bản sao demo. Giảng viên phát triệu chứng, học viên chưa xem đáp án.

1. Ghi story và expected trước khi gửi prompt.
2. Tái hiện tối thiểu hai lần nếu lỗi ổn định; với lỗi chập chờn ghi số lần/thử và điều kiện.
3. Chụp/ghi bằng chứng đã che token và dữ liệu nhạy cảm; đừng xuất HAR nguyên bản để nộp.
4. Viết H1-H3, mỗi giả thuyết có phép kiểm chứng; thực hiện từ rẻ/dễ quan sát trước.
5. Nhờ AI đề xuất plan và file nhỏ nhất cần đổi; xem diff rồi mới áp dụng.
6. Chạy retest và ít nhất hai regression theo phạm vi ảnh hưởng.
7. Đổi máy/cặp để người khác tái hiện lại; lưu bản sửa và evidence.

**Checkpoint giảng viên:** hỏi “Bằng chứng nào khiến em sửa file này?” Nếu câu trả lời là “AI bảo vậy”, quay lại bước giả thuyết.

### 7.4 Bộ smoke test chung S01-S12

Expected dưới đây dựa trên role matrix Hana Desk; điều chỉnh có giải thích nếu project khác. Toàn bộ S01-S12 chạy trên bản chuẩn bị phát hành; sau deploy chạy lại sáu nhóm kiểm tra ở mục 10.5. Case dùng mock phải ghi rõ.

| ID | Account/tiền điều kiện | Thao tác | Expected và bằng chứng |
|---|---|---|---|
| S01 | Anonymous, cửa sổ mới | Mở protected URL và gọi API tương ứng | Không nhận dữ liệu riêng; UI yêu cầu login; API deny theo contract |
| S02 | Staff A | Login, refresh protected page, logout, gọi lại API | Session đúng; logout không gọi được dữ liệu riêng; UI xóa dữ liệu nhạy cảm khi đổi user |
| S03 | Staff A, form tạo lịch | Để trống trường bắt buộc/giờ không hợp lệ | Validation rõ, server cũng từ chối request sai; không có row mới |
| S04 | Staff A, dữ liệu hợp lệ | Tạo lịch -> list/detail -> refresh | Một lịch được lưu đúng; owner từ session; dữ liệu còn sau refresh |
| S05 | Staff A, mutation trả lỗi xác định | Bấm Lưu | Không success giả; giữ form; hết loading; không có row/event sai |
| S06 | Staff A, biết ID A2 của B | Truy cập detail/request đọc-sửa; thử direct confirm | Không lộ row B, không sửa/confirm được; DB không đổi |
| S07 | Manager, A1 pending tương lai | Confirm rồi confirm lại | Lần đầu thành công nhất quán với event; lần sau từ chối/không tạo hiệu ứng trùng theo contract |
| S08 | Manager, A3 và A4 | Confirm cancelled/quá khứ | Từ chối cả hai; status/event không đổi |
| S09 | Account được đọc A5 | Tạo nháp AI, chỉnh văn bản | Nháp đúng dữ liệu nguồn, sửa được; không gửi/đổi status tự động; ghi mock/provider |
| S10 | Account hợp lệ | Lần lượt thiếu input, mock timeout và invalid JSON | Validation/lỗi rõ, hết chờ, không render kết quả sai; không gọi provider khi thiếu dữ liệu bắt buộc |
| S11 | Account hợp lệ | Filter không kết quả; mô phỏng lỗi đọc mạng rồi khôi phục | Empty khác error, có hướng thoát; retry phù hợp, không chờ vô tận |
| S12 | Staff/manager, 375 px | Đi luồng chính bằng touch; kiểm keyboard desktop và Console | CTA/validation không bị che, nhãn/focus dùng được; không exception ảnh hưởng luồng |

Với S06, dùng client/session của chính staff A và chỉ ID demo giảng viên cấp. Không gửi request bằng service-role hoặc user quản trị rồi kết luận về quyền staff. Cùng ID có thể được trả 403/404 hoặc không có row theo contract, nhưng dữ liệu riêng và side effect phải được chặn.

**Case mở rộng sau giờ:** admin action đúng matrix, sửa role/owner từ client, hai confirm đồng thời, logout A rồi login B trên cùng browser, provider 429/502, prompt injection vô hại từ buổi 6, thời gian UTC/UTC+7, direct URL 404, reset password và lỗi link hết hạn.

## 8. Prompt thực hành cho từng bước

### Prompt 1 - Chẩn đoán trước khi sửa

```text
Bạn hỗ trợ debug project [stack và phiên bản từ package.json/lockfile].
User story/AC: [...]. Môi trường và commit: [...].
Expected: [...]. Actual: [...]. Steps: [...]. Tần suất: [...].
Log/response đã che secret: [...]. File/query/role matrix liên quan: [...].

Chưa sửa code. Nêu tối đa 3 giả thuyết theo mức hợp lý; với mỗi giả thuyết,
chỉ ra bằng chứng đang có, dữ kiện còn thiếu, phép kiểm tra và điều kiện bác bỏ.
Phân biệt điều đã xác minh với suy đoán. Không tự coi lỗi là RLS/CORS khi chưa có log.
Đề xuất bước kiểm tra tiếp theo ít thay đổi nhất. Không yêu cầu in secret/token.
```

### Prompt 2 - Sửa nguyên nhân đã được kiểm chứng

```text
Giả thuyết đã xác nhận: [...]. Evidence: [...].
Chỉ sửa [file/phạm vi]. Giữ nguyên business rule, quyền và API contract [...].
Trước khi sửa, nêu plan ngắn và regression có thể xảy ra.
Sửa nhỏ nhất; không rewrite app, không tắt RLS, không đưa key ra client,
không thêm thư viện hoặc nâng phiên bản nếu chưa có lý do cụ thể.
Sau sửa, trình bày diff, cơ chế khắc phục và cách retest + regression.
Chỉ ghi test đã chạy khi thực sự chạy; case chưa chạy phải nói rõ.
```

### Prompt 3 - Từ user story thành test có thể chạy

```text
Story/acceptance criteria: [...]. Role matrix: [...]. Contract API: [...].
Data fixture giả: [...]. Thay đổi vừa làm: [...].
Tạo test matrix ưu tiên theo rủi ro, gồm happy, validation, deny quyền,
network/provider failure, empty, mobile và regression của diff.
Mỗi case: ID, AC nguồn, account/session, tiền điều kiện, steps/input,
expected cụ thể ở UI/API/DB, evidence cần thu, manual/automation.
Đừng dùng hành vi hiện tại của code để suy ra expected. Chưa điền actual/PASS.
Chỉ đề xuất automation có ích với hạ tầng hiện có; không gọi mock là test RLS thật.
```

### Prompt 4 - Cải thiện chất lượng có giới hạn

```text
Vấn đề quan sát được: [...]. Evidence baseline, viewport/device: [...].
Trang public hay private: [...]. File và phiên bản hiện tại: [...].
Đề xuất tối đa 3 thay đổi theo tác động/công sức, chọn 1 thay đổi nhỏ phù hợp.
Nêu cách đo/quan sát lại cùng điều kiện; kiểm ảnh, layout, keyboard, nhãn,
metadata theo nhu cầu thực tế. Không mở rộng scope hoặc cache dữ liệu riêng
bằng cache dùng chung. Sau sửa, báo trước-sau và những gì chưa được xác minh.
```

### Prompt 5 - Review release

```text
Commit/build/deployment: [...]. QA actual/evidence: [...].
Bug còn mở và ưu tiên: [...]. Tên env và scope, không có giá trị: [...].
Schema/migration state: [...]. AI mode và limitations: [...].
Dựa vào release gate của lớp, chỉ ra PASS/FAIL/BLOCKED/NOT RUN,
đề xuất GO hoặc HOLD với lý do. Không đánh PASS từ checklist dự kiến.
Nêu production smoke test, trigger rollback, cách quay về bản tốt và
rủi ro tương thích database. Không tự khẳng định đã deploy/test nếu chưa có evidence.
```

## 9. Tối ưu chất lượng: từ quan sát đến một thay đổi có ích

### 9.1 SEO và metadata theo loại trang

- Landing public: title/description đúng dịch vụ, favicon, nội dung có heading hợp lý; ảnh chia sẻ nếu đã có asset phù hợp.
- Dashboard/lịch riêng: title rõ nhưng không chứa tên khách hay nội dung riêng nhạy cảm; không đưa vào sitemap công khai; có thể dùng `noindex` theo nhu cầu. `noindex`/robots không thay thế Auth.
- Với App Router, dùng `metadata` hoặc `generateMetadata` ở Server Component theo project; không ép export metadata từ component có `"use client"`. Xem [Next.js metadata](https://nextjs.org/docs/app/getting-started/metadata-and-og-images).

**Bài làm 5 phút:** tab browser đang là “Create Next App”; sửa title theo sản phẩm và kiểm lại DOM/head ở public page. Không thêm dữ liệu khách vào metadata chi tiết lịch.

### 9.2 Ảnh và bố cục

Kiểm ảnh lớn bất thường trong Network, kích thước vùng hiển thị và layout có nhảy khi tải. Cân nhắc resize/nén asset, khai báo kích thước, `sizes` phù hợp và dùng image optimization hiện có. Với nguồn ảnh remote, đối chiếu cấu hình theo version. Không thêm gallery/animation mới để “tối ưu”. Các thực hành ảnh/error page/metadata tham khảo [Next.js production checklist](https://nextjs.org/docs/app/guides/production-checklist).

**Ví dụ bài đo:** tải landing bằng cùng browser, viewport, cache state và throttling; ghi dung lượng ảnh hero, thời gian tải quan sát được, bố cục trước-sau. Nếu dùng Lighthouse, ghi máy/cấu hình và điều kiện; điểm lab là tín hiệu chẩn đoán, không chứng minh trải nghiệm mọi người dùng. Không bịa số cải thiện hoặc chỉ chọn lần đo tốt nhất.

### 9.3 Accessibility và mobile

| Kiểm tra | Cách làm ngay | Đạt khi |
|---|---|---|
| Input có nhãn | Click label; Tab đến field | Nhãn liên kết input, không chỉ có placeholder |
| Nút có tên | Xem nút icon và accessibility tree khi cần | Người dùng biết thao tác sẽ làm gì |
| Keyboard/focus | Tab, Shift+Tab, Enter; đóng/mở modal | Thứ tự hợp lý, focus thấy được, không kẹt |
| Validation | Gửi form sai | Chỉ rõ field/lý do, không chỉ đổi màu |
| 375 px | Đi hết luồng chính | Nút chính nhìn/bấm được; table có phương án đọc hợp lý |
| Loading/error | Làm chậm hoặc lỗi request | Có nội dung trạng thái, thông báo lỗi và hành động tiếp theo |

### 9.4 Performance của dashboard

Quan sát số request khi gõ filter/đổi trang, lượng row trả về, ảnh/bundle và thời gian API. Chỉ tối ưu điểm đã thấy: giới hạn dữ liệu trả về, phân trang, chọn cột cần, debounce tìm kiếm nếu hợp lý. Kiểm stale response khi gõ nhanh. Index phải dựa query thực tế, không thêm mọi cột. Không dùng cache chung cho dữ liệu user riêng; không tăng tốc bằng bỏ kiểm quyền.

**Phiếu trước-sau:** vấn đề -> tác động người dùng -> baseline và điều kiện -> thay đổi -> cùng phép đo/quan sát -> regression -> giới hạn. Một thay đổi nhỏ có evidence đủ cho buổi học.

## 10. Build, deploy và khôi phục

### 10.1 Local dev khác production build

`dev` phục vụ phát triển; build kiểm quá trình đóng gói production. Build xanh chưa chứng minh Auth/RLS/runtime đúng. Đọc scripts thật trước khi chạy. Với project dùng npm có lockfile, quy trình minh họa:

```bash
# Thực hiện ở thư mục app, trên bản sao/checkout phù hợp.
node --version
npm run

# Nếu cần cài lại dependency theo package-lock.json:
npm ci

# Chạy typecheck/lint/test nếu scripts tương ứng đã có.
# Không dùng --if-present để che việc chưa có kiểm tra bắt buộc.
npm run build
npm run start
```

Chạy từng bước, dừng đọc lỗi khi thất bại. Nếu `build`/`start` không tồn tại, xem cấu hình framework/project trước; không tự thêm scripts sai. Với pnpm/yarn dùng lockfile và công cụ tương ứng. Kiểm type/lint riêng theo phiên bản vì không mặc định build luôn chạy mọi kiểm tra. Không xóa lockfile hoặc nâng framework chỉ để vượt lỗi.

Trên local production server: login, đọc dữ liệu, tạo một record demo, gọi AI, refresh và mở direct URL. Những gì cần mạng/tài khoản mà chưa test phải ghi rõ.

### 10.2 Env theo môi trường

| Biến tham chiếu | Nơi dùng | Có thể ở browser? | Kiểm tra |
|---|---|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Client/server theo kiến trúc | Có | Đúng project demo cho environment |
| `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` hoặc tên anon key đang dùng | Client theo cấu hình thật | Có, theo loại key | Không phải secret/service-role; RLS/grants vẫn phải đúng |
| `AI_API_KEY` | Server gọi provider | Không | Chỉ có ở server env, đúng scope, không log giá trị |
| Tên model/timeout/base URL nếu app có dùng | Theo code hiện có | Tùy loại, xem code | Giá trị hợp lệ theo provider/môi trường |
| Supabase secret/service-role nếu kiến trúc thật cần | Server đặc quyền | Không | Không thêm chỉ để bypass quyền khi debug |

Tên biến phải khớp code, không đổi sang tên minh họa rồi làm hỏng app. `.env.example` chỉ có placeholder. Giá trị public được bundle theo cơ chế Next.js; thay cấu hình cần build/deploy mới. Xem [Next.js env](https://nextjs.org/docs/app/guides/environment-variables). Trên Vercel, chọn đúng Development/Preview/Production và tạo deployment mới sau thay đổi; deployment cũ không tự nhận env mới. Xem [Vercel environment variables](https://vercel.com/docs/environment-variables).

Nếu có điều kiện, tách Supabase demo cho preview và production; tối thiểu dùng dữ liệu giả, biết deployment nào đang trỏ DB nào. Không để preview vô tình ghi vào dữ liệu thật.

### 10.3 Release gate trước deploy

Chỉ GO khi:

- Có commit xác định, diff đã xem, không lẫn thay đổi ngoài scope.
- S01-S12 và regression bắt buộc có kết quả/evidence; không còn P0/P1 trong scope. Nếu một tính năng bị loại khỏi release phải vô hiệu hóa đường truy cập liên quan, cập nhật AC/scope và test lại; không chỉ bỏ khỏi video.
- Type/lint/test phù hợp đã chạy, build và production start đạt; không bỏ kiểm tra để cho xanh.
- RLS/grants, Auth, server secrets và AI mode đúng; chi phí/quota theo giới hạn của project.
- Schema/migration/seed đã được kiểm trên DB đích demo; có phương án tương thích với bản trước.
- Có env đúng scope, Auth redirect URL, deployment trước tốt hoặc kế hoạch tắt tính năng/dừng truy cập nếu chưa có bản cũ.

Người học ghi quyết định; AI hỗ trợ review bằng chứng. Đây là checkpoint học tập, không có thao tác phát hành thật nào được thực hiện chỉ bởi việc đọc tài liệu này.

### 10.4 Các bước deploy Vercel

1. Chọn repo/project và root directory đúng app; xác nhận production branch, framework, build command, Node và lockfile.
2. Cấu hình env cần thiết cho Preview/Production; không đưa secret vào trường public. Kiểm DB đích và migration đã áp dụng, không giả định deploy frontend sẽ tự chạy migration Supabase.
3. Nếu dùng preview, tạo preview từ commit đã kiểm; smoke test chính. Không giả định mỗi lượt push đều là production: đối chiếu branch và môi trường trên dashboard.
4. Cấu hình Supabase Auth Site URL/redirect allowlist theo domain và callback/reset route thật; giữ URL production chính xác, preview chỉ cho domain cần thiết. Site URL mặc định rất quan trọng với link xác nhận/reset; xem [Supabase redirect URLs](https://supabase.com/docs/guides/auth/redirect-urls).
5. Deploy/promote production theo workflow của project. Chờ build hoàn tất, đọc lỗi đầu nếu fail; không chỉ nhìn trạng thái Git push.
6. Ghi URL production, deployment ID, commit, thời gian, env names/scope và migration state. Mở URL từ cửa sổ chưa đăng nhập Vercel để kiểm khả năng truy cập public; app riêng vẫn giữ Auth.
7. Chạy mục 10.5. Nếu lỗi P0/P1 mới xuất hiện: ngừng công bố release, ghi incident và thực hiện phương án khôi phục phù hợp.

### 10.5 Sáu nhóm smoke test sau deploy

| Bước | Kiểm trên URL production | Evidence |
|---|---|---|
| 1 | Public page/metadata và protected URL khi anonymous | URL, ảnh; không có dữ liệu riêng |
| 2 | Login staff A, refresh và logout/relogin | Session đúng, không redirect về localhost |
| 3 | Tạo lịch giả, mở detail và refresh | Record ID, owner đúng, dữ liệu còn |
| 4 | Staff B không thấy/sửa lịch A; staff không confirm; manager confirm hợp lệ và lần hai | Kết quả deny/allow và audit, không chỉ ảnh nút |
| 5 | AI happy path và failure có kiểm soát | Ghi mode; draft chỉnh được, lỗi kết thúc loading; không bật endpoint fault công khai |
| 6 | Mobile 375 px, Console/Network và server runtime logs của luồng vừa chạy | Luồng dùng được, không exception ảnh hưởng, evidence đã che secret |

Với lỗi AI trên production, dùng fixture an toàn/thiếu input hoặc cơ chế fault nội bộ chỉ dành cho tester đã xác thực nếu hệ thống đã có. Timeout/invalid provider output có thể kiểm ở preview cùng commit và ghi phạm vi, không mở cờ lỗi public để đủ checklist. Case nào chưa xác minh trên production phải ghi môi trường evidence, không gộp chung thành PASS production.

### 10.6 Khi nào và cách nào khôi phục?

**Trigger:** login/luồng Must-have hỏng cho người dùng, đọc chéo dữ liệu, lỗi ghi sai/trùng nghiêm trọng, secret bị lộ hoặc AI gọi ngoài kiểm soát. Ghi thời điểm, deployment, tác động, điều kiện tái hiện và người phụ trách.

1. Chặn đường gây hại bằng cách tắt tính năng/quyền truy cập phù hợp hoặc quay về deployment tốt đã xác định. Nếu chưa có deployment tốt, dùng phương án maintenance/tạm dừng đã chuẩn bị.
2. Dùng tính năng rollback/promote hiện có của hosting theo quyền/gói; kiểm deployment được chọn và cấu hình của chính nó. Không giả định env mới được giữ khi quay về build cũ. Tham khảo [Vercel Instant Rollback](https://vercel.com/docs/instant-rollback).
3. Rollback app **không hoàn tác dữ liệu hay migration Supabase**. Kiểm schema còn tương thích; không tự drop cột/restore DB làm mất dữ liệu. Khi đã có dữ liệu mới, cân nhắc sửa tiến lên tương thích và quy trình phục hồi DB riêng.
4. Chạy lại smoke test liên quan; xác nhận đường lỗi đã đóng và luồng chính dùng được. Key lộ cần rotate/thu hồi kể cả đã rollback code.
5. Ghi incident, nguyên nhân, test cần thêm và điều kiện cho phát hành lại. Với buổi học có thể diễn tập trên project demo, không phá production để chứng minh biết rollback.

## 11. Test chéo và review

Mỗi cặp đưa cho cặp khác: URL, mục tiêu sản phẩm, account demo được cấp riêng, dữ liệu giả và ba tác vụ cần hoàn thành. Không dẫn từng click; để người test tự tìm đường. Sau 7 phút thao tác, dành 3 phút viết một bug report hoặc xác nhận các case đã chạy.

Người nhận feedback phân biệt:

- Bug: hành vi sai AC/contract đã chốt.
- Vấn đề UX: vẫn hoàn thành được nhưng khó hiểu/chậm/sai thao tác.
- Feature request: hành vi mới chưa nằm trong scope; đưa backlog, không vội code.

Mỗi phản hồi phải có route/viewport, steps, expected/actual và tác động. “Chưa đẹp” hoặc “AI chưa thông minh” chưa đủ để hành động.

## 12. Chấm điểm, nộp bài và chuyển sang buổi 8

### 12.1 Rubric 100 điểm

| Hạng mục | Điểm | Dấu hiệu đạt |
|---|---:|---|
| Bug report, giả thuyết và bằng chứng nguyên nhân | 20 | Người khác tái hiện được; sửa dựa trên kiểm chứng |
| Thay đổi nhỏ, retest và regression | 15 | Diff đúng scope; lỗi gốc hết; luồng liên quan còn đúng |
| QA nghiệp vụ, quyền và AI | 25 | Expected cụ thể, actual trung thực, có DB/API evidence khi cần |
| Mobile, accessibility và cải thiện chất lượng | 10 | Một vấn đề thực đã được sửa, có trước-sau |
| Build, deploy và production smoke | 20 | Build/run thật, URL đúng commit, kiểm runtime và env |
| Release record, rollback, video và handoff | 10 | Bàn giao được, nêu giới hạn và việc tiếp theo |

**Đạt:** từ 70/100 và thỏa toàn bộ điều kiện bắt buộc ở mục 3. Điểm hình thức không bù cho lỗi lộ dữ liệu/secret, P0/P1 còn mở trong scope, build fail hoặc QA không trung thực. Bài dùng mock minh bạch vẫn được đánh giá phần đã làm; không tính là đã kiểm chứng provider thật, và ghi backlog nối sang buổi 8.

### 12.2 Hồ sơ nộp bài

```text
release-buoi-07/
  01-bug-report.md
  02-test-matrix.csv
  03-quality-before-after.md
  04-release-record.md
  05-known-issues-and-backlog.md
  evidence/                 # ảnh/log đã che dữ liệu nhạy cảm
  demo-video-url.md         # video 2-3 phút, có quyền xem phù hợp
```

Kèm repository/commit, URL production và diff/PR nếu có. Có thể dùng phiếu mẫu ở tài liệu bài tập thay vì tự tạo mới.

**Kịch bản video 2-3 phút:** 20 giây vấn đề -> 40 giây lỗi gốc và cách chứng minh nguyên nhân -> 50 giây luồng sau sửa + một deny quyền/failure path -> 30 giây production/mobile -> 20 giây giới hạn và bước tiếp theo. Không quay key, mật khẩu, token hay dữ liệu thật.

### 12.3 Bài tập sau giờ và exit ticket

- Hoàn thiện case chưa chạy; chọn một bài toán ngành trong tài liệu thực hành, sửa ít nhất một lỗi có evidence.
- Với bug nghiệp vụ/quyền đáng kể và hạ tầng test sẵn: thêm một regression có ích; nếu chưa có, mô tả automation đề xuất và chạy manual đầy đủ, không tự ghi test tự động đã chạy.
- Nộp backlog 5 việc cho buổi 8, mỗi việc có tác động, công sức, ưu tiên, người làm và điều kiện đạt.

Exit ticket: “Lỗi em sửa ảnh hưởng công việc gì? Bằng chứng nào xác nhận nguyên nhân? Test nào bảo vệ bản sửa? URL đang chạy commit nào? Khi lỗi production quay lại, em khôi phục thế nào?”

## 13. Nguồn và giới hạn sử dụng

**Tài liệu nội bộ đã đối chiếu:** lộ trình PDF trang 2, 9-11; `vibe_code_scripts/GIAO-AN-BUOI-05-SUPABASE-CRUD-THAT.md`; `buoi_6/GIAO-AN-BUOI-06-AI-FEATURE-API-TU-DONG-HOA.md`; bộ bài tập liên thông 5 buổi. Giáo án Auth/RLS bổ trợ trong `buoi_6` không thay thế chủ đề AI của buổi 6 theo lộ trình.

Các đường dẫn tài liệu chính thức được đặt ngay cạnh hướng dẫn kỹ thuật; đã tra cứu khi soạn ngày 10/11/2026. Giảng viên đối chiếu phiên bản thực tế của project trước giờ dạy. Tình huống, kết quả minh họa và tiêu chí ở đây là bài thiết kế; bộ tài liệu không kèm repo ứng dụng chạy sẵn, không xác nhận đã thực thi test, sửa hay deploy project nào của học viên.
