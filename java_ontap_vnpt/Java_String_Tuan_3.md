# 03. String ⭐⭐⭐⭐⭐

> Đây là nhóm kiến thức Java Core xuất hiện rất nhiều trong phỏng vấn Fresher, Junior, Middle Backend.

---

# 1. String là gì?

## Kiến thức

`String` là một class trong `java.lang`.

Đặc điểm:

- Immutable (không thay đổi được)
- Override `equals()`
- Có String Pool
- Có thể dùng làm key của HashMap
- Thread-safe vì immutable

---

## Câu hỏi

### String là class hay primitive?

**Đáp án**

Là class.

---

### Vì sao String immutable?

- Bảo mật (URL, Password, Token...)
- Thread-safe
- Dùng được String Pool
- HashCode không thay đổi → phù hợp làm key của HashMap

---

### Immutable nghĩa là gì?

```java
String s = "Hello";

s += " World";
```

Không sửa object cũ.

Tạo object String mới.

---

## Câu bẫy

### Immutable có nghĩa biến không đổi?

Sai.

```java
String s = "Hello";

s = "Hi";
```

Reference đổi sang object khác.

Object `"Hello"` vẫn tồn tại.

---

# 2. String Pool ⭐⭐⭐⭐⭐

## Kiến thức

String literal được lưu trong **String Constant Pool**.

```java
String a = "Hello";
String b = "Hello";
```

Chỉ có một object.

```
Heap
└── String Pool
      "Hello"

a ─┐
   ├──> "Hello"
b ─┘
```

---

Nếu dùng

```java
String c = new String("Hello");
```

Nếu Pool chưa có `"Hello"`:

- 1 object trong Pool
- 1 object mới trong Heap

Nếu Pool đã có:

- chỉ tạo thêm object trong Heap

```
Heap
 ├── String Object ("Hello")  <-- c
 └── String Pool
       └── "Hello"
```

---

## Câu hỏi

### String Pool dùng để làm gì?

Tiết kiệm bộ nhớ bằng cách tái sử dụng String giống nhau.

---

### Khi nào String vào Pool?

Có:

```java
String s = "Hello";
```

Không:

```java
String s = new String("Hello");
```

Object do `new` tạo nằm ngoài Pool.

---

### String Pool nằm ở đâu?

Java 7+

→ Trong Heap.

(Java 6 nằm ở PermGen.)

---

## Câu bẫy

### Có phải String luôn ở Pool?

Không.

```java
new String("Hello");
```

Object này nằm ngoài Pool.

---

### Bao nhiêu object?

```java
String s = new String("Hello");
```

Nếu Pool chưa có:

- Pool
- Heap

→ 2 object

Nếu Pool đã có:

→ chỉ thêm 1 object trên Heap.

---

# 3. intern() ⭐⭐⭐⭐

## Kiến thức

`intern()` trả về object String trong Pool.

```java
String s1 = new String("Hello");

String s2 = s1.intern();
```

`s2` trỏ tới object trong Pool.

---

## Câu hỏi

### intern() dùng để làm gì?

Đưa hoặc lấy String trong Pool để tái sử dụng.

---

### Ví dụ

```java
String a = new String("Hello");

String b = a.intern();

System.out.println(a == b);
```

```
false
```

---

```java
String c = "Hello";

System.out.println(c == b);
```

```
true
```

---

## Câu bẫy

### intern() có tạo object mới không?

Không phải lúc nào cũng tạo.

- Pool đã có → trả về object cũ.
- Chưa có → thêm vào Pool rồi trả về.

---

# 4. == vs equals() ⭐⭐⭐⭐⭐

## Kiến thức

`==`

→ So sánh reference.

`equals()`

→ So sánh nội dung.

---

```java
String a = "Hello";
String b = "Hello";

System.out.println(a == b);
```

```
true
```

---

```java
String a = new String("Hello");
String b = new String("Hello");

System.out.println(a == b);
System.out.println(a.equals(b));
```

```
false
true
```

---

## Câu hỏi

### Khi nào dùng == ?

So sánh reference.

---

### Khi nào dùng equals()?

So sánh giá trị.

---

### Vì sao String override equals()?

Để so sánh nội dung thay vì địa chỉ.

---

## Câu bẫy

```java
String a = "Hello";
String b = new String("Hello");

System.out.println(a == b);
```

```
false
```

---

```java
System.out.println(a.equals(b));
```

```
true
```

---

# 5. equalsIgnoreCase()

## Kiến thức

So sánh không phân biệt chữ hoa chữ thường.

```java
"abc".equalsIgnoreCase("ABC")
```

```
true
```

---

## Câu hỏi

Khác gì equals()?

| equals() | equalsIgnoreCase() |
|----------|--------------------|
| Có phân biệt hoa thường | Không phân biệt |

---

## Câu bẫy

```java
"JAVA".equals("java")
```

```
false
```

---

```java
"JAVA".equalsIgnoreCase("java")
```

```
true
```

---

# 6. substring()

## Kiến thức

Lấy chuỗi con.

```java
String s = "abcdef";

s.substring(2);
```

```
cdef
```

---

```java
s.substring(2,5);
```

```
cde
```

Index cuối **không được lấy**.

---

## Câu hỏi

Index cuối có lấy không?

Không.

---

## Câu bẫy

```java
substring(0, length())
```

Lấy toàn bộ chuỗi.

Không lỗi.

---

# 7. replace()

## Kiến thức

Không sửa String cũ.

```java
String s = "abc";

String x = s.replace("a","z");
```

```
s = abc

x = zbc
```

---

