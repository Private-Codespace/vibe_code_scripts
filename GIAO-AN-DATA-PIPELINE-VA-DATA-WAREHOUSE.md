# GIÁO ÁN - VIBE CODE DỰ ÁN ADVENTUREWORKS DATA PLATFORM

**Thời lượng:** 180 phút, gồm 15 phút nghỉ; 4-8 giờ hoàn thiện sau lớp tùy năng lực và môi trường.  
**Dự án đích:** tương tự AdventureWorks Sales Data Platform trong thư mục `project_code` người dùng cung cấp.  
**Stack:** AWS S3, Snowflake Storage Integration/Stage/Snowpipe, SQL, dbt Projects on Snowflake, GitHub Actions/OIDC.  
**Cách học:** brief -> contract -> chia task -> prompt -> review diff -> chạy -> kiểm số -> sửa -> lưu checkpoint.  
**Vị trí:** sau buổi 7 của khóa; nếu mở rộng thành 9 buổi thì chuyên đề là buổi 8, demo là buổi 9.

> Mục tiêu là dạy **cách làm việc với AI để xây ra dự án**, từ yêu cầu và file dữ liệu đến repository có pipeline, warehouse, tests và runbook. Mỗi vòng chỉ yêu cầu AI tạo một phần có thể kiểm chứng. Repo tham chiếu dùng để đối chiếu lời giải sau mỗi checkpoint; học viên làm trong repo sandbox mới.

## 1. Sản phẩm cuối và phạm vi thời gian

### 1.1 Kiến trúc cần xây đến

```text
7 CSV full snapshot, một delivery = một batch immutable
  -> AWS S3 batch_date=.../batch_id=.../
  -> Storage Integration + External Stage + CSV File Format
  -> 7 Snowpipe pipes, append-only RAW có metadata
  -> dbt sources + inventory + latest-observed completeness gate
  -> staging trim/cast/dedup, một batch nhất quán
  -> intermediate sales/order-line business logic
  -> snapshots SCD2 + dimensions + incremental fact
  -> tests + reconciliation + release decision
  -> Snowflake Task + GitHub Actions OIDC + runbook
```

Câu hỏi người dùng: bán được bao nhiêu, quantity/doanh thu/chi phí/lợi nhuận theo sản phẩm, kênh và khu vực; số trong warehouse có khớp nguồn hay không; batch nào tạo ra số này?

### 1.2 Đầu ra 180 phút và đầu ra dự án hoàn chỉnh

| Trong lớp, trên sandbox chuẩn bị trước | Sau lớp để đạt kiến trúc đầy đủ |
|---|---|
| Brief, data contract, source-to-target mapping | Hoàn thiện docs và mọi dataset/column còn thiếu |
| AI tạo một RAW/COPY mẫu, mở rộng 7 dataset có review | Kiểm IAM/integration/notification từ account riêng nếu có |
| Một staging tự viết bằng AI, mở rộng theo mẫu | Hoàn thiện tests, snapshot history và edge cases |
| Batch gate, join sales/order, fact/mart từ scaffold | Hoàn thiện 4 snapshots, 5 dimensions, incremental fact |
| Unique/not-null/channel và reconciliation có expected | CI/CD OIDC, Task, freshness và runbook sự cố |
| Một bài debug + prompt/diff/evidence | Replay, batch thiếu, dimension change và release review |

**Scaffold phải có sẵn:** hạ tầng sandbox và connection đã kiểm; quyền tối thiểu; skeleton project/dbt profile; mẫu dimension/snapshot để không dành cả buổi gõ code lặp. Học viên cần ghi file nào tự tạo, file nào AI tạo, file nào được cung cấp. Không coi đọc scaffold là tự xây từ đầu.

Nếu yêu cầu học viên tự dựng AWS/Snowflake/OIDC từ account trắng và viết toàn bộ model/test, tổ chức thành mini-project nhiều buổi; không hứa hoàn thành production trong 3 giờ. Cả chuỗi prompt dưới đây bao phủ dự án đầy đủ để tiếp tục sau lớp.

## 2. Tiền điều kiện và mục tiêu đo được

### Học viên cần có

