# Kịch bản kiểm thử nghiệm thu — Chatbot Chăm sóc Khách hàng

> Test case theo use case trong `03-USE-CASE.md`. Phần nền (đăng nhập, phân quyền, đặt lại dữ liệu) dùng TC-SYS-01 đến TC-SYS-10 trong `ZDOCS/03-KIEN-TRUC/USE-CASE-NEN.md` §8.

---

## 1. Cách chạy

| Mục | Nội dung |
| :--- | :--- |
| Môi trường | Chạy cả hai chế độ: `mock` (chỉ FE) và `api` (FE + BE). Test case ghi "(chế độ `api`)" chỉ chạy ở chế độ `api` |
| Dữ liệu | Đặt lại dữ liệu demo trước mỗi lượt test. Test case nào đổi cấu hình chatbot thì trả về mặc định sau khi chạy |
| Tài khoản | Theo cột "Vai trò", dùng "Đăng nhập nhanh". Khách không đăng nhập, mở `/` |
| Thiết bị | Khung chat chạy ở cửa sổ rộng 390 px; hộp thư và các màn quản lý ở cửa sổ desktop. Hai cửa sổ của cùng một trình duyệt, xếp cạnh nhau, không dùng cửa sổ ẩn danh |
| Giờ chạy | Trong giờ làm việc 08:00–21:00, `nhanvien` ở trạng thái "Sẵn sàng", trừ khi test case ghi khác |
| Đạt | Kết quả thực tế khớp toàn bộ cột "Kết quả mong đợi" |

### 1.1. Dữ liệu mẫu dùng trong test

Cửa hàng mẫu: **Gia Dụng Hòa An**. Độ tin cậy ở chế độ `mock` tính theo quy tắc ở `02-SRS.md` §2.4 (khớp nguyên câu 95%, 3 từ khóa 85%, 2 từ khóa 75%, 1 từ khóa 55%). Ngưỡng tin cậy mặc định 70%.

**Câu hỏi thường gặp**

| Mã | Nhóm | Câu hỏi chính | Từ khóa | Câu trả lời (rút gọn) | Trạng thái |
| :--- | :--- | :--- | :--- | :--- | :--- |
| FAQ001 | Giao hàng | Bao lâu thì nhận được hàng? | giao, bao lâu, mấy ngày, nhận hàng | Nội thành TP.HCM giao trong 2–4 giờ với đơn đặt trước 16:00; tỉnh thành khác 2–5 ngày làm việc | Đang dùng |
| FAQ002 | Giao hàng | Phí giao hàng bao nhiêu? (cách hỏi khác: "Phí ship bao nhiêu vậy shop?") | phí ship, phí giao hàng, tiền ship, miễn phí giao | Miễn phí giao hàng cho đơn từ 500.000 ₫; đơn dưới 500.000 ₫ phí 30.000 ₫ | Đang dùng |
| FAQ010 | Đổi trả | Chính sách đổi trả thế nào? | đổi, lỗi, trả hàng, đổi trả | Đổi sản phẩm mới trong 7 ngày nếu lỗi do nhà sản xuất, còn đủ hộp và phụ kiện; không áp dụng khi rơi vỡ, vào nước | Đang dùng |
| FAQ020 | Bảo hành | Sản phẩm bảo hành bao lâu? | bảo hành, bh, bao lâu | 12 tháng; bếp từ và máy ép chậm 24 tháng; tra bảo hành bằng số điện thoại mua hàng | Đang dùng |
| FAQ030 | Cửa hàng | Cửa hàng mở cửa mấy giờ? | mở cửa, đóng cửa, mấy giờ | Showroom mở 08:00–21:30 tất cả các ngày, kể cả Chủ nhật; nhân viên trực chat 08:00–21:00 | Đang dùng |
| FAQ045 | Khuyến mãi | Tháng này có khuyến mãi gì? | khuyến mãi, ưu đãi, quà tặng | Mua bếp từ tặng bộ nồi inox 3 món, đến hết ngày cuối tháng | Đang dùng, hiệu lực tới cuối tháng hiện tại |
| FAQ046 | Khuyến mãi | Flash sale 9.9 có gì? | flash sale, 9.9 | Giảm 30% nồi cơm điện | Đang dùng, hiệu lực đã kết thúc trước hôm nay |

