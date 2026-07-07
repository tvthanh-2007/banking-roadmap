# CẨM NANG GIẢI BẪY SQL - ÔN THI TUYỂN DỤNG VNPT KHỐI TỈNH

Tài liệu tổng hợp các dạng câu hỏi "gạt giò" ứng viên kinh điển trong ngân hàng đề thi hệ thống TAMS của VNPT. Giúp bạn nhận diện bẫy nhanh trong vòng 5 giây và giành điểm tuyệt đối phần SQL dưới áp lực thời gian.

---

## BẪY 1: Toán tử `NOT IN` kết hợp tập hợp có chứa giá trị `NULL`
* **Mức độ xuất hiện:** 95% (Cực kỳ phổ biến để loại ứng viên hấp tấp).
* **Mục tiêu kiểm tra:** Kiến thức về logic tam trị (True/False/Unknown) của `NULL`.

### Đề bài mô phỏng
Cho bảng `NHANVIEN`:

| MaNV | TenNV | MaNguoiQuanLy |
| :--- | :--- | :--- |
| 1 | Nguyễn Văn A | NULL |
| 2 | Trần Thị B | 1 |
| 3 | Lê Văn C | 1 |
| 4 | Phạm Văn D | 2 |

**Câu hỏi:** Kết quả của câu lệnh SQL sau đây trả về bao nhiêu dòng?
```sql
SELECT COUNT(*) 
FROM NHANVIEN 
WHERE MaNV NOT IN (SELECT MaNguoiQuanLy FROM NHANVIEN);
```
* **A.** 2
* **B.** 0
* **C.** 3
* **D.** Báo lỗi hệ thống

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B. 0**
* **Cơ chế bẫy:** Câu lệnh con (subquery) trả về tập hợp gồm các mã người quản lý: `(NULL, 1, 2)`. Phép toán lúc này tương đương với: `WHERE MaNV <> 1 AND MaNV <> 2 AND MaNV <> NULL`. 
In SQL, bất kỳ phép so sánh nào với `NULL` đều trả về `UNKNOWN`. Vì các điều kiện nối với nhau bằng toán tử `AND`, chỉ cần một vế bị `UNKNOWN` thì toàn bộ mệnh đề `WHERE` bị hủy bỏ (False). Hệ thống không duyệt bất kỳ dòng nào.
* **Cách sửa đúng:** Sử dụng `NOT EXISTS` để thay thế nhằm đảm bảo an toàn, hoặc loại bỏ phần tử NULL trong subquery: `WHERE MaNguoiQuanLy IS NOT NULL`.

---

## BẪY 2: Đặt điều kiện lọc ở mệnh đề `WHERE` thay vì `ON` trong `LEFT JOIN`
* **Mức độ xuất hiện:** 80% (Chuyên dùng trong các bài toán đếm/tính tổng dữ liệu đối tác, dự án).
* **Mục tiêu kiểm tra:** Hiểu bản chất thứ tự xử lý dữ liệu của các loại JOIN.

### Đề bài mô phỏng
Cho hai bảng `PHONG_BAN` (MaPB, TenPB) và `DU_AN` (MaDA, TenDA, MaPB, NganSach). 
**Câu hỏi:** Yêu cầu lấy ra danh sách tên tất cả phòng ban và tổng ngân sách dự án của phòng đó. Phải giữ lại các phòng ban chưa có dự án (tổng ngân sách là NULL hoặc 0), nhưng chỉ lọc các dự án có `NganSach > 100`. Câu lệnh nào đúng?

* **A.** 
```sql
SELECT P.TenPB, SUM(D.NganSach)
FROM PHONG_BAN P LEFT JOIN DU_AN D ON P.MaPB = D.MaPB
WHERE D.NganSach > 100 GROUP BY P.TenPB;
```
* **B.** 
```sql
SELECT P.TenPB, SUM(D.NganSach)
FROM PHONG_BAN P LEFT JOIN DU_AN D ON P.MaPB = D.MaPB AND D.NganSach > 100
GROUP BY P.TenPB;
```

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B**
* **Cơ chế bẫy:** Nếu chọn câu **A**, mệnh đề `WHERE D.NganSach > 100` được thực thi *sau* khi kết nối 2 bảng. Những phòng ban không có dự án, giá trị `NganSach` của họ sẽ bị gán là `NULL`. Phép so sánh `NULL > 100` là sai nên các phòng ban trống lịch lập tức bị xóa bỏ khỏi kết quả đầu ra. Điều này vô tình biến phép `LEFT JOIN` thành `INNER JOIN`.
* **Cách giải quyết:** Đưa điều kiện lọc thẳng vào mệnh đề `ON`. Hệ thống lọc bảng `DU_AN` trước rồi mới ghép nối, đảm bảo giữ nguyên bảng bên trái (`PHONG_BAN`).

