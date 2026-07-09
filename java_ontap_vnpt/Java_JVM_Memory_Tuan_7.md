# 7. JVM ⭐⭐⭐⭐

## 1. JVM / JRE / JDK

### JDK (Java Development Kit)

Dành cho **developer**.

Bao gồm:

- JRE
- javac (compiler)
- debugger
- tools

```
.java
   |
 javac
   |
.class
```

---

### JRE (Java Runtime Environment)

Dành để **chạy chương trình Java**.

Bao gồm:

- JVM
- Java Standard Library

Không có compiler.

---

### JVM (Java Virtual Machine)

Máy ảo thực thi file `.class`.

Nhiệm vụ:

- Load Class
- Quản lý bộ nhớ
- Garbage Collection
- Thực thi Bytecode
- Quản lý Thread

```
Java Code
     |
   javac
     |
Bytecode (.class)
     |
     JVM
     |
Windows / Linux / macOS
```

---

### Câu hỏi phỏng vấn

**JDK khác JRE?**

> JDK = JRE + Compiler + Development Tools.

**JRE khác JVM?**

> JRE chứa JVM và thư viện để chạy chương trình Java.

**Tại sao Java đa nền tảng?**

> Vì chỉ cần biên dịch thành Bytecode một lần, JVM của từng hệ điều hành sẽ thực thi Bytecode đó.

---

# 2. Heap

Heap là vùng nhớ dùng để lưu:

- Object
- Array
- Instance Variable

Ví dụ

```java
User user = new User();
```

```
Stack
-----
user
 |
 |

Heap
---------
User Object
```

Heap được chia thành:

- Young Generation
- Old Generation

Garbage Collector chủ yếu hoạt động tại đây.

### Heap có dùng chung không?

Có.

Tất cả các Thread đều dùng chung một Heap.

---

# 3. Stack

Mỗi Thread có một Stack riêng.

Stack lưu:

- Local Variable
- Method Call
- Primitive Variable
- Reference Variable

Ví dụ

```java
void test() {
    int age = 20;
    User user = new User();
}
```

```
Stack

age = 20

user ---->

Heap

User Object
```

Khi method kết thúc:

- Stack Frame bị xóa.
- Object trong Heap chỉ được GC nếu không còn reference.

---

# 4. Metaspace

(Java 8 trở lên)

Lưu:

- Class Metadata
- Method Information
- Field Information
- Annotation

Không lưu Object.

Java 7 trở xuống:

```
PermGen
```

Java 8 trở lên:

```
Metaspace
```

Metaspace nằm trên **Native Memory**.

---

# 5. String Pool

Là vùng đặc biệt trong Heap dùng để lưu String Literal.

```java
String a = "Java";
String b = "Java";
```

```
Pool

"Java"

a ----^

b ----^
```

Chỉ có **1 object**.

---

```java
String c = new String("Java");
```

```
String Pool

"Java"

Heap

new String("Java")
```

Có **2 object**.

---

### intern()

```java
String s = new String("Java");

String x = s.intern();
```

`x` sẽ trỏ đến String trong Pool.

---

# 6. Garbage Collection (GC)

GC tự động thu hồi Object không còn được tham chiếu.

```java
User user = new User();

user = null;
```

Object đủ điều kiện để GC thu hồi.

Lưu ý:

GC **không chạy ngay lập tức**.

JVM sẽ quyết định thời điểm phù hợp.

---

### Có nên gọi System.gc()?

Không.

`System.gc()` chỉ là một lời gợi ý, JVM có thể bỏ qua.

---

# 7. OutOfMemoryError

Xảy ra khi JVM không thể cấp phát thêm bộ nhớ.

Ví dụ

```java
List<byte[]> list = new ArrayList<>();

while (true) {
    list.add(new byte[1024 * 1024]);
}
```

```
OutOfMemoryError
```

Nguyên nhân:

- Memory Leak
- Tạo quá nhiều Object
- Heap quá nhỏ

Khắc phục:

- Tăng Heap (`-Xmx`)
- Tối ưu code
- Sửa Memory Leak

---

# 8. StackOverflowError

Do Stack của Thread bị đầy.

Thường gặp khi đệ quy vô hạn.

