# GIÁO ÁN SCRUM VÀ VIBE CODING — TỪ YÊU CẦU MƠ HỒ ĐẾN SẢN PHẨM CHẠY ĐƯỢC

**Thời lượng:** 420 phút học, chưa tính nghỉ; có thể chia thành hai buổi theo hướng dẫn cuối tài liệu.  
**Đối tượng:** học viên đã biết mở, chạy và chỉnh sửa một ứng dụng bằng AI; không yêu cầu lập trình thành thạo.  
**Quy mô:** 15–25 người; nhóm 5 người, gồm 1 Product Owner, 1 Scrum Master và 3 Developers.  
**Sản phẩm thực hành:** ứng dụng đặt phòng họp; phiên bản đầu dùng một trình duyệt và dữ liệu demo lưu cục bộ.  
**Đầu ra:** sản phẩm sử dụng được, backlog có thứ tự, mục tiêu hai Sprint, kết quả kiểm tra và một cải tiến có bằng chứng.

> Đây là giáo án thực hành được đề xuất cho khóa vibe coding. Các tình huống, số liệu và tên công ty là giả định phục vụ đào tạo. Hai Sprint 105 phút và điểm kiểm tra 5 phút là mô phỏng trong lớp; không phải lịch tổ chức Scrum mặc định cho doanh nghiệp. Tài liệu này không kèm starter code, tài khoản công cụ hoặc báo cáo test đã chạy.

## 1. Cách sử dụng bộ tài liệu

| Tài liệu | Người dùng | Mục đích |
|---|---|---|
| Giáo án này | Giảng viên và học viên | Mục tiêu, lý thuyết, timeline, lab, prompt và đánh giá |
| [Bài tập và phiếu thực hành](BAI-TAP-VA-PHIEU-THUC-HANH-SCRUM.md) | Học viên | Brief, dữ liệu, acceptance criteria, phiếu trống và bài tập mở rộng |
| [Hướng dẫn giảng viên](HUONG-DAN-GIANG-VIEN-VA-GOI-Y-XU-LY.md) | Giảng viên | Thông tin khách hàng, thẻ phát sinh, đáp án định hướng và rubric |

**Khối lượng bắt buộc:** một sản phẩm, hai Sprint, mỗi Sprint có mục tiêu, phần sản phẩm đã Done, Review và Retrospective. Không bắt buộc hoàn thành toàn bộ backlog. Phần bài tập tình huống: mỗi nhóm xử lý hai tình huống, không phải triển khai tất cả bằng code.

**Quan hệ với khóa hiện có:** có thể mang kiến thức brief, UI, CRUD, kiểm thử và deploy từ các buổi trước vào bài tập. Đây là chuyên đề riêng; không thay thế hay đánh số lại buổi 8 của lộ trình hiện tại. Không bắt buộc xây lại Auth, AI endpoint và deployment trong ngày học Scrum.

## 2. Mục tiêu đo được

Sau buổi học, học viên có thể:

1. Phỏng vấn khách hàng, phân biệt vấn đề, yêu cầu và giả định.
2. Viết một Product Goal hướng vào kết quả người dùng.
3. Tạo 8–12 Product Backlog items, sắp xếp và giải thích ba ưu tiên đầu.
4. Viết acceptance criteria có thể kiểm tra; thống nhất Definition of Done.
5. Lập Sprint Goal, chọn phạm vi và xây kế hoạch thực hiện khả thi.
6. Dùng AI triển khai từng phần nhỏ, kiểm tra kết quả thay vì chỉ tin thông báo của AI.
7. Ra quyết định khi có yêu cầu mới, lỗi nghiệp vụ hoặc thiếu năng lực thực hiện.
8. Thu phản hồi người dùng, cập nhật backlog và thử một cải tiến cách làm việc.

**Điều kiện đạt cốt lõi:** có ít nhất một luồng sử dụng được và đáp ứng DoD; có evidence kiểm tra; hai Sprint có mục tiêu và phản hồi; trạng thái Done phản ánh đúng chất lượng thực tế. Nhóm chưa có Increment vẫn nộp hồ sơ học tập, nhưng phải ghi rõ phần chưa đạt và kế hoạch hoàn thiện.

## 3. Chuẩn bị trước lớp