- Kiến thức SQL: SELECT, JOIN, GROUP BY, CASE, NULL, PK/FK; hiểu CSV header, decimal và terminal.
- Repo học tập riêng, Git, AI coding assistant; biết xem diff và chạy command.
- Sandbox AWS/Snowflake được phân quyền, Snowflake CLI/connection đã kiểm. Không dùng app Next.js làm runtime pipeline này.
- [Hai snapshot nhỏ](adventureworks/README.md), hợp đồng nguồn và expected. Không cần workbook lớn để làm bài trong lớp.

### Sau buổi học, học viên chứng minh được

1. Viết brief và contract đủ cụ thể để AI không tự chọn grain, tiền hay CDC.
2. Chia kiến trúc thành task có file đầu ra và acceptance criteria.
3. Đưa đúng ngữ cảnh vào prompt: field/header mẫu, file liên quan, ràng buộc và test expected.
4. Review code AI ở ba lớp: cú pháp, business rule, vận hành/quyền.
5. Sinh và kiểm một luồng từ 7 CSV vào RAW, qua dbt đến fact/báo cáo.
6. Chứng minh số liệu khớp nguồn, replay không nhân dòng và batch thiếu bị chặn.
7. Dùng evidence để yêu cầu AI sửa nhỏ, không sửa dữ liệu hoặc tắt test cho xanh.
8. Giải thích phần đã chạy, phần chỉ kiểm tĩnh và phần cloud còn BLOCKED.

## 3. Kiến thức cần dạy trước mỗi vòng code

| Khái niệm | Giải thích trong dự án | Việc học viên làm ngay |
|---|---|---|
| ELT | Load CSV thô vào RAW rồi transform bằng dbt | Chỉ ra file tạo raw, staging và fact |
| Object storage / warehouse | S3 lưu file, Snowflake lưu/query table | Không gọi S3 bucket là warehouse |
| Stage / integration / pipe | Địa chỉ và format / quyền truy cập / lệnh ingest | Vẽ đường đi file và danh tính sử dụng |
| Full snapshot | Batch chứa toàn bộ 7 dataset, không có CDC operation | Không tạo filter updated_at tưởng tượng |
| Batch consistency | Fact không ghép sales của batch A với order/dim batch B | Chọn cùng batch ở tất cả staging |
| Grain | Một fact row = một SalesOrderLineKey | Đếm distinct key trước/sau join |
| Dimension / fact | Ngữ cảnh sản phẩm/khu vực/ngày và measures | Vẽ star schema tối giản |
| SCD2 | Giữ phiên bản dimension khi hệ thống quan sát thay đổi | Đổi tên product giữa 2 batch và kiểm version |
| Current vs as-of | Repo fact join version current, không order-time join | Mô tả đúng nghĩa báo cáo lịch sử |
| Reconciliation | So key hai chiều, count, quantity, totals và fields | Viết expected trước khi chạy |
| Idempotency | Replay không nhân business rows/totals | Chạy lại build; warehouse_updated_at có thể đổi |
| Publish | Build/test nhiều bảng không là một transaction toàn dự án | Ghi HOLD nếu test fail; candidate/promotion là nâng cao |

Một AI có thể viết SQL chạy được nhưng sai ở cả grain lẫn doanh thu. Người học phải quyết định quy tắc trước và tự kiểm con số sau.

## 4. Contract AdventureWorks phải khóa trước khi code

### 4.1 Bảy nguồn bắt buộc

| File | Nội dung / khóa |
|---|---|
| `sales_data.csv` | Measures và dimension keys; SalesOrderLineKey |
| `sales_order_data.csv` | Channel, sales order label; cũng theo SalesOrderLineKey |
| `customer_data.csv` | CustomerKey và thuộc tính customer |
| `reseller_data.csv` | ResellerKey và thuộc tính reseller |
| `product_data.csv` | ProductKey và thuộc tính product |
| `date_data.csv` | DateKey; dùng cho order/due/ship |
| `sales_territory_data.csv` | SalesTerritoryKey và region/country/group |

Đọc header thật trong `adventureworks/expected_headers.json`, không để AI đổi tên nguồn cho thuận tiện. RAW mapping `t.$n` phụ thuộc thứ tự cột; validator phải kiểm cả thứ tự header.

### 4.2 Quy tắc nghiệp vụ

