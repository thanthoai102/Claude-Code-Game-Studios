# Mythkeep: Match & Defend – Game Design Document

Tên game anh chốt 17:51 (dự án nội bộ: Dopamin; tên làm việc cũ: "Hand of God × ghép 3"). "Bàn tay thần" (Hand of God) giờ là tên chiêu đổi 2 ô 🌈.

| | |
|---|---|
| Phiên bản | 1.0 (2026-10-23, 17:55) |
| Trạng thái | Đã duyệt thiết kế, chuyển CTO lập kế hoạch P0 |
| Người quyết định thiết kế | Anh Thoại (chủ dự án) |
| Nguồn quyết định | `mythkeep-concept.md` (bản gốc: concepts-v5-hand-of-god.md) mục 0 (bảng quyết định có giờ chốt) |
| Bảng điểm | `mythkeep-scorecard.md` (8,2/10, đáng làm) |
| Bản chơi thử tham chiếu | `prototypes/dopamin-mythkeep/index.html` (HTML, chạy trên trình duyệt; logic tham chiếu, không phải code sản phẩm) |
| Thị trường | Mỹ và Nhật, iOS trước (App Store) |
| Mục tiêu kinh doanh | 5.000 USD doanh thu trong năm đầu sau khi lên App Store; thời gian làm game tối đa 5 năm |

Mọi con số trong tài liệu là **giá trị khởi đầu để cân bằng**, phải nằm trong file cấu hình (không viết cứng trong code), xem mục 7.

---

## 1. Tổng quan

Người chơi là vị thần bảo hộ một ngôi làng. Màn hình dọc chia đôi: nửa trên là cảnh (làng ban ngày, đồng cỏ ban đêm), nửa dưới là một bàn ghép 3 kích thước 7 × 7. **Ban ngày** (2 phút) ghép ô ra tài nguyên (thịt, gỗ, mana, vàng) để nâng công trình, thuê dân, nâng pháp sư. **Ban đêm** (45 giây) cùng bàn ghép đó lật thành 4 nguyên tố (Lửa, Khí, Nước, Đất). Mỗi lần ghép, pháp sư cùng nguyên tố đứng trên cổng làng, ngay trên bàn ghép, bắn phép vào bầy quái đang tràn xuống. Cổng vỡ thì mất kho tài nguyên. Pháp sư có từ quán rượu (quay thẻ bằng kim cương) và mạnh lên bằng mana, thẻ trùng và kinh nghiệm đánh quái.

**Tên game:** Mythkeep (Myth + Keep: giữ thành bằng các pháp sư thần thoại). Tên phụ "Match & Defend" cho người xem App Store hiểu ngay thể loại. Tiếng Nhật: ミスキープ. Trước khi phát hành phải kiểm tra trùng tên trên App Store và nhãn hiệu tại Mỹ, Nhật.

**Ba trụ cột thiết kế**
1. **Mỗi lần vuốt là một đòn thấy ngay.** Pháp sư đứng sát bàn ghép, phép bay ra trong vòng 0,2 giây sau khi ô nổ.
2. **Một bàn, hai vai.** Ban ngày là kinh tế, ban đêm là chiến đấu. Người chơi học một thao tác duy nhất: vuốt.
3. **Bàn ghép là của người chơi.** Quái không bao giờ chạm vào bàn ghép. Mọi khó khăn nằm ở nửa trên màn hình.

## 2. Cảm giác người chơi muốn có

- **30 giây đầu:** "Mình vừa bắn cháy cả bầy sói bằng một cú vuốt." Chuỗi rơi, cột lửa giáng từ trời xuống, chữ CRIT bật to, Bàn tay thần khổng lồ búng bay sói đầu đàn.
- **Ban ngày:** dễ chịu, kiểu "xây tổ". Thấy số +N trên ô tăng dần nhờ dân mình thuê, mua sắm không bị đuổi giờ.
- **Ban đêm:** dồn dập, căng. Chọn màu khắc chế, dành ô đặc biệt cho con trùm, giữ kho không mất.
- **Dài hạn:** sưu tầm pháp sư thần thoại, nuôi người mình thích lên tiềm năng 5 rồi nâng phẩm chất, mở dần 47 trang sách phép.

---

## 3. Luật chi tiết

### 3.1 Vòng lặp một ngày