### 3.1 Giảng viên

- Chuẩn bị starter có khung ứng dụng chạy được, danh sách phòng mẫu và vị trí lưu dữ liệu; thử từ môi trường sạch.
- Giữ stack mà học viên đã dùng. Không đổi framework hoặc bắt tạo tài khoản mới giữa buổi.
- Chuẩn bị bản tốt và bản lỗi trên bản sao; có hướng khôi phục nếu AI sửa hỏng.
- Chạy thử lab với thời gian thực tế; chọn scope đủ nhỏ cho 50 phút phát triển mỗi Sprint.
- Chuẩn bị bảng công việc, đồng hồ, phiếu trống và thẻ phát sinh trong tài liệu giảng viên.
- Chuẩn bị phương án mất mạng: bản ứng dụng có lỗi chạy local để phân tích, viết test và sửa; ghi rõ việc không dùng được AI.
- Với trên 4 nhóm, bố trí trợ giảng hoặc tổ chức Review theo cặp nhóm song song. Không yêu cầu mọi nhóm demo nối tiếp trong 20 phút.

### 3.2 Học viên

- Trước lớp, mở dự án, chạy ứng dụng, đổi một dòng nội dung và nhìn thấy kết quả.
- Mang laptop; công cụ AI đã truy cập được; lưu lại phiên bản trước khi sửa.
- Biết xem Console và chạy hướng dẫn kiểm tra cơ bản của dự án.
- Dùng dữ liệu giả; không cần tài khoản khách hàng thật để làm lab.

### 3.3 Quy ước phạm vi

- Một văn phòng, một múi giờ, các lịch trong cùng ngày; nhập và so sánh thời gian thống nhất.
- Bản đầu là prototype trên một trình duyệt; local storage không phải cơ chế đồng bộ nhiều người dùng.
- Các khung giờ được hiểu dạng `[bắt đầu, kết thúc)`: lịch 09:00–10:00 và 10:00–11:00 không trùng.
- Giảng viên phát ngày demo D khi bắt đầu lab; dùng cùng ngày trong toàn bộ fixture và ghi ngày thực tế vào phiếu.
- Nếu ứng dụng đã có backend, giữ kiến trúc hiện có nhưng thu hẹp số chức năng. Không hạ tiêu chuẩn đang áp dụng chỉ để kịp giờ.

## 4. Kiến thức cốt lõi và cách dùng ngay

### 4.1 Trách nhiệm trong nhóm

| Trách nhiệm | Thực hiện trong lớp | Dấu hiệu cần điều chỉnh |
|---|---|---|
| Product Owner | Làm rõ giá trị, truyền đạt Product Goal, sắp xếp backlog | Nhận mọi yêu cầu, ép scope mà không trao đổi năng lực |
| Scrum Master | Hỗ trợ tự quản, giữ sự kiện hữu ích, giúp tháo gỡ trở ngại | Giao việc như cấp trên, đọc báo cáo thay cho nhóm |
| Developers | Cùng tạo Increment: phân tích, thiết kế, code, test, tích hợp | Mỗi người làm một phần nhưng không có luồng chung chạy được |

