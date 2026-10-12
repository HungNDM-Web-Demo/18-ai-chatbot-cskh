# Kịch bản demo — Chatbot Chăm sóc Khách hàng

> Dùng khi trình bày cho chủ cửa hàng, trưởng bộ phận CSKH của doanh nghiệp bán lẻ, dịch vụ, giáo dục. Thời lượng 12–15 phút. Đi theo một ngày làm việc: tối qua khách nhắn khi không ai trực, hôm nay khách hỏi liên tục, có khách khiếu nại, cuối ngày quản lý xem trợ lý ảo gánh được bao nhiêu việc.

---

## 1. Chuẩn bị trước buổi demo

- [ ] Mở sẵn link demo, thử "Đăng nhập nhanh" cả 3 vai trò
- [ ] Đặt lại dữ liệu demo (thanh công cụ demo → Đặt lại dữ liệu) để có 90 ngày hội thoại tính tới hôm nay, hàng chờ có sẵn 1 lời nhắn ngoài giờ từ tối qua
- [ ] Đổi tên, logo, màu theo thương hiệu khách (Cài đặt → Thương hiệu) và tên trợ lý ảo (Cài đặt → Chatbot) nếu đã biết
- [ ] Nếu khách cùng ngành bán lẻ, sửa trước 3–5 câu hỏi thường gặp theo chính sách thật của khách (phí ship, đổi trả, giờ mở cửa); khách hỏi đúng chính sách của mình sẽ rất ấn tượng
- [ ] Mở 2 cửa sổ của cùng một trình duyệt, xếp cạnh nhau: trái là website mẫu `/` thu về cỡ điện thoại (khoảng 390 px), phải là trang đăng nhập. Không dùng cửa sổ ẩn danh: chế độ `mock` đồng bộ hai cửa sổ qua trình duyệt
- [ ] Bật loa để khách nghe âm báo khi có hội thoại mới vào hộp thư
- [ ] Kiểm tra `nhanvien` đang "Sẵn sàng" và giờ demo nằm trong giờ làm việc 08:00–21:00. Demo sau 21:00 thì nói trước sẽ thấy luồng để lại lời nhắn, hoặc nới giờ làm việc trong Cài đặt → Chatbot
- [ ] Chế độ `mock` là đủ cho kịch bản dưới. Muốn cho khách hỏi tự do thì chạy chế độ `api` với `AI_API_KEY` trong `BE/src/API/.env`, bấm "Kiểm tra kết nối" ở Cài đặt → Chatbot trước buổi demo
- [ ] Dán sẵn các câu hỏi trong kịch bản vào một file ghi chú để sao chép nhanh

| Link | Ghi chú |
| :--- | :--- |
| FE | `http://localhost:3118` hoặc link online; trang `/` là website mẫu có khung chat |
| Hộp thư chung | `http://localhost:3118/inbox` |
| Swagger | `https://localhost:7118/swagger` (khi khách hỏi về tích hợp) |

| Tài khoản | Vai trò | Dùng để cho xem |
| :--- | :--- | :--- |
| (không đăng nhập) | Khách | Website mẫu, khung chat, gợi ý sản phẩm, gặp nhân viên, đánh giá |
| `admin` | Quản trị viên | Cấu hình chatbot (ngưỡng tin cậy, giờ làm việc), phân quyền |
| `quanly` | Quản lý | Tổng quan, kho kiến thức, câu hỏi cần bổ sung, thống kê |
| `nhanvien` | Nhân viên CSKH (Nguyễn Thu Hà) | Hộp thư chung: nhận, trả lời, đóng hội thoại |

Mật khẩu: giá trị `DEMO_PASSWORD` trong `.env`, hoặc bấm "Đăng nhập nhanh".

---

## 2. Câu chuyện demo

> Mở đầu: "Anh/chị hình dung bên trái là website của cửa hàng mình, bên phải là máy tính của nhân viên CSKH. Em đi đúng một ngày làm việc để anh/chị thấy trợ lý ảo đỡ cho nhân viên những việc gì, và việc nào vẫn để người làm."