---

## BẪY 3: Ép kiểu số nguyên trong phép chia (Integer Division)
* **Mức độ xuất hiện:** 70% (Thường nằm trong các câu hỏi tính tỷ lệ phần trăm).

### Đề bài mô phỏng
Cho bảng `KHOATHI` chứa dữ liệu thi tuyển gồm 2 cột kiểu dữ liệu `INT`: `KiSuDaPass` (giá trị là 15) và `TongSoKiSu` (giá trị là 20).
**Câu hỏi:** Kết quả trả về của câu lệnh dưới đây là bao nhiêu?
```sql
SELECT KiSuDaPass / TongSoKiSu AS TyLe FROM KHOATHI;
```
* **A.** 0.75
* **B.** 0
* **C.** 1
* **D.** Báo lỗi kiểu dữ liệu

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B. 0**
* **Cơ chế bẫy:** Trong các hệ quản trị CSDL lớn (SQL Server, Oracle), phép toán lấy số nguyên (`INT`) chia cho số nguyên (`INT`) sẽ trả về kết quả là một **số nguyên** bằng cách cắt bỏ hoàn toàn phần thập phân phía sau. Vì $15 / 20 = 0.75$, phần nguyên thu được là `0`.
* **Cách giải quyết:** Nhân tử số với `1.0` để ép hệ thống hiểu đây là phép chia số thực:
```sql
SELECT (KiSuDaPass * 1.0) / TongSoKiSu FROM KHOATHI;
```

---

## BẪY 4: Đếm `COUNT(Tên_Cột)` so với `COUNT(*)` khi có dữ liệu `NULL`
* **Mức độ xuất hiện:** 85% (Bẫy lý thuyết cực kỳ dễ lừa người làm bài nhanh).

### Đề bài mô phỏng
Cho bảng `DANH_GIA` lưu điểm thi tuyển dụng:

| MaUngVien | MonThi | DiemSo |
| :--- | :--- | :--- |
| A101 | SQL | 8 |
| A101 | Java | NULL |
| A102 | SQL | 9 |
| A102 | Java | 7 |

**Câu hỏi:** Kết quả trả về của câu lệnh dưới đây là gì?
```sql
SELECT MaUngVien, COUNT(DiemSo) FROM DANH_GIA GROUP BY MaUngVien;
```
* **A.** A101: 2, A102: 2
* **B.** A101: 1, A102: 2
* **C.** A101: 1, A102: 1

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B. A101: 1, A102: 2**
* **Cơ chế bẫy:** 
  * `COUNT(*)`: Đếm tổng số dòng dữ liệu bất kể dòng đó chứa gì.
  * `COUNT(DiemSo)`: Chỉ đếm những dòng nào mà giá trị tại cột `DiemSo` **khác NULL**. Vì ứng viên A101 có môn Java dính điểm `NULL` nên dòng đó bị bỏ qua không đếm.

---

## BẪY 5: Thứ tự thực thi câu lệnh SQL (SQL Order of Execution)
* **Mức độ xuất hiện:** 75% (Kiểm tra xem ứng viên có hiểu cách trình biên dịch SQL hoạt động không).

### Đề bài mô phỏng
**Câu hỏi:** Câu lệnh SQL nào dưới đây chạy thành công và không báo lỗi?
* **A.** 
```sql
SELECT TenNV, Luong + PhuCap AS TongThuNhap 
FROM NHANVIEN WHERE TongThuNhap > 10000000;
```
* **B.** 
```sql
SELECT TenNV, Luong + PhuCap AS TongThuNhap 
FROM NHANVIEN ORDER BY TongThuNhap DESC;
```

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B**
* **Cơ chế bẫy:** Trình biên dịch SQL không chạy từ trên xuống dưới theo thứ tự viết. Thứ tự thực thi chuẩn là: `FROM` ➔ `WHERE` ➔ `GROUP BY` ➔ `HAVING` ➔ `SELECT` ➔ `ORDER BY`. 
  * Ở câu **A**: Do `WHERE` chạy trước `SELECT`, lúc này bí danh `TongThuNhap` chưa hề được khởi tạo ➔ Báo lỗi `Invalid column name`.
  * Ở câu **B**: Do `ORDER BY` chạy cuối cùng, sau khi `SELECT` đã xử lý xong nên nó nhận diện được bí danh `TongThuNhap` hoàn toàn bình thường.