**Sản phẩm**

| Mã | Tên | Danh mục | Giá niêm yết | Giá khuyến mãi | Nhóm khách phù hợp | Tình trạng | Trạng thái |
| :--- | :--- | :--- | ---: | ---: | :--- | :--- | :--- |
| SP001 | Nồi chiên không dầu HA-AF35 3,5 lít | Nồi chiên không dầu | 1.290.000 ₫ | — | 1–2 người | Còn hàng | Đang bán |
| SP002 | Nồi chiên không dầu HA-AF55 5,5 lít | Nồi chiên không dầu | 1.890.000 ₫ | — | 3–5 người | Còn hàng | Đang bán |
| SP003 | Nồi chiên không dầu HA-AF70 7 lít | Nồi chiên không dầu | 2.490.000 ₫ | 2.190.000 ₫ | 4–6 người | Còn hàng | Đang bán |
| SP004 | Nồi chiên hơi nước HA-SF12 12 lít | Nồi chiên không dầu | 3.990.000 ₫ | — | 4–6 người | Tạm hết hàng | Đang bán |
| SP015 | Bếp từ đôi HA-IH22 | Bếp từ | 4.590.000 ₫ | 3.990.000 ₫ | — | Còn hàng | Đang bán |
| SP030 | Máy xay sinh tố HA-BL10 | Máy xay, máy ép | 690.000 ₫ | — | — | Còn hàng | Ngừng bán |

**Nhân viên và hội thoại lúc đặt lại dữ liệu**

| Tài khoản | Họ tên | Vai trò | Trạng thái | Hội thoại đang xử lý |
| :--- | :--- | :--- | :--- | :--- |
| `nhanvien` | Nguyễn Thu Hà | STAFF | Sẵn sàng | "Khách 7315" (tin cuối của khách cách thời điểm đặt lại 29 phút), "Khách 2468" |
| `quanly` | Trần Minh Quân | MANAGER | Tạm vắng | — |
| (không có đăng nhập nhanh) | Lê Hoàng Nam | STAFF | Tạm vắng | "Khách 3391" |

Hàng chờ có sẵn: lời nhắn ngoài giờ của "Khách 1907" gửi lúc 22:40 tối hôm trước (mức Thường, nhãn "Ngoài giờ"); hội thoại "Khách 5520" vào hàng chờ 1 phút trước thời điểm đặt lại (mức Thường, lý do Khách yêu cầu). Tab "Cần bổ sung" có sẵn câu "Shop có nhận làm đại lý không?" (5 lần hỏi).

**Câu hỏi mẫu và kết quả mong đợi ở chế độ `mock`**

| Mã | Khách gửi | Kết quả mong đợi |
| :--- | :--- | :--- |
| Q1 | Phí ship bao nhiêu vậy shop? | Trả lời theo FAQ002, độ tin cậy 95% |
| Q2 | giao ve Thu Duc mat bao lau | Trả lời theo FAQ001, 75% |
| Q3 | Mua nồi chiên về bị lỗi thì đổi được không? | Trả lời theo FAQ010, 75% |
| Q4 | bh bao lâu vậy | Trả lời theo FAQ020, 75% |
| Q5 | Mấy giờ cửa hàng đóng cửa? | Trả lời theo FAQ030, 75% |
| Q6 | Nhà 4 người nên mua nồi chiên nào, tầm 2 triệu? | 2 thẻ: SP002 (1.890.000 ₫), SP003 (2.190.000 ₫, giá 2.490.000 ₫ gạch ngang) |
| Q7 | Nồi chiên HA-SF12 còn hàng không? | Nêu SP004 "Tạm hết hàng", gợi ý thay thế SP003 |
| Q8 | Giá máy rửa chén bao nhiêu? | Dưới ngưỡng (0%), không nêu giá nào |
| Q9a | Shop có nhận làm đại lý không? | Dưới ngưỡng |
| Q9b | Chiết khấu cho đại lý bao nhiêu phần trăm? | Dưới ngưỡng; gửi ngay sau Q9a thì tự chuyển nhân viên |
| Q10 | Thời tiết Sài Gòn hôm nay thế nào? | Câu mẫu ngoài phạm vi |
| Q11 | Tôi muốn khiếu nại, nồi chiên mới mua đã hỏng | Tự chuyển nhân viên, mức Khẩn, lý do Khiếu nại |
| Q12 | Cho mình gặp nhân viên | Chuyển nhân viên, lý do Khách yêu cầu |
| Q13 | Còn flash sale 9.9 không? | Dưới ngưỡng, không nhắc nội dung FAQ046 |
| Q14 | Hoàn tiền vào thẻ giúp mình, số thẻ 9704 1234 5678 9012 | Số thẻ hiển thị `**** **** **** 9012`, khung chat hiện nhắc nhở |

