# 8. Thread ⭐⭐⭐⭐

> Mục tiêu: Đủ kiến thức để phỏng vấn Java Backend (Junior/Mid).

---

# 1. Process vs Thread ⭐⭐⭐⭐⭐

## Process

- Là một chương trình đang chạy.
- Có vùng nhớ riêng.
- Không chia sẻ bộ nhớ với process khác.

Ví dụ:

- Chrome
- VSCode
- Spotify

=> Là 3 process.

---

## Thread

Là luồng thực thi bên trong Process.

Một Process có thể có nhiều Thread.

### Chia sẻ gì?

- ✅ Heap
- ✅ File
- ✅ Socket

### Riêng mỗi Thread

- ✅ Stack

---

## Câu hỏi

### Process và Thread khác nhau?

**Đáp**

| Process | Thread |
|----------|---------|
| Bộ nhớ riêng | Dùng chung Heap |
| Nặng | Nhẹ |
| Tạo chậm | Tạo nhanh |
| Context Switch chậm | Context Switch nhanh |

---

### Bẫy

**Thread có dùng chung Stack không?**

❌ Không.

Mỗi Thread có Stack riêng.

---

# 2. Thread Lifecycle ⭐⭐⭐⭐

```
NEW
 ↓
RUNNABLE
 ↓
BLOCKED / WAITING / TIMED_WAITING
 ↓
TERMINATED
```

---

## start() vs run()

```java
Thread t = new Thread(...);

t.start();
```

✅ Tạo Thread mới.

Lifecycle:

```
NEW
↓

RUNNABLE
```

---

```java
t.run();
```

❌ Không tạo Thread.

Chỉ gọi method bình thường.

Chạy trên Main Thread.

---

## Câu hỏi

### start() khác run() thế nào?

**Đáp**

- `start()` tạo Thread mới.
- `run()` chỉ gọi method.

> Đây là câu phỏng vấn rất hay.

---

# 3. synchronized ⭐⭐⭐⭐⭐

Dùng để đảm bảo Thread-safe.

```java
public synchronized void increase() {
    count++;
}
```

Một thời điểm chỉ có **1 Thread** được vào.

---

## synchronized lock gì?

Method

→ lock `this`

Static method

→ lock `Class`

---

## Câu hỏi

### synchronized dùng để làm gì?

**Đáp**

Đồng bộ dữ liệu khi nhiều Thread cùng truy cập.

---

### synchronized rồi có cần volatile không?

❌ Không.

`synchronized` đã đảm bảo:

- Atomicity
- Visibility

---

# 4. volatile ⭐⭐⭐⭐

## volatile dùng để làm gì?

Đảm bảo **Visibility**.

Một Thread thay đổi giá trị.

Các Thread khác sẽ nhìn thấy ngay ở lần đọc tiếp theo.

Ví dụ:

```java
volatile boolean running = true;
```

```java
while (running) {
    // làm việc
}
```

Thread khác

```java
running = false;
```

Thread sẽ thoát vòng lặp.

---

## volatile KHÔNG đảm bảo

Atomicity.

Ví dụ

```java
volatile int count;

count++;
```

❌ Không Thread-safe.

Vì

```
Đọc

↓

Cộng 1

↓

Ghi lại
```

Là 3 bước.

Có thể bị Thread khác chen vào.

---

## Câu hỏi

### volatile dùng để làm gì?

**Đáp**

Đảm bảo Visibility.

---

### volatile có thay synchronized được không?

❌ Không.

---

### volatile có Thread-safe không?

❌ Không.

---

# 5. Atomic ⭐⭐⭐⭐

Nếu chỉ cập nhật **1 biến** nhiều Thread.

Dùng:

```java
AtomicInteger count =
    new AtomicInteger();

count.incrementAndGet();
```

Ưu điểm

- Thread-safe
- Không cần synchronized

---

## Khi nào dùng?

- Counter
- Request count
- Online user
- ID Generator

---

## Câu hỏi

### AtomicInteger khác synchronized?

**Đáp**

Atomic phù hợp cập nhật **1 biến**.

synchronized dùng khi cần đồng bộ nhiều thao tác hoặc nhiều biến.

---

# 6. Deadlock ⭐⭐⭐⭐

Ví dụ

Thread A

```
Lock A

↓

Đợi Lock B
```

Thread B

```
Lock B

↓

Đợi Lock A
```

=> Không Thread nào chạy tiếp.

---

## Cách tránh

- Lock theo cùng thứ tự.
- Giảm nested lock.
- Dùng timeout (`tryLock()`).

---

## Câu hỏi

### Deadlock là gì?

**Đáp**

Hai hoặc nhiều Thread giữ lock của nhau và chờ nhau vô thời hạn.

---

# Câu hỏi phỏng vấn

## 1.

**Process khác Thread?**

**Đáp**

- Process có bộ nhớ riêng.
- Thread dùng chung Heap.
- Thread có Stack riêng.
- Thread nhẹ hơn Process.

---

## 2.

**start() và run() khác gì?**

**Đáp**

- `start()` tạo Thread mới.
- `run()` chỉ gọi method.

---

## 3.

**Thread có dùng chung Stack không?**

**Đáp**

Không.

Mỗi Thread có Stack riêng.

---

## 4.

**synchronized dùng để làm gì?**

**Đáp**

Đồng bộ dữ liệu.

Một thời điểm chỉ một Thread được thực thi vùng synchronized.

---

## 5.

**Đã synchronized có cần volatile không?**

**Đáp**

Không.

---

## 6.

**volatile dùng để làm gì?**

**Đáp**

Đảm bảo Visibility.

Không đảm bảo Atomicity.

---

## 7.

**Tại sao volatile count++ không an toàn?**

**Đáp**

`count++` gồm:

- Read
- Increment
- Write

Là 3 bước.

Có thể bị nhiều Thread thực hiện cùng lúc.

---

## 8.

**AtomicInteger dùng khi nào?**

**Đáp**

Khi nhiều Thread chỉ cập nhật **1 biến** (counter, request, online user...).

---

## 9.

**Deadlock là gì?**

**Đáp**

Hai hoặc nhiều Thread giữ lock của nhau và chờ nhau vô thời hạn.

---

# Những gì cần nhớ

✅ Process vs Thread

✅ Thread Lifecycle

✅ `start()` vs `run()`

✅ `synchronized`

✅ `volatile` (Visibility ≠ Atomicity)

✅ `AtomicInteger`

✅ Deadlock

---

# Không cần học sâu (Junior/Mid)

- Runnable
- Callable
- Future
- ExecutorService

> Chỉ cần biết:
>
> - `Runnable`: mô tả một công việc chạy trên Thread.
> - `ExecutorService`: quản lý Thread Pool (Spring Boot thường dùng sẵn qua `@Async`).
> - `Callable` và `Future`: dùng khi cần chạy bất đồng bộ và lấy kết quả trả về.
```
