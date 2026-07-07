# PHẦN IV: CÁC DẠNG BẪY DESIGN PATTERNS & GIẢI THUẬT

## BẪY 21: Bẫy Đa luồng (Multi-threading) với Singleton Pattern
* **Mức độ xuất hiện:** 80% (Rất hay gặp khi thi vào các vị trí tối ưu hệ thống).
* **Mục tiêu kiểm tra:** Hiểu cơ chế khởi tạo đối tượng trong môi trường đa luồng.

### Đề bài mô phỏng
Bạn viết một class cấu hình hệ thống theo mẫu `Singleton Pattern` (chỉ cho phép tạo duy nhất 1 đối tượng trong suốt vòng đời ứng dụng) như sau:
```csharp
public class Singleton {
    private static Singleton instance;
    public static Singleton GetInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
**Câu hỏi:** Đoạn code trên sẽ gặp phải vấn đề nghiêm trọng nào khi chạy trong môi trường đa luồng (Multi-threading)?
* **A.** Hệ thống báo lỗi biên dịch vì biến `instance` dính từ khóa `static`.
* **B.** Có nguy cơ tạo ra nhiều hơn 1 đối tượng `Singleton` nếu có 2 luồng cùng nhảy vào câu lệnh `if (instance == null)` tại cùng một thời điểm.
* **C.** Gây ra hiện tượng tràn bộ nhớ RAM (Stack Overflow).
* **D.** Đoạn code hoàn toàn tối ưu, không gặp lỗi gì.

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B**
* **Cơ chế bẫy:** Đây gọi là lỗi **Race Condition** trong lập trình đa luồng. Nếu Luồng A và Luồng B cùng chạy song song, tại thời điểm ban đầu `instance` đang bằng `NULL`, cả hai luồng đều thỏa mãn điều kiện `if` và cùng thực hiện lệnh `new Singleton()`. Kết quả là class Singleton bị tạo thành 2 bản thể khác nhau, phá vỡ hoàn toàn quy tắc của mẫu thiết kế này.
* **Cách sửa đúng:** Phải sử dụng cơ chế khóa từ xa (`lock` hoặc `synchronized`) trước khi kiểm tra `NULL` để ép các luồng phải xếp hàng chờ nhau.

---

## BẪY 22: Phân biệt String vs StringBuilder (Quản lý bộ nhớ Heap)
* **Mức độ xuất hiện:** 85% (Kiểm tra tư duy viết code tối ưu hiệu năng).

### Đề bài mô phỏng
**Câu hỏi:** Tại sao khi cần thực hiện việc cộng chuỗi liên tiếp nhiều lần (ví dụ đọc dữ liệu từ file văn bản lớn hoặc log hệ thống), lập trình viên được khuyến cáo nên dùng `StringBuilder` thay vì dùng toán tử `+` của đối tượng `String` thông thường?
* **A.** Vì `String` tiết kiệm bộ nhớ hơn nhưng chạy chậm hơn.
* **B.** Vì đối tượng `String` là bất biến (Immutable). Mỗi phép cộng chuỗi `+` thực chất sẽ bắt hệ thống hủy đối tượng cũ và tạo ra một đối tượng hoàn toàn mới trên bộ nhớ Heap, gây lãng phí tài nguyên và làm chậm ứng dụng.
* **C.** `StringBuilder` tự động mã hóa dữ liệu an toàn hơn `String`.
* **D.** Cả hai bản chất giống nhau, chỉ khác nhau về cách viết.

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B**
* **Cơ chế bẫy:** Đề thi đố bạn bản chất của chuỗi. Trong hầu hết các ngôn ngữ (C#, Java, Python), `String` một khi đã tạo ra thì **không thể sửa đổi nội dung**. Lệnh `str = str + "abc"` không phải là nối thêm chữ vào chuỗi cũ, mà là tạo ra một chuỗi thứ 3 chứa cả 2 nội dung trên. Nếu bạn cộng chuỗi trong một vòng lặp 10,000 lần, bạn sẽ tạo ra 10,000 đối tượng rác trên bộ nhớ Heap. `StringBuilder` sinh ra với bộ đệm động (Mutable) cho phép sửa đổi trực tiếp trên một ô nhớ duy nhất, giúp tốc độ nhanh hơn hàng trăm lần.

---

## BẪY 23: Độ phức tạp thuật toán thời gian $O(N)$ vs $O(1)$ của cấu trúc dữ liệu
* **Mức độ xuất hiện:** 75% (Kiểm tra nền tảng giải thuật cốt lõi).

### Đề bài mô phỏng
**Câu hỏi:** Bạn cần lưu trữ danh sách mã số nhân viên VNPT để phục vụ cho tính năng **tìm kiếm thông tin cực nhanh** dựa vào mã số đó (biết mã số là duy nhất). Bạn nên ưu tiên chọn cấu trúc dữ liệu nào dưới đây để việc tìm kiếm đạt độ phức tạp thời gian tối ưu nhất là $O(1)$?
* **A.** `List` / `ArrayList` (Danh sách mảng tuyến tính).
* **B.** `LinkedList` (Danh sách liên kết đôi).
* **C.** `HashSet` / `HashMap` / `Dictionary` (Bảng băm).
* **D.** `Binary Search Tree` (Cây tìm kiếm nhị phân).

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **C**
* **Cơ chế bẫy:** 
  * Nếu dùng `List` hoặc `LinkedList`, khi tìm một phần tử, hệ thống phải quét từ đầu đến cuối mảng, độ phức tạp là $O(N)$ (Mảng có 1 triệu phần tử thì phải quét tối đa 1 triệu lần).
  * Nếu dùng Bảng băm (`HashMap`/`Dictionary`), hệ thống sử dụng thuật toán băm để chuyển mã nhân viên thẳng thành địa chỉ ô nhớ lưu dữ liệu đó. Bạn chỉ cần đưa mã vào là hệ thống lấy ra được ngay kết quả trong **1 bước duy nhất ($O(1)$)**, bất kể danh sách có lớn bao nhiêu đi chăng nữa.

---

## BẪY 24: Lỗi tràn tầng bộ nhớ Stack do Đệ quy (Stack Overflow)
* **Mức độ xuất hiện:** 70% (Bẫy tư duy logic giải thuật).

### Đề bài mô phỏng
**Câu hỏi:** Nguyên nhân phổ biến nhất khiến một hàm viết theo phương pháp Đệ quy (Function tự gọi lại chính nó) bị sập hệ thống và ném ra lỗi `StackOverflowException` khi thực thi là gì?
* **A.** Do truyền vào tham số có kiểu dữ liệu quá lớn.
* **B.** Do hàm đệ quy không có **điểm dừng (Base case)**, hoặc điểm dừng được thiết kế sai khiến hàm tự gọi lại chính nó vô hạn lần, làm cạn kiệt phân vùng bộ nhớ Stack của luồng đó.
* **C.** Do hệ thống bị mất kết nối Internet khi đang tính toán.
* **D.** Do hàm đệ quy chạy quá 60 giây.

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B**
* **Cơ chế bẫy:** Mỗi lần một hàm được gọi, hệ thống sẽ sinh ra một `Stack Frame` để lưu trữ các biến cục bộ và tham số của hàm đó vào bộ nhớ Stack. Nếu hàm đệ quy chạy vô hạn, các Stack Frame này sẽ xếp chồng lên nhau liên tục cho đến khi vượt quá giới hạn vật lý của bộ nhớ Stack, gây sập ứng dụng ngay lập tức.