| Pha | Thời lượng | Bàn ghép | Nửa trên | Kết thúc khi |
|---|---|---|---|---|
| Ban ngày | 120 giây (dừng khi đang ở màn hình công trình) | Ghép ra tài nguyên | Làng, 4 bệ pháp sư ở cổng, công trình, quán rượu | Hết giờ hoặc bấm "Vào đêm" |
| Hoàng hôn | 0,7–2 giây | Ô lật mặt thành nguyên tố cùng màu | Làng trượt xuống, đồng cỏ hiện ra | Hết hoạt cảnh |
| Ban đêm | 45 giây sinh quái + thời gian dọn quái còn lại | Ghép ra nguyên tố, pháp sư tấn công | Quái đi từ trên xuống cổng | Hết 45 giây và không còn quái sống; hoặc cổng về 0 |
| Bình minh | ~1–5 giây | Ô lật lại thành tài nguyên; 🌈 còn lại nổ thành KN | Làng trở lại; dân và pháp sư ăn thịt; tính độ khó đêm sau | Hết hoạt cảnh, sang ngày mới |

**Trạng thái game:** `Menu → Day ⇄ BuildingScreen(dừng giờ) → Dusk → Night → (GateBroken → Revive | GiveUp) → Dawn → Day …`

### 3.2 Bàn ghép
- **Kích thước 7 cột × 7 hàng**, 4 loại ô. Chuẩn bố cục iPhone 15 (393 × 852 pt): ô 52 pt, bàn 364 pt. Máy khác giữ 7 × 7, cỡ ô = min(chiều rộng khả dụng ÷ 7, nửa chiều cao khả dụng ÷ 7). Tối thiểu 44 pt (iPhone SE ~46 pt).
- **Bố cục dọc (iPhone 15):** vùng an toàn trên 59 pt | cảnh ~352 pt ban ngày, ~388 pt ban đêm | dòng tài nguyên 36 pt (**chỉ ban ngày**) | lề 8 pt | bàn 364 pt | vùng an toàn dưới 34 pt.
- **Thao tác:** vuốt đổi 2 ô kề nhau. Chỉ đổi khi tạo ra ít nhất một hàng 3 (hoặc khi có ô đặc biệt tham gia, xem 3.5). Chạm (không vuốt) dùng cho ô 💎 và ô 🌈.
- **Không giới hạn lượt.** Áp lực duy nhất là đồng hồ ban ngày và quái ban đêm.
- **Hết nước đi:** tự xáo bàn, giữ nguyên ô 💎 và ô đặc biệt.
- **Ô mới rơi** từ trên xuống, ngẫu nhiên 4 màu, không tạo sẵn hàng 3 khi sinh bàn.

| Ô (ban ngày) | Màu | Ban đêm thành | Công trình tăng chỉ báo |
|---|---|---|---|
| 🥩 Thịt | Đỏ | 🔥 Lửa | Trại săn |
| 🪵 Gỗ | Xanh lá | 🌪️ Khí | Xưởng gỗ |
| 💠 Mana | Xanh dương | 💧 Nước | Đền thờ |
| 🪙 Vàng | Vàng | 🪨 Đất | Mỏ vàng |

**Chỉ báo +N:** mỗi ô hiện số +N ở góc. N = 1 + số dân đang làm ở công trình tương ứng **tại lúc ô rơi xuống**. Ô đã nằm trên bàn giữ N cũ.

### 3.3 Ban ngày: kinh tế
- **Ghép ra tài nguyên = tổng chỉ báo +N của các ô đã ăn** (kể cả ô bị Quả cầu, Chữ thập ăn thêm). **Không nhân theo chuỗi rơi.** Chuỗi rơi chỉ có lợi vì ăn thêm ô.
- Ban ngày, Quả cầu và Chữ thập chỉ dọn thêm ô (gom thêm tài nguyên).
- **Ghép 5 ban ngày** sinh ô **💎 kim cương (tiền nạp)** thay vì ô đặc biệt: chạm = +1 💎; đổi 2 ô 💎 với nhau = +5 💎. Ô 💎 không bao giờ bị phá, không lật ban đêm, tối đa 5 ô trên bàn (quá 5 thì ghép 5 không sinh thêm).
- **Màn hình công trình:** chạm công trình (cả quán rượu) mở **toàn màn hình**, **đồng hồ ban ngày dừng**. Trong màn hình có kho hiện tại, nút nâng cấp, thuê dân, sản lượng.
- **Thịt:** mỗi bình minh, mỗi dân và mỗi pháp sư đang đứng ở cổng ăn 1 🥩. Thiếu thịt thì đêm sau pháp sư bị **Đói**: sức ×0,75.
- **Gỗ:** nâng cổng (+2 máu tối đa, hồi đầy) giá 12 🪵; nâng công trình.
- **Mana:** lên cấp pháp sư.
- **Vàng:** nâng công trình, thuê dân.

### 3.4 Công trình, dân, tự sản xuất

| Công trình | Sản xuất | Ghi chú |
|---|---|---|
| Trại săn | 🥩 | |
| Xưởng gỗ | 🪵 | |
| Đền thờ | 💠 | |
| Mỏ vàng | 🪙 | |
| Quán rượu | – | Quay thẻ pháp sư (3.8) |

