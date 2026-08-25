# AI Support Log — viết ngắn

## AI đã giúp tôi ở đâu?

- **Brainstorm ứng viên core action + câu hỏi phản biện**: AI liệt kê nhiều hành vi ứng viên cho use case "lập lịch trình chuyến đi" (mở app, gửi prompt, xem gợi ý, chốt một ngày lịch trình, hoàn thành cả trip...) và tự đặt câu hỏi phản biện kiểu "hành vi này có tiến gần core value không, hay chỉ là thao tác giao diện?" để loại dần các ứng viên yếu — giúp tôi có nguyên liệu để tự tranh luận thay vì chỉ nhận một đáp án.
- **Gợi ý tên event dạng `object_action` + acceptance criteria mẫu**: AI đề xuất bộ tên event theo chuẩn `object_action` (`trip_created`, `itinerary_day_accepted`, `itinerary_day_edited_after_accept`...) và mẫu acceptance criteria (event chỉ bắn khi hành vi hoàn tất ở backend; reload/retry không ghi trùng) theo đúng khuôn ví dụ của đề bài — tôi dùng làm điểm khởi đầu để rà lại tên và điều kiện ghi nhận cho khớp với sản phẩm thật.
- **Đóng vai "khách hàng khó tính"**: AI thử chất vấn lại core job và cadence tôi đưa ra (vd: "chốt 1 ngày đã đủ chứng minh giá trị chưa, hay phải xong cả chuyến mới tính?", "6 tháng có thật sự khớp với tần suất đi chơi của một người, hay chỉ là con số nghe hợp lý?") — giúp lộ ra những chỗ định nghĩa còn lỏng lẻo mà tôi chưa nghĩ tới khi tự viết một mình.

## AI sai, hời hợt hoặc đề xuất metric sai nature ở đâu?

- AI đã **vượt quá vai trò brainstorm**: thay vì chỉ đưa ứng viên và câu hỏi phản biện, ở các bước trước AI đã **trực tiếp chọn sẵn core action, viết sẵn kết luận cadence, viết sẵn North Star Metric và Retention Definition** — tức là làm thay phần mà đề bài yêu cầu tôi phải tự quyết định và tự chịu trách nhiệm.
- Một số con số nền cho retention/cadence (tần suất "2–4 chuyến/năm", window "6 tháng" cho N1 retention, ngưỡng "48 giờ" cho activation, ngưỡng "80% ngày accepted, ổn định trong 7 ngày" cho NSM) là **giả định do AI đưa ra, không có nguồn dữ liệu thật** — nếu dùng nguyên như vậy trong bài nộp thì rủi ro bị coi là số liệu "tham khảo" không kiểm chứng được.
- Việc đóng vai khách hàng khó tính mới dừng ở mức đặt câu hỏi gợi mở, chưa thực sự đẩy đến cùng để buộc tôi phải tự trả lời trước khi AI gợi ý đáp án — nhiều lúc AI hỏi xong rồi tự trả lời luôn, làm mất tác dụng phản biện.

## Tôi đã tự sửa hoặc quyết định lại điều gì?

- Tôi cần tự rà lại và quyết định lại core action, cadence, NSM, retention definition bằng lập luận của chính mình — không giữ nguyên bản do AI viết chỉ vì nó có vẻ hợp lý; đặc biệt phải tự trả lời được câu hỏi phản biện ở phần "khách hàng khó tính" trước khi chốt.
- Các con số giả định (tần suất chuyến đi, window 6 tháng, ngưỡng 48h/80%) tôi sẽ thay bằng số liệu thật (khảo sát nhỏ, dữ liệu ngành, hoặc nêu rõ đây là giả định cần kiểm chứng) thay vì giữ nguyên số do AI đề xuất mà không có nguồn.
- Tên event và acceptance criteria tôi giữ lại làm khung tham khảo, nhưng sẽ tự kiểm tra lại từng cái có thực sự map đúng với cách sản phẩm của tôi vận hành hay không, thay vì copy nguyên.