- Fact grain: SalesOrderLineKey. Join sales/order bằng key này, không bằng số đơn.
- Quantity là số nguyên dương; tiền dùng DECIMAL/NUMBER, độ chính xác theo contract; fixture kiểm 4 chữ số thập phân, tolerance monetary 0,01 như repo tham chiếu. Không tự gán currency VND/USD nếu chưa có xác nhận.
- Extended Amount ≈ quantity x unit price; Total Product Cost ≈ quantity x product standard cost.
- **Sales Amount từ nguồn là authoritative.** Discount = extended - sales; gross profit = sales - cost.
- Không dùng `extended * (1-discount_pct)` để ghi đè Sales Amount. Repo ghi dữ liệu gốc có 3.282 record không tuân công thức đó; bài học nhỏ cũng cố ý có trường hợp lệch.
- Internet: customer key thật, reseller key -1. Reseller: reseller key thật, customer -1. Unknown/not-applicable member phải có trong dimension.
- Ship date có thể NULL; order/due bắt buộc. Postal code giữ string để không mất số 0 đầu.
- Mỗi batch đúng 7 file non-empty ở path bất biến; correction xuất batch mới đầy đủ. Không suy luận hard delete chỉ vì key biến mất.
- Snapshot dùng check strategy vì không có reliable updated_at. Lịch sử là system-observed; current join của repo không tương đương lịch sử tại ngày order.

### 4.3 Expected của fixture trong giáo án

| Metric | Batch 1 | Batch 2 |
|---|---:|---:|
| Fact rows / distinct key | 3 / 3 | 4 / 4 |
| Quantity | 6 | 7 |
| Extended amount | 650,0000 | 750,0000 |
| Total product cost | 390,0000 | 450,0000 |
| Sales amount | 600,0000 | 690,0000 |
| Discount amount | 50,0000 | 60,0000 |
| Gross profit | 210,0000 | 240,0000 |

Batch 2 là snapshot đầy đủ: line 1001 sales 180 -> 170, thêm 1004 sales 100, product 101 đổi tên. Vì vậy tổng sales tăng 90, không phải append tổng cả hai batch thành 1.290. Baseline workbook lớn trong repo chỉ dùng khi có đúng export gốc, không hard-code vào test mọi batch.

## 5. Timeline 180 phút - dạy bằng chu kỳ vibe code

| Phút | Giảng viên | Học viên giao việc cho AI | Checkpoint |
|---|---|---|---|
| 00-10 | Demo repo đích và báo cáo, chỉ đường đi một row | Viết brief 5 dòng, 3 câu hỏi business | Mục tiêu dự án |
| 10-25 | Grain, snapshot, KPI và nguồn | Prompt 01: phân tích contract/mapping, chưa code | Contract được review |
| 25-40 | Cách chia task/file, acceptance criteria | Prompt 02: scaffold/backlog, xem diff | Cây thư mục và task nhỏ |
| 40-60 | Stage/RAW/COPY, mở sandbox sẵn | Prompt 03-04: tạo mẫu sales rồi mở rộng 7 nguồn | 7 RAW có rows/metadata |
| 60-80 | Batch gate và staging, TRY cast/NULL | Prompt 05-06: gate + stg_sales, mở rộng mẫu | Cùng complete batch |
| 80-90 | Chẩn đoán query fail/rơi row | Query count/key, lưu context/diff | Evidence trước nghỉ |
| 90-105 | Nghỉ | Nghỉ | Giữ repo/DB sandbox |
| 105-125 | Join và star schema; dùng dim scaffold | Prompt 07: intermediate/fact/KPI | 3 fact; sales 600 |
| 125-145 | Test và reconciliation | Prompt 08: unique/channel/key/totals; chạy replay | Kiểm không nhân dòng |
| 145-163 | Review một task đang xây và giao một phép kiểm khó | Prompt 09: tối đa 3 giả thuyết, sửa theo evidence | Một follow-up có evidence + retest |
| 163-173 | SCD2/CI/CD/Task qua file mẫu | Lập kế hoạch với Prompt 10-12, phân phần sau giờ | Backlog dự án đầy đủ |
| 173-180 | Demo 60 giây và exit ticket | Chỉ prompt -> diff -> query -> expected/actual | Nộp evidence và giới hạn |