Developers không chỉ có người viết code. Scrum Team cùng chịu trách nhiệm tạo ra Increment hữu ích và có giá trị. Giữ nguyên vai hai Sprint, sau đó phản tư về cả ba trách nhiệm. [Nguồn: Scrum Guide](https://scrumguides.org/scrum-guide.html)

### 4.2 Ba trụ cột và năm giá trị

| Nội dung | Hành vi quan sát được |
|---|---|
| Minh bạch | Công khai lỗi, blocker và việc chưa kiểm tra |
| Kiểm tra | Người khác thử sản phẩm; đối chiếu với expected |
| Thích nghi | Sửa backlog hoặc cách phối hợp từ bằng chứng |
| Cam kết, tập trung | Bám mục tiêu chung, hỗ trợ nhau hoàn thành |
| Cởi mở, tôn trọng, can đảm | Nêu rủi ro sớm, lắng nghe phản biện, dám nói chưa Done |

[Nguồn lý thuyết: Scrum Guide](https://scrumguides.org/scrum-guide.html). Các hành vi trong bảng là ví dụ đào tạo do người soạn đề xuất.

### 4.3 Các tạo tác và cam kết tương ứng

| Tạo tác | Cam kết | Áp dụng |
|---|---|---|
| Product Backlog | Product Goal | Danh sách công việc có thứ tự hướng tới mục tiêu sản phẩm |
| Sprint Backlog | Sprint Goal | Mục tiêu, hạng mục được chọn và kế hoạch thực hiện |
| Increment | Definition of Done | Phần sản phẩm dùng được, đáp ứng chuẩn chất lượng |

[Nguồn: Scrum Guide](https://scrumguides.org/scrum-guide.html).

### 4.4 Những điểm tránh hiểu sai

- Sprint bao gồm Planning, phát triển, Daily Scrum, Review và Retrospective; không chỉ là giờ viết code.
- Sprint có độ dài cố định, tối đa một tháng. Trong lớp, hai Sprint mô phỏng đều dài 105 phút.
- Trong Sprint có thể làm rõ và thương lượng lại phạm vi với Product Owner; không thay đổi gây nguy hiểm cho Sprint Goal, không hạ chất lượng.
- Nếu Sprint Goal trở nên lỗi thời, chỉ Product Owner có quyền hủy Sprint. Không dùng việc hủy như cách thông thường để xử lý ước lượng sai.

[Nguồn: Scrum.org — Sprint](https://www.scrum.org/resources/what-is-a-sprint-in-scrum) và [Scrum Guide](https://scrumguides.org/scrum-guide.html).

User story, story point, bảng Kanban, mẫu ba câu hỏi và công cụ cụ thể không phải yêu cầu bắt buộc của Scrum. Trong lớp, chọn kỹ thuật giúp minh bạch và ra quyết định; không chấm theo việc dùng đúng một phần mềm.

## 5. Timeline 420 phút

Thời gian dưới đây tính theo phút học tích lũy, không bao gồm nghỉ. Có thể nghỉ sau phút 135 và nghỉ trưa sau phút 240; nghỉ giữa hai Sprint không chứa công việc được giao.

| Phút | Giảng viên | Học viên | Đầu ra |
|---|---|---|---|
| 000–020 | Demo một lịch đặt trùng; khảo sát đầu vào | Nêu tác động với người dùng, trả lời câu hỏi ban đầu | Baseline hiểu biết |
| 020–050 | Giải thích Scrum bằng chính ví dụ | Phân loại vai trò, mục tiêu và tạo tác | Sơ đồ Scrum của nhóm |
| 050–080 | Đóng vai khách hàng, trả lời theo câu hỏi | Phỏng vấn, tách sự thật và giả định | Product Goal và câu hỏi còn mở |
| 080–115 | Phản biện thứ tự ưu tiên và scope | Tạo 8–12 hạng mục, viết AC cho hạng mục đầu | Product Backlog có thứ tự |
| 115–135 | Phân biệt AC và DoD; làm mẫu test biên | Chốt DoD, expected cho test matrix | DoD và kế hoạch kiểm tra |
| 135–155 | Hỗ trợ Planning 1 | Chốt Why/What/How | Sprint Goal và Sprint Backlog 1 |
| 155–205 | Quan sát, phát thẻ yêu cầu mới | Phát triển, kiểm tra tiến độ, test và tích hợp | Increment 1 hoặc trạng thái chưa Done |
| 205–225 | Làm stakeholder; hỗ trợ test chéo | Review, quan sát người dùng, cập nhật backlog | Phản hồi có quyết định |
| 225–240 | Hướng dẫn Retro | Chọn một cải tiến cụ thể | Hành động thử ở Sprint 2 |
| 240–260 | Đưa phản hồi khách hàng | Planning 2 dựa trên bằng chứng | Sprint Goal và Sprint Backlog 2 |
| 260–310 | Phát thẻ lỗi hồi quy | Phát triển, kiểm tra và áp dụng cải tiến | Increment 2 |
| 310–330 | Hỗ trợ Review song song | Thử tác vụ và so sánh với Sprint 1 | Bằng chứng trước–sau |
| 330–345 | Dẫn Retro 2 | Đánh giá cải tiến đã thử | Bài học và hành động tiếp |
| 345–395 | Giao hai tình huống/nhóm, phản biện | Phân tích, quyết định và bảo vệ lựa chọn | Phiếu xử lý tình huống |
| 395–420 | Kiểm tra cá nhân, phản hồi, chốt | Hoàn thiện hồ sơ, viết kế hoạch áp dụng | Bộ bài nộp và exit ticket |

## 6. Lab khám phá bài toán — 30 phút

### 6.1 Brief phát cho học viên

> Công ty An Phát có 80 nhân viên và 4 phòng họp. Nhân viên đặt phòng qua tin nhắn và bảng tính. Thường có lịch trùng, thay đổi chưa được cập nhật hoặc phòng bị giữ dù cuộc họp đã hủy. Hành chính muốn một ứng dụng dễ dùng: xem phòng trống, đặt nhanh, thông báo, phân quyền, báo cáo và kết nối lịch.

### 6.2 Cách tổ chức

1. Đọc brief và ghi điều chưa rõ — 5 phút.
2. Phỏng vấn khách hàng — 10 phút.
3. Viết vấn đề, giả định và Product Goal — 10 phút.
4. Đọc lại hiểu biết cho khách hàng xác nhận — 5 phút.

Gợi ý câu hỏi: Ai gặp vấn đề thường xuyên nhất? Thiệt hại là gì? Đặt trùng được hiểu thế nào? Hai lịch nối tiếp có được phép? Việc gì có thể làm thủ công trước? Bằng chứng nào cho thấy giải pháp hữu ích?

**Product Goal tham khảo:** nhân viên tự tìm và đặt phòng phù hợp, giảm đặt trùng khi tổ chức họp.

**Bằng chứng cần có:** một nhóm người dùng, một vấn đề ưu tiên, một mục tiêu, ít nhất ba điều còn cần kiểm chứng. Không kết luận đã giảm đặt trùng trong doanh nghiệp chỉ từ demo trong lớp.

## 7. Lab backlog và chất lượng — 55 phút

### 7.1 Backlog tham khảo

| ID | Nhu cầu | Lý do cân nhắc |
|---|---|---|
| B01 | Xem phòng và sức chứa | Có thông tin chọn phòng |
| B02 | Xem lịch theo ngày | Biết phòng bận lúc nào |
| B03 | Tạo lịch đặt hợp lệ | Hoàn thành nhu cầu cốt lõi |
| B04 | Ngăn đặt trùng | Giải quyết vấn đề ưu tiên |
| B05 | Hủy lịch đặt | Giải phóng khung giờ |
| B06 | Lọc theo số người | Hỗ trợ chọn phòng |
| B07 | Nhắc lịch | Tăng tiện ích |
| B08 | Báo cáo sử dụng | Phục vụ quản trị |
| B09 | Đồng bộ lịch bên ngoài | Mở rộng tích hợp |

Cho học viên tự đề xuất trước khi đưa mẫu. B03/B04 có thể được gộp thành một hạng mục nhỏ đi hết luồng: tạo lịch có kiểm tra trùng. Không coi một chức năng tạo lịch cho phép trùng là đã dùng được chỉ vì màn hình hoạt động.

### 7.2 Acceptance criteria mẫu cho tạo lịch

1. Nhập được phòng, người đặt, ngày, giờ bắt đầu và kết thúc.
2. Thiếu trường bắt buộc thì báo lỗi rõ và không lưu.
3. Giờ kết thúc phải sau giờ bắt đầu.
4. Hai khoảng giờ giao nhau trong cùng phòng/ngày thì từ chối.
5. Cùng giờ nhưng khác phòng được phép.
6. Lịch liền kề được phép.
7. Lịch hợp lệ xuất hiện sau khi lưu và vẫn còn sau refresh.

### 7.3 Definition of Done mẫu

- Tích hợp vào phiên bản chung, không chỉ chạy trên phần riêng của một người.
- AC liên quan được kiểm tra và đạt; các luồng cốt lõi bị ảnh hưởng đã kiểm tra lại.
- Không còn lỗi đã biết làm sai quy tắc đặt phòng hoặc chặn luồng cốt lõi trong phạm vi.
- Dữ liệu vẫn còn sau refresh theo mô hình lưu trữ đã thống nhất.
- Có một người khác kiểm tra kết quả; có evidence và hướng dẫn chạy.
- Giới hạn phiên bản được ghi rõ; không trình bày local prototype như hệ thống nhiều người dùng.

AC mô tả yêu cầu riêng của hạng mục; DoD là chuẩn chất lượng chung. Hạng mục chưa đạt DoD không được tính là Increment hoàn thành. Không hạ DoD để chuyển trạng thái kịp giờ. [Nguồn: Scrum.org — Definition of Done](https://www.scrum.org/resources/what-definition-done)

## 8. Sprint mô phỏng 1 — 105 phút

### 8.1 Planning — 20 phút

Thảo luận Why: Sprint đem lại giá trị gì; What: có thể hoàn thành gì; How: làm và kiểm tra thế nào. Developers chọn công việc qua trao đổi với Product Owner và tự xây kế hoạch. [Nguồn: Sprint Planning](https://www.scrum.org/resources/what-is-sprint-planning)

**Sprint Goal gợi ý:** người dùng xem lịch và đặt một phòng còn trống mà không tạo lịch trùng trong phiên bản thử nghiệm.

Phân rã theo phần chạy được: dữ liệu/lịch mẫu, luồng xem–đặt có validation, kiểm tra và tích hợp. Không chia thành ba silo UI/backend/test rồi chờ đến cuối mới ghép.

### 8.2 Phát triển — 50 phút

| Phút trong block | Việc thực hiện |
|---|---|
| 0–10 | Thống nhất dữ liệu, luồng và phần việc nhỏ |
| 10–25 | Yêu cầu AI triển khai từng phần, chạy thử ngay |
| 25–30 | Điểm kiểm tra tiến độ mô phỏng |
| 30–40 | Điều chỉnh kế hoạch, tích hợp, sửa lỗi |
| 40–50 | Chạy test liên quan, đối chiếu DoD |

Tại điểm kiểm tra: Sprint Goal có nguy cơ gì? Việc quan trọng tiếp theo là gì? Ai cần phối hợp? Có việc nào nên dừng hoặc thương lượng giảm scope?

Điểm 5 phút này mô phỏng mục đích kiểm tra và điều chỉnh, không phải thay thế quy định thực tế. Daily Scrum là sự kiện 15 phút của Developers mỗi ngày làm việc; hình thức do Developers chọn, tập trung tiến độ tới Sprint Goal và kế hoạch tiếp theo. [Nguồn: Daily Scrum](https://www.scrum.org/resources/what-is-a-daily-scrum)

**Thẻ phát sinh khoảng phút 30:** giám đốc hành chính muốn thêm biểu đồ để demo ấn tượng. Nhóm ghi nhận quyết định và lý do; không tự động nhận hoặc từ chối mà chưa làm rõ giá trị, tác động.

### 8.3 Review — 20 phút

- 3 phút: nêu mục tiêu, phần Done và giới hạn.
- 7 phút: stakeholder hoặc nhóm khác tự dùng: tìm phòng cho 6 người, đặt, thử đặt trùng.
- 5 phút: trao đổi điều quan sát được và nhu cầu mới.
- 5 phút: cập nhật backlog và ghi lý do.

Review theo cặp nhóm song song nếu lớp đông. Ghi phần chưa hoàn thành minh bạch; không demo nó như Increment đã Done. Review là phiên làm việc để kiểm tra kết quả và quyết định hướng tiếp, không chỉ trình chiếu. [Nguồn: Sprint Review](https://www.scrum.org/resources/what-is-a-sprint-review)

### 8.4 Retrospective — 15 phút

Mỗi người ghi một việc hiệu quả, một trở ngại và một đề xuất. Nhóm chọn một cải tiến có người theo dõi và cách kiểm chứng, ví dụ: trước khi AI sửa quy tắc, viết ba test biên; người khác kiểm tra trước khi chuyển Done.

## 9. Sprint mô phỏng 2 — 105 phút

### 9.1 Planning — 20 phút

Phản hồi: “Tôi đặt được phòng nhưng không biết hủy khi cuộc họp thay đổi; tôi cũng khó chọn phòng đủ chỗ.” Nhóm sắp lại backlog và chọn mục tiêu; không mặc định làm hết.

**Sprint Goal gợi ý:** người dùng có thể hủy lịch và đặt lại vào khung giờ vừa được giải phóng.

Mang hành động cải tiến từ Retro 1 vào kế hoạch. Dùng dữ liệu Sprint 1 để dự báo năng lực, tránh cam kết dựa trên kỳ vọng AI làm nhanh hơn.

### 9.2 Phát triển — 50 phút

Giữ nhịp như Sprint 1. Phát thẻ: lịch đã hủy vẫn bị tính khi kiểm tra trùng. Nhóm tái hiện, xác định expected, kiểm tra code, sửa nhỏ và chạy lại luồng đặt–hủy–đặt lại. Nếu nhóm không chọn hủy lịch, dùng thẻ lỗi phù hợp phần đã chọn, không ép đổi mục tiêu chỉ để diễn tình huống.

### 9.3 Review — 20 phút

Người dùng tự tạo lịch, hủy và đặt lại cùng phòng/giờ. Ghi task hoàn thành, thời gian, số lần cần giúp, lỗi và lời phản hồi. So sánh cùng tác vụ/điều kiện nếu muốn nhận xét trước–sau. Không suy rộng kết quả vài người trong lớp thành hiệu quả doanh nghiệp.

### 9.4 Retrospective — 15 phút

Hành động cải tiến đã được thực hiện chưa? Có evidence gì? Điều gì vẫn chưa biết? Chọn hành động tiếp theo; không mặc định cải tiến thành công chỉ vì nhóm cảm thấy tốt hơn.

## 10. Xử lý tình huống thực tế — 50 phút

5 phút giao bài; 15 phút thảo luận; 20 phút trình bày/phản biện; 10 phút chốt. Mỗi nhóm nhận hai tình huống trong phiếu bài tập.

| Tình huống | Năng lực cần luyện |
|---|---|
| Khách hàng yêu cầu “nhanh, dễ dùng” | Làm rõ mục tiêu và cách đo |
| Yêu cầu tích hợp lịch xuất hiện giữa Sprint | Thương lượng giá trị, scope và rủi ro |
| AI chỉ so sánh giờ bắt đầu để phát hiện trùng | Tìm phản ví dụ, kiểm chứng quy tắc |
| Đã code xong nhưng chưa test khi gần hết giờ | Minh bạch chất lượng, giữ DoD |
| Người giữ phần kiểm tra thời gian rời nhóm | Chia sẻ kiến thức, điều chỉnh năng lực |
| Người dùng vẫn nhắn hành chính dù có app | Tìm nguyên nhân trước khi thêm tính năng |

Mẫu báo cáo: vấn đề → điều chưa rõ → lựa chọn → quyết định → tác động tới mục tiêu/chất lượng → bước kiểm chứng.

## 11. Prompt hỗ trợ vibe coding

### P1. Làm rõ yêu cầu

```text
Bạn hỗ trợ phân tích bài toán sau: [brief].
Liệt kê người dùng, nhu cầu, câu hỏi cần hỏi, giả định cần kiểm chứng
và rủi ro làm sản phẩm không sử dụng được.
Tách rõ thông tin đã được khách hàng xác nhận và suy luận.
Không tự biến giả định thành yêu cầu.
```

### P2. Thực hiện một hạng mục

```text
Sprint Goal: [...]
Hạng mục duy nhất cần triển khai: [...]
Acceptance criteria: [...]
Definition of Done: [...]
Đọc cấu trúc hiện có. Tóm tắt cách hiểu và phạm vi sửa trước khi sửa.
Hỏi lại quy tắc nghiệp vụ chưa rõ; không bổ sung tính năng ngoài scope.
Sau khi sửa, nêu thay đổi, cách chạy kiểm tra, kết quả thực sự đã chạy
và phần chưa xác minh. Không tự kết luận Done chỉ vì code đã được tạo.
```

### P3. Chẩn đoán lỗi

```text
Expected: [...]
Actual: [...]
Bước tái hiện, dữ liệu và môi trường: [...]
Evidence hiện có: [...]
Phân tích code liên quan, nêu giả thuyết và cách kiểm chứng.
Đề xuất sửa nhỏ nhất phù hợp; thêm trường hợp kiểm tra chống tái diễn.
Không sửa nhiều nguyên nhân chưa có bằng chứng cùng lúc.
```

### P4. Chuẩn bị Review

```text
Sprint Goal: [...]
DoD và kết quả kiểm tra: [...]
Tổng hợp phần Done có evidence, phần chưa Done, giới hạn,
câu hỏi cho người dùng và giả thuyết cần kiểm chứng.
Không thay evidence bằng nhận xét của AI.
```

## 12. Đánh giá và bộ bài nộp

| Tiêu chí | Điểm tối đa | Bằng chứng |
|---|---:|---|
| Hiểu vấn đề và giá trị | 15 | Product Goal, câu hỏi và lý do ưu tiên |
| Backlog và AC | 15 | Thứ tự, scope nhỏ, expected kiểm tra được |
| Mục tiêu và kế hoạch Sprint | 15 | Why/What/How, công việc hỗ trợ mục tiêu |
| Increment và chất lượng | 25 | Luồng chạy được, test, DoD, giới hạn |
| Xử lý thay đổi | 15 | Quyết định có dữ kiện và tác động |
| Học từ phản hồi | 15 | Backlog đổi có lý do, cải tiến được thử |
| **Tổng** | **100** | |

Điểm số phản ánh mức độ học; không thay điều kiện đạt cốt lõi ở mục 2. Không dùng số dòng code, số prompt, số tính năng hoặc velocity để xếp hạng cá nhân.

**Hồ sơ nộp:** Product Goal; backlog đầu/cuối; hai Sprint Goal và kế hoạch; DoD; source/phiên bản app và hướng dẫn chạy; test matrix có actual/evidence; feedback; quyết định thay đổi; Retro và cải tiến; known limitations.

**25 phút cuối:** 10 phút kiểm tra cá nhân, 10 phút hoàn thiện hồ sơ/nhận phản hồi, 5 phút viết kế hoạch áp dụng.

Exit ticket:

1. AI tạo xong UI nhưng chưa kiểm tra trùng lịch: có được Done không? Vì sao?
2. Có yêu cầu mới giữa Sprint: cần làm rõ, trao đổi và cập nhật những gì?
3. App chạy đúng nhưng người dùng không dùng: bước kiểm chứng tiếp theo là gì?

## 13. Chuyển vào công việc và điều chỉnh thời lượng

**Sau lớp:** chọn một công việc thật, xác định người nhận giá trị, mục tiêu, backlog nhỏ và chuẩn hoàn thành. Thử một chu kỳ, xin phản hồi, ghi điều chỉnh. Không gọi mọi checklist cá nhân là Scrum; muốn áp dụng đầy đủ cần tổ chức các trách nhiệm và sự kiện phù hợp.

**Chia hai buổi:** buổi A 240 phút tới hết Retro 1; buổi B 180 phút từ Planning 2 tới tổng kết. Khoảng cách là lịch đào tạo; hai Sprint trong lớp vẫn là mô phỏng, không coi khoảng giữa là một Sprint làm việc thực tế.

**Nếu chỉ có 180 phút, gồm 15 phút nghỉ:** 15 phút lý thuyết; 20 phút khám phá; 20 phút backlog/DoD; 75 phút một Sprint mô phỏng (Planning 15, phát triển 35, Review 15, Retro 10); nghỉ 15; tình huống 20; tổng kết 15. Bản này chỉ có một vòng trải nghiệm, chưa kiểm chứng việc áp dụng cải tiến sang Sprint sau.

## 14. Tài liệu tham chiếu

- [Scrum Guide 2020 — bản HTML chính thức](https://scrumguides.org/scrum-guide.html).
- [Scrum.org — Sprint](https://www.scrum.org/resources/what-is-a-sprint-in-scrum).
- [Scrum.org — Sprint Planning](https://www.scrum.org/resources/what-is-sprint-planning).
- [Scrum.org — Daily Scrum](https://www.scrum.org/resources/what-is-a-daily-scrum).
- [Scrum.org — Sprint Review](https://www.scrum.org/resources/what-is-a-sprint-review).
- [Scrum.org — Definition of Done](https://www.scrum.org/resources/what-definition-done).

Các tài liệu lý thuyết được đối chiếu khi soạn nội dung hội thoại. Timeline, case, dữ liệu, prompt, phiếu và rubric là thiết kế đào tạo riêng.
