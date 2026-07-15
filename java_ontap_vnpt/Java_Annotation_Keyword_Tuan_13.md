# 14. Annotation ⭐⭐⭐

---

# 1. @Override

## Khái niệm

Đánh dấu một method đang **ghi đè (override)** method của class cha hoặc interface.

Compiler sẽ kiểm tra xem có đúng là override hay không.

## Ví dụ

```java
class Animal {
    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog");
    }
}
```

Nếu viết sai tên method:

```java
@Override
void sounds() {
}
```

Compiler báo lỗi ngay.

---

## Khi nào dùng?

Luôn dùng khi override method.

---

## Phỏng vấn

### @Override có bắt buộc không?

Không.

Nhưng nên dùng để compiler kiểm tra lỗi.

---

### Override constructor được không?

Không.

Constructor không được kế thừa.

---

# 2. @Deprecated

## Khái niệm

Đánh dấu API cũ, không nên dùng nữa.

Compiler sẽ cảnh báo.

---

## Ví dụ

```java
class Demo {

    @Deprecated
    void oldMethod() {
    }

    void newMethod() {
    }
}
```

Gọi:

```java
Demo d = new Demo();
d.oldMethod();
```

IDE sẽ hiện warning.

---

## Khi nào dùng?

Khi muốn thay thế API cũ bằng API mới nhưng vẫn giữ tương thích.

Ví dụ:

```java
@Deprecated
public void login() {
}

public void loginV2() {
}
```

---

## Phỏng vấn

### @Deprecated có xóa method không?

Không.

Chỉ cảnh báo.

---

### Vì sao không xóa luôn?

Để tránh làm hỏng các project cũ.

---

# 3. @SuppressWarnings

## Khái niệm

Tắt warning của compiler.

---

## Ví dụ

```java
@SuppressWarnings("unchecked")
List list = new ArrayList();
```

Không hiện warning Generic.

---

Ví dụ

```java
@SuppressWarnings("deprecation")
demo.oldMethod();
```

---

Có thể dùng nhiều warning

```java
@SuppressWarnings({"unchecked", "deprecation"})
```

---

## Khi nào dùng?

Chỉ dùng khi chắc chắn warning không gây vấn đề.

Không nên lạm dụng.

---

## Phỏng vấn

### Có nên dùng @SuppressWarnings khắp nơi?

Không.

Warning tồn tại để giúp phát hiện lỗi.

---

# Câu hỏi phỏng vấn

## @Override dùng để làm gì?

Đảm bảo method đang override đúng method cha.

---

## @Deprecated khác comment như thế nào?

Comment chỉ dành cho người đọc.

@Deprecated được compiler và IDE hiểu.

---

## @SuppressWarnings có làm code chạy nhanh hơn không?

Không.

Chỉ tắt cảnh báo.

---

# 15. Từ khóa quan trọng ⭐⭐⭐⭐⭐

---

# 1. static

## Ý nghĩa

Thuộc về class, không thuộc object.

Chỉ có 1 bản trong JVM.

---

Ví dụ

```java
class Student {

    static int count = 0;

    Student() {
        count++;
    }
}
```

---

### static method

```java
class MathUtil {

    static int add(int a, int b) {
        return a + b;
    }
}
```

Gọi:

```java
MathUtil.add(1, 2);
```

---

### Phỏng vấn

#### static có truy cập instance variable được không?

Không.

---

#### static có override được không?

Không.

Chỉ bị hiding.

---

# 2. final

## Ý nghĩa

Không thể thay đổi.

---

### final variable

```java
final int AGE = 18;
```

Không được gán lại.

---

### final method

```java
final void run() {
}
```

Không override được.

---

### final class

```java
final class A {
}
```

Không kế thừa được.

Ví dụ:

```java
String
```

là final.

---

## Phỏng vấn

### final có làm object immutable không?

Không.

```java
final List<String> list = new ArrayList<>();
list.add("A");
```

Được phép.

Không đổi reference, nhưng object vẫn thay đổi.

---

# 3. this

## Ý nghĩa

Tham chiếu đến object hiện tại.

---

Ví dụ

```java
class Student {

    String name;

    Student(String name) {
        this.name = name;
    }
}
```

---

Gọi constructor khác

```java
Student() {
    this("Unknown");
}
```

---

## Phỏng vấn

### this() gọi ở đâu?

Phải là dòng đầu tiên trong constructor.

---

