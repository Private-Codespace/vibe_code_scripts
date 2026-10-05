# Giáo án chi tiết - Buổi 1: Tư duy Vibe Coding và dựng web đầu tiên

**Thời lượng:** 180 phút  
**Đối tượng:** Người mới bắt đầu, marketer, chủ doanh nghiệp nhỏ, sinh viên; không yêu cầu biết lập trình.  
**Sản phẩm cuối buổi:** Một landing page tiếng Việt có thể truy cập bằng URL công khai, chạy tốt trên điện thoại, có README ngắn và mã nguồn trên GitHub.

## 1. Vị trí của buổi học trong lộ trình

Buổi 1 không nhằm biến học viên thành người viết code bằng AI. Mục tiêu là thiết lập thói quen làm sản phẩm đúng ngay từ đầu:

`vấn đề thật -> brief nhỏ -> prompt có ràng buộc -> AI tạo/sửa code -> chạy thử -> kiểm tra -> sửa -> deploy`

Trong 8 buổi, landing page hôm nay là "vỏ sản phẩm". Từ Buổi 2, học viên sẽ đặc tả MVP; Buổi 3 phát triển giao diện nhiều màn hình; Buổi 4-6 gắn dữ liệu, đăng nhập và AI; Buổi 7-8 kiểm thử, triển khai và demo. Vì thế, Buổi 1 ưu tiên một sản phẩm nhỏ nhưng hoàn chỉnh, thay vì nhồi nhiều tính năng.

## 2. Mục tiêu đầu ra có thể quan sát

Kết thúc buổi, mỗi học viên có thể:

1. Giải thích bằng lời của mình frontend, backend, database và hosting khác nhau thế nào.
2. Phân biệt web tĩnh với web có dữ liệu/người dùng; chỉ ra phần nào của landing page hôm nay là tĩnh.
3. Viết brief 6 dòng và prompt có bối cảnh, nội dung, ràng buộc, đầu ra và tiêu chí kiểm tra.
4. Dùng AI để tạo hoặc sửa **một phần nhỏ** của dự án; biết yêu cầu AI nêu file sẽ thay đổi và giải thích thay đổi.
5. Chạy dự án Next.js ở máy cá nhân, đọc lỗi console cơ bản, kiểm tra giao diện mobile.
6. Đưa landing page lên Vercel, gửi được URL công khai và mô tả cách chạy trong README.

### Tiêu chí đạt tối thiểu

| Hạng mục | Điều kiện đạt |
|---|---|
| Nội dung | Rõ khách hàng mục tiêu, vấn đề, lời hứa giá trị, CTA; không dùng lorem ipsum |
| Giao diện | Có ít nhất 5 section: Hero, lợi ích, cách hoạt động/sản phẩm, giá/đăng ký, FAQ hoặc đánh giá, liên hệ/footer |
| Chất lượng | Không vỡ ở khổ 375 px và desktop; không có lỗi đỏ trong Console khi tải trang |
| Kỹ thuật | Chạy local bằng `npm run dev`; mã nguồn có trên GitHub; deploy Vercel thành công |
| Tài liệu | README có tên dự án, mục đích, cách chạy, URL production và công nghệ dùng |

## 3. Chuẩn bị trước giờ học

### Giảng viên

- Chuẩn bị một website mẫu đã deploy, một project Next.js + TypeScript + Tailwind chạy sẵn, và tài khoản GitHub/Vercel dự phòng.
- Chuẩn bị Wi-Fi, ổ cắm, QR/link hướng dẫn cài Node.js LTS, VS Code, Git, GitHub Desktop (tuỳ chọn) và Vercel.
- Tạo một repository mẫu chỉ dùng để demo. Không gõ API key, mật khẩu hoặc thông tin khách hàng thật trong giờ học.
- Chuẩn bị ảnh miễn phí/placeholder hợp lệ hoặc để học viên dùng ảnh tạo bởi AI. Tránh lấy ảnh có bản quyền không rõ nguồn.
- Mở sẵn các tab: trang mẫu, VS Code, terminal, GitHub, Vercel, DevTools của trình duyệt.

### Học viên

- Laptop, trình duyệt Chrome/Edge, VS Code, Node.js LTS, Git; đăng nhập GitHub và Vercel.
- Một ý tưởng dịch vụ thật hoặc chọn một đề bài trong phần 8.
- Không cần cài Supabase ở buổi này. Không cần mua tên miền.

### Kế hoạch dự phòng khi môi trường gặp sự cố

