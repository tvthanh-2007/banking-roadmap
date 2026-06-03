# Day 5 - Deadlock Practice

## Mục tiêu

Hiểu:

* Deadlock xảy ra như thế nào
* PostgreSQL xử lý Deadlock ra sao
* Tại sao phải rollback một transaction
* Cách tránh Deadlock trong hệ thống Banking

---

# Lab 1 - Deadlock Flow

Giả sử có 2 account:

```text
Account 1
Account 2
```

---

## Transaction 1

```sql
BEGIN;

SELECT *
FROM accounts
WHERE id = 1
FOR UPDATE;
```

Kết quả:

```text
Tx1 đang giữ lock Account 1
```

---

## Transaction 2

```sql
BEGIN;

SELECT *
FROM accounts
WHERE id = 2
FOR UPDATE;
```

Kết quả:

```text
Tx2 đang giữ lock Account 2
```

---

## Transaction 1

```sql
SELECT *
FROM accounts
WHERE id = 2
FOR UPDATE;
```

Kết quả:

```text
WAIT
```

vì Account 2 đang bị Tx2 giữ lock.

---

## Transaction 2

```sql
SELECT *
FROM accounts
WHERE id = 1
FOR UPDATE;
```

Kết quả:

```text
WAIT
```

vì Account 1 đang bị Tx1 giữ lock.

---

# Trạng thái hệ thống

```text
Tx1 đang chờ Tx2 release Account 2

Tx2 đang chờ Tx1 release Account 1
```

---

Biểu diễn dưới dạng graph:

```text
Tx1 -> Tx2

Tx2 -> Tx1
```

Tạo thành vòng chờ.

Đây chính là:

```text
Deadlock
```

---

# Nếu PostgreSQL không xử lý

Kết quả:

```text
Tx1 chờ mãi

Tx2 chờ mãi
```

Không transaction nào hoàn thành.

Hệ thống bị treo.

---

# PostgreSQL xử lý như thế nào

PostgreSQL liên tục theo dõi:

```text
Transaction nào đang chờ transaction nào
```

Nếu phát hiện vòng:

```text
Tx1 -> Tx2 -> Tx1
```

Nó sẽ báo:

```text
ERROR: deadlock detected
```

---

Sau đó PostgreSQL chọn:

```text
1 transaction làm victim
```

Ví dụ:

```text
Rollback Tx2
```

---

Kết quả:

```text
Release lock Account 2

Tx1 tiếp tục chạy
```

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

Rất dễ gây Deadlock.

---

# Banking Example

Giả sử:

```text
Account 10
Account 20
```

---

Transfer 1:

```text
10 -> 20
```

---

Transfer 2:

```text
20 -> 10
```

---

Nếu code:

```text
Lock source account trước
Lock destination account sau
```

thì:

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

Dù giao dịch:

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

---

Kết quả:

```text
Tx2 phải đợi Tx1
```

nhưng:

```text
Không tạo vòng chờ
```

=> Không Deadlock.

---

# Production Impact

Nếu Deadlock xảy ra thường xuyên:

```text
Rollback
↓
Retry
↓
Rollback
↓
Retry
```

---

Hậu quả:

```text
CPU tăng
Latency tăng
Throughput giảm
```

Ảnh hưởng trực tiếp tới hiệu năng hệ thống.

---

# Câu hỏi phỏng vấn

## Deadlock là gì?

Deadlock xảy ra khi nhiều transaction giữ lock của nhau và cùng chờ tài nguyên mà transaction khác đang giữ, dẫn đến không transaction nào tiếp tục được.

---

## PostgreSQL xử lý Deadlock như thế nào?

PostgreSQL phát hiện vòng chờ (wait cycle) và rollback một transaction để phá vỡ Deadlock.

---

## Làm sao giảm Deadlock?

* Lock theo cùng thứ tự
* Giữ transaction ngắn
* Chỉ lock dữ liệu cần thiết
* Retry transaction khi gặp deadlock

---

# Tôi học được gì

* Deadlock là lỗi thiết kế transaction, không phải lỗi PostgreSQL.
* PostgreSQL rollback một transaction để phá vòng chờ.
* Transaction thực chất đang chờ transaction khác release lock.
* Lock Ordering là cách phổ biến nhất để giảm Deadlock.
* Trong Banking, transfer tiền là nơi rất dễ phát sinh Deadlock.