# 4. super

## Ý nghĩa

Truy cập class cha.

---

Ví dụ

```java
class Animal {
    void sound() {
    }
}

class Dog extends Animal {

    void test() {
        super.sound();
    }
}
```

---

Gọi constructor cha

```java
Dog() {
    super();
}
```

---

## Phỏng vấn

### super() gọi khi nào?

Luôn được gọi đầu constructor.

Nếu không viết, Java tự thêm.

---

# 5. instanceof

## Ý nghĩa

Kiểm tra object có thuộc kiểu nào không.

---

Ví dụ

```java
Animal a = new Dog();

System.out.println(a instanceof Dog);
```

Output

```
true
```

---

Ví dụ Java 16+

```java
if (obj instanceof String s) {
    System.out.println(s.length());
}
```

---

## Phỏng vấn

### instanceof có kiểm tra interface không?

Có.

---

# 6. transient

## Ý nghĩa

Không serialize field.

---

Ví dụ

```java
class User implements Serializable {

    String username;

    transient String password;
}
```

Serialize xong:

password = null

---

## Phỏng vấn

### transient dùng khi nào?

Password

Token

OTP

Cache

---

# 7. volatile

## Ý nghĩa

Đảm bảo thread luôn đọc giá trị mới nhất từ RAM.

Không cache trong CPU.

---

Ví dụ

```java
volatile boolean running = true;
```

Thread khác sửa:

```java
running = false;
```

Thread hiện tại thấy ngay.

---

## Lưu ý

volatile **không đảm bảo atomic**.

Ví dụ

```java
volatile int count;
count++;
```

Vẫn lỗi race condition.

---

## Phỏng vấn

### volatile khác synchronized?

volatile

- Chỉ đảm bảo visibility

synchronized

- Visibility
- Atomicity
- Mutual exclusion

---

# 8. synchronized

## Ý nghĩa

Chỉ cho 1 thread vào critical section.

---

Ví dụ

```java
public synchronized void deposit() {
    balance += 100;
}
```

---

Block synchronized

```java
synchronized(lock) {
}
```

---

## Phỏng vấn

### synchronized khóa cái gì?

Instance method

→ khóa object hiện tại.

Static method

→ khóa Class.

---

# 9. native

## Ý nghĩa

Method được viết bằng ngôn ngữ khác (thường C/C++).

Java chỉ khai báo.

---

Ví dụ

```java
public native void hello();
```

JNI sẽ gọi code C/C++.

---

## Phỏng vấn

### native dùng khi nào?

- Gọi thư viện hệ điều hành
- Driver
- Hiệu năng rất cao
- JNI

---

# 10. strictfp

## Ý nghĩa

Đảm bảo phép tính số thực (float/double) cho kết quả giống nhau trên mọi nền tảng.

---

Ví dụ

```java
strictfp class Calculator {

    double calc() {
        return 10.0 / 3;
    }
}
```

---

## Lưu ý

Từ Java 17 trở đi, `strictfp` hầu như không còn cần thiết vì các JVM hiện đại đã thống nhất cách xử lý phép toán dấu chấm động.

---

# Tổng hợp nhanh

| Từ khóa | Ý nghĩa |
|----------|----------|
| static | Thuộc class |
| final | Không thay đổi |
| this | Object hiện tại |
| super | Class cha |
| instanceof | Kiểm tra kiểu |
| transient | Không serialize |
| volatile | Đảm bảo visibility giữa các thread |
| synchronized | Đồng bộ nhiều thread |
| native | Gọi code C/C++ |
| strictfp | Chuẩn hóa phép tính dấu chấm động |

---

# Câu hỏi phỏng vấn

## static có override được không?

Không.

Chỉ bị method hiding.

---

## final List có thêm phần tử được không?

Có.

Không đổi reference nhưng object vẫn thay đổi.

---

## volatile có thay thế synchronized được không?

Không.

volatile không đảm bảo atomicity.

---

## synchronized khóa object hay method?

Khóa object (instance) hoặc Class (static).

---

## transient có lưu xuống database không?

Có.

`transient` chỉ ảnh hưởng đến **Java Serialization**, không ảnh hưởng đến JPA/Hibernate. Muốn JPA bỏ qua một field thì dùng `@Transient`.

---

## instanceof có kiểm tra null không?

Có.

```java
String s = null;

System.out.println(s instanceof String);
```

Kết quả:

```
false
```