- Mỗi cấp công trình mở **1 chỗ dân**. Bản thân cấp không tăng chỉ báo.
- Thuê dân bằng vàng vào chỗ trống. Mỗi dân: **+1 chỉ báo** cho ô mới rơi của loại đó, và **+1 tài nguyên mỗi 15 giây** (tự sản xuất).
- **Tự sản xuất chạy theo giờ thật**: lúc ghép, lúc ở màn hình công trình, cả ban đêm (chỉ ra tài nguyên, không ra nguyên tố).
- **Ngoại tuyến:** tắt game vẫn sản xuất, tích ở công trình tối đa **3 giờ** (Thẻ bạc 6 giờ, Thẻ vàng 10 giờ). Mở game lại thì chạm từng công trình để thu. Phần chưa thu nằm ở công trình, **không** tính là kho (không mất khi cổng vỡ).
- **Thời gian ngoại tuyến phải lấy từ máy chủ**, không lấy từ đồng hồ điện thoại (chống chỉnh giờ).

### 3.5 Ô đặc biệt

| Ghép | Ô sinh ra | Trên bàn khi kích hoạt | Ban đêm, pháp sư cùng màu |
|---|---|---|---|
| 4 thẳng | 🔮 Quả cầu (giữ màu) | Nổ 3 × 3 | Quả cầu nổ vùng quanh quái gần cổng nhất |
| L hoặc T | ✚ Chữ thập (giữ màu) | Xoá cả hàng và cột qua tâm | Cột nguyên tố giáng từ trời xuống quái gần cổng nhất; **luôn chí mạng, ×1,5** |
| 5 thẳng **ban đêm** | 🌈 Cầu vồng (giữ màu của lần ghép 5) | **Chạm** vào: ăn hết ô cùng màu với nó | **5 tia sét** vào quái bị màu đó khắc; hết quái bị khắc thì số tia còn lại đánh quái gần nhất |
| 5 thẳng **ban ngày** | 💎 Kim cương tiền nạp | Không nổ | – |

- Ô đặc biệt được kích hoạt khi nằm trong một hàng ghép, hoặc khi đổi với một ô đặc biệt khác.
- **Đổi 2 ô 🌈 với nhau = Bàn tay thần:** xoá toàn bộ bàn, tiêu diệt mọi quái đang có trên sân (kể cả trùm), màn hình rung, bàn tay khổng lồ.
- **🌈 + Quả cầu / Chữ thập cùng màu:** mọi ô màu đó hoá Quả cầu / Chữ thập rồi nổ (tổ hợp cấp 1).
- **Hai ô đặc biệt khác màu đổi với nhau = Phép cấp 2** (3.6).
- **Hai ô đặc biệt cùng màu:** Quả cầu + Quả cầu nổ 5 × 5; Quả cầu + Chữ thập xoá 3 hàng, 3 cột; Chữ thập + Chữ thập xoá 2 hàng, 2 cột (tổ hợp cấp 1).
- 🌈 còn lại lúc bình minh tự nổ thành **5 KN mỗi ô**.

### 3.6 Phép cấp 2 (30 phép)
Đổi hai ô đặc biệt **khác nguyên tố**. Tên và hiệu ứng thêm theo **cặp nguyên tố**. Cách đánh và hệ số theo **hình dạng** (Quả cầu < Chữ thập < Cầu vồng).

| Cặp | Tên | Hiệu ứng thêm lên mọi quái trúng đòn |
|---|---|---|
| 🔥 + 🌪️ | Bão lửa | Cháy 3 giây, mỗi giây 10% sát thương đòn |
| 🔥 + 🪨 | Dung nham | Cháy 3 giây, mỗi giây 15% |
| 🔥 + 💧 | Hơi nước | Đứng yên 1 giây |
| 🌪️ + 🪨 | Bão cát | Đẩy lùi 20% quãng đường |
| 🌪️ + 💧 | Bão tuyết | Đóng băng (đứng yên) 2 giây |
| 💧 + 🪨 | Lũ bùn | Chậm 50% trong 4 giây |

| Hình dạng | Tên đuôi | Trên bàn | Ban đêm | Hệ số |
|---|---|---|---|---|
| Quả cầu + Quả cầu | Vùng | Nổ 5 × 5 | Nổ vùng lớn quanh quái gần nhất | ×1 |
| Quả cầu + Chữ thập | Cột nổ | Xoá 3 hàng + 3 cột | Cột giáng xuống quái gần nhất rồi nổ lan | ×1,25 |
| Chữ thập + Chữ thập | Cột đôi | Xoá 2 hàng + 2 cột | 2 cột giáng xuống 2 quái gần nhất (chí mạng) | ×1,5 |
| Cầu vồng + Quả cầu | Mưa sao | Mọi ô màu của Quả cầu hoá Quả cầu và nổ | Nổ vùng trên 5 quái gần nhất | ×1,75 |
| Cầu vồng + Chữ thập | Thiên phạt | Mọi ô màu của Chữ thập hoá Chữ thập và nổ | 5 cột giáng xuống 5 quái gần nhất (chí mạng) | ×2 |

