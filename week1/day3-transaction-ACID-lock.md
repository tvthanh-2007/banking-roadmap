# Day 3 - Transaction, ACID và Lock

## Mục tiêu

Hiểu cách ngân hàng đảm bảo giao dịch tiền luôn chính xác.

---

## Transaction là gì

Transaction là một nhóm thao tác:

```text
Hoặc thành công tất cả
Hoặc thất bại tất cả
```

Ví dụ:

```text
A -> B : 100
```

Bao gồm:

```text
1. Trừ tiền A
2. Cộng tiền B
```

---

## Transaction thuộc về Database

Rails:

```ruby
ActiveRecord::Base.transaction do
end
```

Spring:

```java
@Transactional
```

Cuối cùng đều biến thành:

```sql
BEGIN;
...
COMMIT;
```

trên PostgreSQL.

---

# ACID

## Atomicity

Làm hết hoặc không làm gì.

Ví dụ:

```text
✓ Trừ A + Cộng B

hoặc

✗ Không làm gì cả
```

---

## Consistency

Dữ liệu luôn đúng.

Ví dụ:

```text
A = 1000
B = 500

Tổng = 1500
```

Sau transfer:

```text
A = 900
B = 600

Tổng = 1500
```

---

## Isolation

Các transaction không được ảnh hưởng lẫn nhau.

Ví dụ:

```text
2 request cùng rút tiền
```

không được làm sai số dư.

---

## Durability

Sau:

```sql
COMMIT;
```

Mất điện.

Restart database.

Dữ liệu vẫn còn.

---

# Lock

Isolation là mục tiêu.

Lock là một trong các cơ chế để đạt được Isolation.

---

## Row Lock

```sql
SELECT *
FROM accounts
WHERE id = 1
FOR UPDATE;
```

Khóa row tương ứng.

---

## Tại sao cần Lock

Tài khoản:

```text
1000
```

Request 1:

```text
Rút 800
```

Request 2:

```text
Rút 500
```

Nếu không lock:

```text
Cả hai cùng đọc 1000
```

=> Có thể rút quá số tiền thực có.

---

## FOR UPDATE

Transaction đầu tiên:

```sql
SELECT ...
FOR UPDATE;
```

sẽ khóa row.

Transaction thứ hai phải:

```text
WAIT
```

cho đến khi transaction đầu tiên COMMIT hoặc ROLLBACK.

---

## Tôi học được gì

* Transaction là tính năng của Database.
* ACID là nền tảng của mọi hệ thống ngân hàng.
* Isolation là mục tiêu.
* Lock là cơ chế giúp đạt được Isolation.
* FOR UPDATE giúp tránh race condition khi cập nhật dữ liệu tài chính.
