# VÍ DỤ MẪU - APPLYING DATABASE QUERY AGENT INTO BANKING SYSTEM MANAGEMENT

**Phiên bản:** 14/11/2026. **Dùng cùng:** bài học viên 180 phút. **Quy mô:** 3 Features, 6 US, 18 AC.

**Luồng:** Epic → Features → US → AC → Analyze/Dev/Test → point và lý do → nhóm review/chốt.

> Ví dụ đã điền để giảng viên giải thích sau khi học viên tự làm. Mọi đầu ra AI, estimate chốt và hoạt động nhóm là minh họa, chưa phải evidence thực thi.

## 01. Epic, Product Goal và cách đọc ví dụ

**Epic ID:** E01.

**Epic:** Applying Database Query Agent into Banking System Management.

**Người dùng:** nhân viên vận hành HN01/HCM01, quản lý được cấp nhiều chi nhánh và chuyên viên đối soát.

**Vấn đề:** nhân viên phải nhờ đội dữ liệu viết query cho câu hỏi báo cáo lặp lại. Cách hiểu “thành công”, “số dư” và “ngày báo cáo” chưa thống nhất; câu trả lời khó truy lại nguồn.

**Product Goal:** người dùng pilot tự trả lời được các câu hỏi ưu tiên về thanh toán chuyển đi và số dư sổ cái cuối ngày, đúng quyền và có thể đối chiếu với nguồn chuẩn.

**Trong phạm vi:** nguồn reporting giả lập; truy vấn chỉ đọc; kết quả tổng hợp theo branch/ngày/currency; clarify, deny, nguồn, data_as_of và trace_id. Không gửi dữ liệu định danh khách hàng vào model.

**Ngoài phạm vi:** chuyển tiền, điều chỉnh số dư, xuất dữ liệu khách hàng, quy đổi tỷ giá và báo cáo mọi nghiệp vụ ngân hàng.

**Tiền đề estimate:** có sandbox, fixture, khung ứng dụng, đăng nhập và test runner; metric/policy còn cần làm rõ. Có mẫu executor/audit, nhưng chưa tích hợp đủ quyền vào luồng hỏi. Không giả định chỉ thêm prompt là xong.

**Ba câu hỏi đã được làm rõ trong ví dụ:** (1) “Thành công” nghĩa là POSTED; (2) “số dư” trong MVP là ledger balance cuối ngày; (3) scope do principal server quyết định, không do model hay request tự khai.

**Cách dùng:** ví dụ điền mẫu cho bài học viên 180 phút: 3 Features, 6 US, mỗi US 3 AC; có nhãn và point kèm lý do. Dùng sau khi học viên đã tự nộp bản nháp.

**Lưu ý về bằng chứng:** đầu ra AI, phản biện nhóm, point chốt và Review dưới đây là tình huống minh họa được soạn cho lớp, không phải nhật ký dùng AI, estimate của một đội thật hoặc kết quả test đã thực chạy. Point mẫu không phải đáp án duy nhất.

**Quy trình:** Epic → AI sinh Features → AI sinh US → AI sinh AC → phân loại Analyze/Dev/Test → AI estimate và lý do → nhóm review/chốt → chọn Sprint.

## 02. Quy tắc và fixture giống phiếu học viên

Ngày D = 12/11/2026, timezone Asia/Ho_Chi_Minh. Lọc ngày theo [đầu D, đầu D+1), chuyển biên sang timezone lưu trữ thống nhất. Mỗi dòng là một payment duy nhất; tổng chuyển đi chỉ tính POSTED và không gồm phí. Tách currency, không tự quy đổi.

| ID | Branch | Tiền | Amount | Status | Thời điểm |
|---|---|---|---:|---|---|
| P01 | HN01 | VND | 1.000.000 | POSTED | D 09:00 |
| P02 | HN01 | VND | 2.000.000 | POSTED | D 10:00 |
| P03 | HN01 | VND | 500.000 | PENDING | D 11:00, chưa ghi sổ |
| P04 | HN01 | USD | 100,00 | POSTED | D 12:00 |
| P05 | HCM01 | VND | 3.000.000 | POSTED | D 09:30 |
| P06 | HN01 | VND | 700.000 | FAILED | D 13:00, chưa ghi sổ |
| P07 | HN01 | VND | 4.000.000 | POSTED | D+1 00:00 |

**Oracle thanh toán:** HN01, D, VND, POSTED → 3.000.000 VND/count 2. P03/P06 chưa có posted_at; giờ bảng là giờ ghi nhận. P04 khác currency; P05 khác branch; P07 ngoài ngày.