Cần đủ pháp sư cho cả hai màu ở cổng, nếu thiếu thì bàn vẫn nổ nhưng không ra phép (hiện "Cần đủ 2 pháp sư").

### 3.7 Pháp sư

**Bản phát hành đầu: 13 pháp sư.** Đủ bộ 29 người để dành cho các bản cập nhật (danh sách trong concept mục 6b).

| Nguyên tố | Thường (×1) | Anh hùng (×1,6) | Anh hùng sử thi (×2) |
|---|---|---|---|
| 🔥 Lửa | Salamander | Prometheus | Surtr |
| 🌪️ Khí | Sylph | Raven | Zeus |
| 💧 Nước | Undine | Ægir | Poseidon |
| 🪨 Đất | Gnome | Merlin | Gaia |
| ✨ Huyền thoại (×2,5) | Hermes Trismegistus: mang cả 4 nguyên tố | | |

- **Quy tắc tên:** thần thoại Hy Lạp, Bắc Âu, Châu Âu, và **biểu tượng phổ biến** của thổ dân Mỹ (không dùng đối tượng thờ cúng).
- **Khởi đầu:** tặng 4 pháp sư Thường (Salamander, Sylph, Undine, Gnome), cấp 1, đứng sẵn ở 4 bệ.
- **Cổng có 4 bệ**, mỗi bệ một nguyên tố. Bệ nhận pháp sư cùng nguyên tố hoặc **Huyền thoại**. Huyền thoại đứng ở bệ nào thì thay pháp sư của nguyên tố bệ đó: màu đó không có pháp sư riêng (không Quả cầu, Chữ thập, Ngũ lôi riêng), Huyền thoại đánh thay.
- **Huyền thoại:** đánh **mỗi lần ăn ô bất kỳ màu nào**, sát thương = tổng nguyên tố ăn được × sức × 2 (luôn khắc chế), bắn vào quái gần cổng nhất.
- **Khắc chế (vòng tròn):** 💧 > 🔥 > 🌪️ > 🪨 > 💧. Đánh hệ mình khắc ×2, hệ khắc mình ×0,5, cặp đối diện ×1.
- **Cấp:** lên cấp L → L+1 tốn 8L 💠. Cấp tối đa = 5 + 5 × tiềm năng (5 → 30).
- **Tiềm năng (tối đa 5):** bậc P tốn **4P thẻ trùng của chính pháp sư đó + 50P KN** (bậc 1–5: 4/8/12/16/20 thẻ, 50/100/150/200/250 KN; tổng 60 thẻ + 750 KN). Mỗi bậc +20% sức và mở thêm 5 cấp.
- **Nâng phẩm chất ⭐:** khi tiềm năng = 5, nâng **1 lần** lên bậc kế (Thường → Tinh anh → Anh hùng → Sử thi → Huyền thoại), miễn phí. Giữ nguyên cấp và tiềm năng, chỉ đổi hệ số phẩm chất. **Pháp sư đã nâng và pháp sư gốc cùng phẩm chất có sức như nhau.** Sử thi nâng lên Huyền thoại có hệ số ×2,5 nhưng vẫn **một nguyên tố**. Hermes không nâng.
- **Chưa có pháp sư ở bệ màu đó:** ghép màu đó không ra đòn.

### 3.8 Quán rượu (quay thẻ)
- 1 lượt = **1 💎**, ra 1 thẻ. Có nút quay 10 (10 💎). Thẻ của pháp sư chưa có: dùng 1 thẻ để triệu hồi. Thẻ trùng dùng cho tiềm năng.
- **Tỷ lệ công bố trong game (bản 13 pháp sư):** Thường 90% · Anh hùng 8,99% · Anh hùng sử thi 1% · Huyền thoại 0,01%. Trong mỗi phẩm chất, chia đều cho các pháp sư thuộc phẩm chất đó.
- **Bảo hiểm:** chắc chắn có Sử thi ở lượt thứ 100. Bộ đếm về 0 khi ra Sử thi hoặc Huyền thoại, giữ qua các ngày. Hiện số lượt còn lại trên màn hình quán.
- Khi có đủ 29 pháp sư: Thường 60 · Tinh anh 30 · Anh hùng 8,99 · Sử thi 1 · Huyền thoại 0,01.
- **Sự kiện tuần:** mỗi tuần một Huyền thoại "ghé quán" (hiện chỉ có Hermes). Tỷ lệ 0,01% ra Huyền thoại đang ghé.
- Kết quả quay **phải do máy chủ quyết định** (có tiền thật).