```java
void test() {
    test();
}
```

```
test()
test()
test()
...
```

↓

```
StackOverflowError
```

Khắc phục:

- Dừng đệ quy
- Giảm độ sâu recursion
- Dùng vòng lặp nếu phù hợp

---

# So sánh Heap và Stack

| Heap | Stack |
|------|------|
| Lưu Object | Lưu Local Variable |
| Chia sẻ giữa các Thread | Mỗi Thread có Stack riêng |
| GC quản lý | Tự giải phóng khi Method kết thúc |
| Lớn | Nhỏ |
| Chậm hơn | Nhanh hơn |

---

# So sánh Heap và Metaspace

| Heap | Metaspace |
|------|-----------|
| Lưu Object | Lưu Metadata của Class |
| GC quản lý | Lưu Class Information |
| Có thể OutOfMemoryError | Có thể OutOfMemoryError |

---

# String Literal vs new String()

```java
String a = "Java";
String b = "Java";
```

→ Chỉ có **1 object** trong String Pool.

---

```java
String a = new String("Java");
```

→ Có **1 object** trong String Pool và **1 object** trên Heap.

---

# Các lỗi thường gặp

| Lỗi | Nguyên nhân |
|------|-------------|
| OutOfMemoryError | Hết Heap hoặc Metaspace |
| StackOverflowError | Đệ quy quá sâu hoặc Stack đầy |

---

# Câu hỏi phỏng vấn ⭐

### 1. JVM là gì?

> Máy ảo thực thi Bytecode Java, quản lý bộ nhớ, Class Loading, Thread và Garbage Collection.

---

### 2. JDK khác JRE như thế nào?

> JDK = JRE + Compiler + Development Tools.

---

### 3. Heap lưu gì?

> Object, Array và dữ liệu được tạo bằng `new`.

---

### 4. Stack lưu gì?

> Stack Frame, Local Variable, Primitive Variable và Reference Variable.

---

### 5. Heap có dùng chung giữa các Thread không?

> Có. Heap được chia sẻ giữa tất cả Thread.

---

### 6. GC hoạt động ở đâu?

> Chủ yếu trên Heap.

---

### 7. GC có chạy ngay khi Object mất Reference không?

> Không. JVM quyết định thời điểm chạy GC.

---

### 8. `System.gc()` có chắc chắn chạy GC không?

> Không. Chỉ là lời gợi ý cho JVM.

---

### 9. Khi nào xảy ra OutOfMemoryError?

> Khi JVM không thể cấp phát thêm bộ nhớ.

---

### 10. Khi nào xảy ra StackOverflowError?

> Khi Stack của Thread bị đầy do đệ quy quá sâu hoặc vô hạn.

---

### 11. Metaspace lưu gì?

> Metadata của Class như Method, Field, Annotation...

---

### 12. String Pool nằm ở đâu?

> Trong Heap, là vùng đặc biệt lưu String Literal và String sau khi `intern()`.

---

# Câu hỏi bẫy ⭐⭐⭐

### ❓ Object nằm ở đâu?

✅ Heap.

---

### ❓ Primitive nằm ở đâu?

✅ Local Variable → Stack.

✅ Field của Object → Heap.

---

### ❓ Reference nằm ở đâu?

✅ Reference nằm trên Stack (nếu là biến cục bộ), còn Object nằm trên Heap.

---

### ❓ GC có dọn Stack không?

✅ Không.

Stack tự được thu hồi khi Method hoặc Thread kết thúc.

---

### ❓ Mọi String đều nằm trong String Pool?

✅ Không.

Chỉ String Literal và String sau khi gọi `intern()`.

---

### ❓ OutOfMemoryError có phải Exception không?

✅ Không.

Là **Error**, không phải Exception.

---

### ❓ StackOverflowError có phải do Heap đầy không?

✅ Không.

Do Stack của Thread bị đầy.

---

### ❓ Object mất Reference có bị GC ngay không?

✅ Không.

Chỉ đủ điều kiện để GC thu hồi.

---

### ❓ Có nên dùng `finalize()` để giải phóng tài nguyên?

✅ Không.

`finalize()` đã deprecated.

Nên dùng:

- try-with-resources
- `close()`