| Sự cố | Cách xử lý |
|---|---|
| Chưa cài Node/Git | Ghép cặp với bạn đã chạy được; dùng repo starter do giảng viên chuẩn bị; hoàn tất cài đặt trong giờ nghỉ |
| `npm install` chậm/lỗi mạng | Phát starter zip hoặc dùng CodeSandbox/StackBlitz tạm thời; vẫn hoàn thành brief, prompt và UI |
| Chưa đăng nhập Vercel | Deploy từ tài khoản giảng viên hoặc để cuối buổi/hoàn thành sau giờ; không chặn phần thực hành UI |
| AI tạo mã lỗi | Không dán đè cả dự án; quay lại commit/phiên bản chạy được, gửi lỗi đầy đủ cho AI và sửa từng file |

## 4. Kiến thức cốt lõi, giải thích bằng ngôn ngữ người mới

### 4.1 Vibe coding là gì và không là gì

Vibe coding là làm sản phẩm cùng AI. Người học quyết định **vấn đề cần giải quyết, người dùng, dữ liệu, tiêu chí đúng/sai và việc có phát hành hay không**. AI giúp chuyển yêu cầu thành mã, giải thích, tìm lỗi và đề xuất phương án.

AI không tự biết khách hàng thật cần gì; không tự bảo đảm code an toàn; và không được tin chỉ vì trang nhìn đẹp. Quy tắc lớp học: **AI viết được, người học chịu trách nhiệm kiểm tra được.**

### 4.2 Bốn mảnh của một ứng dụng web

| Thành phần | Hình dung đơn giản | Ví dụ trong app đặt lịch spa (các buổi sau) | Trạng thái ở Buổi 1 |
|---|---|---|---|
| Frontend | Cửa hàng/không gian khách nhìn và bấm | Trang giới thiệu dịch vụ, form chọn lịch | Có |
| Backend | Nhân viên xử lý phía sau | Kiểm tra slot, gửi xác nhận | Chưa có |
| Database | Sổ dữ liệu có cấu trúc | Khách, dịch vụ, lịch hẹn | Chưa có |
| Hosting | Mảnh đất/địa chỉ để web hoạt động | Vercel đưa web lên Internet | Có |

**Web tĩnh** hiển thị nội dung đã định sẵn: landing page hôm nay. **Web có dữ liệu** đọc/ghi dữ liệu thay đổi theo người dùng: dashboard đặt lịch ở những buổi sau. Một form liên hệ chỉ đang hiển thị chưa phải "form hoạt động" nếu bấm gửi không lưu/gửi dữ liệu đi đâu - phải nói rõ điều này để tránh hứa quá mức.

### 4.3 Công thức prompt sản phẩm

Prompt tốt không cần dài, nhưng đủ ngữ cảnh để AI ra quyết định đúng:

`Vai trò + bối cảnh/người dùng + mục tiêu trang + nội dung bắt buộc + ràng buộc kỹ thuật + tiêu chí kiểm tra + cách trả lời`

Ví dụ xấu: "Làm web bán cà phê đẹp đẹp."  
Vì sao xấu: không biết bán cho ai, điểm khác biệt, nội dung, CTA, tone hay file nào được phép sửa.

Ví dụ tốt dùng ở thực hành:

> Bạn là senior frontend developer. Dự án là Next.js App Router, TypeScript và Tailwind CSS. Hãy tạo landing page tiếng Việt cho **Bean Corner**, dịch vụ cà phê hạt rang theo tuần cho nhân viên văn phòng ở Quận 1, TP.HCM. Mục tiêu là người xem để lại nhu cầu tư vấn qua CTA "Nhận menu tuần này". Dùng nội dung thật bên dưới, không dùng lorem ipsum. Trang có: header, hero, 3 lợi ích, 3 gói, 3 đánh giá ngắn, FAQ, form liên hệ chỉ có giao diện, footer. Thiết kế tối giản, ấm, ưu tiên mobile, tương phản đủ đọc. Không thêm thư viện mới. Trước khi sửa, nêu các file sẽ đổi; sau khi sửa, giải thích ngắn và đưa checklist kiểm tra 375 px/1440 px/console.

### 4.4 Vòng kiểm tra sau mỗi lần AI sửa code

1. Đọc tóm tắt: AI sửa file nào, tại sao?
2. Chạy app: terminal có lỗi build/type không?
3. Kiểm tra hành vi: nội dung đúng brief, CTA/nút không gây hiểu nhầm?
4. Kiểm tra giao diện: 375 px, 768 px, 1440 px; chữ không tràn, nút chạm được.
5. Mở Console: có lỗi đỏ/cảnh báo nghiêm trọng không?
6. Chỉ khi ổn mới commit thay đổi nhỏ và tiếp tục.