### 3.9 Ban đêm: quái và cổng

| Loại | Tỷ lệ sinh | Máu (đêm đầu) | Tốc độ | Đánh cổng | KN khi giết |
|---|---|---|---|---|---|
| Sói thường | 80% | 6 | Tới cổng trong 5 giây | 1 | 1 |
| Sói to | 20% | 15 | 5 giây | 3 | 3 |
| Trùm (mỗi đêm 1 con) | Xuất hiện ở giây 38 | 40 | Chậm gấp đôi (10 giây) | 5 | 10 |

- Quái sinh **mỗi 2 giây** trong 45 giây (khoảng 22 con + 1 trùm), ở vị trí ngang ngẫu nhiên, đi thẳng xuống cổng.
- Nguyên tố: quái thường ngẫu nhiên; **trùm được báo trước nguyên tố ngay từ đầu ban ngày** (kèm "khắc nó: …").
- Quái chạm cổng: trừ máu cổng rồi biến mất (không cho KN).
- **Quái không cản trở bàn ghép**: không ném ô, không khoá ô.
- **Chế độ chậm** (tuỳ chọn trong cài đặt): quái tới cổng trong 7 giây.
- **Máu cổng** đầu game 10; nâng cổng +2.
- **Độ khó theo phong độ:** sau một đêm thắng, nếu cổng mất **dưới 30%** máu tối đa thì **độ mạnh quái ×1,05** (cả máu và sát thương vào cổng, cả trùm). Mất từ 30% trở lên thì giữ nguyên. Không bao giờ giảm.
- **KN:** giết quái nhận KN theo bảng × độ mạnh quái hiện tại, vào **kho KN chung**. Đây là **phần thưởng duy nhất** của ban đêm (cùng 5 KN cho mỗi 🌈 thừa).

### 3.10 Cổng về 0
Trận dừng, hiện màn hình:
1. **5 💎: hồi 50% máu cổng** và đánh tiếp ngay (quái trên sân giữ nguyên).
2. **Xem video +4 💎** (tính vào giới hạn 10 video/ngày). Thẻ vàng: nhận ngay, không xem. Thẻ bạc: dùng vé nếu còn.
3. **Chịu thua:** **mất hết tài nguyên trong kho** (🥩 🪵 💠 🪙). **Không mất:** 💎, pháp sư, thẻ, KN, công trình, dân, sản lượng ngoại tuyến chưa thu. Sau đó chơi lại đêm đó với cổng 50% máu, độ mạnh quái giữ nguyên.

Ban ngày cũng có nút **5 💎 hồi 50% máu cổng** (không vượt máu tối đa).

### 3.11 Sách phép
- 47 trang: 17 phép cấp 1 (mỗi nguyên tố: Tia, Quả cầu, Cột, Ngũ lôi; cộng Bàn tay thần) và 30 phép cấp 2.
- Trang mở khoá **lần đầu phép được dùng trong trận đêm**, có thông báo "📖 Mở khoá: …".
- Mỗi trang ghi: cách ghép, công thức sát thương, hiệu ứng, số lần dùng, đòn mạnh nhất.

### 3.12 Kiếm tiền

| Sản phẩm | Giá | Nội dung |
|---|---|---|
| Video có thưởng | Miễn phí | +4 💎 mỗi video, **tối đa 10 video/ngày** |
| Thẻ bạc (thuê bao tháng) | 2,99 USD | Kho ngoại tuyến 6 giờ; **10 vé miễn video mỗi tháng**, vé chưa dùng tích lại |
| Thẻ vàng (thuê bao tháng) | 4,99 USD | Kho ngoại tuyến 10 giờ; **miễn mọi video** (nhận thưởng ngay; vẫn tính giới hạn 10/ngày) |
| Gói kim cương | Theo bậc giá App Store | Dùng quay thẻ, hồi cổng |
| Gói khởi đầu, skin (sau P0) | – | – |

Kim cương miễn phí chỉ từ: ghép 5 ban ngày (3.3) và video. Kim cương không bao giờ mất.