| # | Vai trò | Thao tác | Điểm cần nhấn mạnh |
| :---: | :--- | :--- | :--- |
| 1 | Quản lý | Đăng nhập `quanly`, mở Tổng quan | Từ sáng tới giờ phần lớn hội thoại do trợ lý ảo tự trả lời; có 1 lời nhắn từ tối qua đang chờ. Đêm không ai trực vẫn không mất khách |
| 2 | Khách | Ở cửa sổ website mẫu, bấm nút chat, đọc lời chào | Khách biết ngay đang nói chuyện với trợ lý ảo (AI). Minh bạch, không giả làm người |
| 3 | Khách | Bấm gợi ý "Phí giao hàng"; gõ tiếp không dấu "giao ve Thu Duc mat bao lau" | Trả lời trong 1–2 giây, hiểu cả kiểu gõ không dấu; nội dung đúng nguyên văn chính sách của cửa hàng |
| 4 | Khách | Hỏi "Nhà 4 người nên mua nồi chiên nào, tầm 2 triệu?" | Trợ lý ảo tư vấn như nhân viên bán hàng: 2 thẻ sản phẩm đúng giá, hiện giá khuyến mãi; không gợi ý hàng đang hết |
| 5 | Khách | Hỏi "Giá máy rửa chén bao nhiêu?", rồi "Thời tiết Sài Gòn hôm nay thế nào?" | Không có trong kho thì nói chưa có thông tin, không bịa giá. Câu ngoài lề thì từ chối lịch sự, kéo khách về chủ đề cửa hàng |
| 6 | Khách | Gõ "Nồi chiên mình mới mua đã bong lớp chống dính, mình muốn khiếu nại" | Có chữ "khiếu nại" là tự chuyển người, mức Khẩn. Tiêu đề khung chat đổi thành "Đang chờ nhân viên" |
| 7 | Nhân viên | Đổi sang Nhân viên trên thanh công cụ demo; Hộp thư kêu âm báo, hội thoại Khẩn đứng đầu; bấm "Nhận" | Nhân viên đọc được toàn bộ đoạn khách đã nói với trợ lý ảo, không phải hỏi lại. Bên khách hiện "Nguyễn Thu Hà – Nhân viên CSKH đã tham gia trò chuyện" |
| 8 | Nhân viên | Bấm "Kho kiến thức" cạnh ô soạn, gõ "đổi trả", chèn chính sách, sửa vài chữ rồi gửi; gắn nhãn Khiếu nại, ghi chú "Gọi lại trong chiều nay" | Trả lời nhanh mà vẫn đúng chính sách. Ghi chú nội bộ khách không thấy |
| 9 | Khách | Gõ "Hoàn tiền vào thẻ giúp mình, số thẻ 9704 1234 5678 9012" | Hệ thống tự che số thẻ ở cả hai phía và nhắc khách không gửi. Dữ liệu nhạy cảm không bị lưu, không gửi cho AI |
| 10 | Nhân viên, Khách | Nhân viên đóng hội thoại, chọn "Hẹn liên hệ lại"; khách chấm 5 sao | Mỗi cuộc chat có kết quả và điểm hài lòng để đo chất lượng từng nhân viên |
| 11 | Quản lý | Đổi sang Quản lý; Kho kiến thức → tab "Cần bổ sung": thấy "Giá máy rửa chén bao nhiêu?", "Shop có nhận làm đại lý không?"; tạo câu hỏi thường gặp cho câu đại lý, bấm "Lưu và thử" | Trợ lý ảo giỏi lên mỗi tuần nhờ quản lý bổ sung đúng những gì khách hỏi. Không cần lập trình |
| 12 | Quản lý, Quản trị viên | Mở Thống kê 30 ngày: tỉ lệ AI tự giải quyết, chủ đề hỏi nhiều, tỉ lệ ngoài giờ. Đổi sang Quản trị viên, mở Cài đặt → Chatbot: ngưỡng tin cậy 70%, giờ làm việc từng ngày | Đo được trợ lý ảo gánh bao nhiêu việc, khách hỏi gì nhiều nhất. Tự chỉnh độ "chặt" của trợ lý ảo và giờ trực, không phải gọi kỹ thuật |

> Kết: "Câu hỏi và sản phẩm anh/chị vừa thấy là của một cửa hàng mẫu. Bản triển khai sẽ nạp chính sách, sản phẩm thật của mình, chạy thử 2–4 tuần có nhân viên theo dõi rồi mới mở cho toàn bộ khách."

---

## 3. Câu hỏi khách hay hỏi