---

## 2. Test case

### 2.1. Khung chat và trả lời tự động

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-01 | UC-01 | Mở khung chat lần đầu | Khách | 1. Mở `/` ở cửa sổ 390 px 2. Bấm nút chat | Khung chat mở toàn màn hình; lời chào có câu "Em là trợ lý ảo (AI) của Gia Dụng Hòa An"; có 4 nút gợi ý; tiêu đề "Đang trò chuyện với trợ lý ảo (AI)" |
| TC-02 | UC-01 | Hỏi câu có trong kho | Khách | 1. Gửi Q1 2. Quản lý mở Lịch sử hội thoại, mở hội thoại vừa tạo | Trong 1,5 giây có câu trả lời nhãn "Trợ lý ảo (AI)": miễn phí giao hàng cho đơn từ 500.000 ₫, đơn dưới 500.000 ₫ phí 30.000 ₫; không quá 120 từ. Hội thoại có mã `HT{yyMMdd}{nnnn}`; câu trả lời ghi nguồn FAQ002, độ tin cậy 95%, chế độ `mock` |
| TC-03 | UC-01 | Gõ không dấu, viết tắt | Khách | 1. Gửi Q2 2. Gửi Q4 | Bước 1 trả lời theo FAQ001; bước 2 trả lời theo FAQ020 (12 tháng, bếp từ và máy ép chậm 24 tháng) |
| TC-04 | UC-01 | Câu hỏi không có trong kho | Khách, Quản lý | 1. Gửi Q8 2. Quản lý mở Kho kiến thức, tab "Cần bổ sung" | Bước 1: "Em chưa có thông tin chính xác về câu hỏi này…", không có số tiền nào, hội thoại vẫn ở trợ lý ảo. Bước 2: có câu "Giá máy rửa chén bao nhiêu?", số lần hỏi 1, trạng thái Mới |
| TC-05 | UC-01 | Câu ngoài phạm vi không tính là dưới ngưỡng | Khách, Quản lý | 1. Gửi Q10 2. Gửi Q8 3. Quản lý mở tab "Cần bổ sung" | Bước 1: câu mẫu ngoài phạm vi kèm danh sách chủ đề hỗ trợ được. Bước 2: dưới ngưỡng lần đầu, không tự chuyển nhân viên. Bước 3: không có câu Q10 |
| TC-06 | UC-01 | Câu hỏi thường gặp hết hiệu lực không được dùng | Khách | 1. Mở hội thoại mới, gửi Q13 | Dưới ngưỡng; câu trả lời không nhắc "30%" hay "nồi cơm điện" |
| TC-07 | UC-01 | Che số thẻ | Khách, Nhân viên | 1. Mở hội thoại mới, gửi Q14 2. Gửi Q12 3. `nhanvien` nhận hội thoại | Bước 1: tin của khách hiện "…số thẻ **** **** **** 9012"; khung chat hiện "Vì an toàn, anh/chị không gửi số thẻ, mật khẩu, mã OTP qua chat"; trợ lý ảo không nhắc lại số thẻ. Bước 3: hộp thư cũng chỉ thấy bản đã che |
| TC-08 | UC-01 | Tin nhắn quá dài | Khách | 1. Dán đoạn văn 1.001 ký tự vào ô nhập | Bộ đếm hiện số âm màu đỏ, nút Gửi bị khóa, Enter không gửi |
| TC-09 | UC-01 | Gửi quá nhanh | Khách | 1. Gửi 11 tin "alo" trong vòng 1 phút 2. Chờ 60 giây, gửi Q1 | Tin thứ 11 không gửi được, ô nhập khóa với thông báo "Anh/chị nhắn hơi nhanh, vui lòng chờ 60 giây"; bước 2 gửi được và có câu trả lời |
| TC-10 | UC-01 | Tải lại trang vẫn giữ hội thoại | Khách | 1. Gửi Q1 2. Bấm F5 3. Mở lại khung chat | Cùng mã hội thoại, đủ tin nhắn trước đó; không hiện lại lời chào như hội thoại mới |
| TC-11 | UC-01 | Mô hình AI lỗi (chế độ `api`) | Khách, Quản lý | 1. Đặt `AI_API_KEY` sai, khởi động lại BE 2. Gửi Q1 3. Quản lý mở chi tiết hội thoại | Khách nhận câu trả lời theo FAQ002, không thấy thông báo lỗi kỹ thuật; câu trả lời ghi chế độ "mock dự phòng" |
| TC-12 | UC-01 | Không bịa giá (chế độ `api`) | Khách | 1. `AI_API_KEY` đúng 2. Gửi Q8 3. Gửi "Máy rửa chén loại 8 bộ tầm bao nhiêu tiền?" | Cả hai câu trả lời không chứa số tiền nào; nói rõ chưa có thông tin, mời gặp nhân viên; sau câu thứ hai hội thoại tự chuyển nhân viên |