## 5. Kịch bản đứng lớp 180 phút

| Thời gian | Mục tiêu | Hoạt động của giảng viên | Hoạt động của học viên | Sản phẩm/kiểm tra ngay |
|---|---|---|---|---|
| 00:00-00:10 | Đặt kỳ vọng | Chiếu landing page và app hoàn chỉnh; hỏi: "Trang đẹp nhưng nút Đặt lịch chưa lưu được thì đã là app chưa?" | Trả lời nhanh, nêu một website muốn tự làm | Hiểu landing page là bước đầu, không phải toàn bộ app |
| 00:10-00:20 | Tư duy trách nhiệm | Dạy "AI là đồng đội, không là người chịu trách nhiệm"; cho ví dụ prompt mơ hồ vs rõ | Tìm 3 điểm thiếu trong prompt mơ hồ | 1 prompt được cải thiện trên giấy/chat |
| 00:20-00:45 | Nền tảng web | Vẽ sơ đồ frontend-backend-database-hosting; demo một trang tĩnh và dashboard có dữ liệu | Ghép 4 khái niệm với ví dụ | Trả lời được phần nào chưa làm ở Buổi 1 |
| 00:45-01:15 | Có môi trường chạy được | Hướng dẫn kiểm tra `node -v`, `git --version`, tạo repo, tạo/chạy Next.js starter | Cài đặt, clone/tạo repo, chạy `npm run dev` | Trình duyệt mở localhost; ghi nhận học viên cần hỗ trợ |
| 01:15-01:30 | Nghỉ + phân luồng hỗ trợ | Xử lý lỗi cài đặt theo nhóm nhỏ | Hoàn tất đăng nhập GitHub/Vercel | Mỗi người có đường dẫn project local hoặc starter |
| 01:30-01:45 | Từ brief sang prompt | Demo với đề Bean Corner: brief 6 dòng -> prompt -> AI liệt kê file trước khi sửa | Điền brief ý tưởng cá nhân | Brief được giảng viên/bạn cùng bàn phản biện 2 phút |
| 01:45-02:00 | Sửa có kiểm soát | Cho AI tạo section Hero trước; đọc code theo component, đổi một câu CTA bằng tay | Thực hiện một thay đổi nhỏ, chạy và xem kết quả | 1 commit hoặc snapshot "hero chạy được" |
| 02:00-02:25 | Dựng nội dung thật | Làm mẫu cấu trúc 5-7 section; nhấn mạnh hierarchy và CTA | Dùng prompt tạo landing page theo brief | Trang có khung section, nội dung không phải placeholder |
| 02:25-02:40 | Tăng độ tin cậy | Demo chỉnh màu/chữ/ảnh, FAQ, trạng thái form "chưa gửi"; nhắc không giả vờ form đã hoạt động | Hoàn thiện gói giá/FAQ/liên hệ | Tối thiểu 5 section và 1 CTA chính |
| 02:40-02:55 | QA mini | Chiếu DevTools responsive; kiểm tra 375/768/1440 px, tab keyboard, console | Đổi kích thước, tự chạy checklist, sửa 1 lỗi | Ghi 1 lỗi tìm được và cách đã sửa |
| 02:55-03:00 | Deploy và chốt | Demo import repo vào Vercel, deploy; phát bài tập/tiêu chí | Deploy hoặc ghi rõ việc còn lại; nộp URL GitHub/Vercel | Link production + README hoặc kế hoạch hoàn tất sau giờ |

## 6. Hướng dẫn thực hành theo từng chặng

### Chặng A - Viết brief 6 dòng (10 phút)

Học viên không bắt đầu bằng giao diện. Mỗi người điền mẫu sau:

```text
Tên tạm của sản phẩm:
Khách hàng cụ thể:
Vấn đề/nhu cầu họ đang có:
Giá trị hứa hẹn trong một câu:
Hành động chính (CTA) muốn họ làm:
3 bằng chứng/tính năng/lợi ích sẽ đưa lên trang:
```

**Ví dụ đạt:** "Mẹ Bỉm Gọn Nhà giúp phụ huynh bận rộn tại TP.HCM đặt lịch dọn dẹp nhà theo gói. CTA: Nhận báo giá 15 phút. Bằng chứng: nhân viên xác thực, chọn khung giờ, giá minh bạch."  
**Chưa đạt:** "App dọn nhà cho mọi người, nhiều tính năng, đẹp và hiện đại." Nó chưa có người dùng, nhu cầu hay hành động cụ thể.

