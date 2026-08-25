# Track1_Day23_2A202601955_NguyenQuangMinh
## Metrics Pack — AI Travel Planner

---

## 00 — Phạm vi

- **Dự án**: AI Travel Planner — trợ lý AI giúp lên kế hoạch chuyến đi qua hội thoại (nhập sở thích, ngân sách, thời gian → AI đề xuất lịch trình theo ngày).
- **Persona**: Người đi du lịch tự túc (solo hoặc nhóm nhỏ), tự lên lịch trình cho chuyến đi sắp tới thay vì mua tour trọn gói.
- **Core job**: "Tôi mất hàng giờ nhảy giữa review, bản đồ, và lịch cá nhân để ghép ra một lịch trình hợp lý, mà vẫn không chắc nó có khả thi về thời gian di chuyển hay không — rốt cuộc vẫn lo sẽ vỡ kế hoạch khi tới nơi."
- **Use case phân tích sâu**: tạo và tinh chỉnh lịch trình chuyến đi (itinerary building) qua hội thoại với AI. Không phân tích các phần khác của sản phẩm (đặt phòng, thanh toán, cộng đồng chia sẻ...).

---

## 01 — Core Action

### 1. Phân biệt bốn khái niệm

| Khái niệm | Câu hỏi | Trả lời (AI Travel Planner) |
|---|---|---|
| **Core job** | User đang cố hoàn thành việc gì? | Có một kế hoạch chuyến đi đủ cụ thể, khả thi để tin tưởng đi theo — không lo vỡ lịch khi đã tới nơi |
| **Core action** | User làm gì trong sản phẩm để tiến tới giá trị? | Chốt (accept/save) một ngày lịch trình do AI đề xuất vào kế hoạch chuyến đi thật |
| **Core value** | User nhận được lợi ích gì? | Có một phần lịch trình cụ thể, đáng tin cậy để dùng thật — bớt lo lắng, bớt công sức tự ghép kế hoạch |
| **Core value event** | Sự kiện nào chứng minh value đã xảy ra? | `itinerary_day_accepted` |

*Core action và core value event trùng nhau ở đây (hành vi chốt = sự kiện xác nhận value), vì bản thân hành động "chốt" đã là cam kết dùng thật — không có bước trung gian nào khác (khác với ví dụ đặt xe: bấm đặt là action, xe chạy xong mới là event xác nhận value).*

### 2. Core Action Card

| Thành phần | Câu trả lời |
|---|---|
| **Target user** | Người du lịch tự túc đang có một chuyến đi cụ thể sắp tới (đã nhập điểm đến + khung thời gian) |
| **Core job** | "Tôi mất hàng giờ nhảy giữa review, bản đồ, lịch cá nhân để ghép ra một lịch trình hợp lý, vẫn không chắc nó khả thi — lo sẽ vỡ kế hoạch khi tới nơi." |
| **Core action** | Chốt (accept/save) một ngày lịch trình do AI đề xuất vào kế hoạch chuyến đi |
| **Object** | Một "ngày lịch trình" (itinerary day) — tập hoạt động/địa điểm theo khung giờ cho một ngày cụ thể trong chuyến đi |
| **Preconditions** | User đã tạo chuyến đi (điểm đến + khoảng ngày) VÀ AI đã sinh ra ít nhất một đề xuất cho ít nhất một ngày |
| **Completion rule** | Trạng thái của ngày lịch trình chuyển từ `suggested` → `accepted` (user bấm nút chốt/lưu — không tính chỉ xem) |
| **Core value** | Có một phần kế hoạch chuyến đi cụ thể, đáng tin để dùng thật, thay vì tự ghép từ nhiều nguồn |
| **Evidence of value** | User quay lại chốt thêm ngày khác trong cùng chuyến, không xóa/làm lại ngày đã chốt, và có xu hướng tiến tới chốt toàn bộ chuyến |
| **Candidate event** | `itinerary_day_accepted` |

### 3. Tự kiểm 5 tiêu chí