**Fixture số dư:** tại business_date D, HN01 có A01 = 5.000.000 VND, A02 = 7.000.000 VND, A03 = 200,00 USD. Các account ref chỉ là dữ liệu demo nội bộ; không trả chúng ra model/người dùng trong MVP tổng hợp.

**Oracle số dư:** ledger balance HN01 ngày D/VND = 12.000.000 VND. Thiếu snapshot D thì DATA_UNAVAILABLE; không tự chọn ngày cũ hoặc cộng số dư nhiều ngày/currency.

**Danh tính:** U1 chỉ HN01, U2 chỉ HCM01, U3 cả hai. Scope được thực thi từ principal server qua executor tới nguồn reporting. MVP không bật cache kết quả dùng chung; nếu bổ sung cache phải kiểm chứng phân tách quyền trước khi bật.

**Điểm chung mọi câu trả lời:** đúng scope, ngày/currency và định nghĩa; có nguồn, data_as_of, trace_id. Lỗi query/model không được thay bằng số liệu tự sinh. “Nguồn chưa có dữ liệu” khác với kết quả 0.

## 03. Bước 1 - Epic → AI sinh Features

**Prompt mẫu:**

```text
Epic E01: Applying Database Query Agent into Banking System Management.
Dùng brief, Product Goal, scope và fixture ở mục 1–2.
Sinh 3 Features theo khả năng có giá trị cho người dùng.
Mỗi Feature có ID, người dùng, giá trị, phạm vi/ngoài phạm vi,
giả định/phụ thuộc và ưu tiên. Chưa sinh US/code.
Không lấy frontend/backend/database làm tên Feature.
```

**Bản AI chưa đạt, dùng để thảo luận:** “F01 xây database; F02 xây AI; F03 xây giao diện và cho phép sửa giao dịch.”

**Nhóm sửa:** ba tên công nghệ không thể hiện kết quả người dùng; sửa giao dịch nằm ngoài read-only. Bản Feature sau review:

### F01 - Hỏi báo cáo thanh toán chuyển đi

Nhân viên vận hành tự hỏi tổng/count theo ngày và currency trong branch được cấp. Giá trị: bớt nhờ viết SQL và có số để đối chiếu. Scope: POSTED, một ngày, một currency mỗi câu; grouping theo currency là mở rộng. Ưu tiên 1. Phụ thuộc: metric/oracle và executor đúng quyền.

### F02 - Hỏi số dư sổ cái cuối ngày

Quản lý/nhân viên có quyền xem tổng ledger balance tại business_date, kèm thời điểm dữ liệu. Scope: snapshot đã có, tách currency; không phải available balance hoặc real-time balance. Ưu tiên 2. Phụ thuộc: nguồn snapshot và định nghĩa được xác nhận.

### F03 - Đối soát và kiểm chứng kết quả theo quyền

Chuyên viên đối soát có bộ kiểm chứng dùng lại để so oracle, lần theo trace và phát hiện hồi quy semantic/quyền. Scope: harness/báo cáo kiểm chứng trong sandbox; không phải UI phân tích đầy đủ cho mọi nhân viên. Ưu tiên theo rủi ro, có thể làm song song với F01/F02.

**Quan hệ:** quyền/trace là chất lượng bắt buộc của luồng Dev F01/F02. F03 bổ sung công cụ/chiến dịch kiểm chứng độc lập; không làm Feature bảo mật “để cuối mới có”.

## 04. Bước 2 - Features → AI sinh 6 US

**Prompt mẫu:**

```text
Từ E01 và ba Features đã review, sinh 6 US/PBI nhỏ.
Giữ Feature cha và US ID; nêu người nhận giá trị, đầu ra, lợi ích,
scope và phụ thuộc. Chưa estimate hoặc viết code.
Chỉ tách Analyze/Test khi có đầu ra độc lập.
US Dev vẫn gồm phân tích và test cần để đáp ứng AC/DoD.
```

| Feature | US ID | Đầu ra chính / người nhận giá trị |
|---|---|---|
| F01 | F01-US01 | Chủ metric xác nhận định nghĩa thanh toán và oracle |
| F01 | F01-US02 | Nhân viên hỏi tổng/count chuyển đi đúng quyền |
| F02 | F02-US01 | Chủ chỉ tiêu xác nhận ledger balance và snapshot |
| F02 | F02-US02 | Người dùng hỏi số dư cuối ngày có freshness |
| F03 | F03-US01 | Chuyên viên có harness đối soát hai metric dùng lại |
| F03 | F03-US02 | Người kiểm chứng có bộ đối kháng quyền/trace độc lập |

