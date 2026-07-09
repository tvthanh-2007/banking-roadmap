# Java Core - Tuần 1: Java Basics

> Mục tiêu: Nắm chắc các kiến thức Java Core thường xuất hiện trong bài
> test trắc nghiệm.

---

# 1. Primitive vs Wrapper

## Kiến thức

Primitive: `byte, short, int, long, float, double, char, boolean`

Wrapper: `Byte, Short, Integer, Long, Float, Double, Character, Boolean`

Primitive Wrapper

---

Không phải object Là object
Không null Có thể null
Nhanh hơn Chậm hơn một chút
Không dùng Generic Dùng được với Generic/Collection

Ví dụ:

```java
int a = 10;
Integer b = 10;
```

### Lưu ý

- `ArrayList<int>` ❌
- `ArrayList<Integer>` ✅

Vì Generic chỉ nhận **Reference Type**.

## Câu hỏi hay gặp

- Primitive khác Wrapper?
- Khi nào dùng Wrapper?
- Tại sao Collection không dùng primitive?

## Câu bẫy

```java
Integer a=100,b=100;
System.out.println(a==b);
```

✅ `true` (Integer Cache từ -128 đến 127)

```java
Integer a=200,b=200;
System.out.println(a==b);
```

✅ `false`

```java
Integer a=null;
int b=a;
```

✅ `NullPointerException` (Unboxing)

---

# 2. Local / Instance / Static Variable

## Kiến thức

**Local** - Trong method - Phải khởi tạo - Không có giá trị mặc định

**Instance** - Thuộc object - Có giá trị mặc định - Mỗi object một bản

**Static** - Thuộc class - Dùng chung - Khởi tạo một lần khi class được
load

## Câu hỏi

- Local variable có default value không? → Không.
- Static thuộc object hay class? → Class.

## Câu bẫy

```java
int x;
System.out.println(x);
```

✅ Compile Error

---

# 3. final

## Kiến thức

- `final` variable: không gán lại.
- `final` method: không override.
- `final` class: không kế thừa.

`final` **không làm object immutable**, chỉ làm reference không đổi.

## Câu bẫy

```java
final List<Integer> list=new ArrayList<>();
list.add(1);
```

✅ Hợp lệ.

```java
list=new ArrayList<>();
```

❌ Compile Error

---

# 4. Casting

## Kiến thức

Implicit (Widening)

```java
int a=10;
double b=a;
```

Explicit (Narrowing)

```java
double a=10.5;
int b=(int)a;
```

Kết quả: `10`

## Câu bẫy

```java
byte a=1,b=2;
byte c=a+b;
```

❌ Compile Error

Vì `byte + byte` được nâng lên `int`.

---

# 5. Toán tử

## ++i vs i++

- `++i`: tăng trước rồi dùng.
- `i++`: dùng trước rồi tăng.

Ví dụ

```java
int i=5;
System.out.println(i++);
```

Kết quả: `5`

```java
System.out.println(++i);
```

Kết quả: `6`

## == và equals()

- Primitive: `==` so sánh giá trị.
- Object: `==` so sánh địa chỉ.
- `equals()` so sánh nội dung (nếu được override).

## Câu bẫy

```java
int i=5;
System.out.println(i++ + ++i);
```

Đáp án: `12`

---

# 6. Autoboxing / Unboxing

## Kiến thức

Autoboxing

```java
Integer a=10;
```

=\> Java tự gọi `Integer.valueOf(10)`.

Unboxing

```java
Integer a=10;
int b=a;
```

=\> Java tự gọi `a.intValue()`.

## Câu hỏi

- Autoboxing là gì?
- Unboxing là gì?

## Câu bẫy

```java
Integer a=null;
int b=a;
```

✅ `NullPointerException`

---

# Checklist trước khi thi

- Primitive vs Wrapper
- Integer Cache
- `==` vs `equals()`
- Local/Instance/Static
- `final`
- `++i` vs `i++`
- Casting
- `byte + byte`
- Autoboxing/Unboxing
- Compile Error vs Runtime Error