| Tiêu chí | Đạt? | Giải thích |
|---|---|---|
| **Gần core value** | ✅ | Chốt một ngày = cam kết dùng thật; không có hành vi nào khác trong luồng đứng gần "có kế hoạch đáng tin" hơn |
| **Có thể lặp lại** | ✅ | Lặp lại mỗi khi còn ngày chưa chốt trong 1 chuyến, và lặp lại ở mỗi chuyến đi mới |
| **Có thể quan sát** | ✅ | Completion rule rõ ràng (`suggested` → `accepted`), track chính xác thời điểm xảy ra |
| **Có ý nghĩa** | ✅ | Accept rate tăng phản ánh thật sự AI đề xuất tốt hơn / sản phẩm hữu ích hơn — không dễ "làm giả" bằng cách chỉ tăng lượt mở app hay số prompt gửi đi |
| **Có thể tác động** | ✅ | Team cải thiện được: chất lượng đề xuất AI, mức độ cá nhân hóa theo preferences, UX của thao tác chốt |

**Kết quả: 5/5 — đạt.**

**Vì sao không chọn "mở app" hay "hỏi AI":**
- "Mở app" là thao tác vào giao diện — không nói lên gì về việc user có nhận được giá trị hay không, không có object, không có completion rule ý nghĩa.
- "Hỏi AI" (gửi prompt) chỉ là input; kết quả AI trả về là **output hệ thống**, không phải hành vi của user tạo ra giá trị — user có thể hỏi liên tục mà không hài lòng với bất kỳ gợi ý nào, hành vi đó tăng lên không có nghĩa sản phẩm tốt hơn (trượt tiêu chí "có ý nghĩa"). Chỉ khi user chủ động **chốt** một đề xuất cụ thể, hành động đó mới vừa quan sát được, vừa thật sự tiến gần core value, vừa có thể bị "spam" hoá thấp hơn nhiều so với việc gửi prompt.

**GATE 1 — Core action đứng vững: ĐẠT** (có actor/object/completion rule rõ ràng; 5/5 tiêu chí tự kiểm; giải thích được vì sao khác "mở app"/"hỏi AI").

---

## 02 — Nature & cadence

### 1. Action Nature Card

| Thành phần | Trả lời |
|---|---|
| **Actor** | Cá nhân user đang lên kế hoạch cho một chuyến đi cụ thể của chính họ (không phải account chung hay object hệ thống) |
| **Intent** | Nhu cầu có một kế hoạch cụ thể, đáng tin cho **một** chuyến đi sắp diễn ra — không phải nhu cầu thường trực, chỉ nảy sinh khi đã có ý định đi đâu đó |
| **Trigger** | Chủ động bởi user, khởi phát khi họ quyết định "sắp có một chuyến đi" (gắn với dịp cụ thể: kỳ nghỉ, lễ, sự kiện) — không phải do hệ thống hay người khác kích hoạt |
| **Effort** | Trung bình–cao ở lần đầu mỗi chuyến (nhập preferences, ngân sách, so sánh nhiều gợi ý); giảm dần ở các ngày còn lại của cùng chuyến |
| **Value timing** | Xuất hiện gần như ngay sau mỗi lần chốt (thấy kế hoạch rõ hơn), nhưng **tích lũy** — value "trọn vẹn" chỉ đạt khi cả chuyến được lấp đầy |
| **State** | Chuyến đi (trip) cùng danh sách ngày lịch trình ở trạng thái `suggested`/`accepted`/`rejected` được lưu lại, gắn với trip record đó |
| **Dependency** | Phụ thuộc việc **có một chuyến đi thật sắp diễn ra** (điểm đến + khoảng ngày đã nhập); nếu đi nhóm, có thể phụ thuộc thêm sự đồng thuận của người đi cùng |
| **Repeat condition** | Lặp lại khi (a) trong cùng chuyến còn ngày chưa chốt, hoặc (b) một chuyến đi **mới** được lên kế hoạch — hết hai điều kiện này thì không còn lý do tự nhiên để hành vi xuất hiện lại |

### 2. Kết luận cadence