### 3.13 30 giây đầu (hướng dẫn không chữ)
1. 0–3 giây: đêm, bầy sói Khí tràn xuống, bàn tay mờ chỉ cách vuốt.
2. 3–8 giây: lần vuốt đầu được dàn sẵn thành chuỗi rơi 3 tầng ô Lửa (Lửa khắc Khí), số lớn bật lên.
3. 8–14 giây: ghép 4 có sẵn, Quả cầu nổ giữa bầy sói.
4. 14–20 giây: ghép chữ T có sẵn, cột lửa giáng xuống, chữ CRIT.
5. 20–26 giây: sói đầu đàn lao vào cổng. Ghép 5 có sẵn ra 🌈 cạnh một 🌈 quà tặng, đổi 2 ô → Bàn tay thần.
6. 26–30 giây: bình minh, một ô 💎 rơi làm quà (+1 💎), làng hiện ra, "Đêm 1: sống sót".

---

## 4. Công thức

| Tên | Công thức | Biến |
|---|---|---|
| Tài nguyên ban ngày | `R_c = Σ N_i` của các ô màu c bị ăn trong lượt (mọi tầng rơi) | N_i = chỉ báo của ô i |
| Chỉ báo ô mới | `N = 1 + workers[c]` | workers tại lúc ô rơi |
| Tự sản xuất | mỗi 15 giây: `res[c] += workers[c]`; ngoại tuyến: `min(t_off, cap) / 15 × workers[c]` | cap = 3 / 6 / 10 giờ |
| Giá nâng công trình (cấp L → L+1) | `gỗ = round(10 × 1,6^L)`, `vàng = round(5 × 1,6^L)` | L bắt đầu 0 |
| Giá thuê dân thứ k+1 | `vàng = round(8 × 1,5^k)` | k = số dân đang có |
| Sức pháp sư | `P = cấp × M[phẩm chất] × (1 + 0,2 × tiềm năng) × (Đói ? 0,75 : 1)` | M = 1 / 1,25 / 1,6 / 2 / 2,5 |
| Cấp tối đa | `5 + 5 × tiềm năng` | |
| Giá lên cấp | `8 × L` 💠 | L = cấp hiện tại |
| Giá tiềm năng bậc P | `4P` thẻ + `50P` KN | P = 1..5 |
| Đòn cấp 1 | `D = n × P × K(c, e)` (Chữ thập ×1,5) | n = tổng nguyên tố màu c ăn được trong lượt; K = 2 / 1 / 0,5 |
| Đòn Huyền thoại | `D = n_tất_cả × P × 2` | |
| Ngũ lôi (🌈) | mỗi tia `n × P × K` | 5 tia |
| Phép cấp 2 | `D = n × (P_a + P_b)/2 × 2 × S × max(K(a,e), K(b,e))` | S = 1 / 1,25 / 1,5 / 1,75 / 2 |
| Cháy | mỗi giây `D × 0,10` (Bão lửa) hoặc `D × 0,15` (Dung nham), 3 giây | |
| Độ mạnh quái | `F ← F × 1,05` nếu `(máu đầu đêm − máu cuối đêm) / máu tối đa < 0,30` | F khởi đầu 1 |
| Máu quái | `HP = HP_gốc × F` | gốc 6 / 15 / 40 |
| Sát thương cổng | `dmg = dmg_gốc × F` | gốc 1 / 3 / 5 |
| KN mỗi con | `KN_gốc × F` | gốc 1 / 3 / 10 |
| Thịt bình minh | `ăn = số dân + số pháp sư ở cổng` | |

**Ví dụ:** Anh hùng cấp 30, tiềm năng 5: 30 × 1,6 × 2 = 96. Nâng lên Sử thi: 30 × 2 × 2 = 120, bằng Sử thi gốc cấp 30, tiềm năng 5.

---

## 5. Trường hợp đặc biệt