**Bản AI quá rộng:** “Là nhân viên, tôi muốn hỏi tất cả báo cáo, export Excel và chỉnh dữ liệu sai.”

**Nhóm sửa:** giữ hai luồng báo cáo ưu tiên; bỏ export/chỉnh dữ liệu khỏi MVP; tách công việc xác nhận metric và các bộ kiểm chứng có đầu ra riêng. Không chia UI/API/DB thành các US mà chưa có kết quả sử dụng.

**Vì sao không mỗi Feature ba US?** Bài giao cần ít nhất 6 US có cả Analyze/Dev/Test. Số US phụ thuộc đầu ra và độ lớn, không phải công thức “mỗi Feature đúng một Analyze, một Dev, một Test”.

**Truy vết ví dụ:** E01 → F01 → F01-US02 → F01-US02-AC01. Loại và point ở các bước sau gắn vào F01-US02; AC không được estimate riêng.

## 05. Bước 3–5 - Sinh AC, phân loại, estimate

**Prompt sinh AC:**

```text
Từ 6 US đã review và fixture, sinh 3 AC/US với ID truy vết.
Mỗi AC có Given/When/Then hoặc dữ liệu/hành vi/expected rõ.
Bao phủ biên, quyền và failure khi liên quan.
Analyze: đầu ra và cách/người xác nhận. Test: oracle, cách chạy, evidence.
Không tự quyết định quy tắc nghiệp vụ còn thiếu.
```

**Prompt phân loại và estimate, dùng sau khi review AC:**

```text
Với từng US, chọn loại chính Analyze/Dev/Test theo đầu ra và nêu lý do.
Mốc 3 SP: đầu ra nhỏ, vài điều kiện, contract rõ và nền có thể dùng lại.
Hạ tầng giả định như mục 1; chưa có tích hợp quyền hoàn chỉnh.
Đề xuất 1,2,3,5,8,13 SP; giải thích phức tạp, khối lượng,
bất định/phụ thuộc, test cần để Done và độ tin cậy.
Chỉ ra điều làm point thay đổi. Để trống point nhóm chốt.
Không estimate lại cùng việc ở Feature/Epic/task hoặc US Test khác.
```

**Mốc nhóm chọn trong tình huống minh họa:** F01-US01 = 3 SP: một metric, nhiều điều kiện lọc/biên ngày, một chủ nghiệp vụ và fixture sẵn. Không coi đây là đơn vị thời gian hoặc mốc chung mọi nhóm.

**Nhóm chốt:** từng người ước lượng độc lập; người chọn thấp/cao làm rõ scope và giả định; sau trao đổi mới thống nhất. Không lấy trung bình máy móc và không giữ point của AI chỉ vì AI giải thích trôi chảy.

**Ba loại:** Analyze tạo định nghĩa/quyết định giảm bất định; Dev tạo khả năng phần mềm tích hợp và được kiểm tra; Test tạo bộ kiểm chứng/chiến dịch độc lập dùng lại. US Dev vẫn phải có test AC và regression liên quan.

**Phân biệt:** 18 AC dưới đây là điều kiện riêng của 6 US. DoD sản phẩm còn yêu cầu tích hợp, đúng quyền, read-only, audit/trace, kiểm tra hồi quy và hướng dẫn/giới hạn. Tài liệu phân tích hoàn thành không tự thành Increment phần mềm.

## 06. F01-US01 - Xác nhận metric thanh toán và oracle

**Epic/Feature/US:** E01 → F01 → F01-US01.

**US:** Là chủ metric vận hành, tôi muốn thống nhất tổng/count chuyển đi để đội xây và kiểm tra cùng một định nghĩa.

**Phạm vi:** Một metric, một ngày, branch/currency xác định; không bao gồm giao dịch hoàn/reversal hoặc phí chưa được định nghĩa.

### Acceptance Criteria sau review

**F01-US01-AC01:** Given brief/fixture, When đối chiếu với chủ metric, Then có định nghĩa được xác nhận: POSTED, posted_at, grain payment duy nhất, branch, currency và loại phí; câu chưa rõ được ghi riêng.

**F01-US01-AC02:** Given fixture HN01/D/VND, When lập oracle, Then tổng = 3.000.000 VND và count = 2; giải thích từng P03–P07 bị loại, kể cả biên D+1 00:00.