**Dạng hành vi**: theo dự án (project-based) — dồn cụm quanh vòng đời của mỗi chuyến đi, không phải thói quen lặp đều.

> Đối với **người du lịch tự túc đang lên kế hoạch cho một chuyến đi cụ thể**, core action **chốt ngày lịch trình** thường xuất hiện **dồn cụm trong một cửa sổ ngắn (vài ngày–vài tuần) quanh mỗi chuyến đi, rồi ngưng hẳn cho tới chuyến kế tiếp** vì **nhu cầu gắn chặt với sự tồn tại của một chuyến đi sắp diễn ra, không phải nhu cầu thường trực — một khi chuyến đã lên xong, không còn lý do tự nhiên nào để quay lại cho tới khi có chuyến mới**. Do đó, nhịp đo phù hợp là **theo chu kỳ chuyến đi (trip-cycle), tổng hợp theo tháng/quý ở cấp cohort**, ở cấp **trip — không phải ngày**.

**Vì sao không dùng daily/weekly mặc định:**
Không chọn D1/W1 chỉ vì dashboard hay dùng — sản phẩm này không có nhu cầu tự nhiên nào lặp lại theo ngày/tuần. Ép nhịp đó sẽ luôn ra số thấp và đánh giá sai sản phẩm.

**Frequency cao hơn ≠ value cao hơn:**
Với use case này, một user gửi ít prompt hơn nhưng **chốt được cả chuyến nhanh hơn, ít vòng chỉnh sửa hơn** là tín hiệu AI tốt — không phải user tương tác nhiều lần mới là tốt. Vì vậy hệ metric ở Phase 3 ưu tiên **accept rate / completion** hơn là đếm số lượt tương tác.

**GATE 2 — Cadence từ nature, không từ dashboard: ĐẠT** (kết luận đúng template, có lý do "vì" đứng được từ Nature Card, nhịp đo trip-cycle không mâu thuẫn với dạng hành vi "theo dự án").

---

## 03 — Metric System

### Active ≠ Activated
| Trạng thái | Định nghĩa |
|---|---|
| **Active** | User mở app / gửi ít nhất 1 tin nhắn cho AI trong kỳ đo — **không chứng minh giá trị** |
| **Activated** | User đã thực hiện Core Action lần đầu (chốt ≥1 ngày lịch trình) — **mốc chứng minh giá trị đầu tiên**, xác suất họ hoàn thành cả trip và quay lại cho chuyến sau cao hẳn so với user chỉ "active" |

### 1. Activation metric

| Thành phần | Trả lời |
|---|---|
| **Start event** | `trip_created` — user tạo chuyến đi mới (nhập điểm đến + khung thời gian) |
| **Activation event** | `itinerary_day_accepted` — chốt core action đầu tiên (≥1 ngày lịch trình được accept) |
| **Time window** | 48 giờ kể từ `trip_created` |

*Vì sao 48h, không phải "đăng nhập" hay "xem hết tour giới thiệu": cả hai cái đó user chưa chạm core value — họ có thể đăng nhập, lướt qua tour rồi rời đi mà chưa từng thấy một ngày lịch trình nào đáng tin. 48h được chọn vì đó là khung thời gian đủ để user thử ít nhất một vòng đề xuất → chỉnh → chốt trong phiên lập kế hoạch ban đầu, mà vẫn đủ chặt để phản ánh "giá trị phải đến sớm" chứ không đợi cả tuần.*

### 2. Engagement metric

Chọn 2 góc đo (bỏ breadth — phạm vi bài chỉ phân tích 1 use case, đo breadth ở đây vô nghĩa):

| Góc đo | Định nghĩa | Vì sao chọn |
|---|---|---|
| **Frequency** | Số ngày lịch trình được accept / tổng số ngày của chuyến, trong vòng đời 1 trip | Phản ánh trực tiếp mức độ core action lặp lại trong đúng cadence tự nhiên (trip-cycle), không phải theo ngày |
| **Depth** | Tỷ lệ ngày đã accept **không** bị chỉnh sửa lại sau đó (edit-after-accept thấp = mỗi lần accept tạo nhiều value hơn, đúng ngay từ đầu) | Đo "chất" của mỗi lần core action, tránh đếm số lượt tương tác như một proxy giả cho giá trị |