Trong 20 phút ingestion, sandbox phải sẵn integration/stage/notification. Nếu không có, dùng RAW fixture giảng viên đã ingest để tiếp tục dbt; ghi ingestion của học viên BLOCKED, không đánh đã tự thiết lập Snowpipe. Batch 2/SCD2 đầy đủ có thể hoàn thiện sau giờ; tất cả nhóm phải biết expected và kế hoạch kiểm.

## 6. Quy trình làm việc với AI áp dụng cho mọi task

### Một task gồm sáu phần

```text
BỐI CẢNH: bài toán và bước hiện tại
INPUT: header thật, sample giả, file/code liên quan
YÊU CẦU: hành vi cần đạt
RÀNG BUỘC: stack, quyền, grain, không đổi contract
OUTPUT: file cụ thể/diff nhỏ, giải thích
KIỂM CHỨNG: command/query, expected, failure case
```

### Chu kỳ bắt buộc

1. Người học viết expected và giới hạn task.
2. Yêu cầu AI phân tích/đưa kế hoạch; kiểm giả định trước khi nhận code.
3. AI tạo/sửa số file nhỏ đã chỉ định, giải thích logic chính.
4. Người học đọc diff: có đổi contract, tắt test, hard-code hay thêm quyền quá rộng không?
5. Chạy kiểm tĩnh và runtime phù hợp; lưu actual/log, không chỉ nhìn dấu xanh của editor.
6. Nếu sai: gửi evidence đã lọc, kiểm giả thuyết và sửa nhỏ. Nếu đạt: lưu checkpoint/commit rồi chuyển task.

Với cloud command có tác dụng tạo tài nguyên hoặc chạy warehouse, học viên hiểu role/schema/cost trước khi chạy trong sandbox đã cấp. Không dán secret, trust external ID riêng của tổ chức, dữ liệu khách hàng vào prompt/log bài nộp.

## 7. Chuỗi prompt xây dự án từ đầu đến bàn giao

Các prompt sau là kịch bản dùng ngay. Điền placeholder theo repo mới; AI phải hỏi lại điểm chưa rõ hoặc ghi assumption để người học chốt. Không yêu cầu AI tạo toàn repo trong một lần.

### Prompt 01 - Chuyển brief thành contract và mapping

**Input cấp cho AI:** mô tả business, 7 header thật, 3-5 row giả mỗi file, expected fixture.

```text
Bạn là data engineer hướng dẫn tôi xây AdventureWorks Sales Data Platform.
Đích: S3 -> Snowpipe -> Snowflake RAW -> dbt -> star schema -> reconciliation.
Nguồn gồm đúng 7 CSV full snapshot; không có CDC/reliable updated_at.
Đây là header/sample và expected: [DÁN].
Grain fact = SalesOrderLineKey; sales_order_data cũng theo line.
Sales Amount là authoritative; discount = extended-sales; profit = sales-cost.
Internet dùng reseller=-1; Reseller dùng customer=-1. Không tự chọn currency.
Chưa viết code. Hãy trả:
1) data contract; 2) source-to-target mapping; 3) star schema;
4) câu hỏi/assumption; 5) 8 failure modes và tiêu chí kiểm.
Đánh dấu unknown và phân biệt source fact với suy luận.
```

**Review:** nếu AI tạo fact một row/order, tự thêm CDC hoặc đổi doanh thu theo discount pct, yêu cầu sửa thiết kế trước khi code.  
**Đầu ra:** `docs/business-definitions.md`, `docs/source-to-target-mapping.md` trong repo học tập.

### Prompt 02 - Scaffold và backlog có checkpoint

```text
Dựa trên contract đã chốt, lập implementation plan và tạo skeleton repo.
Cấu trúc: aws/iam; snowflake/{00_bootstrap,01_storage,02_raw,03_ingestion,
05_reconciliation,06_orchestration}; dbt/{models/{staging,intermediate,marts,
audit},macros,snapshots,tests}; scripts; docs; .github/workflows.
Mỗi task có input, file output, dependency, acceptance command/query và expected.
Tạo README, .gitignore, docs/ai-usage-log.md, skeleton dbt config trước.
Chưa viết tất cả model hoặc tạo cloud resource. Chưa thêm credential.
Giữ riêng dev/prod/snapshots. Pin version chỉ sau khi kiểm runtime được hỗ trợ.
```

**Review:** local validation không được nhận là cloud build; skeleton còn TODO không được gọi done.  
**Checkpoint:** cây thư mục, contract, task board; người khác hiểu được thứ tự triển khai.