**F01-US01-AC03:** Given câu “tổng giao dịch hôm qua?”, When soạn hợp đồng hỏi lại, Then xác định loại giao dịch/currency còn thiếu và ngày cụ thể theo timezone; chủ nghiệp vụ xác nhận cách hiểu trước Dev.

### Phân loại và point kèm lý do

**Loại: Analyze.** Vì đầu ra chính là định nghĩa/oracle/quyết định được xác nhận, giúp giảm bất định.

**AI đề xuất: 3 SP.** Phạm vi chỉ một metric nhưng cần đối chiếu trạng thái, grain, ngày, tiền tệ và ví dụ biên; lớn hơn việc ghi một định nghĩa đơn lẻ.

**Nhóm chốt minh họa: 3 SP.** Giữ 3 SP vì scope và oracle đã rõ, một chủ metric có thể xác nhận. Không cộng thêm point vì phải chờ cuộc họp; quản lý thời gian chờ như phụ thuộc.

**Giả định/điều làm estimate đổi:** Có fixture và người xác nhận metric. Nếu bổ sung reversal/đa nguồn thì phải làm rõ và estimate lại.

**Đầu ra/evidence cần nộp:** Từ điển metric, oracle và biên bản xác nhận/điểm còn mở. Đây là đầu ra Analyze, chưa phải phần mềm.

## 07. F01-US02 - Hỏi tổng chuyển đi đúng quyền

**Epic/Feature/US:** E01 → F01 → F01-US02.

**US:** Là nhân viên U1, tôi muốn hỏi tổng/count chuyển đi của HN01 ngày D bằng VND để tự đối chiếu báo cáo.

**Phạm vi:** Một intent tổng/count, một ngày và currency; có clarify/deny/error/source/trace. Nguồn là reporting chỉ đọc, không bật cache dùng chung.

### Acceptance Criteria sau review

**F01-US02-AC01:** Given U1 và fixture đã xác nhận, When hỏi tổng POSTED HN01/D/VND, Then nhận 3.000.000 VND/count 2, đúng bộ lọc, nguồn, data_as_of và trace_id; không gửi định danh khách hàng vào model.

**F01-US02-AC02:** Given câu thiếu ngày/currency hoặc câu hỏi HCM01 trái quyền U1, When xử lý request kể cả sửa branch ở API, Then hỏi lại hoặc deny phù hợp; chưa đủ dữ kiện/không có quyền thì không chạy query nghiệp vụ ngoài scope.

**F01-US02-AC03:** Given executor trong sandbox, When thử ghi dữ liệu, query lỗi hoặc timeout, Then thao tác bị chặn/kết thúc có kiểm soát, nguồn không đổi và không bịa số; test AC, role/session thực và hồi quy cần thiết đều có evidence trước Done.

### Phân loại và point kèm lý do

**Loại: Dev.** Vì đầu ra chính là khả năng phần mềm dùng được và đã kiểm tra, không chỉ code.

**AI đề xuất: 5 SP.** AI đề xuất 5 vì thấy một intent, nguồn và khung UI/API có sẵn; giả định nhầm quyền/trace đã tích hợp đầy đủ.

**Nhóm chốt minh họa: 8 SP.** Đổi 5 → 8 SP: còn tích hợp principal → executor → reporting, kiểm tra direct API/role, trace và đường lỗi. Đây là phức tạp/khối lượng bị bỏ sót, không chỉ tăng point vì “ngân hàng rủi ro”.

**Giả định/điều làm estimate đổi:** Login/reporting/test runner có mẫu; chưa có policy end-to-end. Nếu phải xây IAM mới hoặc hỗ trợ SQL tự do thì tách US, không coi 8 SP đủ.

**Đầu ra/evidence cần nộp:** Luồng dùng được cùng test happy/clarify/deny/error, kiểm tra nguồn không đổi và trace. Test thiết yếu đã nằm trong 8 SP; không chờ US Test mới Done.

## 08. F02-US01 - Xác nhận ledger balance và snapshot

**Epic/Feature/US:** E01 → F02 → F02-US01.

**US:** Là chủ chỉ tiêu số dư, tôi muốn chốt cách tính ledger balance cuối ngày để người dùng không nhầm với số dư khả dụng hoặc thời gian thực.

**Phạm vi:** Một business_date/currency, theo branch tại snapshot; không suy diễn từ các payment trong ngày.

### Acceptance Criteria sau review

**F02-US01-AC01:** Given yêu cầu “số dư”, When làm rõ với chủ chỉ tiêu, Then tài liệu phân biệt ledger/available balance, business_date, data_as_of và grain account/currency/date; quy tắc branch được xác nhận.