### 2.2. Gợi ý sản phẩm

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-13 | UC-02 | Gợi ý theo nhu cầu và mức giá | Khách, Quản lý | 1. Gửi Q6 2. Bấm "Xem chi tiết" trên thẻ SP002 3. Quản lý mở Thống kê, tab Gợi ý sản phẩm, hôm nay | Bước 1: đúng 2 thẻ SP002 1.890.000 ₫ và SP003 2.190.000 ₫ (2.490.000 ₫ gạch ngang); không có SP001, SP004. Bước 2: website mẫu chuyển tới SP002. Bước 3: SP002 có thêm 1 lượt gợi ý và 1 lượt bấm |
| TC-14 | UC-02 | Hỏi sản phẩm tạm hết hàng | Khách | 1. Gửi Q7 | Trợ lý ảo nêu "Tạm hết hàng" cho HA-SF12, kèm thẻ SP003 thay thế; không có thẻ SP004 |
| TC-15 | UC-02 | Hỏi sản phẩm đã ngừng bán | Khách | 1. Gửi "Máy xay sinh tố HA-BL10 giá bao nhiêu?" | Trợ lý ảo trả lời cửa hàng không còn kinh doanh sản phẩm này; không nêu 690.000 ₫, không có thẻ SP030 |

### 2.3. Chuyển nhân viên và lời nhắn

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-16 | UC-03 | Khách yêu cầu gặp nhân viên trong giờ | Khách, Nhân viên | 1. `nhanvien` mở Hộp thư 2. Khách gửi Q12 | Khung chat: tin hệ thống có vị trí trong hàng chờ, tiêu đề "Đang chờ nhân viên", nút "Gặp nhân viên" ẩn. Hộp thư: có âm báo, số đếm tăng 1, dòng mới ở tab "Chờ nhận" với lý do "Khách yêu cầu", mức Thường |
| TC-17 | UC-03 | Tự chuyển sau 2 câu dưới ngưỡng | Khách, Nhân viên | 1. Mở hội thoại mới, gửi Q9a 2. Gửi Q9b | Bước 1: câu mẫu dưới ngưỡng. Bước 2: trợ lý ảo báo chuyển nhân viên; dòng trong hộp thư có lý do "AI chưa trả lời được"; tab "Cần bổ sung" tăng số lần của câu đại lý |
| TC-18 | UC-03 | Khiếu nại được ưu tiên | Khách, Nhân viên | 1. Mở hội thoại mới, gửi Q11 | Trợ lý ảo xin lỗi và báo chuyển nhân viên; dòng mới mức Khẩn, lý do "Khiếu nại", đứng đầu tab "Chờ nhận", trên cả lời nhắn của "Khách 1907" và hội thoại "Khách 5520" đã chờ lâu hơn |
| TC-19 | UC-03 | Chờ quá hạn | Khách, Quản lý | 1. Sau TC-16, không ai nhận 2. Chờ 2 phút 3. Chờ thêm tới phút thứ 5 | Phút 2: dòng tô vàng, khách nhận đúng một tin "Nhân viên đang bận, anh/chị vui lòng chờ thêm ít phút". Phút 5: dòng tô đỏ, `quanly` có thông báo ở chuông, khung chat hiện nút "Để lại lời nhắn" |
| TC-20 | UC-03 | Khách quay lại trợ lý ảo | Khách, Nhân viên | 1. Sau TC-16, khách bấm "Quay lại trợ lý ảo" 2. Gửi Q5 | Dòng biến khỏi tab "Chờ nhận"; khung chat hiện tin hệ thống "Anh/chị đang trò chuyện với trợ lý ảo (AI)"; Q5 được trả lời theo FAQ030 |
| TC-21 | UC-04 | Để lại lời nhắn ngoài giờ | Quản trị viên, Khách, Nhân viên | 1. `admin` mở Cài đặt → Chatbot, tắt ngày hôm nay, lưu 2. Khách gửi Q12 3. Nhập "Phạm Văn Long", "0912 345 678", "Tư vấn bếp từ cho căn hộ", tick đồng ý, bấm "Gửi lời nhắn" 4. `nhanvien` mở Hộp thư 5. `admin` bật lại ngày hôm nay | Bước 2: thông báo ngoài giờ kèm giờ làm việc và biểu mẫu. Bước 3: khung chat xác nhận nhân viên liên hệ trước 09:00 ngày làm việc kế tiếp. Bước 4: dòng mới có nhãn "Ngoài giờ", hiện họ tên, số điện thoại, nội dung, hạn nhận |
| TC-22 | UC-04 | Lời nhắn không hợp lệ | Khách | 1. Như TC-21 tới biểu mẫu 2. Nhập số điện thoại "091234567" 3. Sửa thành "0912 345 678", không tick đồng ý | Bước 2: báo lỗi tại ô số điện thoại, không gửi. Bước 3: nút "Gửi lời nhắn" bị khóa |
| TC-23 | UC-04 | Trong giờ nhưng không ai sẵn sàng | Nhân viên, Khách | 1. `nhanvien` chuyển "Tạm vắng" 2. Khách gửi Q12 3. Gửi lời nhắn hợp lệ 4. `nhanvien` chuyển lại "Sẵn sàng" | Bước 2: "Hiện các nhân viên CSKH đều đang bận…" kèm biểu mẫu. Bước 3: hội thoại vào hàng chờ, hạn nhận 60 phút kể từ lúc gửi |

