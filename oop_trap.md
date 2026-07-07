# PHẦN II: CÁC DẠNG BẪY LẬP TRÌNH HƯỚNG ĐỐI TƯỢNG (OOP)

## BẪY 9: Phân biệt Overriding (Ghi đè) và Overloading (Nạp chồng)
* **Mức độ xuất hiện:** 90% (Câu hỏi kinh điển để kiểm tra tư duy Đa hình).
* **Mục tiêu kiểm tra:** Khả năng phân biệt cơ chế chạy ở Compile-time và Runtime.

### Đề bài mô phỏng
**Câu hỏi:** Phát biểu nào sau đây về `Overriding` và `Overloading` là **SAI**?
* **A.** Overloading xảy ra trong cùng một lớp, còn Overriding xảy ra giữa lớp cha và lớp con.
* **B.** Overloading được quyết định tại thời điểm biên dịch (Compile-time), còn Overriding được quyết định tại thời điểm thực thi (Runtime).
* **C.** Khi Overriding một phương thức, bạn có thể thay đổi danh sách tham số (thêm/bớt tham số) tùy ý miễn là giữ nguyên tên hàm.
* **D.** Phương thức `static` có thể Overload nhưng không thể Override.

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **C** (Phát biểu này sai).
* **Cơ chế bẫy:** 
  * Nhiều bạn nhầm lẫn giữa hai khái niệm này. **Overriding** (Ghi đè) bắt buộc phương thức ở lớp con phải **giống y hệt** lớp cha về cả: Tên hàm, số lượng tham số, kiểu dữ liệu tham số và kiểu trả về. 
  * Nếu bạn thay đổi danh sách tham số thì hệ thống sẽ hiểu đó là một phương thức **Overloading** (Nạp chồng) hoàn toàn mới, chứ không còn là ghi đè nữa.

---

## BẪY 10: Sự khác biệt bản chất giữa Interface và Abstract Class
* **Mức độ xuất hiện:** 85% (Bẫy lý thuyết sâu về kiến trúc phần mềm).

### Đề bài mô phỏng
**Câu hỏi:** Khi nào ứng viên nên ưu tiên sử dụng `Interface` thay vì `Abstract Class`?
* **A.** Khi muốn các lớp con kế thừa lại các thuộc tính (biến) và các hàm đã được viết sẵn mã xử lý (Code implementation).
* **B.** Khi hệ thống cần thiết kế các tính năng đa kế thừa (một lớp có thể triển khai nhiều khuôn mẫu hành vi khác nhau).
* **C.** Khi muốn định nghĩa các phương thức có phạm vi truy cập là `protected` hoặc `private`.
* **D.** Khi muốn tạo ra một lớp có thể khởi tạo trực tiếp bằng từ khóa `new`.

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B**
* **Cơ chế bẫy:** Đề thi rất hay đố mẹo về giới hạn kế thừa:
  * Trong các ngôn ngữ như C#, Java, một lớp con **chỉ được phép kế thừa duy nhất từ 1 lớp cha** (kể cả Abstract Class). Do đó chọn Abstract Class sẽ mất đi khả năng đa kế thừa.
  * Ngược lại, một lớp có thể **triển khai (implement) cùng lúc nhiều Interface**. Do đó Interface là vũ khí duy nhất để đạt được đa kế thừa hành vi trong OOP.
  * *Lưu ý phụ:* Interface không thể chứa các thuộc tính thực thể (instance fields) và không thể dùng modifier `private`/`protected` cho các hàm định nghĩa (đối với các phiên bản ngôn ngữ đời cũ).

---

## BẪY 11: Phạm vi truy cập (Access Modifiers) kết hợp tính Kế thừa
* **Mức độ xuất hiện:** 75% (Kiểm tra khả năng đóng gói và bảo mật mã nguồn).

### Đề bài mô phỏng
Cho cấu trúc lớp như sau:
* Lớp `Cha` có một thuộc tính `protected int MaSo;` và một thuộc tính `private string BiMat;`.
* Lớp `Con` kế thừa từ lớp `Cha`.

**Câu hỏi:** Lớp `Con` có thể truy cập trực tiếp vào những thuộc tính nào của lớp `Cha`?
* **A.** Cả `MaSo` và `BiMat`.
* **B.** Chỉ truy cập được `MaSo`.
* **C.** Chỉ truy cập được `BiMat`.
* **D.** Không truy cập được thuộc tính nào nếu không dùng hàm Getter/Setter.

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B**
* **Cơ chế bẫy:** 
  * Từ khóa `private` giới hạn tuyệt đối: Thuộc tính/phương thức đó **chỉ được phép thấy và dùng bên trong chính cái lớp định nghĩa ra nó**. Lớp con dù có kế thừa cũng không được phép đụng vào trực tiếp.
  * Từ khóa `protected` sinh ra là để phục vụ kế thừa: Cho phép chính lớp đó và **tất cả các lớp con kế thừa từ nó** được quyền truy cập trực tiếp, bất chấp lớp con có nằm ở package/namespace khác hay không.