### Prompt 03 - Sinh hạ tầng và một RAW table theo least privilege

```text
Chỉ tạo template bootstrap/integration/stage và SALES_RAW để review.
Đây là header sales_data.csv đúng thứ tự: [DÁN HEADER].
RAW giữ field nghiệp vụ dạng VARCHAR; thêm source_file, source_row_number,
source_file_content_key, source_file_last_modified, ingested_at.
Tách role ingest / transform-dev / transform-prod / reader / CI-CD.
AWS S3 dùng IAM role + Snowflake storage integration, không access key trong SQL.
ARN/bucket/account dùng placeholder có giải thích chỗ điền.
Trả file, thứ tự chạy, role cần, query kiểm LIST stage/SELECT raw,
và cách chẩn đoán access denied. Không chạy template còn placeholder.
```

**Thực hành:** học viên review mapping field và metadata; admin/giảng viên chuẩn bị integration ngoài giờ. Mở rộng 6 raw table theo header thật, không nhân bản nhầm số cột.  
**Checkpoint:** raw schema đã có; stage `LIST` thấy đúng path trong sandbox.

### Prompt 04 - Sinh Snowpipe/COPY và mở rộng 7 dataset

```text
Dựa vào SALES_RAW và CSV format/stage hiện tại, tạo SALES_PIPE AUTO_INGEST.
COPY phải có target column list và mapping t.$1...t.$15 đúng header.
Thêm METADATA$FILENAME, FILE_ROW_NUMBER, FILE_CONTENT_KEY,
FILE_LAST_MODIFIED, START_SCAN_TIME theo kiểu target.
Pattern chỉ nhận sales_data.csv; ON_ERROR=ABORT_STATEMENT.
Trả query kiểm pipe status/COPY history/raw count và expected fixture.
Giải thích cần S3 event notification đến queue Snowflake cấp, không coi
CREATE PIPE là đã ingest. Sau mẫu sales được review mới mở rộng 6 pipe còn lại.
```

**Kiểm:** 7 file một batch path mới, raw count khớp từng file, source_file có batch ID. Sales count 3 chưa đủ: order/customer/reseller/product/date/territory cũng phải có.  
**Review:** dedup file của ingestion không thay PK/business-key dedup; không dùng force reload tùy tiện.

### Prompt 05 - Batch inventory và gate

```text
Tạo macros/extract_batch_metadata.sql và các model inventory/status/latest.
Path: adventureworks/batch_date=YYYY-MM-DD/batch_id=YYYYMMDDTHHMMSSZ/<file>.
Mỗi batch cần đúng 7 dataset non-empty, đúng một source file/dataset.
Mọi staging dùng cùng batch. Chặn build nếu latest observed chưa complete;
không tự lùi batch cũ và không trộn theo latest từng bảng.
Giai đoạn này bám reference: latest observed sort last_ingested_at DESC,
batch_id DESC. Nêu rủi ro backfill đến muộn và đề xuất manifest sau.
Trả tests: 7/7 pass, 6/7 fail, unbatched fail, không có batch fail,
và query liệt kê dataset thiếu. Ưu tiên thông báo lỗi dễ hiểu.
```

**Kiểm:** thử batch thiếu trên bản sandbox riêng; gate phải chặn trước việc transform downstream. Không chỉ kiểm `COUNT(*)=7` nếu bảy dòng cùng một dataset.  
**Checkpoint:** đọc được inventory và giải thích vì sao batch được chọn.

### Prompt 06 - Một staging mẫu, sau đó mở rộng

```text
Chỉ tạo stg_sales.sql và tests YAML tương ứng theo RAW/contract hiện có.
Dùng source()/ref(), chọn batch từ gate. Convert key/decimal bằng TRY;
ship_date_key rỗng thành NULL, tiền 4 decimals, discount pct 6 decimals.
Giữ batch/source/row/ingested metadata. Không COALESCE giá trị lỗi thành 0.
Dedup theo line key trong batch với thứ tự metadata xác định như reference.
Bổ sung not-null/range tests để TRY NULL không âm thầm đi qua.
Duplicate key nguồn phải bị source validation phát hiện; đừng gọi dedup
là bằng chứng mọi duplicate xung đột đều hợp lệ.
Trả SQL, YAML, query actual và case số sai/ship date NULL.
Chưa viết fact hoặc snapshot.
```