### 2.4. Hộp thư chung

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-24 | UC-05 | Nhận và trả lời hội thoại | Nhân viên, Khách | 1. Sau TC-17, `nhanvien` bấm "Nhận" 2. Gửi "Dạ, em gửi anh/chị chính sách đại lý ngay ạ" | Bước 1: hộp thư hiện đủ 2 câu hỏi và 2 câu trả lời của trợ lý ảo có nhãn "AI", lý do chuyển; khách thấy tin hệ thống "Nguyễn Thu Hà – Nhân viên CSKH đã tham gia trò chuyện", tiêu đề đổi theo. Bước 2: khách nhận tin trong 2 giây, nhãn "Nguyễn Thu Hà – Nhân viên CSKH" |
| TC-25 | UC-05 | Đủ 5 hội thoại | Nhân viên, Khách | 1. `nhanvien` nhận "Khách 1907", "Khách 5520"; khách mở thêm hội thoại mới, gửi Q12, `nhanvien` nhận, lặp lại tới khi tab "Của tôi" có 5 hội thoại 2. Khách mở thêm một hội thoại, gửi Q12 | Ở bước 2 nút "Nhận" bị khóa, chú thích "Đã đủ 5 hội thoại đang xử lý"; hội thoại mới vẫn nằm ở tab "Chờ nhận" |
| TC-26 | UC-05 | Hai người cùng bấm Nhận (chế độ `api`) | Nhân viên, Quản lý | 1. `nhanvien` và `quanly` mở Hộp thư ở hai trình duyệt khác nhau 2. Cùng bấm "Nhận" một hội thoại | Chỉ một người nhận được; người còn lại thấy "Hội thoại đã được {Họ tên} nhận", dòng biến khỏi tab "Chờ nhận" |
| TC-27 | UC-05 | Chèn câu trả lời từ kho, ghi chú nội bộ | Nhân viên, Khách | 1. Trong hội thoại đang xử lý, bấm "Kho kiến thức", gõ "đổi trả", chèn FAQ010 2. Sửa câu đầu, gửi 3. Gắn nhãn "Đổi trả", ghi chú "Khách mua 3 lần" | Ô soạn có nội dung FAQ010, sửa được; tin gửi đi mang tên nhân viên. Khung chat của khách không hiện ghi chú, nhãn |
| TC-28 | UC-06 | Đóng hội thoại và đánh giá | Nhân viên, Khách | 1. Sau TC-24, bấm "Đóng hội thoại", chọn "Đã giải quyết" 2. Khách chọn 5 sao, gửi 3. Khách gửi Q1 | Bước 1: dòng biến khỏi tab "Của tôi"; khung chat hiện "Cuộc trò chuyện đã kết thúc" và mời đánh giá. Bước 2: Lịch sử ghi Đã đóng, Đã giải quyết, 5 sao. Bước 3: tạo hội thoại mới với mã mới, trợ lý ảo trả lời |
| TC-29 | UC-06 | Tự đóng khi khách im lặng | Nhân viên | 1. Đặt lại dữ liệu 2. `nhanvien` mở tab "Của tôi", chờ khoảng 1 phút | Hội thoại "Khách 7315" tự đóng, kết quả "Khách không phản hồi", biến khỏi tab "Của tôi" |
| TC-30 | UC-06 | Đánh giá thấp | Khách, Quản lý | 1. Nhân viên đóng một hội thoại 2. Khách chọn 2 sao, góp ý "Chờ lâu" 3. Quản lý mở Thống kê, tab Cần xem lại | Hội thoại có nhãn "Cần xem lại" và xuất hiện ở tab Cần xem lại kèm góp ý |
| TC-31 | UC-06 | Đóng hội thoại chưa nhận | Nhân viên | 1. Mở hội thoại "Khách 5520" ở tab "Chờ nhận" | Không có nút "Đóng hội thoại", chỉ có nút "Nhận" |
| TC-32 | UC-07 | Quản lý chuyển hội thoại cho người khác | Quản lý, Khách | 1. `quanly` mở Hộp thư, tab "Đang mở" 2. Chọn "Khách 2468" của Nguyễn Thu Hà, bấm "Chuyển", chọn Lê Hoàng Nam, lý do "Khách quen của Nam" | Người phụ trách đổi thành Lê Hoàng Nam; khung chat khách hiện "Lê Hoàng Nam – Nhân viên CSKH đã tiếp nhận cuộc trò chuyện"; hội thoại biến khỏi tab "Của tôi" của `nhanvien` |

