# PHẦN III: CÁC DẠNG BẪY MẠNG MÁY TÍNH & CÔNG CỤ BỔ TRỢ

## BẪY 17: Phân biệt TCP và UDP (Mạng máy tính)
* **Mức độ xuất hiện:** 90% (VNPT là tập đoàn viễn thông nên cực kỳ chú trọng mảng này).
* **Mục tiêu kiểm tra:** Hiểu bản chất các giao thức truyền tải dữ liệu.

### Đề bài mô phỏng
**Câu hỏi:** Khi thiết kế tính năng Gọi điện thoại Video (Video Call) hoặc Xem Livestream cho ứng dụng của VNPT, lập trình viên nên ưu tiên chọn giao thức nào ở tầng giao vận (Transport Layer) để đảm bảo trải nghiệm tốt nhất?
* **A.** TCP (Transmission Control Protocol)
* **B.** UDP (User Datagram Protocol)
* **C.** HTTP
* **D.** FTP

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B**
* **Cơ chế bẫy:** Nhiều bạn thường nghĩ mạng viễn thông thì cái gì cũng phải chính xác tuyệt đối nên chọn TCP. Tuy nhiên:
  * **TCP** có cơ chế bắt tay 3 bước, kiểm tra lỗi và truyền lại gói tin bị mất. Nó bắt buộc dữ liệu phải chính xác nhưng lại gây ra độ trễ (delay) cao. TCP chỉ hợp cho chuyển tiền, gửi tin nhắn văn bản, tải file.
  * **UDP** thì ngược lại, nó cứ thế bắn dữ liệu đi mà không cần quan tâm bên nhận có nhận được hay không. Không có cơ chế truyền lại nên tốc độ cực nhanh, độ trễ cực thấp. Đối với Livestream hay Video Call, việc mất 1-2 khung hình (bị nhiễu nhẹ) hoàn toàn chấp nhận được, nhưng nếu bị delay vài giây thì không thể gọi điện được.

---

## BẪY 18: Hiểu sai cơ chế của HTTP Status Codes (Mã trạng thái HTTP)
* **Mức độ xuất hiện:** 85% (Thường gặp trong các câu hỏi về lập trình Web/API).

### Đề bài mô phỏng
**Câu hỏi:** Ứng dụng client gửi một request lên server và nhận về mã trạng thái hiển thị trên màn hình log là `403 Forbidden`. Nguyên nhân chính xác của lỗi này là gì?
* **A.** Server đang bị sập nguồn hoặc quá tải không thể xử lý.
* **B.** URL đường dẫn API bị viết sai, không tìm thấy tài nguyên trên server.
* **C.** Người dùng chưa đăng nhập (Thiếu token/bằng chứng xác thực danh tính).
* **D.** Hệ thống nhận diện được người dùng, nhưng người dùng này **không có quyền (phân quyền)** để truy cập vào tài nguyên đó.

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **D**
* **Cơ chế bẫy:** Rất nhiều lập trình viên bị nhầm lẫn giữa lỗi `401 Unauthorized` và `403 Forbidden`.
  * **401 Unauthorized:** Là lỗi do bạn chưa đăng nhập (Hệ thống chưa biết bạn là ai).
  * **403 Forbidden:** Bạn đã đăng nhập thành công (Hệ thống biết bạn là ai rồi), nhưng tài khoản của bạn (ví dụ: nhân viên thường) không có quyền can thiệp vào trang của Admin. 
  * *Mở rộng:* Đầu 4xx là lỗi do phía Client, đầu 5xx (như 500, 502) là lỗi do phía Server.

---

## BẪY 19: Phân biệt `git merge` và `git rebase` (Quản lý mã nguồn)
* **Mức độ xuất hiện:** 75% (Kiểm tra kỹ năng làm việc nhóm thực tế).

### Đề bài mô phỏng
**Câu hỏi:** Khi bạn muốn gộp code từ nhánh tính năng (`feature`) vào nhánh chính (`main`), sự khác biệt lớn nhất nếu bạn chọn sử dụng lệnh `git rebase` thay vì `git merge` là gì?
* **A.** `git rebase` sẽ tạo ra một commit gộp (merge commit) để đánh dấu mốc, còn `git merge` thì không.
* **B.** `git rebase` sẽ viết lại lịch sử commit bằng cách đặt các commit của nhánh feature nối tiếp vào ngọn của nhánh main, giúp lịch sử Git là một đường thẳng sạch sẽ.
* **C.** `git merge` chạy nhanh hơn và không bao giờ xảy ra xung đột (conflict).
* **D.** `git rebase` chỉ dùng được khi làm việc Offline.

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B**
* **Cơ chế bẫy:** 
  * **Git merge** giữ nguyên lịch sử phân nhánh của cả 2 bên và tạo ra một commit chung để nối chúng lại (gây rối mắt nếu có quá nhiều nhánh).
  * **Git rebase** điên rồ hơn, nó nhổ toàn bộ các commit ở nhánh feature ra, đem nối vào điểm cuối cùng mới nhất của nhánh main. Nó làm "phẳng" lịch sử Git nhưng nguy hiểm ở chỗ nó làm thay đổi mã Hash của các commit cũ. Quy tắc vàng: Không bao giờ rebase trên các nhánh dùng chung công khai.

---

## Bẫy 20: Tiến trình (Process) vs Tiểu trình (Thread) trong Hệ điều hành
* **Mức độ xuất hiện:** 70% (Bẫy tư duy tối ưu hóa hiệu năng hệ thống).

### Đề bài mô phỏng
**Câu hỏi:** Phát biểu nào sau đây về `Process` và `Thread` là **SAI**?
* **A.** Một Process có thể chứa nhiều Thread bên trong nó.
* **B.** Các Thread nằm trong cùng một Process sẽ dùng chung vùng nhớ và tài nguyên của Process đó.
* **C.** Khi một Thread bị sập (Crash), toàn bộ các Thread khác và cả Process chứa nó vẫn hoạt động bình thường độc lập.
* **D.** Các Process khác nhau hoạt động độc lập và không chia sẻ vùng nhớ trực tiếp với nhau.

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **C** (Phát biểu này sai).
* **Cơ chế bẫy:** 
  * Vì các Thread (luồng) trong cùng một Process dùng chung một không gian địa chỉ ô nhớ và tài nguyên hệ thống, nên nếu một Thread xảy ra lỗi nghiêm trọng (như Access Violation hoặc Unhandled Exception) làm sập vùng nhớ, nó sẽ **kéo theo toàn bộ Process và các luồng khác sập theo ngay lập tức**. 
  * Ngược lại, nếu một Process (tiến trình) bị sập thì các Process khác không ảnh hưởng gì (giống như bạn tắt một tab trên trình duyệt Chrome, các tab khác vẫn chạy bình thường).