**F02-US01-AC02:** Given fixture số dư HN01/D, When lập oracle VND, Then tổng = 12.000.000 VND; USD 200,00 được tách; không cộng snapshot nhiều ngày.

**F02-US01-AC03:** Given snapshot D thiếu hoặc trễ, When xác nhận hành vi, Then thiếu trả DATA_UNAVAILABLE, trễ hiển thị data_as_of và xử lý theo chính sách chủ chỉ tiêu đã xác nhận; ngưỡng chưa rõ phải ghi câu hỏi.

### Phân loại và point kèm lý do

**Loại: Analyze.** Vì đầu ra chính là định nghĩa/oracle/quyết định được xác nhận, giúp giảm bất định.

**AI đề xuất: 2 SP.** AI đề xuất 2 vì cho rằng chỉ cần viết công thức SUM như metric thanh toán đã có.

**Nhóm chốt minh họa: 3 SP.** Đổi 2 → 3 SP: số dư là snapshot, cần thêm định nghĩa ngày chốt, quyền branch tại snapshot và hành vi thiếu/trễ; độ lớn tương đương mốc metric đầu.

**Giả định/điều làm estimate đổi:** Có chủ chỉ tiêu và metadata snapshot. Nếu không có watermark đáng tin, phải ghi rủi ro và đổi phạm vi.

**Đầu ra/evidence cần nộp:** Định nghĩa số dư, oracle và quyết định missing/stale được xác nhận. Không tuyên bố đã đo chất lượng nguồn dữ liệu.

## 09. F02-US02 - Hỏi ledger balance cuối ngày

**Epic/Feature/US:** E01 → F02 → F02-US02.

**US:** Là người dùng có quyền HN01, tôi muốn hỏi ledger balance cuối ngày D bằng VND để đối chiếu báo cáo đúng thời điểm.

**Phạm vi:** Dùng lại executor/identity/trace của F01-US02; thêm metric snapshot và các trạng thái dữ liệu.

### Acceptance Criteria sau review

**F02-US02-AC01:** Given U1 và snapshot HN01/D, When hỏi ledger balance VND, Then trả 12.000.000 VND, đúng business_date, currency, data_as_of, nguồn và trace_id; không trả account ref nội bộ.

**F02-US02-AC02:** Given câu “số dư hôm qua” chưa rõ loại/currency hoặc snapshot D không có, When xử lý, Then clarify hoặc DATA_UNAVAILABLE tương ứng, không tự dùng số dư khả dụng/ngày khác hay coi thiếu nguồn là 0.

**F02-US02-AC03:** Given U1 hỏi branch ngoài quyền, nguồn trễ hoặc DB/model lỗi, When truy vấn, Then deny/cảnh báo theo chính sách/lỗi rõ ràng; không lộ dữ liệu hay sinh số giả; regression metric thanh toán và test snapshot/quyền đạt trước Done.

### Phân loại và point kèm lý do

**Loại: Dev.** Vì đầu ra chính là khả năng phần mềm dùng được và đã kiểm tra, không chỉ code.

**AI đề xuất: 5 SP.** Thêm nguồn/metric và nhánh snapshot/clarify/freshness nhưng tái sử dụng luồng thực thi và trace đã được chứng minh.

**Nhóm chốt minh họa: 5 SP.** Giữ 5 SP vì có nhiều trạng thái hơn mốc 3 nhưng không xây lại quyền/executor. Tách phần hạ tầng nếu giả định tái sử dụng không đúng.

**Giả định/điều làm estimate đổi:** F01-US02 và F02-US01 đã đủ điều kiện dùng lại; metadata snapshot có sẵn; không triển khai balance thời gian thực.

**Đầu ra/evidence cần nộp:** Luồng số dư có test snapshot/thiếu/trễ/quyền, hồi quy thanh toán và trace; không dùng point riêng cho từng AC.

## 10. F03-US01 - Harness đối soát hai metric dùng lại

**Epic/Feature/US:** E01 → F03 → F03-US01.

**US:** Là chuyên viên đối soát, tôi muốn chạy bộ so oracle theo snapshot để phát hiện thay đổi prompt/template làm sai chỉ tiêu.

**Phạm vi:** Đầu ra là harness/dataset/report độc lập cho hai metric, không tính lại các lần kiểm tra AC của hai US Dev.

### Acceptance Criteria sau review

**F03-US01-AC01:** Given oracle đã xác nhận, When chuẩn bị bộ kiểm chứng, Then có case POSTED/PENDING/FAILED, branch, currency, biên ngày, số dư nhiều snapshot và thiếu snapshot với expected độc lập.