| Tình huống | Xử lý |
|---|---|
| Đang chuỗi rơi thì hết giờ ban ngày | Chờ chuỗi rơi xong rồi mới sang hoàng hôn |
| Đang chuỗi rơi thì cổng về 0 | Chuỗi rơi tính xong (có thể giết quái) rồi mới kiểm tra cổng |
| Không còn quái mà chưa hết 45 giây | Quái vẫn sinh tiếp tới giây 45; đêm kết thúc khi hết 45 giây và sân trống |
| Ghép màu không có pháp sư | Bàn vẫn nổ, không ra đòn, hiện chữ nhỏ "chưa có" trên bệ |
| Phép cấp 2 thiếu 1 pháp sư | Bàn nổ theo hình dạng, không ra phép, báo "Cần đủ 2 pháp sư" |
| Huyền thoại ở bệ màu X, người chơi ghép màu X | Chỉ Huyền thoại đánh (không có đòn riêng màu X) |
| Hai 🌈 khác màu đổi nhau | Luôn là Bàn tay thần |
| Đã có 5 ô 💎 trên bàn mà lại ghép 5 ban ngày | Không sinh thêm 💎, hàng 5 nổ như ghép thường |
| Ô 💎 ban đêm | Nằm yên, không lật, không bị nổ; vẫn chạm được để nhận 💎 |
| Hết nước đi | Tự xáo, giữ ô 💎 và ô đặc biệt |
| Thiếu thịt lúc bình minh | Kho thịt về 0, pháp sư Đói (×0,75) đêm sau; dân không bỏ đi ở P0 |
| Chịu thua khi kho đã trống | Không mất gì thêm, chơi lại đêm |
| Đổi giờ điện thoại để lấy ngoại tuyến | Thời gian lấy từ máy chủ; mất mạng thì không tính ngoại tuyến |
| Mất mạng khi đang quay thẻ | Lượt chỉ tính khi máy chủ xác nhận; không trừ 💎 nếu chưa xác nhận |
| Video không tải được | Không trừ lượt video trong ngày, báo thử lại |
| Quay đúng lượt 100 ra Huyền thoại | Tính là lượt bảo hiểm đã dùng (bộ đếm về 0) |
| Thẻ của pháp sư đã nâng phẩm chất | Vẫn là thẻ của pháp sư đó; tiềm năng đã 5 nên thẻ thừa để dành (sau này đổi điểm, chưa chốt) |
| Mở màn hình công trình ban đêm | Không cho mở (chạm làng chỉ có tác dụng ban ngày) |

## 6. Phụ thuộc giữa các hệ thống

```
Bàn ghép ──ngày──> Tài nguyên ──> Công trình/Dân ──> Chỉ báo +N, Tự sản xuất
   │                    │
   │                    └──> Mana ──> Cấp pháp sư
   └──đêm──> Nguyên tố ──> Pháp sư (cổng) ──> Quái ──> KN ──> Tiềm năng
                                  ▲                          ▲
Quán rượu (💎) ──> Thẻ ───────────┴──────────────────────────┘
Video / IAP / ghép 5 ──> 💎 ──> Quán rượu, Hồi cổng
Cổng vỡ ──> Mất kho tài nguyên
Máy chủ: thời gian ngoại tuyến, quay thẻ, IAP, giới hạn video, lưu tiến trình
```

## 7. Thông số cân bằng (để trong file cấu hình)

| Nhóm | Thông số | Giá trị | Khoảng thử an toàn |
|---|---|---|---|
| Nhịp | Ban ngày | 120 giây | 90–180 |
| | Thời gian sinh quái ban đêm | 45 giây | 35–60 |
| | Quái sinh mỗi | 2 giây | 1,5–3 |
| | Quái tới cổng | 5 giây (chậm 7) | 4–8 |
| | Trùm xuất hiện | giây 38 | 30–45 |
| Bàn | Kích thước | 7 × 7 | cố định |
| | Số loại ô | 4 | cố định (3 làm vỡ bàn) |
| | Ô 💎 tối đa | 5 | 3–8 |
| Quái | Máu sói / sói to / trùm | 6 / 15 / 40 | |
| | Đánh cổng | 1 / 3 / 5 | |
| | Tỷ lệ sói to | 20% | 10–35% |
| | Tăng độ mạnh | ×1,05, ngưỡng 30% | 1,03–1,08; 20–40% |
| Cổng | Máu đầu, nâng cổng | 10, +2 với 12 🪵 | |
| | Hồi cổng | 5 💎 = 50% | |
| Kinh tế | Giá công trình | 10 × 1,6^L 🪵 + 5 × 1,6^L 🪙 | hệ số 1,4–1,8 |
| | Thuê dân | 8 × 1,5^k 🪙 | 1,3–1,8 |
| | Tự sản xuất | 1 mỗi dân mỗi 15 giây | 10–30 giây |
| | Kho ngoại tuyến | 3 / 6 / 10 giờ | |
| | Thịt mỗi người mỗi bình minh | 1 | |
| | Đói | ×0,75 | |
| Pháp sư | Hệ số phẩm chất | 1 / 1,25 / 1,6 / 2 / 2,5 | |
| | Giá cấp | 8L 💠 | |
| | Tiềm năng | 4P thẻ + 50P KN, tối đa 5 | |
| | KN mỗi con | 1 / 3 / 10, 🌈 thừa 5 | |
| Đòn | Chí mạng Chữ thập | ×1,5 | |
| | Ngũ lôi | 5 tia | |
| | Hệ số phép cấp 2 | 2 × (1 / 1,25 / 1,5 / 1,75 / 2) | |
| Kiếm tiền | Quay | 1 💎/lượt | |
| | Tỷ lệ (bản 13) | 90 / 8,99 / 1 / 0,01 | công bố |
| | Bảo hiểm Sử thi | lượt 100 | |
| | Video | +4 💎, 10/ngày | |
| | Thẻ bạc / vàng | 2,99 / 4,99 USD | |