## Câu hỏi

replace() có sửa object không?

Không.

Tạo object mới.

---

## Câu bẫy

```java
String s = "abc";

s.replace("a","z");

System.out.println(s);
```

```
abc
```

---

# 8. split()

## Kiến thức

`split()` nhận **Regex**.

```java
"a,b,c".split(",");
```

```
a
b
c
```

---

## Câu hỏi

split() nhận gì?

Regex.

---

## Câu bẫy

```java
"a.b".split(".");
```

Sai.

`.` trong Regex nghĩa là **mọi ký tự**.

Muốn tách theo dấu chấm:

```java
split("\\.");
```

---

# 9. trim()

## Kiến thức

Xóa khoảng trắng đầu và cuối.

```java
"  abc  ".trim()
```

```
abc
```

Không xóa khoảng trắng giữa chuỗi.

---

## Câu hỏi

trim() có xóa khoảng trắng giữa không?

Không.

---

## Câu bẫy

```java
" a b ".trim()
```

```
a b
```

---

# 10. StringBuilder ⭐⭐⭐⭐⭐

## Kiến thức

- Mutable
- Không thread-safe
- Nhanh hơn String khi nối chuỗi nhiều lần

```java
StringBuilder sb = new StringBuilder();

sb.append("Hello")
  .append(" Java");
```

---

## Câu hỏi

### Vì sao StringBuilder nhanh hơn String?

Vì sửa trực tiếp trên buffer.

Không tạo object mới sau mỗi lần nối.

---

### append() trả về gì?

Trả về chính `StringBuilder`.

Có thể chain.

```java
sb.append("A")
  .append("B")
  .append("C");
```

---

## Câu bẫy

StringBuilder immutable?

Không.

---

# 11. StringBuffer ⭐⭐⭐⭐

## Kiến thức

- Mutable
- Thread-safe
- Các method được synchronized

---

## Câu hỏi

Khác gì StringBuilder?

| StringBuilder | StringBuffer |
|---------------|--------------|
| Không synchronized | synchronized |
| Nhanh hơn | Chậm hơn |
| Không thread-safe | Thread-safe |

---

## Câu bẫy

Có phải luôn dùng StringBuffer?

Không.

Một thread → dùng StringBuilder.

---

# 12. Toán tử + ⭐⭐⭐⭐⭐

## Compile-time

Nếu toàn literal:

```java
String a = "Hello";
String b = "He" + "llo";

System.out.println(a == b);
```

```
true
```

Compiler tối ưu thành:

```java
String b = "Hello";
```

---

## Runtime

```java
String a = "Hello";
String b = "He";

String c = b + "llo";
```

Compiler sinh code tương đương:

```java
String c = new StringBuilder()
        .append(b)
        .append("llo")
        .toString();
```

`toString()` tạo object String mới trên Heap.

Không tự động gọi `intern()`.

```
a == c

false
```

```
a.equals(c)

true
```

---

## Vì sao Java không tự động dùng String Pool?

Nếu mỗi lần tạo String đều phải:

- tính hash
- tra String Pool
- synchronized

→ rất tốn hiệu năng.

Java chỉ dùng Pool khi:

- String literal
- gọi `intern()`

---

## Dùng intern()

```java
String a = "Hello";
String b = "He";

String c = (b + "llo").intern();

System.out.println(a == c);
```

```
true
```

---

## Câu bẫy

```java
final String b = "He";

String c = b + "llo";

System.out.println(c == "Hello");
```

```
true
```

Vì `b` là hằng số compile-time (`final` + literal).

Compiler tối ưu thành:

```java
String c = "Hello";
```

---

# 13. Các câu hỏi phỏng vấn hay gặp ⭐⭐⭐⭐⭐

### 1

```java
String a = "Hello";
String b = "Hello";

a == b ?
```

✅ true

---

### 2

```java
String a = new String("Hello");
String b = new String("Hello");

a == b ?
```

✅ false

---

### 3

```java
String a = new String("Hello");
String b = "Hello";

a.equals(b)?
```

✅ true

---

### 4

```java
String a = new String("Hello");
String b = a.intern();

a == b ?
```

✅ false

---

### 5

```java
String a = "Hello";
String b = "He" + "llo";

a == b ?
```

✅ true

---

### 6

```java
String a = "Hello";
String b = "He";
String c = b + "llo";

a == c ?
```

✅ false

---

### 7

```java
String a = "Hello";
String b = "He";
String c = (b + "llo").intern();

a == c ?
```

✅ true

---

### 8

```java
String s = "abc";

s.concat("def");

System.out.println(s);
```

✅ abc

---

### 9

```java
StringBuilder sb = new StringBuilder("abc");

sb.append("def");

System.out.println(sb);
```

✅ abcdef

---

### 10

```java
System.out.println("JAVA".equalsIgnoreCase("java"));
```

✅ true

---

### 11

```java
System.out.println("a.b".split("\\.").length);
```

✅ 2

---

### 12

```java
final String s = "Ja";

String x = s + "va";

System.out.println(x == "Java");
```

✅ true

---

## Tóm tắt cần nhớ

- String là immutable.
- Literal được lưu trong String Pool.
- `new String()` luôn tạo object mới trên Heap.
- `==` so sánh reference, `equals()` so sánh nội dung.
- `intern()` trả về object trong Pool.
- `+` với literal → compiler tối ưu.
- `+` với biến → runtime nối chuỗi, tạo String mới.
- `StringBuilder` mutable, nhanh, không thread-safe.
- `StringBuffer` mutable, thread-safe nhưng chậm hơn.