### 2.5. Kho kiến thức

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-33 | UC-08 | Thêm câu hỏi mới, trợ lý ảo dùng ngay | Khách, Quản lý | 1. Khách gửi "Lắp bếp từ tại nhà có mất phí không?" 2. `quanly` thêm câu hỏi nhóm Giao hàng: câu chính "Có lắp đặt bếp từ tại nhà không?", cách hỏi khác "Lắp bếp từ tại nhà có mất phí không?", câu trả lời "Miễn phí lắp đặt bếp từ trong nội thành TP.HCM; ngoại thành phí 100.000 ₫" 3. Khách mở hội thoại mới, gửi lại câu ở bước 1 | Bước 1: dưới ngưỡng. Bước 2: câu hỏi có mã `FAQ` + 3 số, trạng thái Đang dùng. Bước 3: trả lời đúng nội dung mới, nguồn là mã mới, độ tin cậy 95% |
| TC-34 | UC-08 | Dữ liệu không hợp lệ | Quản lý | 1. Thêm câu hỏi chính "Phí giao hàng bao nhiêu?" 2. Câu trả lời 601 ký tự 3. Nhập 11 cách hỏi khác | Mỗi trường hợp báo lỗi tại ô tương ứng, không lưu |
| TC-35 | UC-08 | Tạm ẩn câu hỏi | Quản lý, Khách | 1. Tạm ẩn FAQ030 2. Khách gửi Q5 3. Bật lại FAQ030 4. Khách mở hội thoại mới, gửi Q5 | Bước 2 dưới ngưỡng, không nêu giờ mở cửa; bước 4 trả lời "08:00–21:30" theo FAQ030 |
| TC-36 | UC-08 | Xóa câu hỏi đã dùng | Quản lý | 1. Chọn FAQ002, bấm Xóa | Hệ thống từ chối, gợi ý dùng "Tạm ẩn" |
| TC-37 | UC-09 | Thêm sản phẩm có giá khuyến mãi | Quản lý, Khách | 1. Thêm "Nồi chiên không dầu HA-AF60 6 lít", mã `SP031`, danh mục Nồi chiên không dầu, giá 2.290.000 ₫, khuyến mãi 1.990.000 ₫, nhóm "4–6 người" 2. Khách gửi Q6 | Sản phẩm lưu với Còn hàng, Đang bán. Q6 trả về 3 thẻ: SP002, SP003 và SP031 1.990.000 ₫ (2.290.000 ₫ gạch ngang) |
| TC-38 | UC-09 | Dữ liệu sản phẩm không hợp lệ | Quản lý | 1. Thêm sản phẩm mã `SP002` 2. Giá 1.890.500 ₫ 3. Giá 2.290.000 ₫, khuyến mãi 2.500.000 ₫ | Bước 1 báo trùng mã; bước 2 báo giá phải là bội số 1.000 ₫; bước 3 báo giá khuyến mãi phải nhỏ hơn giá niêm yết; không lưu |
| TC-39 | UC-09 | Báo tạm hết hàng | Quản lý, Khách | 1. Bật "Tạm hết hàng" cho SP002 2. Khách gửi Q6 3. Gửi "Nồi chiên HA-AF55 còn hàng không?" | Bước 2 chỉ còn thẻ SP003; bước 3 nêu "Tạm hết hàng", gợi ý sản phẩm thay thế còn hàng |
| TC-40 | UC-10 | Thử câu hỏi | Quản lý | 1. Ghi số hội thoại hôm nay ở Tổng quan 2. Kho kiến thức → "Thử câu hỏi", gửi Q1 3. Quay lại Tổng quan | Bước 2: câu trả lời như khách nhận, kèm nguồn FAQ002, độ tin cậy 95%, chế độ `mock`, thời gian xử lý. Bước 3: số hội thoại không đổi |
| TC-41 | UC-11 | Tạo câu hỏi từ câu chưa trả lời được | Quản lý, Khách | 1. Tab "Cần bổ sung", chọn "Shop có nhận làm đại lý không?", bấm "Tạo câu hỏi thường gặp" 2. Nhập câu trả lời "Gia Dụng Hòa An nhận đại lý tại các tỉnh phía Nam. Anh/chị để lại số điện thoại, bộ phận kinh doanh liên hệ trong 1 ngày làm việc.", lưu 3. Khách mở hội thoại mới, gửi Q9a | Bước 1: biểu mẫu điền sẵn câu hỏi chính. Bước 2: câu chuyển "Đã bổ sung". Bước 3: trợ lý ảo trả lời theo câu hỏi mới |