## 8. Tiêu chí nghiệm thu

**Chức năng (tự động kiểm tra được)**
1. Ghép 3 ô thịt +1 ra đúng 3 thịt; ô +3 ra 9; chuỗi rơi không nhân thêm.
2. Ô mới rơi có N = 1 + số dân của công trình tương ứng tại thời điểm rơi.
3. Ở màn hình công trình, đồng hồ ban ngày không chạy; tự sản xuất vẫn chạy.
4. Công trình có k dân cộng đúng k tài nguyên mỗi 15 giây; ngoại tuyến không vượt 3/6/10 giờ.
5. Giá nâng cấp và thuê dân khớp công thức mục 4 ở bậc 1, 2, 3, 5, 8, 10 (bảng trong concept mục 6).
6. Đêm thắng với cổng mất 29% thì độ mạnh ×1,05 (máu và sát thương); mất 30% thì không đổi.
7. Cổng về 0: có 3 lựa chọn; trả 5 💎 thì cổng = 50% và quái giữ nguyên; chịu thua thì 4 tài nguyên về 0, 💎/pháp sư/thẻ/KN không đổi.
8. Video tăng 4 💎; lần thứ 11 trong ngày bị chặn.
9. Mô phỏng 1.000.000 lượt quay: tỷ lệ sai lệch < 0,1 điểm % so với công bố; không có chuỗi nào quá 100 lượt không ra Sử thi.
10. Lên tiềm năng P trừ đúng 4P thẻ và 50P KN; không đủ một trong hai thì nút tắt.
11. Nâng phẩm chất chỉ khi tiềm năng 5, chỉ 1 lần; sức sau khi nâng bằng pháp sư gốc cùng phẩm chất, cùng cấp, cùng tiềm năng.
12. Huyền thoại đặt được ở mọi bệ; khi ở bệ màu X, ghép màu X không gọi đòn riêng màu X.
13. Đủ 30 phép cấp 2 và 17 phép cấp 1 mở được trang sách khi dùng lần đầu.

**Cảm giác (chơi thử với 10–20 người, P0)**
- Thời gian từ lúc ô nổ tới lúc phép chạm quái ≤ 0,3 giây.
- 30 FPS ổn định trên iPhone 11; 60 FPS trên iPhone 13 trở lên.
- Tỷ lệ thắng đêm 1–3 ≥ 80% với người mới.
- Ít nhất 70% người chơi thử nói họ thấy rõ đòn của pháp sư nào khi ghép màu nào.
- Đo: % bỏ game ngay sau lần mất kho đầu; % xem video; % trả 5 💎 hồi cổng; độ mạnh quái sau 10 đêm.

## 9. Phạm vi đề xuất cho P0 (CTO quyết định cuối)

| Có trong P0 | Để sau |
|---|---|
| Bàn 7 × 7, ngày/đêm, 4 công trình + quán rượu, dân, tự sản xuất (ngoại tuyến giả lập) | Máy chủ thật, IAP thật, thẻ tháng thật |
| 13 pháp sư (có thể bắt đầu với 8: Thường + Anh hùng) | Đủ 29 pháp sư, Tinh anh |
| Quả cầu, Chữ thập, Cầu vồng, Bàn tay thần | Phép thần, thanh Lòng tin (concept mục 5, chưa chốt) |
| 6 phép cấp 2 hình "Vùng" trước, sau đó đủ 30 | Thời đại, kỳ quan, sự kiện tuần |
| Quay thẻ chạy trên máy, video giả lập | Bang hội, bảng xếp hạng |
| Sách phép, độ khó theo phong độ, cổng vỡ mất kho | Đổi điểm quay thẻ (chưa chốt) |
| 10 đêm chơi được, phân tích cơ bản | |

**Bản chơi thử trình duyệt** đã có hầu hết luật trên (trừ máy chủ, ngoại tuyến, thẻ tháng): dùng để đối chiếu hành vi khi làm bản engine.

## 10. Câu hỏi còn mở
1. Giới hạn số ô 💎 ban ngày mỗi ngày thật (đề xuất 10 ô)? Hiện khoảng 20–60 💎 miễn phí mỗi 20 phút chơi.
2. Đổi điểm quay thẻ (đề xuất 300 lượt đổi 1 Sử thi tuỳ chọn)?
3. Thưởng khi mở hết một nhóm trang sách phép là gì?
4. Thời đại, kỳ quan, phép thần (Lòng tin) có vào bản phát hành đầu không?
5. Engine (Unity hay Godot) và cách làm máy chủ: CTO đề xuất.
6. Phong cách hình ảnh: anh chốt sau khi Artist đưa 2–3 hướng.