---

## BẪY 6: Ký tự đại diện `_` (Underscore) trong mệnh đề `LIKE`
* **Mức độ xuất hiện:** 60% (Đánh lừa thị giác ứng viên).

### Đề bài mô phỏng
**Câu hỏi:** Để tìm tất cả các mã dự án có chứa ký tự gạch dưới thực sự (Ví dụ: `VNPT_IT`, `VNPT_MEDIA`), câu lệnh nào đúng?
* **A.** `SELECT * FROM DU_AN WHERE MaDA LIKE '%_%';`
* **B.** `SELECT * FROM DU_AN WHERE MaDA LIKE '%!_%' ESCAPE '!';`

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B**
* **Cơ chế bẫy:** Trong mệnh đề `LIKE`, ký tự `_` được công nhận là một ký tự đại diện (Wildcard) cho **một ký tự đơn bất kỳ**. Nếu viết giống câu **A**, hệ thống sẽ hiểu là tìm các chuỗi có độ dài tối thiểu là 1 ký tự (tức là quét sạch cả bảng).
* **Cách giải quyết:** Sử dụng từ khóa `ESCAPE` để biến ký tự liền sau nó thành một ký tự text thông thường.

---

## BẪY 7: Phân biệt các hàm xếp hạng RANK, DENSE_RANK và ROW_NUMBER
* **Mức độ xuất hiện:** 70% (Thường xuất hiện trong các câu hỏi tìm top dữ liệu lớn thứ N).

### Đề bài mô phỏng
Giả sử có 3 ứng viên đồng hạng 1 với số điểm là 10. Ứng viên tiếp theo được 9 điểm. 
**Câu hỏi:** Nếu sử dụng hàm `RANK() OVER (ORDER BY Diem DESC)`, ứng viên 9 điểm sẽ nhận được thứ hạng (Rank) là bao nhiêu?
* **A.** 2
* **B.** 4
* **C.** 3

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B. 4**
* **Cơ chế bẫy:** 
  * `ROW_NUMBER()`: Đánh số thứ tự lũy tiến không trùng nhau (1, 2, 3, 4).
  * `RANK()`: Đồng hạng thì chung số hạng, nhưng số hạng tiếp theo sẽ **bị nhảy cóc** dựa trên số lượng phần tử trùng trước đó (1, 1, 1, 4). Do có 3 người hạng 1, vị trí thứ 2 và 3 bị bỏ qua.
  * `DENSE_RANK()`: Đồng hạng thì chung số hạng, nhưng số hạng tiếp theo **không bị nhảy cóc** (1, 1, 1, 2).

---

## BẪY 8: Mất dữ liệu khi thực hiện phép toán cộng với `NULL`
* **Mức độ xuất hiện:** 65% (Bẫy tính toán thực tế).

### Đề bài mô phỏng
Cho bảng `NHANVIEN` có cột `LuongChinh` là 10,000,000 và cột `TienThuong` mang giá trị `NULL`.
**Câu hỏi:** Giá trị cột `ThuNhap` nhận được sau câu lệnh dưới đây là bao nhiêu?
```sql
SELECT (LuongChinh + TienThuong) AS ThuNhap FROM NHANVIEN;
```
* **A.** 10,000,000
* **B.** NULL
* **C.** 0

### Giải đáp & Cơ chế bẫy
* **Đáp án đúng:** **B. NULL**
* **Cơ chế bẫy:** Trong SQL, `NULL` đại diện cho một giá trị chưa xác định. Do đó, bất kỳ phép toán số học nào (`+`, `-`, `*`, `/`) khi thực hiện với `NULL` đều cho ra kết quả là `NULL`. Nhân viên sẽ bị nuốt toàn bộ lương chính.
* **Cách giải quyết:** Luôn bọc các cột có nguy cơ bị trống bằng hàm kiểm tra NULL như `ISNULL(TienThuong, 0)` hoặc `COALESCE(TienThuong, 0)` để chuyển dữ liệu về số 0 trước khi tính toán.