**Ví dụ điền hoàn chỉnh - Bean Corner:**

```text
Tên tạm của sản phẩm: Bean Corner
Khách hàng cụ thể: nhân viên văn phòng tại Quận 1, TP.HCM thường tự pha cà phê tại công ty
Vấn đề/nhu cầu: khó chọn hạt hợp gu, không có thời gian mua và sợ cà phê không tươi
Giá trị hứa hẹn: nhận cà phê hạt rang trong tuần, có hướng dẫn pha nhanh, giao theo lịch văn phòng
CTA: Nhận menu tuần này
3 lợi ích: rang mới mỗi tuần; chọn gu theo tư vấn; giao định kỳ đến văn phòng
```

### Chặng B - Biến brief thành prompt (10 phút)

Học viên sử dụng mẫu này, thay phần trong ngoặc vuông:

```text
Bạn là senior frontend developer.
Bối cảnh: Tôi đang làm landing page cho [TÊN], phục vụ [KHÁCH HÀNG CỤ THỂ].
Mục tiêu: thuyết phục họ [HÀNH ĐỘNG CTA] vì [GIÁ TRỊ].
Nội dung thật: [3 lợi ích], [gói/giá hoặc cách hoạt động], [3 câu FAQ].
Kỹ thuật: dùng Next.js + TypeScript + Tailwind trong project hiện có; không cài thêm thư viện;
dùng component rõ ràng; form chỉ là UI, chưa tích hợp gửi dữ liệu.
UX: tiếng Việt, mobile-first, tương phản dễ đọc, ảnh có alt, nút có nhãn rõ.
Quy trình: trước tiên liệt kê file định sửa và kế hoạch ngắn. Chỉ sau đó mới đưa code.
Kiểm tra: xác nhận không lỗi TypeScript/build và nêu cách kiểm tra ở 375 px, 768 px, 1440 px.
```

Quy tắc sửa prompt khi kết quả chưa ổn: mô tả **sai ở đâu** (ví dụ "CTA nằm dưới màn hình đầu tiên ở mobile"), nêu **mong muốn đo được** ("CTA hiển thị trong phần hero ở 375 px"), cung cấp **file/đoạn liên quan**, rồi yêu cầu thay đổi nhỏ nhất. Không dùng câu mơ hồ như "sửa cho đẹp hơn".

**Ví dụ prompt đã điền - Bean Corner:**

> Bạn là senior frontend developer. Tôi đang làm landing page cho Bean Corner, phục vụ nhân viên văn phòng ở Quận 1, TP.HCM tự pha cà phê tại công ty. Mục tiêu là thuyết phục họ bấm "Nhận menu tuần này" vì họ sẽ nhận cà phê hạt rang trong tuần và giao theo lịch văn phòng. Nội dung có 3 lợi ích: rang mới mỗi tuần, tư vấn chọn gu, hướng dẫn pha trong 3 phút; 3 gói: Khởi đầu 250g, Văn phòng 1kg, Quà tặng; FAQ về lịch giao, chọn gu và bảo quản. Dùng Next.js, TypeScript, Tailwind trong project hiện có; không cài thư viện mới. Form chỉ là giao diện, chưa gửi dữ liệu. Trước khi sửa, hãy nêu file sẽ đổi. Sau đó tạo riêng Hero và section lợi ích, không thay các phần khác. Kiểm tra ở 375 px, 768 px, 1440 px và không để lỗi Console.

### Chặng C - Tạo và chạy project (30 phút, giảng viên chọn starter hoặc tạo mới)

Đường đi an toàn cho người mới: giảng viên cung cấp starter Next.js đã cấu hình TypeScript/Tailwind. Học viên fork/clone rồi chạy:

```bash
npm install
npm run dev
```

Nếu tạo mới từ đầu, giảng viên chạy lệnh theo phiên bản công cụ hiện có và giải thích các lựa chọn. Không bắt học viên hiểu mọi file. Chỉ giới thiệu các điểm cần đụng hôm nay:

- `app/page.tsx`: trang chính;
- `app/layout.tsx`: khung chung và metadata;
- `app/globals.css`: style toàn cục;
- `public/`: ảnh/tài nguyên tĩnh.

**Checkpoint:** mở được `http://localhost:3000` (hoặc cổng terminal báo), thay một câu chữ, lưu file và thấy trang tự cập nhật.