### 3. North Star Metric + leading + counter

**North Star Metric** (unit of value + quality threshold + frequency):
> **Tỷ lệ chuyến đi đạt "Completed Itinerary chất lượng"** — chuyến có ≥80% số ngày được accept **và** giữ nguyên không bị sửa/xoá trong 7 ngày trước ngày khởi hành — trên tổng số chuyến được tạo, tính theo quý.

- *Unit of value*: 1 chuyến đi có lịch trình hoàn chỉnh (`trip_itinerary_completed` / `trip_created`)
- *Quality threshold*: ≥80% số ngày accepted, ổn định (không bị đảo lại sát ngày đi — tránh tính cả những lịch trình "chốt cho có" rồi bỏ)
- *Frequency*: theo quý — khớp cadence theo dự án đã kết luận ở Phase 2

*(Sửa so với bản nháp trước: bỏ chia theo "mỗi user active" — khái niệm đó chưa có event nào track được, vi phạm luật "metric nào cũng phải có event để tính". Dùng `trip_created` làm denominator vì event này đã có sẵn trong bảng tracking ở mục 06.)*

**Leading indicators** (tối đa 3):

| Indicator | Vì sao tin nó dự báo core action lặp lại |
|---|---|
| **Suggestion Accept Rate** | Accept rate cao ở những ngày đầu chứng tỏ AI bắt đúng preferences ngay từ đầu → xác suất user tiếp tục chốt hết các ngày còn lại (thay vì bỏ giữa chừng) cao hơn hẳn |
| **Time-to-first-accept** | Chốt nhanh nghĩa là user thấy giá trị ngay, ít ma sát → nhóm activate nhanh có tỷ lệ hoàn thành cả trip cao hơn rõ rệt so với nhóm lưỡng lự lâu |
| **Edit-after-accept rate** (nghịch) | Tỷ lệ càng thấp, AI càng sát nhu cầu thật → dự báo user tiếp tục tin dùng AI cho các ngày còn lại thay vì tự làm tay, giảm rủi ro bỏ sản phẩm giữa chừng |

**Counter-metric** (core action tăng nhưng cái gì không được xấu đi):
> **Late itinerary abandonment rate** — tỷ lệ ngày đã accept nhưng bị huỷ/thay đổi lại trong 7 ngày trước khởi hành. Nếu Suggestion Accept Rate tăng chỉ vì user "chốt bừa cho AI ngừng hỏi" chứ không thật sự tin, chỉ số này sẽ tăng theo — đó là dấu hiệu accept rate đang bị game, không phản ánh chất lượng thật.

---

## 04 — Retention Definition

Retention phải khớp kết luận cadence ở Phase 2 (theo dự án, đo ở cấp trip) — **không dùng D7/D30** vì action không xảy ra theo tuần/tháng cố định.

| Thành phần | Trả lời |
|---|---|
| **Unit** | User (cá nhân tạo trip) — dù có thể đi nhóm, người quyết định lịch trình là đơn vị đo |
| **Cohort entry** | Event `itinerary_day_accepted` **đầu tiên** trong đời user (tức thời điểm activated) — cohort nhóm theo **tháng activated**, không theo tháng signup (signup có thể xảy ra rất lâu trước khi có chuyến đi thật) |
| **Return event** | `itinerary_day_accepted` xảy ra lại trên **một trip khác** (chuyến mới) — không tính việc quay lại chỉnh sửa trip cũ |
| **Window** | Project-based / custom bracket: 6 tháng (N1), khớp với cadence trip-cycle đã kết luận ở Phase 2 (trung bình 2–4 chuyến/năm) |
| **Threshold** | 1 lần là đủ trong window — bản chất project-based, không cần lặp nhiều lần mới tính là "quay lại" |
| **Segment** | User đã activated (có ít nhất 1 chuyến hoàn chỉnh), tách theo cohort tháng activated; có thể chia nhỏ thêm solo vs nhóm nếu cần độ chi tiết cao hơn |