**F03-US01-AC02:** Given phiên bản ứng dụng và snapshot cố định, When chạy harness theo hướng dẫn, Then báo actual/expected, PASS/FAIL/BLOCKED, metric/query version và trace; chưa chạy không được ghi PASS.

**F03-US01-AC03:** Given bản lỗi sandbox cố ý tính PENDING hoặc cộng số dư hai ngày, When chạy bộ kiểm chứng, Then phát hiện chênh lệch tương ứng; evidence nêu rõ dataset và giới hạn, không suy rộng an toàn mọi câu hỏi.

### Phân loại và point kèm lý do

**Loại: Test.** Vì đầu ra chính là bộ kiểm chứng độc lập dùng lại, không phải bước test bị hoãn của Dev.

**AI đề xuất: 3 SP.** Có runner sẵn và hai oracle đã chốt; cần fixture, cách so và báo cáo dùng lại, ít bất định triển khai.

**Nhóm chốt minh họa: 3 SP.** Giữ 3 SP: phạm vi là công cụ/đánh giá dùng lại cho hai metric, không phải dựng hệ thống kiểm thử từ đầu.

**Giả định/điều làm estimate đổi:** Có quyền sandbox và cách cố định snapshot; nếu phải dựng test infrastructure mới thì cần tách/estimate lại.

**Đầu ra/evidence cần nộp:** Mã/hướng dẫn harness, dataset và báo cáo thực chạy. Các trạng thái trong ví dụ này là yêu cầu evidence, không phải kết quả đã thực hiện.

## 11. F03-US02 - Bộ đối kháng quyền và truy vết

**Epic/Feature/US:** E01 → F03 → F03-US02.

**US:** Là người kiểm chứng quyền, tôi muốn bộ đối kháng dùng lại để phát hiện lỗi scope hoặc trace khi người dùng và cách hỏi thay đổi.

**Phạm vi:** Chiến dịch/harness bổ sung: direct API, executor role, session/trace và cấu hình cache. Không thay quyền/test nền bắt buộc trong hai Dev US.

### Acceptance Criteria sau review

**F03-US02-AC01:** Given U1/U2/U3, When chạy bộ truy cập cùng branch/chéo branch và sửa scope trong API, Then allow/deny đúng ma trận, không trả số liệu ngoài quyền và ghi trace_id theo quyền audit phù hợp.

**F03-US02-AC02:** Given yêu cầu bỏ giới hạn, ghi dữ liệu hoặc phiên đổi user, When kiểm chứng qua API/executor thực và cấu hình cache, Then nguồn không đổi, không mở thêm công cụ, không dùng kết quả user khác; MVP phải xác nhận cache dùng chung đang tắt.

**F03-US02-AC03:** Given bản lỗi chỉ trong sandbox bỏ kiểm tra scope hoặc làm mất trace, When chạy bộ đối kháng, Then phát hiện lỗi; báo version/principal/evidence và giới hạn; việc đã vượt qua bộ test không được diễn giải là an toàn tuyệt đối.

### Phân loại và point kèm lý do

**Loại: Test.** Vì đầu ra chính là bộ kiểm chứng độc lập dùng lại, không phải bước test bị hoãn của Dev.

**AI đề xuất: 1 SP.** AI đề xuất 1 vì coi đây là kiểm tra một câu hỏi trái quyền trên UI, bỏ sót API/DB/session/trace.

**Nhóm chốt minh họa: 5 SP.** Đổi 1 → 5 SP: phải chuẩn bị nhiều identity, kiểm tra nhiều lớp và bản lỗi có kiểm soát, thu evidence không lộ dữ liệu. Nếu bật cache thật, cần thêm test theo cơ chế cache và estimate lại.

**Giả định/điều làm estimate đổi:** Có executor/trace và test identities đã tích hợp; công việc không bao gồm sửa mọi lỗi được phát hiện. Lỗi sửa là backlog riêng theo tác động.

**Đầu ra/evidence cần nộp:** Bộ đối kháng, hướng dẫn và báo cáo theo ma trận quyền/trace. Không double-count test quyền cơ bản đã nằm trong Dev.

## 12. Tổng hợp loại, point và phản biện

**Đây là bảng đã điền minh họa**, khác phiếu học viên để trống. Các point chốt thể hiện tình huống nhóm phản biện, không phải biên bản estimate thật.