---

## BẪY 12: Hàm khởi tạo (Constructor) trong mối quan hệ Cha - Con
* **Mức độ xuất hiện:** 70% (Bẫy luồng chạy của bộ nhớ).

### Đề bài mô phỏng
**Câu hỏi:** Khi bạn khởi tạo một đối tượng của lớp `Con` (lớp này kế thừa từ lớp `Cha`), thứ tự gọi các hàm khởi tạo (Constructor) của hệ thống diễn ra như thế nào?
* **A.** Chỉ có Constructor của lớp `Con` được chạy.
* **B.** Constructor của lớp `Con` chạy trước, sau đó mới gọi Constructor của lớp `Cha`.
* **C.** Constructor của lớp `Cha` chạy trước để tạo nền móng, sau đó mới gọi Constructor của lớp `Con`.
* **D.** Hệ thống chọn ngẫu nhiên tùy thuộc vào bộ nhớ RAM trống.

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **C**
* **Cơ chế bẫy:** Đây là bẫy tư duy logic. Lớp con muốn tồn tại thì vùng bộ nhớ đại diện cho các đặc tính của lớp cha phải được hình thành trước. Do đó, khi bạn gọi `new Con()`, trình biên dịch sẽ âm thầm gọi lệnh `super()` (hoặc `base()`) để **chạy Constructor của lớp Cha trước**, sau đó mới quay lại thực thi các dòng lệnh trong Constructor của lớp Con. Nếu lớp cha không có constructor không tham số mặc định, lớp con sẽ báo lỗi biên dịch ngay lập tức nếu không gọi tường minh.

## BẪY 13: Bẫy thay đổi giá trị của hằng số đối tượng (Final / Readonly)
* **Mức độ xuất hiện:** 80% (Kiểm tra sự hiểu biết về vùng nhớ Stack và Heap).
* **Mục tiêu kiểm tra:** Phân biệt giữa hằng số nguyên thủy và hằng số tham chiếu.

### Đề bài mô phỏng
Trong Java hoặc C#, bạn khai báo một đối tượng là hằng số bằng từ khóa `final` hoặc `readonly`:
```java
final NhanVien nv = new NhanVien("Nguyễn Văn A");
```
**Câu hỏi:** Hành động nào sau đây sẽ bị hệ thống **BÁO LỖI BIÊN DỊCH**?
* **A.** `nv.setTen("Trần Thị B");` (Thay đổi thuộc tính Tên của nhân viên đó).
* **B.** `nv = new NhanVien("Lê Văn C");` (Gán đối tượng `nv` cho một vùng nhớ mới).
* **C.** Cả A và B đều không báo lỗi.
* **D.** Cả A và B đều báo lỗi.

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B**
* **Cơ chế bẫy:** Rất nhiều bạn nghĩ rằng đã là `final` hoặc `readonly` thì hoàn toàn không thể sửa đổi bất cứ thứ gì liên quan đến biến đó. 
  * Bản chất của `final`/`readonly` đối với một đối tượng (Reference Type) là **cố định địa chỉ ô nhớ** mà biến đó trỏ tới (nằm ở vùng nhớ Stack). Bạn không được phép bắt biến `nv` trỏ sang một ô nhớ mới (Hành động B sai).
  * Tuy nhiên, nội dung bên trong ô nhớ đó (vùng nhớ Heap) thay đổi thoải mái. Việc bạn sửa tên nhân viên (Hành động A) hoàn toàn hợp lệ và chạy bình thường.

---