### 3 mốc retention (so sánh, không so với số cứng — theo S34)

| Mốc | Định nghĩa | Dùng để so sánh với |
|---|---|---|
| **Natural cycle (N1)** | Return event trong vòng 6 tháng | Có khớp đúng nửa chu kỳ đi chơi trung bình của chính segment đó không |
| **Cohort đúng segment** | So cohort tháng activated này với cohort tháng khác | Không so với một con số tuyệt đối duy nhất — mùa cao điểm/thấp điểm sẽ lệch nhau tự nhiên |
| **Benchmark category** | So với các sản phẩm cùng nhịp project-based (travel planning, không phải app daily-habit) | Tránh so sánh sai chuẩn với benchmark của app mạng xã hội/nhắn tin |

> Ghi chú: khung 6 tháng là **giả định ban đầu**, cần hiệu chỉnh khi có dữ liệu tần suất đi chơi thật của segment.

### Metric Definition Contract — "Suggestion Accept Rate"
- **Name**: Suggestion Accept Rate
- **Definition**: Tỷ lệ số ngày lịch trình do AI đề xuất được user chốt (accept), trên tổng số ngày AI đã đề xuất, tính theo chuyến đi.
- **Numerator**: `count(itinerary_day_accepted)`
- **Denominator**: `count(itinerary_day_suggested)`
- **Window**: Trong vòng đời của 1 trip (từ lúc tạo trip đến ngày khởi hành hoặc đến khi trip bị hủy)
- **Exclusion**: Loại các gợi ý bị AI tự thu hồi do lỗi hệ thống (không phải do user từ chối)
- **Owner**: Product/AI quality team

**GATE 3 — Metric tính được, retention đủ nghĩa: ĐẠT**
- Activation có start event + activation event + time window (48h) rõ ràng.
- Retention đủ 6/6 thành phần, window (6 tháng) khớp cadence "theo dự án" từ Phase 2.
- NSM đúng công thức 3 thành phần (unit + quality threshold + frequency), không phải revenue/lượt mở app.
- Có ít nhất 1 counter-metric (Late itinerary abandonment rate) chống game hoá accept rate.

---

## 05 — Product Loop

**Loại loop chính**: **project loop** — mỗi vòng gắn với một chuyến đi cụ thể, có điểm "hoàn thành" rõ ràng rồi lặp lại ở dự án (chuyến đi) tiếp theo, đúng với kết luận cadence "theo dự án" ở Phase 2.

**Sơ đồ 2 chu kỳ:**

```
Chu kỳ 1 (trong 1 chuyến đi):
Natural trigger: user có ý định cho 1 chuyến đi cụ thể sắp tới
  → Core action: chốt (accept) 1 ngày lịch trình do AI đề xuất
  → Immediate value: thấy ngay 1 phần kế hoạch cụ thể, đáng tin — bớt lo
  → Saved state / investment: ngày đó được lưu vào trip record, trip "đầy" dần
  → Next natural trigger: còn ngày khác trong cùng chuyến chưa có kế hoạch
  → Core action tiếp theo: chốt ngày kế tiếp
  → Repeat value: kế hoạch càng đầy, càng tin để dùng thật → Trip Itinerary Completed

Chu kỳ 2 (giữa các chuyến đi):
Natural trigger: trip đã hoàn thành + đi theo lịch trình thật ngoài đời
  → Core action: dùng lại AI khi có 1 chuyến đi MỚI xuất hiện (dịp mới, không phải nhắc nhở)
  → Immediate value: lại thấy kế hoạch cụ thể, đáng tin, nhanh hơn lần trước (nhờ đã tin AI)
  → Saved state / investment: trip mới được lưu, lịch sử trải nghiệm tốt tích lũy thêm
  → Next natural trigger: chuyến đi tiếp theo sau đó...
```