**Ví dụ thao tác - Bean Corner:** mở `app/page.tsx`, đổi tiêu đề mặc định thành `Cà phê rang mới cho tuần làm việc tỉnh táo`, lưu file. Nếu trình duyệt tại `localhost:3000` tự đổi tiêu đề mà không cần tải lại thủ công, học viên đã xác nhận môi trường chạy đúng. Nếu không thấy đổi, kiểm tra đã lưu đúng file, terminal còn chạy và URL/cổng có đúng không.

### Chặng D - Làm theo lát cắt nhỏ (55 phút)

Không yêu cầu AI xây cả trang rồi hy vọng đúng. Làm theo thứ tự:

1. Hero: tên, lời hứa giá trị, CTA, ảnh/minh hoạ.
2. Ba lợi ích: mỗi lợi ích trả lời một nỗi đau của khách hàng.
3. Cách hoạt động hoặc sản phẩm/gói giá: tạo sự rõ ràng, không bịa ưu đãi.
4. Bằng chứng: testimonial ghi rõ là nội dung minh hoạ nếu chưa có khách thật.
5. FAQ: trả lời rào cản mua phổ biến.
6. Footer/liên hệ: thông tin liên hệ hợp lệ hoặc gắn nhãn "demo".

Sau mỗi bước: chạy app, nhìn phần vừa làm, rồi mới đi tiếp. Nếu AI thay quá nhiều file, yêu cầu dừng và giải thích; khôi phục bản chạy được trước khi thử lại.

**Ví dụ nội dung cụ thể - Bean Corner:**

| Phần | Nội dung nên tạo | Điều không được làm |
|---|---|---|
| Hero | "Cà phê rang mới cho tuần làm việc tỉnh táo"; nút "Nhận menu tuần này"; ảnh minh hoạ ly cà phê/văn phòng | Viết "đặt hàng thành công" khi chưa có chức năng đặt hàng |
| Ba lợi ích | "Rang trong tuần", "Chọn gu trong 2 phút", "Giao đúng lịch văn phòng" | Ghi các câu chung chung như "chất lượng số 1" mà không giải thích giá trị |
| Gói | Khởi đầu 250g, Văn phòng 1kg, Quà tặng; ghi giá thật hoặc "liên hệ để nhận báo giá" | Bịa giá khuyến mãi, tồn kho hoặc thời hạn ưu đãi |
| Bằng chứng | "Nội dung đánh giá minh hoạ cho bản demo" dưới 3 quote mẫu | Gắn tên, ảnh, công ty hoặc số lượng khách hàng giả như dữ liệu thật |
| FAQ | "Giao vào ngày nào?", "Tôi chưa biết chọn hạt?", "Bảo quản ra sao?" | Chỉ dùng FAQ vô nghĩa như "Bean Corner có tốt không?" |
| Footer | Email `hello@beancorner.demo` và dòng "Thông tin minh hoạ cho bài học" | Để số điện thoại/email ngẫu nhiên khiến khách thật liên hệ nhầm |

**Ví dụ prompt theo lát cắt:** "Chỉ thêm section FAQ cho Bean Corner vào `app/page.tsx`. Có đúng 3 câu hỏi: lịch giao, tư vấn chọn gu, bảo quản. Dùng accordion đơn giản không cần thư viện mới. Không đổi Hero, gói giá hoặc màu hiện có. Sau khi sửa, cho biết cách test mở/đóng FAQ bằng chuột và bàn phím."

### Chặng E - QA responsive và console (15 phút)

Mỗi học viên kiểm tra bằng DevTools hoặc thu nhỏ cửa sổ:

- [ ] 375 px: menu/nội dung không tràn ngang; CTA vẫn thấy và bấm được.
- [ ] 768 px: card/gói giá không quá chật; khoảng cách đọc được.
- [ ] 1440 px: nội dung không kéo dài khó đọc; phần hero không bị rỗng.
- [ ] Tiêu đề có cấp bậc hợp lý; ảnh có `alt`; input có label.
- [ ] Không còn lỗi đỏ Console khi load; click CTA không làm trang crash.
- [ ] Mọi tuyên bố như "đã gửi"/"đã đặt lịch" đều đúng với chức năng thật.

**Ví dụ kiểm tra - Bean Corner:** tại 375 px, học viên thấy nút "Nhận menu tuần này" bị sát mép và chữ trong card "Văn phòng 1kg" bị xuống dòng xấu. Học viên chụp lại lỗi, yêu cầu AI chỉ chỉnh padding của Hero và layout card ở mobile, rồi kiểm tra lại tại 375 px và 1440 px để chắc rằng desktop không bị ảnh hưởng. Cuối cùng mở Console; nếu không có lỗi đỏ khi mở/đóng FAQ và bấm CTA, đánh dấu đạt phần QA.