**Mở rộng:** stg_order/customer/reseller/product/date/territory; trim string, postal code text; sources YAML và freshness config. Freshness sẽ chạy riêng, không chỉ khai báo.  
**Checkpoint:** 3 sales, 3 orders, keys cover nhau, invalid conversion có test fail.

### Prompt 07 - Business join và fact/dim

```text
Tạo int_sales_order_line theo mapping, join sales/order đúng line key.
Sales Amount giữ nguyên; derive discount/profit. Tạo dim_date theo nguồn
và vai trò order/due/ship. Dùng scaffold dimensions hiện có.
Fact grain line key, incremental merge unique_key=sales_order_line_key.
Bám reference: join dimension version is_current, không as-of order date.
Giữ batch và lineage file, source_ingested_at, warehouse_updated_at, hash.
Không tự xử lý delete key vắng nguồn. Đưa query key coverage để phát hiện.
Trả query expected batch 1: rows=3, quantity=6, sales=600,
cost=390, discount=50, profit=210; kiểm theo channel và từng key.
Giải thích chỗ INNER JOIN có thể làm rơi fact và test phát hiện.
```

**Kiểm:** Internet sales 330, Reseller 270; tổng 600. Replay business rows/totals giữ nguyên dù timestamp load thay đổi.  
**Lưu ý:** trước khi triển khai snapshots, dimensions current đơn giản chỉ là mốc tạm; bài đầy đủ phải hoàn tất Prompt 10 và cập nhật lineage version keys.

### Prompt 08 - Tests và đối soát độc lập

```text
Tạo tests/YAML và audit_reconciliation_summary, chưa sửa dữ liệu.
Kiểm unique/not-null fact key; accepted channel; customer/reseller -1 rule;
sales/order key coverage hai chiều; FK; date sequence; measures/ranges.
Audit source vs fact: count, distinct grain, quantity, extended, cost, sales,
key coverage hai chiều và hash trường quan trọng. Decimal tolerance 0.01.
Không dùng công thức discount pct làm blocking test. Không hard-code
baseline workbook lớn cho batch nhỏ hoặc batch tương lai.
Gate fail khi audit có FAIL, rỗng, metric thiếu/NULL hoặc batch không đúng.
Trả expected độc lập từ fixture và lệnh chạy từng nhóm.
Giải thích scope hash; không nói hash đã chứng minh mọi cột đều khớp.
```

**Kiểm:** batch 2 full snapshot -> 4 rows, sales 690; replay không thành 7 rows/1.290 sales. Audit phải phát hiện join rơi key chứ không chỉ tiền.  
**Review:** SQL test dbt trả row vi phạm; không viết query chỉ hiển thị chữ PASS/FAIL rồi coi 0-row semantics đã đúng.

### Prompt 09 - Debug với bằng chứng, không viết lại dự án

```text
Mục tiêu: [VD sales=600 và rows=3]. Actual: [ĐIỀN].
Tôi chạy [COMMAND] tại target/schema [ĐIỀN], batch [ID].
Log/query/sample giả và diff liên quan: [DÁN ĐÃ LỌC].
Nêu tối đa 3 giả thuyết, mỗi giả thuyết một query phân biệt.
Chưa sửa trước khi kiểm evidence. Không tắt tests/gate hoặc sửa expected
cho khớp code. Khi xác định nguyên nhân, trả diff nhỏ nhất, giải thích
và retest lỗi gốc + replay + reconciliation. Chỉ sửa file liên quan.
```

**Tình huống dạy:** thiếu member -1, join theo order number, cast postal code, tự tính lại sales, hoặc batch 6/7. Người học phải chỉ ra nguyên nhân và chứng minh sửa, không chỉ nói “AI đã fix”.

### Prompt 10 - SCD2 cho bốn dimensions

```text
Tạo snapshots customer/reseller/product/territory từ staging.
Nguồn không có reliable updated_at: dùng check strategy, unique natural key,
check_cols chỉ thuộc tính business cần theo dõi, không gồm ingest timestamp.
Hard deletes ignore theo contract; dev và prod snapshot schema tách nhau.
Dim xuất version key=dbt_scd_id, valid_from/to, is_current.
Fact tiếp tục join current theo reference. Nêu rõ đây không phải historical
as-of fact; thay đổi version có thể gán lại fact cũ khi merge snapshot đầy đủ.
Test một current version/natural key, không overlap, replay không sinh
version mới nếu business fields không đổi. Product101 đổi tên ở batch2
phải có 2 versions sau khi chạy lần lượt batch1 rồi batch2.
```