**Reason to return nếu bỏ notification đi**: Lý do quay lại không đến từ nhắc nhở, mà từ **investment đã lưu (trip record + trải nghiệm thực tế đi đúng theo lịch trình AI gợi ý)** — khi có một chuyến đi mới xuất hiện tự nhiên trong đời họ, niềm tin đã tích lũy từ lần trước khiến họ tự chọn quay lại công cụ này thay vì tự làm tay hoặc dùng công cụ khác. Đây là lý do nội tại, không phụ thuộc external trigger → hợp lệ là một loop thật, không phải giả loop dựa vào notification.

**Metric hypothesis (bắt buộc):**
> Nếu loop này hoạt động, metric **Suggestion Accept Rate** sẽ thay đổi theo hướng **tăng dần qua các ngày liên tiếp trong cùng một chuyến đi**, trong vòng đời của trip đó (vài ngày đến vài tuần), vì mỗi lần chốt thành công (immediate value + saved state) làm tăng niềm tin của user vào AI, khiến họ chốt các ngày tiếp theo nhanh hơn và ít cần chỉnh sửa hơn.

---

## 06 — Tracking nhanh

| Tên event | Ý nghĩa | Thời điểm ghi nhận | Metric sử dụng |
|---|---|---|---|
| `trip_created` | Chuyến đi mới được tạo với điểm đến + khung thời gian xác định | Ngay khi trip record được lưu thành công (không phải khi mở form) | Start event của Activation; denominator Activation Rate |
| `itinerary_day_suggested` | AI đã sinh đề xuất lịch trình cho 1 ngày cụ thể | Khi đề xuất render thành công cho user (không phải khi request gửi đi) | Denominator Suggestion Accept Rate |
| `itinerary_day_accepted` | User chốt 1 ngày lịch trình vào kế hoạch thật | Khi trạng thái ngày chuyển `suggested → accepted` được xác nhận ở backend | Core Action; Activation event; numerator Suggestion Accept Rate; numerator NSM |
| `itinerary_day_edited_after_accept` | User chỉnh sửa lại 1 ngày đã accept trước đó | Khi thay đổi được lưu thành công (không phải khi mở form chỉnh sửa) | Depth (engagement); input cho Edit-after-accept rate (leading indicator) |
| `trip_itinerary_completed` | Trip vượt ngưỡng chất lượng NSM (≥80% ngày accepted, ổn định) | Ngay sau lần accept khiến tỷ lệ lần đầu vượt 80% | Numerator North Star Metric |
| `itinerary_day_unaccepted_late` | Một ngày đã accept bị huỷ/đổi trong 7 ngày trước ngày khởi hành | Khi thay đổi trạng thái được lưu, và ngày hiện tại nằm trong cửa sổ 7 ngày trước `start_date` | Counter-metric: Late itinerary abandonment rate |
| `second_trip_created` | User đã activated tạo thêm 1 chuyến đi khác (khác trip đầu tiên) | Khi trip record mới lưu thành công, với điều kiện user đã có ≥1 `itinerary_day_accepted` trước đó | Return event của Retention (N1) |

*(7 events — trong khoảng 4–8 theo yêu cầu, mỗi event map về đúng 1 metric ở Phase 3, không track "mọi click")*

### Tiêu chí nghiệm thu

1. `itinerary_day_accepted` chỉ được ghi khi trạng thái ngày **thật sự** chuyển từ `suggested` sang `accepted` ở backend (giao dịch đã xác nhận) — không bắn ngay khi user bấm nút chốt trên UI trước khi request hoàn tất.
2. Với mỗi cặp `(user_id, trip_id, day_id)`, tải lại trang / retry request / autosave **không** tạo thêm event `itinerary_day_accepted` mới cho cùng một lần chuyển trạng thái — hệ thống kiểm tra trạng thái hiện tại trước khi ghi event (idempotent theo `day_id`).
3. `trip_itinerary_completed` chỉ ghi **đúng một lần** cho mỗi `trip_id`, tại thời điểm đầu tiên tỷ lệ ngày accepted vượt ngưỡng 80% — các lần accept tiếp theo sau đó không tạo thêm event trùng.