## BẪY 14: Hàm khởi tạo tĩnh (Static Constructor) chạy khi nào?
* **Mức độ xuất hiện:** 70% (Dành riêng cho các ngôn ngữ như C# hoặc các khối static block trong Java).

### Đề bài mô phỏng
Cho một lớp `HeThongVNPT` có cả hàm khởi tạo thông thường (Instance Constructor) và hàm khởi tạo tĩnh (Static Constructor). 
```csharp
public class HeThongVNPT {
    static HeThongVNPT() { Console.WriteLine("Static Run"); }
    public HeThongVNPT() { Console.WriteLine("Instance Run"); }
}
```
Bạn thực hiện đoạn code sau:
```csharp
HeThongVNPT p1 = new HeThongVNPT();
HeThongVNPT p2 = new HeThongVNPT();
```
**Câu hỏi:** Kết quả in ra màn hình của đoạn code trên là gì?
* **A.** 
  Static Run
  Instance Run
  Instance Run
* **B.** 
  Static Run
  Instance Run
  Static Run
  Instance Run
* **C.** 
  Instance Run
  Instance Run

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **A**
* **Cơ chế bẫy:** Đề thi đố bạn về vòng đời của thành phần `static`.
  * Hàm khởi tạo tĩnh (`Static Constructor`) **chỉ được gọi duy nhất một lần** trong suốt vòng đời của ứng dụng, ngay khi lớp đó được nạp vào bộ nhớ (lần đầu tiên bạn tạo đối tượng hoặc gọi một thành phần static của lớp đó).
  * Ở lần tạo đối tượng `p2`, hệ thống nhận thấy lớp này đã được nạp rồi, nên nó bỏ qua Static Constructor và chỉ chạy Instance Constructor thông thường.

---

## BẪY 15: Bản chất của Kiểu tham chiếu (Reference Type) khi truyền vào hàm
* **Mức độ xuất hiện:** 85% (Bẫy kinh điển về quản lý bộ nhớ).

### Đề bài mô phỏng
Cho phương thức xử lý dữ liệu sau:
```csharp
void TangLuong(NhanVien nhanVien) {
    nhanVien.Luong = 1000;
    nhanVien = new NhanVien();
    nhanVien.Luong = 2000;
}
```
Ngoài hàm `Main`, bạn chạy đoạn code:
```csharp
NhanVien nvChinh = new NhanVien();
nvChinh.Luong = 500;
TangLuong(nvChinh);
Console.WriteLine(nvChinh.Luong);
```
**Câu hỏi:** Màn hình sẽ in ra giá trị lương của `nvChinh` là bao nhiêu?
* **A.** 500
* **B.** 1000
* **C.** 2000
* **D.** 0

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B. 1000**
* **Cơ chế bẫy:** Câu này lừa thị giác ở chỗ có hai lần gán lương (1000 và 2000).
  * Khi truyền `nvChinh` vào hàm, hệ thống truyền một **bản sao của địa chỉ ô nhớ**. 
  * Dòng lệnh `nhanVien.Luong = 1000;` tác động trực tiếp vào ô nhớ gốc của `nvChinh` ngoài hàm Main, làm lương tăng lên 1000.
  * Dòng lệnh tiếp theo `nhanVien = new NhanVien();` đã bắt cái bản sao địa chỉ này trỏ sang một ô nhớ hoàn toàn mới. Kể từ giây phút này, mọi thay đổi (như gán lương bằng 2000) chỉ có tác dụng trên ô nhớ mới đó, ô nhớ gốc của `nvChinh` không bị ảnh hưởng nữa.

---

## BẪY 16: Ép kiểu ngược (Downcasting) trong Kế thừa
* **Mức độ xuất hiện:** 75% (Bẫy gây lỗi Runtime nguy hiểm).

### Đề bài mô phỏng
Cho mối quan hệ: Lớp `BoGiaoThong` là lớp cha, lớp `TrungTamVNPT` là lớp con kế thừa từ lớp cha.
Xét 2 đoạn mã sau:
* **Đoạn 1:** `BoGiaoThong obj1 = new TrungTamVNPT(); TrungTamVNPT t1 = (TrungTamVNPT)obj1;`
* **Đoạn 2:** `BoGiaoThong obj2 = new BoGiaoThong(); TrungTamVNPT t2 = (TrungTamVNPT)obj2;`

**Câu hỏi:** Nhận xét nào sau đây về 2 đoạn mã trên là đúng khi biên dịch và chạy?
* **A.** Cả hai đoạn mã đều chạy thành công tốt đẹp.
* **B.** Cả hai đoạn mã đều bị lỗi ngay khi bấm Biên dịch (Compile error).
* **C.** Đoạn 1 chạy thành công, Đoạn 2 vượt qua vòng biên dịch nhưng sẽ **bị sập (Crash/Runtime Error)** khi chạy.
* **D.** Đoạn 2 chạy thành công, Đoạn 1 bị lỗi.

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **C**
* **Cơ chế bẫy:** Đây gọi là hiện tượng **Downcasting** (ép kiểu từ lớp cha về lớp con).
  * Trình biên dịch (Compiler) rất dễ tính, nó chỉ kiểm tra xem hai lớp này có quan hệ cha con hay không. Vì có quan hệ nên cả đoạn 1 và đoạn 2 đều **qua được vòng biên dịch**.
  * Tuy nhiên, khi chạy thực tế (Runtime), ở **Đoạn 2**, bản chất cái vùng nhớ của `obj2` được sinh ra thuần túy là của lớp cha `BoGiaoThong`. Lớp cha thì không thể có các thuộc tính, hàm đặc thù của lớp con. Do đó, hệ thống sẽ ném ra một ngoại lệ `ClassCastException` hoặc `InvalidCastException` và làm sập ứng dụng ngay lập tức.