### Chặng F - Deploy và README (5-15 phút, có thể hoàn tất sau giờ)

1. Đẩy source lên GitHub; kiểm tra không có file `.env` hoặc secret.
2. Import repository vào Vercel, chọn framework Next.js, deploy.
3. Mở URL bằng điện thoại hoặc chế độ mobile, chạy lại checklist tối thiểu.
4. Điền README theo mẫu dưới đây; dán link production.

**Ví dụ triển khai - Bean Corner:** repository là `bean-corner-landing`; sau khi đẩy source lên GitHub, học viên vào Vercel, chọn **Add New -> Project -> Import** repository này, giữ framework Next.js được tự nhận diện rồi bấm Deploy. Khi nhận URL như `bean-corner-landing.vercel.app`, học viên mở URL trên điện thoại, bấm CTA, chụp màn hình và thay `[Dán URL Vercel]` trong README bằng URL thật. Vì form chưa kết nối backend, README ghi rõ: "Form liên hệ hiện là giao diện minh hoạ; chưa lưu hoặc gửi dữ liệu."

~~~~md
# [Tên dự án]

Landing page cho [khách hàng/vấn đề].

## Chức năng hiện có
- Giới thiệu giá trị và lợi ích
- Hiển thị gói dịch vụ / FAQ / CTA liên hệ
- Responsive cho mobile và desktop

## Chạy local
```bash
npm install
npm run dev
```

## Công nghệ
Next.js, TypeScript, Tailwind CSS, Vercel.

## Link production
[Dán URL Vercel]
~~~~

## 7. Demo mẫu của giảng viên: Bean Corner (30 phút phân bổ trong giờ)

**Bối cảnh:** Bean Corner bán cà phê hạt rang theo tuần cho nhân viên văn phòng tại Quận 1. Khách ngại cà phê pha không ổn định, mất thời gian chọn hạt và không biết bắt đầu từ đâu.

**Thông điệp hero:** "Cà phê ngon cho tuần làm việc tỉnh táo - chọn gói, nhận hạt rang mới, pha dễ tại văn phòng."  
**CTA chính:** "Nhận menu tuần này".  
**Nội dung section:**

- Lợi ích: rang trong tuần; hướng dẫn pha 3 phút; giao đúng lịch văn phòng.
- Gói: Khởi đầu 250g, Văn phòng 1kg, Quà tặng; ghi "giá minh hoạ" nếu không có giá thật.
- FAQ: giao khi nào, có tư vấn chọn gu không, bảo quản thế nào.
- Form: họ tên, email/công ty; nút "Nhận tư vấn". Chú thích rõ: form chưa lưu dữ liệu ở Buổi 1.

**Luồng demo chuẩn:**

1. Cho lớp đọc brief, yêu cầu chỉ ra CTA và dữ liệu chưa nên làm.
2. Gửi prompt chỉ tạo Hero, không tạo cả trang.
3. Mở `page.tsx`, chỉ ra JSX là phần mô tả giao diện và class Tailwind là phần style; không đi sâu cú pháp.
4. Đổi CTA "Nhận menu tuần này" thành "Xem gói cho văn phòng" bằng tay, lưu và quan sát hot reload.
5. Yêu cầu AI thêm section gói giá, kèm điều kiện "không thay Hero".
6. Thu nhỏ xuống 375 px, chỉ ra một lỗi thực tế (ví dụ card tràn ngang), rồi gửi prompt sửa có tiêu chí.
7. Mở Console, giải thích lỗi phải được đọc cùng hành vi và bước tái hiện.

## 8. Bộ bài toán thực tế để học viên chọn

Mỗi bài đều cố ý giới hạn trong landing page để phù hợp Buổi 1. Không làm đăng nhập, thanh toán, lưu form hay dashboard hôm nay.

### Bài toán 1 - Đặt lịch chăm sóc thú cưng tại nhà

**Tình huống:** Pawsome Care nhận nhiều tin nhắn rời rạc trên Facebook/Zalo, khách không biết giá và khu vực phục vụ.  
**Khách hàng:** người nuôi chó/mèo bận rộn ở nội thành.  
**CTA:** "Nhận tư vấn khung giờ".  
**Landing phải trả lời:** dịch vụ nào, quy trình 3 bước, khu vực, gói chăm sóc, câu hỏi về an toàn thú cưng.  
**Ràng buộc thực tế:** form chỉ thu nhu cầu, không được nói "đặt lịch thành công"; không hiển thị số bác sĩ/chứng chỉ bịa đặt.