**GATE 4 — Loop nối metric, event nối loop: ĐẠT**
- Loop có 2 chu kỳ đầy đủ (trong 1 trip, và giữa các trip), metric hypothesis trỏ về **Suggestion Accept Rate** — một metric đã định nghĩa ở Phase 3.
- Cả 7/7 event trong bảng đều map về ít nhất 1 metric cụ thể, không có event "track cho có".

---

## 07 — Tự soi lỗi kinh điển

| # | Câu tự soi | Đối chiếu | Kết luận |
|---|---|---|---|
| 1 | Core action không phải thao tác giao diện hay output hệ thống? | `itinerary_day_accepted` là hành vi user chủ động chốt (state chuyển `suggested → accepted`), không phải "mở app" hay câu trả lời AI sinh ra | ✅ Không cần sửa |
| 2 | Activation không phải "xem hết hướng dẫn" hay "đăng nhập"? | Activation event = `itinerary_day_accepted` trong 48h từ `trip_created` — không dùng login/tour giới thiệu | ✅ Không cần sửa |
| 3 | Frequency không cao hơn nhu cầu thật? | Frequency đo theo tỷ lệ ngày/trip và NSM theo quý — khớp cadence project-based; window activation 48h chỉ để bắt tín hiệu sớm, không ép lặp lại nhiều lần | ✅ Không cần sửa |
| 4 | Loop có reason to return ngoài notification? | Lý do quay lại là investment đã lưu (trip record) + trải nghiệm thực tế đi theo lịch trình — nội tại, không phụ thuộc nhắc nhở | ✅ Không cần sửa |
| 5 | Retention không dùng chung một window cho mọi cadence? | Phạm vi bài chỉ có **một** cadence (project-based, đã kết luận ở Phase 2) nên chỉ cần **một** window (6 tháng), không áp nhầm cho nhịp khác. Bản Phase 5 nháp trước có nhắc sót mốc "12 tháng" từ một bản thiết kế cũ đã bị thay ở mục 04 — đã dọn sạch | ⚠️ Đã sửa (xoá phần thừa) |
| 6 | Mọi event đều map về một metric? | 7/7 event ở mục 06 đều có ít nhất một metric đích, không event "track cho có" | ✅ Không cần sửa |
| 7 | Metric nào cũng có event để tính nó? | North Star Metric bản nháp trước chia theo "mỗi user active" — nhưng "active" ở mục 03 chỉ là khái niệm phân biệt với "activated", **chưa có event nào track được** → phát hiện lỗi | ❌ Đã sửa (xem revision log) |

**GATE 5 — Bài sạch lỗi kinh điển: ĐẠT** (đối chiếu đủ 7 câu; 2 chỗ mắc đã sửa ở mục #5 và #7, không giữ nguyên lựa chọn "phá rule" nào).

### Revision log

- **[Sửa lớn] North Star Metric**: đổi denominator từ "mỗi user active" sang `trip_created` — tỷ lệ `trip_itinerary_completed / trip_created` theo quý. Lý do: "active user" chưa có event nào track trong bảng tracking (mục 06), vi phạm luật "metric nào cũng phải có event để tính nó"; `trip_created` đã có sẵn event, giữ đúng công thức NSM (unit + quality threshold + frequency).
- **[Sửa] Mục tự soi lỗi (nay là 07)**: viết lại toàn bộ vì bản nháp trước còn tham chiếu thiết kế cũ đã bị thay thế ở các phase sau (mốc retention 12 tháng, tên metric "Trip Itinerary Completed", event `itinerary_shared`, "2 loop riêng biệt") — gây mâu thuẫn nội bộ giữa các phần của tài liệu. Đã đồng bộ lại toàn bộ theo đúng thiết kế hiện hành (1 loop 2 chu kỳ, NSM dạng tỷ lệ, retention 1 window 6 tháng).
- **Core action và cadence giữ nguyên** — không phát hiện lỗi kinh điển nào buộc phải đổi hai lựa chọn nền tảng này.