| US ID | Loại | AI SP | Nhóm SP | Lý do giữ/đổi |
|---|---|---:|---:|---|
| F01-US01 | Analyze | 3 | 3 | Giữ: một metric, fixture rõ, nhiều điều kiện tương đương mốc. |
| F01-US02 | Dev | 5 | 8 | 5 → 8: thiếu tích hợp quyền/executor/trace và đường lỗi. |
| F02-US01 | Analyze | 2 | 3 | 2 → 3: thêm ngữ nghĩa snapshot, ngày chốt, thiếu/trễ. |
| F02-US02 | Dev | 5 | 5 | Giữ: thêm metric nhưng dùng lại executor/trace đã kiểm chứng. |
| F03-US01 | Test | 3 | 3 | Giữ: harness hai metric, runner/oracle đã có. |
| F03-US02 | Test | 1 | 5 | 1 → 5: nhiều identity và đường API/DB/session/trace. |

**Tổng số học:** AI đề xuất 19 SP, nhóm chốt minh họa 27 SP. Đây là độ lớn tương đối của backlog mẫu, không phải thời lượng hay capacity một Sprint. Không suy ra “27 SP chắc chắn xong trong hai tuần”. Không dùng point để xếp hạng người.

**Đã tính gì trong Dev?** Phân tích kỹ thuật cần thiết, code, tích hợp, test AC và regression liên quan. **Test riêng thêm gì?** Harness/dataset/report độc lập và chiến dịch đối kháng dùng lại. Không tính hai lần cùng phần việc.

**Điều làm cả bảng đổi:** thiếu sandbox/IAM, không có snapshot đáng tin, SQL tự do, đa nguồn hoặc cache dùng chung. Làm rõ/tách US rồi estimate lại; không chỉ nâng số cho một US mơ hồ.

**Khi tách US:** viết US con với AC riêng, estimate từng con và loại estimate cha khỏi tổng. Không cộng point Epic/Feature/task vào tổng US. Không sửa point US đã Done để làm đẹp kết quả.

## 13. Ví dụ xử lý bốn thẻ của bài tập

### C01 - Tính cả PENDING thành 3.500.000 VND

**Ảnh hưởng:** F01-US01-AC01/02 và F01-US02-AC01. Đối chiếu fixture xác nhận expected 3.000.000/count 2. Nếu đang triển khai trước Done, sửa query và regression trong scope đã estimate, không tự thêm point cho cùng AC. Nếu phát hiện sau Done, tạo bug liên kết, sắp ưu tiên theo tác động; không sửa point lịch sử.

### C02 - Cộng VND và USD thành một số

**Ảnh hưởng:** F01-US02-AC01 và bộ oracle. MVP hỏi một currency, nên hỏi lại nếu thiếu. Nếu cần nhóm theo currency, tạo US mở rộng có AC riêng: VND 3.000.000/count 2 và USD 100,00/count 1. Không tự thêm tỷ giá. US mới mới cần estimate; sửa sai requirement hiện có không tự động thành Feature mới.

### C03 - “Quyền chỉ là WHERE branch_id”, AI đề xuất 1 SP

**Nhóm phản biện:** ai đặt branch, scope ở API/DB có thể vượt không, đổi user/trace/cache thế nào? Với phần triển khai F01-US02, nhóm giữ scope đầy đủ và chốt 8 thay 5 SP; nếu AI bỏ gần hết scope và đưa 1 thì cũng phải phân tích lại từ AC. Với chiến dịch độc lập F03-US02, nhóm chốt 5 thay 1 SP. Đây là hai đầu ra khác nhau, không cộng trùng test quyền cơ bản.

### C04 - Muốn agent chỉnh số dư giữa Sprint

**Ảnh hưởng:** ngoài read-only, chưa có Feature/US được chọn. PO hỏi nhu cầu và quy trình chỉnh sửa hiện hành, ghi đề xuất riêng; giữ Sprint Goal tra cứu, không cấp quyền ghi cho executor tạm thời. Nếu phát triển sau này phải có scope/AC/estimate và quy trình nghiệp vụ riêng.

**Cách ghi quyết định:** sự kiện → ID ảnh hưởng → điều đã biết/chưa biết → lựa chọn → người trao đổi → quyết định → AC/loại/point thay đổi gì → evidence cần kiểm chứng. Một quyết định hợp lý có thể là giữ nguyên point vì scope không đổi.

## 14. Ví dụ Planning, Review và Retro

**Sprint Goal minh họa:** U1 tự hỏi tổng/count chuyển đi HN01 ngày D bằng VND, nhận kết quả đối chiếu được, đúng quyền và có trace.

