# Day 4-5 - Deadlock

## Mục tiêu

Hiểu:

* Deadlock là gì
* Tại sao Deadlock xảy ra
* PostgreSQL xử lý Deadlock như thế nào
* Cách tránh Deadlock trong hệ thống ngân hàng

---

# Deadlock là gì

Deadlock xảy ra khi nhiều transaction giữ lock của nhau và cùng chờ tài nguyên mà transaction khác đang giữ.

Kết quả:

```text
Transaction A chờ Transaction B

Transaction B chờ Transaction A
```

Không transaction nào tiếp tục được.

---

# Ví dụ đơn giản

## Transaction 1

```text
Lock Account A
↓
Muốn Lock Account B
```

---

## Transaction 2

```text
Lock Account B
↓
Muốn Lock Account A
```

---

## Kết quả

```text
Tx1 chờ Tx2

Tx2 chờ Tx1
```

Đây chính là Deadlock.

---

# Deadlock có phải lỗi của PostgreSQL?

Không.

Deadlock là hậu quả của việc thiết kế transaction không hợp lý.

Ví dụ:

```text
Tx1:
Lock A
Lock B

Tx2:
Lock B
Lock A
```

Thiết kế này rất dễ gây Deadlock.

---

# PostgreSQL xử lý Deadlock như thế nào

PostgreSQL liên tục theo dõi:

```text
Transaction nào đang chờ transaction nào
```

Ví dụ:

```text
Tx1 -> Tx2

Tx2 -> Tx1
```

Tạo thành vòng:

```text
Tx1 -> Tx2 -> Tx1
```

PostgreSQL phát hiện:

```text
deadlock detected
```

và chọn một transaction làm nạn nhân.

Ví dụ:

```text
Rollback Tx2
```

Sau đó:

```text
Tx1 tiếp tục chạy
```

---

# Ví dụ thực tế trong ngân hàng

Giả sử:

```text
Account 10
Account 20
```

Có hai giao dịch:

```text
Tx1:
10 -> 20

Tx2:
20 -> 10
```

---

Nếu lock theo chiều transfer:

```text
Tx1:
Lock 10
Lock 20

Tx2:
Lock 20
Lock 10
```

Có nguy cơ Deadlock.

---

# Cách tránh Deadlock

Nguyên tắc:

> Luôn lock dữ liệu theo cùng một thứ tự.

Ví dụ:

```text
Lock account có id nhỏ trước
Lock account có id lớn sau
```

---

Dù:

```text
10 -> 20
```

hay:

```text
20 -> 10
```

đều:

```text
Lock 10
Lock 20
```

Nhờ vậy không tạo vòng chờ.

---

# Ví dụ PostgreSQL

```sql
BEGIN;

SELECT *
FROM accounts
WHERE id = 10
FOR UPDATE;

SELECT *
FROM accounts
WHERE id = 20
FOR UPDATE;

COMMIT;
```

---

# Ví dụ Spring Boot (sẽ học sau)

```java
@Transactional
public void transfer(...) {
    ...
}
```

Nếu lock tài khoản sai thứ tự vẫn có thể gây Deadlock.

Do đó:

```text
Transaction
≠
Không có Deadlock
```

---

# Mối liên hệ với bài trước

```text
Transaction
↓
ACID
↓
Isolation
↓
Lock
↓
Deadlock
```

Deadlock là hệ quả trực tiếp của việc sử dụng Lock.

---

# Câu hỏi phỏng vấn

## Deadlock là gì?

Deadlock xảy ra khi nhiều transaction giữ lock của nhau và cùng chờ tài nguyên mà transaction khác đang giữ, dẫn đến không transaction nào tiếp tục được.

---

## PostgreSQL xử lý Deadlock như thế nào?

PostgreSQL phát hiện vòng chờ (wait cycle) và rollback một transaction để phá vỡ Deadlock.

---

## Cách giảm Deadlock?

* Lock theo cùng thứ tự
* Giữ transaction ngắn
* Chỉ lock dữ liệu cần thiết
* Retry transaction khi gặp deadlock

---

# Tôi học được gì

* Deadlock không phải bug của PostgreSQL.
* Deadlock là lỗi thiết kế transaction.
* PostgreSQL sẽ rollback một transaction để phá vòng chờ.
* Lock theo cùng thứ tự là cách đơn giản nhất để tránh Deadlock.
* Trong hệ thống ngân hàng, transfer tiền là nơi dễ phát sinh Deadlock nhất.

---

# Tiến độ

```text
Week 1

✓ Day 1 - Index
✓ Day 2 - EXPLAIN ANALYZE
✓ Day 3 - Transaction + ACID + Lock
✓ Day 4-5 - Deadlock

Next:
Day 6 - Java Setup
Day 7 - Java Core
```
