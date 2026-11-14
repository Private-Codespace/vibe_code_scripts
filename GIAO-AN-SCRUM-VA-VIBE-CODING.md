# GIÁO ÁN SCRUM VÀ VIBE CODING — EPIC → FEATURES → US → AC → PHÂN LOẠI → ESTIMATE

**Thời lượng:** 420 phút học, chưa tính nghỉ.  
**Đối tượng:** học viên biết dùng AI hỗ trợ xây ứng dụng; làm nhóm 4–5 người.  
**Trọng tâm:** dùng vibe coding để sinh, kiểm tra và tinh chỉnh backlog theo quy trình người học yêu cầu.  
**Quy trình bắt buộc:** Epic → AI sinh Features → AI sinh User Stories → AI sinh Acceptance Criteria → AI phân loại từng US thành Analyze/Dev/Test → AI đề xuất story point kèm lý do → nhóm phản biện và chốt → Sprint thực hành.

> AI tạo bản nháp ở mỗi bước. Học viên chịu trách nhiệm làm rõ giả định, sửa đầu ra và xác nhận với người phụ trách. Phân loại áp dụng cho từng US, không gán nhãn cho từng câu AC. Nếu một US có nhiều loại công việc, ghi loại chính và các task phụ; chỉ estimate một lần ở mức US.

## 1. Bộ tài liệu và kết quả học tập

| Tài liệu | Cách dùng |
|---|---|
| Giáo án này | Timeline, kiến thức, prompt và cách dẫn lab |
| [Phiếu thực hành](BAI-TAP-VA-PHIEU-THUC-HANH-SCRUM.md) | Bài ngân hàng 180 phút: Epic/Feature/US/AC, loại, point và phản biện |
| [Hướng dẫn giảng viên](HUONG-DAN-GIANG-VIEN-VA-GOI-Y-XU-LY.md) | Gợi ý xử lý, ước lượng và rubric |
| [Case ngân hàng](APPLYING-SCRUM-DATABASE-QUERY-AGENT-BANKING.md) | Ví dụ đã điền 3 Features, 6 US, 18 AC; loại, AI SP và nhóm SP kèm lý do |

Sau buổi học, học viên có thể:

1. Viết Epic có người dùng, vấn đề, giá trị, phạm vi và giả định.
2. Yêu cầu AI sinh Features là các khả năng sản phẩm, phát hiện feature trùng hoặc ngoài phạm vi.
3. Từ Feature, sinh các US nhỏ, có quan hệ truy vết và giá trị/đầu ra rõ.
4. Sinh AC kiểm chứng được, có happy path, validation, quyền và failure path khi liên quan.
5. Phân loại mỗi US vào Analyze, Dev hoặc Test theo kết quả chính.
6. Đề xuất point theo độ phức tạp, khối lượng, bất định và phụ thuộc; nêu lý do.
7. Phản biện estimate của AI, ghi point nhóm chốt và lý do điều chỉnh.
8. Chọn US để tạo Increment dùng được, chạy kiểm tra và điều chỉnh từ phản hồi.

## 2. Quy ước để không nhầm backlog với kế hoạch tuần tự

| Thành phần | Ý nghĩa trong buổi học | Ví dụ |
|---|---|---|
| Epic | Bài toán lớn tạo giá trị | Xây ứng dụng đặt phòng họp |
| Feature | Khả năng sản phẩm phục vụ Epic | Đặt phòng không trùng |
| US | Một kết quả nhỏ để thực hiện/kiểm chứng | Nhân viên đặt phòng còn trống |
| Acceptance Criteria | Điều kiện cụ thể để chấp nhận US | Hai lịch liền kề được phép |
| Loại | Mục đích công việc chính của US | Analyze / Dev / Test |
| Story point | Độ lớn tương đối do nhóm ước lượng | 3 SP vì 4 quy tắc biên đã rõ |
| Definition of Done | Chuẩn chất lượng chung | Tích hợp, AC đạt, regression và evidence |