**Review:** snapshot bắt đầu từ thời điểm build quan sát, không hồi dựng ngày đổi tên thật. Không đưa `ingested_at` vào check_cols khiến mỗi run thành phiên bản mới.  
**Nâng cao:** muốn fact as-of cần nguồn lịch sử/effective time và contract mới, không đổi join một cách tùy tiện.

### Prompt 11 - Orchestration, freshness và recovery

```text
Soạn Snowflake Task template chạy dbt project mỗi 60 phút và runbook.
Chỉ enable sau build/reconciliation sandbox đạt. Kiểm quyền CREATE/EXECUTE TASK.
Freshness phải là bước được gọi riêng; build không tự chạy freshness.
Nêu xử lý run chồng, batch đang upload, late backfill, load failure,
role lỗi, test fail, thiếu audit. Có owner và query evidence cho mỗi incident.
Không hứa transaction toàn bộ dbt project. Đề xuất candidate schema + gate
+ controlled promotion nếu cần giữ nguyên bản báo cáo khi build fail.
Rollback code khác rollback data; mô tả forward fix/clone/time travel
như phương án cần kiểm riêng, không chạy tự động trên môi trường thật.
```

**Checkpoint:** runbook phân biệt pipe đang ingest với dbt task đang transform; lịch mỗi giờ không có nghĩa dữ liệu freshness đảm bảo một giờ.  
**Sau giờ:** chủ động test một scheduled run trên sandbox, lưu task/query ID, suspend sau lab nếu không cần nữa.

### Prompt 12 - CI/CD OIDC và bàn giao

```text
Tạo .github/workflows/dbt-ci.yml và dbt-cd.yml theo target đã chốt.
CI PR: static/source checks khi có fixture, native deploy/build target dev,
results + evidence; isolate schema/snapshot theo PR hoặc mô tả giới hạn rõ.
CD main: deploy approved code -> build -> reconciliation gate -> Task update.
Dùng OIDC/trust theo repo/branch/environment, không PAT/password trong YAML.
Cấu hình account/database/schema qua secrets/vars phù hợp; pin runtime
sau khi kiểm CLI/account support, không tự dùng version ví dụ chưa xác minh.
Nêu rõ workflow không thay thế AWS/storage bootstrap một lần.
Tạo README, mapping, ai-usage-log, operations-runbook, known limitations.
Chỉ dùng placeholder; chưa push/trigger workflow trước khi config hoàn chỉnh.
```

**Review:** PR dev không được dùng prod profile/snapshots; log không lộ token; workflow xanh phải có runtime build chứ không chỉ static check. Các placeholder không phải thông tin credential đã cấu hình.

## 8. Chạy và kiểm chứng: lệnh nào cho mục đích nào?

### Offline bắt buộc trước cloud

Từ `buoi_data_pipeline/adventureworks` của bộ giáo án:

```bash
python3 check_fixture.py
```

Lệnh chỉ kiểm fixture/contract và số tay, không build Snowflake. Trong repo học tập mới, yêu cầu AI tạo source validator tương đương rồi chạy với đường dẫn batch 1/2. Không copy đường dẫn repo của giảng viên vào bài nộp cá nhân.

### Sandbox đã chuẩn bị

Trong repo học tập mới đã cấu hình native runtime:

```bash
python3 scripts/static_validate.py
python3 scripts/validate_source_files.py <csv_dir>
snow --version
snow connection test --connection aw-dev
snow dbt deploy AW_ANALYTICS_DEV --source ./dbt --default-target dev --connection aw-dev
snow dbt execute --connection aw-dev AW_ANALYTICS_DEV build --target dev
```

Command là mẫu theo native CLI, phải kiểm `snow dbt deploy --help`, `snow dbt execute --help` và version runtime trong account trước lớp. Deploy có thể dùng context database/schema của connection; xác nhận object được tạo ở nơi nào. Đặt `--dbt-version` sau khi chọn phiên bản đã kiểm, không mặc định pin reference luôn được hỗ trợ.