| Câu hỏi | Trả lời gợi ý |
| :--- | :--- |
| Chi phí AI mỗi tháng khoảng bao nhiêu? | Tính theo lượng chữ gửi và nhận (token). Mỗi câu chỉ gửi kèm tối đa 5 mục kiến thức nên chi phí thấp; vài nghìn hội thoại mỗi tháng thường từ vài trăm nghìn tới vài triệu đồng, tùy mô hình. Hệ thống giới hạn 30 lượt trả lời AI mỗi hội thoại và chặn nhắn dồn dập để không phát sinh bất thường. Báo giá chính xác sau khi biết lượng chat thực tế. |
| AI có trả lời sai, bịa giá không? | Trợ lý ảo chỉ được trả lời từ kho kiến thức. Câu nào độ tin cậy dưới 70% thì nói chưa có thông tin; mọi con số trong câu trả lời phải có trong nguồn, sai là hủy. Vẫn có thể có câu chưa đúng ý, nên mỗi câu có nút "Chưa hữu ích" để quản lý xem lại hằng tuần. |
| Độ chính xác bao nhiêu phần trăm? | Phụ thuộc chủ yếu vào kho kiến thức. Dữ liệu mẫu cho thấy khoảng 3/4 hội thoại AI tự giải quyết; thực tế thường đạt mức này sau 2–4 tuần bổ sung từ tab "Cần bổ sung". Không cam kết con số trước khi chạy thử với câu hỏi thật của khách. |
| Dữ liệu khách có bị gửi ra ngoài, bị đem đi huấn luyện AI không? | Chỉ gửi câu hỏi đã che dữ liệu nhạy cảm và các mục kiến thức liên quan; không gửi tên, số điện thoại. Bản triển khai dùng gói API có cam kết không dùng dữ liệu để huấn luyện, hoặc mô hình chạy trên máy chủ riêng nếu yêu cầu cao. Thiết kế theo Luật Bảo vệ dữ liệu cá nhân: có ô đồng ý khi khách để lại số điện thoại. |
| Có tích hợp Zalo OA, Facebook Messenger không? | Demo chỉ có khung chat trên website, Zalo OA và Messenger nằm ngoài phạm vi demo. Bản triển khai có thể đưa tin từ Zalo OA, Messenger về chung một hộp thư, dùng chung kho kiến thức; cần tài khoản OA đã xác thực và khảo sát thêm. |
| Gắn vào website đang có (WordPress, Haravan, Sapo…) được không? | Được. Bản triển khai cung cấp một đoạn mã nhúng để dán vào website. Demo đang chạy trên website mẫu. |
| Trợ lý ảo có tra đơn hàng, đặt hàng giúp khách không? | Demo chưa có. Bản triển khai có thể nối phần mềm bán hàng để tra đơn theo mã đơn và số điện thoại; đặt hàng qua chat cần khảo sát thêm. |
| Nạp kiến thức có cần lập trình không? | Không. Quản lý tự thêm câu hỏi, sản phẩm ở màn Kho kiến thức và thử ngay. Có sẵn tài liệu chính sách dạng Word, PDF thì bên em hỗ trợ chuyển thành câu hỏi thường gặp lúc triển khai. |
| Ban đêm không có nhân viên thì sao? | Trợ lý ảo trả lời 24/7. Việc cần người thì khách để lại lời nhắn, sáng hôm sau hộp thư báo hạn xử lý trong 60 phút đầu ca. |
| Khách có biết đang nói chuyện với máy không? | Có. Lời chào luôn nói rõ là trợ lý ảo (AI), mỗi tin có nhãn người gửi, câu công khai này không tắt được trong cấu hình. |
| Khách gõ không dấu, viết tắt có hiểu không? | Có. Chế độ `mock` hiểu không dấu và một số viết tắt phổ biến (ship, bh, sp); chế độ `api` hiểu cách nói tự nhiên tốt hơn. |
| Khách gửi ảnh sản phẩm lỗi được không? | Demo chỉ nhận văn bản. Bản triển khai có thể cho gửi ảnh khi đang trò chuyện với nhân viên. |

---

## 4. Sau buổi demo

- [ ] Ghi lại yêu cầu khách nêu thêm vào `01-TONG-QUAN-NGHIEP-VU.md`
- [ ] Ghi lại số lượt chat mỗi tháng, kênh khách đang dùng (website, Zalo OA, Messenger), số nhân viên CSKH, giờ trực hiện tại
- [ ] Xin 20–30 câu hỏi khách hay hỏi nhất và tài liệu chính sách để làm bản demo riêng
- [ ] Ước tính chi phí AI theo lượng chat thực tế
- [ ] Gửi link demo và tài khoản `quanly`, `nhanvien` cho khách tự trải nghiệm (nếu đồng ý)