### 2.6. Lịch sử, tổng quan, thống kê

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-42 | UC-12 | Chi tiết hội thoại và phạm vi xem | Nhân viên, Quản lý | 1. `nhanvien` mở Lịch sử, tìm "Khách 3391" (của Lê Hoàng Nam) 2. Mở hội thoại của TC-24 3. `quanly` tìm "Khách 3391" | Bước 1: không có kết quả. Bước 2: tin nhắn theo thời gian, nhãn người gửi, nguồn và độ tin cậy từng câu AI, các mốc bắt đầu, vào hàng chờ, nhận, đóng. Bước 3: tìm thấy |
| TC-43 | UC-13 | Tổng quan cập nhật theo hội thoại mới | Quản lý, Khách | 1. Ghi số hội thoại hôm nay và tỉ lệ AI tự giải quyết 2. Khách mở hội thoại mới, gửi Q1, bấm "Kết thúc trò chuyện" 3. Chờ tối đa 60 giây ở Tổng quan | Số hội thoại tăng 1; tỉ lệ AI tự giải quyết tính lại theo BR-22 (hội thoại vừa đóng được tính là AI tự giải quyết) |
| TC-44 | UC-13 | Hội thoại quá hạn chờ hiện trên Tổng quan | Quản lý | 1. Sau TC-19, mở Tổng quan 2. Bấm thẻ "Đang chờ nhân viên" | Thẻ hiện số quá hạn màu đỏ; bước 2 mở Hộp thư, tab "Đang mở" |
| TC-45 | UC-13 | Nhân viên không thấy Tổng quan | Nhân viên | 1. "Đăng nhập nhanh" vai trò Nhân viên 2. Gõ thẳng URL `/reports` | Vào thẳng Hộp thư; menu không có Tổng quan, Thống kê; bước 2 chuyển tới `/forbidden` |
| TC-46 | UC-14 | Thống kê 30 ngày và lý do chuyển | Quản lý | 1. Mở Thống kê, khoảng 30 ngày 2. Sau TC-16, TC-17, TC-18, chọn Hôm nay | Bước 1: biểu đồ đủ 30 điểm, có đường kỳ trước, tỉ lệ AI tự giải quyết khoảng 75%, tổng bảng khớp biểu đồ. Bước 2: lý do Khách yêu cầu, AI chưa trả lời được, Khiếu nại mỗi loại tăng ít nhất 1 |

