# Day 2 - EXPLAIN ANALYZE

## Mục tiêu

Hiểu PostgreSQL thực thi query như thế nào.

---

## EXPLAIN

Cho biết PostgreSQL dự định chạy query ra sao.

Ví dụ:

```sql
EXPLAIN
SELECT *
FROM users
WHERE email = 'a@test.com';
```

---

## EXPLAIN ANALYZE

Thực sự chạy query và trả về execution plan.

Ví dụ:

```sql
EXPLAIN ANALYZE
SELECT *
FROM users
WHERE email = 'a@test.com';
```

---

## Seq Scan

PostgreSQL đọc toàn bộ bảng.

Ví dụ:

```text
1 -> 2 -> 3 -> 4 -> 5 -> ...
```

Thường xuất hiện khi:

* Bảng nhỏ
* Không có index
* Selectivity thấp

---

## Index Scan

PostgreSQL sử dụng Index.

Ví dụ:

```sql
WHERE email = 'a@test.com'
```

Thường nhanh hơn khi số lượng record lớn.

---

## PostgreSQL không phải lúc nào cũng dùng Index

Ví dụ:

```sql
WHERE status = 'active'
```

Nếu 95% record là active.

PostgreSQL có thể chọn:

```text
Seq Scan
```

vì rẻ hơn.

---

## Tôi học được gì

* Phải dùng EXPLAIN ANALYZE để kiểm tra giả định.
* Có index không đồng nghĩa với việc PostgreSQL sẽ dùng index.
* Query plan quan trọng hơn cảm giác.
