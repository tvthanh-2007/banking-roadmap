# Day 1 - PostgreSQL Index

## Mục tiêu

Hiểu:

* Index là gì
* Tại sao cần Index
* PostgreSQL sử dụng Index như thế nào
* Khi nào nên và không nên tạo Index

---

# Vấn đề

Giả sử bảng:

```sql
users
```

có:

```text
10,000,000 records
```

Query:

```sql
SELECT *
FROM users
WHERE email = 'user@test.com';
```

Nếu không có Index:

PostgreSQL phải đọc:

```text
row 1
row 2
row 3
...
row 10,000,000
```

để tìm dữ liệu.

Chi phí rất lớn.

---

# Index là gì

Index giống như mục lục của cuốn sách.

Ví dụ:

Muốn tìm chương:

```text
PostgreSQL Transaction
```

Bạn không đọc toàn bộ cuốn sách.

Bạn xem mục lục trước.

Index cũng hoạt động tương tự.

---

# Mục đích của Index

Mục đích chính:

```text
Giảm số lượng row phải đọc
```

Ví dụ:

Không có Index:

```text
10,000,000 rows
```

Có Index:

```text
20 rows
```

hoặc:

```text
1 row
```

---

# Lợi ích của Index

## Tăng tốc độ truy vấn

Ví dụ:

```sql
SELECT *
FROM users
WHERE email = 'user@test.com';
```

Có thể từ:

```text
2 giây
```

thành:

```text
10ms
```

---

## Giảm tải Database

Ít row phải đọc hơn.

Ít CPU hơn.

Ít I/O hơn.

---

## Tăng khả năng mở rộng

Hệ thống vẫn hoạt động tốt khi dữ liệu tăng từ:

```text
10 nghìn
↓
100 nghìn
↓
1 triệu
↓
10 triệu record
```

---

# Nhược điểm của Index

Index không miễn phí.

Mỗi lần:

```sql
INSERT
UPDATE
DELETE
```

PostgreSQL phải cập nhật Index.

Do đó:

```text
Đọc nhanh hơn
Ghi chậm hơn
```

---

# Selectivity

Selectivity là khả năng lọc dữ liệu của một cột.

---

## Ví dụ Selectivity thấp

```sql
gender
```

Chỉ có:

```text
male
female
```

---

10 triệu record:

```text
male   = 5 triệu
female = 5 triệu
```

Query:

```sql
WHERE gender = 'male'
```

vẫn phải đọc:

```text
5 triệu rows
```

Index gần như không có nhiều giá trị.

---

## Ví dụ Selectivity cao

```sql
email
```

Mỗi user có email riêng.

Ví dụ:

```sql
WHERE email = 'abc@test.com'
```

chỉ trả về:

```text
1 row
```

Index cực kỳ hiệu quả.

---

# Composite Index

Ví dụ:

```sql
CREATE INDEX idx_users_status_created_at
ON users(status, created_at);
```

---

# Left-most Prefix Rule

Index:

```text
(status, created_at)
```

Dùng được:

```sql
WHERE status = 'active'
```

```sql
WHERE status = 'active'
AND created_at > NOW()
```

---

Không dùng được tối ưu:

```sql
WHERE created_at > NOW()
```

vì bỏ qua cột đầu tiên.

---

# Khi nào nên tạo Index

## Email

```sql
WHERE email = ?
```

---

## Account Number

```sql
WHERE account_number = ?
```

---

## Foreign Key

```sql
user_id
project_id
account_id
```

---

## Cột thường dùng để tìm kiếm

Ví dụ:

```sql
WHERE customer_id = ?
```

---

# Khi nào không nên tạo Index

Ví dụ:

```sql
is_active
gender
status
```

nếu số lượng giá trị quá ít.

---

Ví dụ:

95% dữ liệu:

```text
status = active
```

Query:

```sql
WHERE status='active'
```

PostgreSQL có thể chọn:

```text
Seq Scan
```

thay vì Index Scan.

---

# Kinh nghiệm thực tế

Không tạo Index vì cảm giác.

Luôn kiểm tra:

```sql
EXPLAIN ANALYZE
```

---

# Câu hỏi phỏng vấn

## Tại sao PostgreSQL có Index nhưng vẫn Seq Scan?

Đáp án:

* Table nhỏ
* Selectivity thấp
* Cost của Seq Scan thấp hơn Index Scan

---

# Tôi học được gì

* Index giúp giảm số row phải đọc.
* Index tăng tốc độ đọc nhưng làm chậm ghi.
* Selectivity càng cao thì Index càng hiệu quả.
* Composite Index tuân theo Left-most Prefix Rule.
* Luôn dùng EXPLAIN ANALYZE để xác nhận PostgreSQL thực sự sử dụng Index.