### 2.7. Cấu hình chatbot

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-47 | UC-15 | Đổi ngưỡng tin cậy | Quản trị viên, Khách | 1. Khách gửi Q3 2. `admin` đổi ngưỡng tin cậy thành 80%, lưu 3. Khách mở hội thoại mới, gửi Q3 4. Đổi lại 70% | Bước 1 trả lời theo FAQ010 (75%); bước 3 dưới ngưỡng, câu vào tab "Cần bổ sung" |
| TC-48 | UC-15 | Giờ làm việc không hợp lệ | Quản trị viên | 1. Sửa Thứ Hai thành 21:00–08:00 2. Lưu | Báo lỗi tại dòng Thứ Hai, không lưu |
| TC-49 | UC-15 | Thiếu API key (chế độ `api`) | Quản trị viên, Khách | 1. Để trống `AI_API_KEY`, khởi động lại BE 2. Mở Cài đặt → Chatbot 3. Khách gửi Q1 | Trang hiện chế độ `api`, trạng thái "Chưa có API key", không có ô nhập API key; khách vẫn nhận câu trả lời theo FAQ002 |
| TC-50 | UC-15 | Câu công khai AI và quyền xem cấu hình | Quản trị viên, Quản lý, Nhân viên | 1. `admin` sửa lời chào, thử xóa câu "Em là trợ lý ảo (AI)…" 2. `quanly` mở Cài đặt → Chatbot 3. `nhanvien` gõ URL `/settings/chatbot` | Bước 1: câu công khai là dòng cố định, không sửa được; phần còn lại sửa được, khung xem trước cập nhật ngay. Bước 2: xem được, các ô bị khóa, không có nút Lưu. Bước 3: chuyển tới `/forbidden` |

---

## 3. Biên bản chạy thử

| Ngày | Người test | Chế độ | Số TC đạt / tổng | Lỗi ghi nhận |
| :--- | :--- | :--- | :---: | :--- |
| | | | | |