Epic, Feature, nhãn công việc và story point là cách tổ chức của lớp, không phải artifact bắt buộc của Scrum. Product Backlog, Sprint Backlog và Increment vẫn là ba artifact; mục tiêu và DoD vẫn phải rõ. [Nguồn: Scrum Guide](https://scrumguides.org/scrum-guide.html)

Analyze/Dev/Test là **nhãn cho mục đích chính**, không phải ba đội hay ba Sprint nối đuôi. Một US Dev vẫn cần phân tích và kiểm tra để Done. US Test riêng dành cho một đầu ra kiểm chứng độc lập, như bộ đánh giá hồi quy dùng lại; không dùng để chuyển việc kiểm thử thiết yếu của US Dev sang Sprint sau. US Analyze có thể là PBI hỗ trợ làm rõ/rủi ro, không tự thân là Increment phần mềm.

## 3. Quy tắc phân loại

| Loại | Dùng khi đầu ra chính là | Không dùng khi |
|---|---|---|
| Analyze | Quy tắc được xác nhận, oracle, quyết định dữ liệu hoặc bằng chứng giảm bất định | Chỉ đổi tên task code thành phân tích |
| Dev | Khả năng phần mềm được triển khai, tích hợp, dùng và kiểm tra được | Chỉ mới có thiết kế hoặc code chưa chạy |
| Test | Bộ kiểm chứng/đánh giá độc lập có expected, cách chạy và báo cáo | Muốn hoãn test AC của US Dev |

Nếu một US quá rộng và chứa ba đầu ra độc lập, tách thành US nhỏ có AC riêng. Nếu chỉ có các bước bình thường để hoàn thành một chức năng, giữ một US Dev và chia task Analyze/Dev/Test bên trong; không nhân ba point cho cùng công việc.

## 4. Quy tắc estimate và lý do

Thang dùng trong lớp: **1, 2, 3, 5, 8, 13 SP**. Mốc 3 SP tham chiếu: một luồng nhỏ trong ứng dụng đã có sẵn, dữ liệu/quy tắc rõ, ít điểm tích hợp, kiểm tra được bằng một bộ test nhỏ. Đây là mốc của nhóm trong bài tập, không so ngang giữa các nhóm.

| Point | Dấu hiệu tham khảo | Câu hỏi phản biện |
|---:|---|---|
| 1 | Một thay đổi rất nhỏ, ít nhánh, hạ tầng có sẵn | Có bỏ sót test/tích hợp không? |
| 2 | Phạm vi nhỏ, một vài điều kiện rõ | Có phụ thuộc chưa được xác nhận không? |
| 3 | Một đầu ra nhỏ, vài quy tắc, rủi ro thấp-vừa | Có tương đương US mốc không? |
| 5 | Nhiều nhánh hoặc nhiều lớp phối hợp | Có thể tách theo giá trị không? |
| 8 | Nhiều tích hợp/quyền/biên, rủi ro đáng kể | Cần làm rõ gì trước khi nhận? |
| 13 | Quá lớn hoặc bất định cao với nhóm | Tách US hoặc làm rõ trước khi đưa Sprint |

Mỗi US phải có: **AI point đề xuất; lý do theo độ phức tạp, khối lượng, bất định/phụ thuộc; giả định; độ tin cậy; point nhóm chốt; lý do chênh lệch**. Không suy ra Analyze luôn 2, Dev luôn 5, Test luôn 3. Không mặc định 1 SP = 1 giờ/ngày. Thời gian chờ cấp quyền là phụ thuộc/rủi ro cần quản lý, không tự cộng point theo số ngày chờ.

Point là ước lượng tương đối, không phải điểm năng lực cá nhân hoặc cam kết giao hàng. Nếu đã estimate ở US thì không cộng thêm point Feature/Epic/task. US Dev tính đủ test cần thiết để đạt AC/DoD; US Test riêng chỉ tính phạm vi kiểm chứng bổ sung được mô tả rõ.

## 5. Chuẩn bị

- Giảng viên chuẩn bị Epic đặt phòng, starter chạy được, bảng backlog và fixture có lịch giao nhau.
- Học viên mở được dự án và công cụ AI trước giờ học; dùng dữ liệu giả.
- Mỗi nhóm có PO, SM và người làm phát triển/kiểm tra; Developers cùng tạo Increment.
- Có bản dự phòng để dùng nếu AI/mạng không khả dụng. Không dùng phần lớn giờ học cài công cụ.
- Với lớp đông, Review theo cặp nhóm song song.

## 6. Timeline 420 phút

Thời gian là phút học tích lũy, không tính nghỉ. Hai Sprint mô phỏng đều 95 phút, gồm cả các sự kiện.

| Phút | Hoạt động | Giảng viên làm gì | Học viên nộp gì |
|---|---|---|---|
| 000–020 | Khởi động | So sánh backlog AI sinh tốt/xấu | Baseline và lỗi phát hiện |
| 020–045 | Scrum và quy ước | Làm rõ mục tiêu, nhãn, point, DoD | Mốc estimate nhóm |
| 045–080 | Epic → Features | Đóng vai khách hàng, phản biện scope | Epic và 3–5 Features đã review |
| 080–125 | Features → US | Gợi ý chia nhỏ theo kết quả | 8–12 US có ID và Feature cha |
| 125–165 | US → AC | Làm mẫu happy/edge/failure | AC kiểm tra được cho US ưu tiên |
| 165–200 | Phân loại và estimate | Tổ chức phản biện point | Loại, AI SP, lý do, SP nhóm |
| 200–295 | Sprint mô phỏng 1 | Hỗ trợ, phát một thẻ thay đổi | Increment, evidence, Review/Retro |
| 295–390 | Sprint mô phỏng 2 | Đưa phản hồi, quan sát điều chỉnh | Increment cải tiến và backlog mới |
| 390–410 | Tình huống | Giao hai tình huống/nhóm | Quyết định sửa AC/loại/point |
| 410–420 | Tổng kết | Kiểm tra cá nhân, chốt bài | Hồ sơ và exit ticket |

## 7. Bước 1 — Epic → vibe code sinh Features

**Epic mẫu:** “Xây ứng dụng giúp nhân viên tự tìm và đặt phòng họp, giảm đặt trùng so với tin nhắn và bảng tính.”

Giảng viên cung cấp 4 phòng, một văn phòng, cùng múi giờ; bản đầu dùng một trình duyệt. Học viên hỏi thêm về người dùng, định nghĩa trùng, hủy lịch và giá trị ưu tiên.

```text
Epic: [nội dung]
Người dùng / vấn đề / giá trị: [...]
Ràng buộc và ngoài phạm vi: [...]
Hãy sinh 3–5 Features theo khả năng người dùng cần có.
Mỗi Feature: ID, tên, người dùng, giá trị, phạm vi, ngoài phạm vi,
giả định, phụ thuộc và lý do ưu tiên.
Không dùng frontend/backend/database làm tên Feature.
Chưa sinh US hoặc code. Tách rõ dữ kiện và điều cần xác nhận.
```

Features tham khảo: F01 xem phòng/lịch; F02 đặt phòng không trùng; F03 hủy và giải phóng khung giờ; F04 lọc phòng theo nhu cầu. Nhóm review và sửa trước khi dùng làm đầu vào bước sau.

## 8. Bước 2 — Features → vibe code sinh US

```text
Epic và Features đã review: [...]
Từ mỗi Feature, sinh các US/PBI nhỏ với ID truy vết về Feature cha.
Mỗi US có người nhận giá trị, nhu cầu/đầu ra, lợi ích,
phạm vi, ngoài phạm vi và phụ thuộc. Chưa estimate.
Nếu cần đầu ra phân tích/kiểm chứng độc lập, mô tả đầu ra đó rõ ràng.
Không bắt buộc tạo đủ ba loại cho mọi Feature nếu không có lý do.
Không để toàn bộ Analyze ở Sprint đầu và Test ở Sprint cuối.
```

Nhóm kiểm tra US có trùng, quá lớn, thuần task kỹ thuật hoặc thiếu người nhận kết quả không. Giữ quan hệ Epic → Feature → US khi sửa.

## 9. Bước 3 — US → vibe code sinh AC

```text
US đã review: [...]
Quy tắc nghiệp vụ / dữ liệu / quyền: [...]
Sinh AC với ID [US-ID]-AC01... theo Given/When/Then hoặc expected cụ thể.
Bao phủ luồng thành công, dữ liệu sai, trường hợp biên,
quyền/failure khi liên quan. Nêu dữ liệu và expected kiểm chứng được.
Không dùng "nhanh", "đẹp", "đúng" nếu không có tiêu chí rõ.
Nếu thiếu thông tin, đặt câu hỏi thay vì tự quyết định nghiệp vụ.
```

Ví dụ F02-US02: có lịch phòng A 09:00–10:00; đặt 09:30–10:30 bị từ chối; đặt 10:00–11:00 được phép; đặt cùng giờ phòng B được phép; refresh vẫn còn lịch hợp lệ. AC là yêu cầu riêng, DoD là chuẩn chung như tích hợp, regression và evidence.

## 10. Bước 4 — Phân loại → estimate kèm lý do

```text
Backlog và AC đã review: [...]
Mốc 3 SP và điều kiện năng lực/hạ tầng của nhóm: [...]
Với từng US, chọn một loại chính: Analyze, Dev hoặc Test.
Nêu lý do phân loại; nếu có task phụ, liệt kê nhưng không estimate trùng.
Đề xuất point từ 1,2,3,5,8,13 dựa trên:
độ phức tạp, khối lượng, bất định, phụ thuộc và test cần để Done.
Ghi giả định, độ tin cậy và điều gì có thể làm estimate thay đổi.
US 13 SP cần đề xuất cách chia nhỏ. Không quy đổi point ra ngày.
Xuất bảng: Feature ID, US ID, tên, loại, AC IDs, AI SP,
lý do SP, giả định, độ tin cậy. Để trống SP nhóm chốt.
```

**Ví dụ thảo luận, không phải định mức:**

| Feature | US | Loại | AI SP | Lý do |
|---|---|---|---:|---|
| F01 | Xác nhận cách hiển thị lịch/ngày | Analyze | 2 | Ít thuật ngữ, một stakeholder, chưa đổi code |
| F01 | Xem lịch phòng theo ngày | Dev | 3 | Nguồn có sẵn, một màn hình và ba trạng thái |
| F02 | Chốt quy tắc giao nhau và oracle | Analyze | 3 | Cần trường hợp liền kề/bao phủ và xác nhận nghiệp vụ |
| F02 | Tạo lịch có validation và chống trùng | Dev | 5 | Nhiều nhánh, lưu dữ liệu, tích hợp và kiểm tra biên |
| F02 | Bộ regression giao nhau dùng lại | Test | 3 | Bộ kiểm chứng độc lập nhiều fixture; không thay test AC của US Dev |
| F03 | Hủy lịch và đặt lại | Dev | 3 | Một chuyển trạng thái, ảnh hưởng kiểm tra trùng |

Sau AI, từng người tự chọn point, mở đồng thời, thảo luận chênh lệch rồi nhóm chốt. Nếu khác 3 và 8, hỏi về giả định và scope thay vì lấy trung bình. Lưu cả đề xuất AI và kết quả nhóm.

## 11. Hai Sprint thực hành — mỗi Sprint 95 phút

### 11.1 Cấu trúc cố định

| Hoạt động | Phút | Nội dung |
|---|---:|---|
| Planning | 15 | Goal, US phù hợp năng lực, kế hoạch và rủi ro |
| Thực hiện | 45 | Phân tích/triển khai/test/tích hợp; checkpoint 5 phút trong block |
| Review | 20 | Người khác tự dùng, đối chiếu AC/DoD, cập nhật backlog |
| Retro | 15 | Một cải tiến có người theo dõi và cách đo |

Điểm kiểm tra 5 phút là mô phỏng. Trong làm việc thực tế, Daily Scrum là sự kiện 15 phút mỗi ngày làm việc của Developers. [Nguồn](https://www.scrum.org/resources/what-is-a-daily-scrum)

### 11.2 Sprint 1

**Goal gợi ý:** người dùng xem lịch và đặt phòng còn trống, không tạo lịch trùng trong phạm vi demo. Chọn lát cắt đủ nhỏ, gồm cả quyền/chất lượng liên quan. Không chỉ chọn US Analyze để có nhiều ticket Done.

Thẻ giữa Sprint: “Thêm báo cáo thống kê để demo đẹp.” Nhóm làm rõ giá trị, xem tác động mục tiêu; nếu nhận thì phải cập nhật Feature/US/AC, loại và estimate phù hợp, không chỉ sửa code.

### 11.3 Sprint 2

Phản hồi: người dùng cần hủy cuộc họp và đặt lại cùng khung giờ. AI đề xuất Feature/US/AC bổ sung hoặc sửa, nhóm review và estimate phần mới/chưa chọn. **Goal gợi ý:** hủy và giải phóng khung giờ để đặt lại được.

Không sửa point của US đã hoàn thành để làm đẹp số liệu. Với phần việc mới, giữ lịch sử thay đổi và nêu rõ phạm vi.

## 12. Definition of Done cho lab

- US Dev: AC đạt, tích hợp bản chung, regression liên quan đạt, dữ liệu còn sau refresh, hướng dẫn/giới hạn và evidence rõ.
- US Analyze: đầu ra phân tích/oracle được xác nhận bởi người phù hợp, câu hỏi chưa rõ được ghi; chưa có phần mềm thì không gọi đây là Increment phần mềm.
- US Test: có expected độc lập, cách chạy, kết quả thực tế và danh sách giới hạn; không báo PASS chưa chạy.
- Các đầu ra hỗ trợ có điều kiện hoàn thành riêng; DoD của Increment sản phẩm vẫn phải thỏa chất lượng chung.

## 13. Tình huống và chấm bài

| Tình huống | Học viên phải làm |
|---|---|
| AI sinh Feature “xây database” | Chuyển thành khả năng có giá trị, giữ task kỹ thuật ở trong US |
| US chứa đặt/hủy/lọc/báo cáo | Tách nhỏ, AC riêng, estimate lại từng phần |
| AC “đặt nhanh, dễ dùng” | Làm rõ người dùng, tác vụ, cách đo và expected |
| AI gán Analyze=2, Dev=5, Test=3 mọi nơi | Phản biện theo độ lớn, bất định và mốc tham chiếu |
| Dev ticket không test vì có Test ticket | Bổ sung test AC/DoD vào Dev; xác định đầu ra Test độc lập |
| Thay đổi yêu cầu giữa Sprint | Cập nhật chuỗi truy vết và thương lượng scope, giữ mục tiêu/chất lượng |

| Tiêu chí | Điểm tối đa |
|---|---:|
| Epic và Features có giá trị, đúng scope | 15 |
| US nhỏ, truy vết được và không trùng | 20 |
| AC cụ thể, có expected và trường hợp biên | 20 |
| Phân loại có lý do, không tạo silo | 10 |
| Point, giả định, lý do và phản biện nhóm | 20 |
| Increment, evidence, Review/Retro | 15 |
| **Tổng** | **100** |

**Bài nộp:** Epic; Features; US/AC; bảng loại/AI SP/lý do/SP nhóm; DoD; hai Sprint Goal; source/phiên bản và kết quả kiểm tra; feedback; log thay đổi và Retro. Không chấm theo số prompt hay tổng point hoàn thành.

**Exit ticket:** giải thích Feature khác task; sửa một AC mơ hồ; giải thích vì sao một US là 5 thay vì 3 SP.

## 14. Chuyển sang case ngân hàng

Dùng [Epic Database Query Agent](APPLYING-SCRUM-DATABASE-QUERY-AGENT-BANKING.md) để luyện cùng quy trình với dữ liệu, quyền và chỉ tiêu phức tạp hơn. Ví dụ đã điền khớp phiếu ngân hàng 180 phút: 3 Features, 6 US và 18 AC; mỗi US có loại, AI SP, nhóm SP minh họa, giả định và lý do giữ/đổi. Chỉ đối chiếu sau khi học viên tự làm bản nháp; các số liệu estimate và hoạt động nhóm không phải evidence thực thi.

Nếu chỉ có 180 phút gồm 15 phút nghỉ: mở đầu/lý thuyết 15; Epic→Feature 20; Feature→US 25; US→AC 25; phân loại/estimate 25; nghỉ 15; Sprint mô phỏng 45 (Planning 10, thực hiện 15, Review 10, Retro 10); tổng kết 10. Bản rút gọn ưu tiên quy trình backlog và một luồng starter rất nhỏ.