Kiểm source freshness bằng lệnh native execute được phiên bản CLI hỗ trợ tương ứng `source freshness --target dev`; kiểm help và lưu output riêng. Không dùng `build PASS` để kết luận freshness PASS. Tham khảo [dbt freshness](https://docs.getdbt.com/docs/deploy/source-freshness).

### Query sau build (đổi DEV đúng schema sandbox)

```sql
SELECT COUNT(*) AS fact_rows,
       COUNT(DISTINCT sales_order_line_key) AS distinct_keys,
       SUM(order_quantity) AS quantity,
       SUM(sales_amount) AS sales,
       SUM(total_product_cost) AS cost,
       SUM(discount_amount) AS discount,
       SUM(gross_profit_amount) AS profit
FROM ADVENTUREWORKS_DE.DEV.FCT_SALES_ORDER_LINE;

SELECT channel, COUNT(*) AS fact_rows, SUM(sales_amount) AS sales
FROM ADVENTUREWORKS_DE.DEV.FCT_SALES_ORDER_LINE
GROUP BY channel;

SELECT * FROM ADVENTUREWORKS_DE.DEV.AUDIT_RECONCILIATION_SUMMARY;
```

Query tổng đúng vẫn chưa đủ: kiểm key coverage và một line cụ thể. Với repo mới đã đổi tên schema/model, cập nhật truy vấn theo manifest thật. Expected cần lấy từ fixture tay trước khi đọc actual.

## 9. Làm sao học viên hiểu code AI tạo?

Mỗi checkpoint yêu cầu giải thích bằng lời và chỉ vào code:

- File nào đọc raw, file nào chọn batch, file nào biến đổi business?
- Một row sales 1001 được ghép với row nào ở order và dimension?
- Vì sao -1 không bị drop? Ship date NULL có làm rơi row không?
- Chỗ nào bảo vệ không merge trùng? Có hỗ trợ delete chưa?
- Nếu TRY cast ra NULL, test nào phát hiện? Nếu test fail, dữ liệu đã đổi phần nào?
- Vì sao source và target đều 600 chưa chứng minh mọi line đúng?
- Mã nào người học tự kiểm, phần nào AI gợi ý, phần nào chưa chạy?

Không yêu cầu thuộc lòng toàn bộ IAM/SQL. Yêu cầu hiểu quyết định tác động số liệu và biết tìm evidence đúng nơi.

## 10. Bàn giao và đánh giá

**Trong lớp:** brief/contract, task board, tối thiểu ba vòng prompt-diff-test, source/static checks, build/query evidence nếu sandbox hoạt động, một bug report và backlog hoàn thiện.

**Dự án đầy đủ sau lớp:** 7 raw/pipe/staging, gate, business join, 4 snapshots/5 dimensions/fact merge, tests/audit, Task/CI/CD templates hoặc runtime evidence theo phần đã chạy, mapping/runbook/AI log.

Không đánh “đã hoàn thành cloud” nếu chỉ có template. Thiếu tài khoản ghi BLOCKED runtime, vẫn chấm được thiết kế và kỹ năng vibe code. Tám chặng bài tập liên thông, tiêu chí và đề bài chi tiết ở [phiếu thực hành](BAI-TAP-VA-PHIEU-THUC-HANH.md); giảng viên dùng [hướng dẫn](HUONG-DAN-GIANG-VIEN-VA-GOI-Y-XU-LY.md).

## 11. Tài liệu đối chiếu

[Đối chiếu dự án tham chiếu](DOI-CHIEU-DU-AN-THAM-CHIEU.md) chỉ rõ file/hành vi/giới hạn đã đọc. Các tài liệu chính thức dùng khi kiểm kỹ thuật:

- [Snowpipe S3 auto-ingest](https://docs.snowflake.com/en/user-guide/data-load-snowpipe-auto-s3).
- [Snowflake CLI deploy dbt](https://docs.snowflake.com/en/developer-guide/snowflake-cli/command-reference/dbt-commands/deploy).
- [dbt snapshots](https://docs.getdbt.com/docs/build/snapshots).

Repo tham chiếu là lời giải để học cách phân rã và kiểm chứng; không phải mọi lựa chọn trong đó đều là yêu cầu bắt buộc cho mọi công ty/dữ liệu.