### Bài toán 2 - Lớp tiếng Anh giao tiếp cho người đi làm

**Tình huống:** trung tâm có nội dung tốt nhưng khách không biết lớp phù hợp trình độ nào.  
**Khách hàng:** người đi làm 23-35 tuổi, ngại nói tiếng Anh trong họp.  
**CTA:** "Làm bài test đầu vào" (chỉ là nút/form giao diện ở Buổi 1).  
**Landing phải trả lời:** kết quả đầu ra, lộ trình 8 tuần, lịch học, cách test trình độ, học phí/gói minh hoạ.  
**Bẫy cần xử lý:** không hứa "thành thạo sau 8 tuần"; thay bằng kết quả có giới hạn, ví dụ "tự tin xử lý tình huống giao tiếp đã luyện".

### Bài toán 3 - Dịch vụ kế toán cho shop online

**Tình huống:** chủ shop phải theo dõi đơn, chi phí và thuế trên nhiều file, sợ sai số liệu.  
**Khách hàng:** shop online nhỏ dưới 5 người.  
**CTA:** "Nhận checklist sổ sách tháng này".  
**Landing phải trả lời:** dịch vụ bao gồm gì, quy trình bàn giao chứng từ, bảo mật, ba gói phù hợp quy mô shop, FAQ về thời gian phản hồi.  
**Bẫy cần xử lý:** không đưa lời khuyên thuế cụ thể/không kiểm chứng; dùng ngôn ngữ "được tư vấn theo hồ sơ".

### Bài toán 4 - Bữa trưa healthy giao theo tuần

**Tình huống:** nhân viên văn phòng muốn ăn lành mạnh nhưng không kịp chọn món mỗi sáng.  
**Khách hàng:** nhân viên văn phòng tại một quận cụ thể.  
**CTA:** "Xem thực đơn tuần".  
**Landing phải trả lời:** cách chọn gói, thành phần chính, lịch giao, khu vực, chính sách đổi món.  
**Bẫy cần xử lý:** không tuyên bố chữa bệnh/giảm cân; tránh sử dụng số liệu dinh dưỡng không có nguồn.

### Bài toán 5 - Portfolio nhiếp ảnh chụp sản phẩm

**Tình huống:** freelancer có ảnh đẹp nhưng khách thương hiệu nhỏ không hiểu quy trình, thời gian và gói phù hợp.  
**Khách hàng:** chủ shop mỹ phẩm/thời trang nhỏ.  
**CTA:** "Nhận báo giá theo brief".  
**Landing phải trả lời:** phong cách, quy trình gửi sản phẩm, gallery có alt text, gói chụp, FAQ về thời hạn bàn giao.  
**Bẫy cần xử lý:** chỉ dùng ảnh có quyền sử dụng; đánh dấu testimonial là minh hoạ nếu không phải phản hồi thật.

### Yêu cầu chung cho mọi bài

- Viết một câu "không làm ở Buổi 1"; ví dụ: "chưa nhận thanh toán/chưa lưu booking/chưa gửi email".
- Viết 3 FAQ bắt nguồn từ lo lắng thật của khách, không phải FAQ chung chung.
- Có một CTA chính lặp lại hợp lý; không dùng 4 CTA cạnh tranh.
- Không để AI tự bịa số khách hàng, đối tác, giải thưởng, chứng nhận hay cam kết pháp lý.

## 9. Hoạt động thực hành có hướng dẫn và phản biện

### Bài tập 1 - Phẫu thuật prompt (8 phút, làm theo cặp)

Prompt ban đầu: "Làm website cho dịch vụ dọn nhà đẹp và xịn."  
Mỗi cặp phải bổ sung ít nhất: khách hàng, pain point, CTA, 5 section, ràng buộc form, mobile-first, file cần sửa và cách kiểm tra. Đổi prompt cho cặp khác, người nhận đánh dấu chỗ vẫn chưa kiểm chứng được.

**Đáp án tham khảo:** prompt phải nói được ai sống ở đâu/đang gặp khó gì, trang muốn họ làm gì, và "form chỉ là giao diện". Chỉ thêm từ "đẹp/xịn/hiện đại" không làm prompt tốt hơn.

### Bài tập 2 - Triage lỗi AI (12 phút)

Cho học viên một trong ba tình huống:

1. Trang chạy nhưng trên mobile card giá bị tràn ngang.
2. AI đổi cả layout và màu thương hiệu trong khi bạn chỉ yêu cầu thêm FAQ.
3. Nút "Đăng ký ngay" hiện thông báo thành công nhưng dữ liệu không được gửi đi.