**Chọn F01-US01 và F01-US02 làm lát cắt đầu tiên.** Tổng minh họa 11 SP chỉ là dữ kiện estimate. Nhóm kiểm tra năng lực/phụ thuộc rồi mới dự báo; không có capacity thực tế trong tài liệu này. F03 là bộ kiểm chứng độc lập bổ sung, không cho phép dời test nền của F01-US02.

| Kế hoạch | Evidence/điều kiện |
|---|---|
| Chốt metric/oracle | Từ điển và expected được chủ nghiệp vụ xác nhận |
| Tích hợp luồng nhỏ | UI/API → principal → executor → reporting → answer |
| Test và trace | Happy/clarify/deny/error, đúng quyền/read-only, nguồn và trace |
| Review | U1 tự hỏi và so oracle; không báo PASS nếu chưa chạy |

**DoD sản phẩm tối thiểu:** AC đạt, tích hợp bản chung, test quyền/lỗi/hồi quy liên quan có evidence, không ghi nguồn, không lộ dữ liệu cấm, có trace/hướng dẫn/giới hạn. US Analyze chỉ Done ở đầu ra phân tích, không thay Increment phần mềm.

**Kịch bản Review minh họa:** U1 hỏi đủ dữ kiện → expected 3.000.000/count 2; hỏi HCM01 → deny; hỏi “tổng hôm qua” → clarify. Đây là expected cho buổi thử, chưa phải kết quả đã chạy. Người dùng góp ý muốn hỏi số dư cuối ngày; PO cân nhắc F02 theo giá trị và năng lực còn lại.

**Cập nhật backlog:** làm rõ F02-US01 trước; tái sử dụng nền F01-US02 cho F02-US02; ưu tiên F03-US02 nếu có rủi ro quyền chưa đủ bằng chứng. Không buộc nhận tất cả chỉ để có nhiều point Done.

**Retro minh họa:** estimate AI thiếu scope vì đầu vào ghi “có đăng nhập” khiến nhóm hiểu nhầm đã có quyền end-to-end. Cải tiến: trước estimate Dev, ghi rõ nền nào đã được kiểm chứng và đường test bắt buộc. Người theo dõi: một Developer; ở lần estimate sau kiểm tra bản scope và AC trước mở point.

## 15. Đối chiếu bài nộp và hướng dẫn giảng viên

### Chuỗi minh họa hoàn chỉnh

E01 → F01 “Hỏi báo cáo chuyển đi” → F01-US02 “Hỏi tổng đúng quyền” → F01-US02-AC01/02/03 → loại Dev → AI 5 SP vì giả định nền sẵn → nhóm 8 SP vì còn scope quyền/trace → đưa vào Sprint Goal theo capacity thực tế → Review bằng oracle/quyền → cập nhật backlog.

### Vì sao ví dụ đáp ứng bài tập?

| Yêu cầu | Đầu ra trong ví dụ |
|---|---|
| Ít nhất 3 Features | F01/F02/F03, theo khả năng và giá trị |
| Ít nhất 6 US | Mỗi Feature 2 US, giữ ID cha |
| Ít nhất 18 AC | 6 US × 3 AC, expected/oracle rõ |
| Có Analyze/Dev/Test | 2 Analyze, 2 Dev, 2 Test độc lập |
| AI SP và nhóm SP có lý do | 3 giữ, 3 đổi; giả định và scope giải thích rõ |
| Có thay đổi và phản biện | C01–C04, không estimate trùng/hạ chất lượng |
| Có Planning/Review/Retro | Lát cắt F01, expected và cải tiến cụ thể |

### Dùng để chấm, không dùng làm đáp án duy nhất

Chấm theo rubric của phiếu: Epic/Features 15; US 20; AC 20; phân loại 10; estimate/phản biện 20; quyết định/Planning/Review 15. Không điền điểm đạt thật cho ví dụ chưa được thực hiện. Nhóm có thể chọn Feature/US/point khác nếu hợp lý và không bỏ quyền/chất lượng.

### Giới hạn của bản mẫu

Đây là worked example cho bài backlog 180 phút. Không có source agent, tài khoản ngân hàng, test report thực chạy, benchmark hay xác nhận production. Các con số là fixture và estimate minh họa. Nếu thực thi kỹ thuật, nhóm phải nộp actual/evidence riêng.

**Đọc cùng:** [Phiếu học viên](BAI-TAP-VA-PHIEU-THUC-HANH-SCRUM.md), [giáo án](GIAO-AN-SCRUM-VA-VIBE-CODING.md), [hướng dẫn giảng viên](HUONG-DAN-GIANG-VIEN-VA-GOI-Y-XU-LY.md).