Mỗi nhóm trả lời theo mẫu: **hành vi hiện tại -> hành vi mong muốn -> bước tái hiện -> file liên quan -> thay đổi nhỏ nhất -> cách test lại**.

Prompt sửa mẫu cho tình huống 1:

> Ở viewport 375 px, ba card gói giá tạo cuộn ngang. Mục tiêu là mỗi card vừa chiều rộng màn hình, vẫn giữ nội dung và style desktop. Hãy chỉ sửa component gói giá và class Tailwind cần thiết; không đổi nội dung Hero. Nêu diff logic, rồi hướng dẫn tôi test lại ở 375 px và 1440 px.

### Bài tập 3 - Review chéo (10 phút)

Đổi máy/link với bạn cùng lớp. Reviewer chỉ được kiểm tra theo câu hỏi:

1. Trong 5 giây đầu, tôi biết trang này dành cho ai và giúp gì không?
2. Tôi biết cần bấm gì tiếp theo không?
3. Có tuyên bố/chức năng nào không đúng sự thật không?
4. Dùng điện thoại có đọc/bấm được không?
5. Có một lỗi P0/P1 cụ thể nào cần sửa trước?

Người làm nhận tối đa 3 góp ý, chọn sửa 1 việc có tác động lớn nhất trước khi deploy.

## 10. Rubric chấm nhanh cuối buổi (10 điểm)

| Tiêu chí | Điểm | Mô tả |
|---|---:|---|
| Vấn đề và khách hàng rõ | 2 | Hero nói được đối tượng, lợi ích, CTA; nội dung phù hợp brief |
| Cấu trúc landing page | 2 | Có từ 5 section, hierarchy rõ, FAQ/liên hệ có ý nghĩa |
| Prompt và cách làm với AI | 2 | Có prompt có ràng buộc; AI sửa theo lát cắt; học viên giải thích được ít nhất một thay đổi |
| Responsive và QA | 2 | Qua 375/768/1440 px; không lỗi console nghiêm trọng; sửa được ít nhất một vấn đề |
| Xuất bản và tài liệu | 2 | GitHub, URL Vercel, README ngắn và không lộ secret |

**Mốc hỗ trợ:** 0-4 cần hoàn thiện môi trường/cấu trúc; 5-7 đạt MVP buổi 1; 8-10 sẵn sàng dùng nền này cho Buổi 2.

## 11. Bài tập về nhà và cầu nối sang Buổi 2

Hoàn thiện trước Buổi 2:

1. Sửa 3 góp ý từ review chéo và cập nhật URL production.
2. Viết README hoàn chỉnh, thêm 3 ảnh chụp màn hình desktop/mobile nếu có thời gian.
3. Viết một trang "Product brief v0" gồm: người dùng, vấn đề, mục tiêu, CTA, 3 tính năng phải có, 3 thứ **không làm** trong MVP.
4. Liệt kê 5 user stories theo mẫu: "Là [ai], tôi muốn [làm gì] để [lợi ích]".

Ví dụ user story chuyển tiếp: "Là chủ shop online, tôi muốn để lại nhu cầu tư vấn kế toán để biết gói dịch vụ phù hợp." Sang Buổi 2, lớp sẽ quyết định đây là landing-page lead form hay một app quản lý thật, các luồng chính là gì, và dữ liệu nào phải lưu.

## 12. Checklist bàn giao dành cho giảng viên

- [ ] Mỗi học viên đã chọn/được gán một bài toán và có brief 6 dòng.
- [ ] Ít nhất 80% lớp chạy được trang local trước phút 90.
- [ ] Mỗi học viên đã dùng AI cho một thay đổi nhỏ và tự kiểm tra kết quả.
- [ ] Có review responsive, console, nội dung thật và tuyên bố chức năng thật.
- [ ] Đã thu link GitHub, Vercel, trạng thái README và lỗi còn lại của từng học viên.
- [ ] Học viên chưa deploy có lịch hoàn tất rõ ràng, không bị đánh giá như đã hoàn thành.
- [ ] Bài tập về nhà tạo đầu vào trực tiếp cho Product brief, user stories và sitemap của Buổi 2.

---

**Thông điệp chốt buổi:** "Một landing page chưa phải ứng dụng hoàn chỉnh. Nhưng nếu bạn biến được một vấn đề thật thành một trang rõ ràng, chạy được, kiểm tra được và có URL công khai, bạn đã có nền móng đúng để xây MVP cùng AI." 
