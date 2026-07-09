# Java OOP - Ghi chú phỏng vấn

## Nội dung
- Encapsulation
- Inheritance
- Polymorphism
- Abstraction
- Constructor
- this, super
- Overloading
- Overriding
- Interface
- Abstract Class
- Access Modifier

---

## 1. Encapsulation
**Khái niệm:** Ẩn dữ liệu, chỉ truy cập thông qua method.

```java
private String name;
public String getName(){ return name; }
public void setName(String name){ this.name = name; }
```

**Lợi ích**
- Bảo vệ dữ liệu
- Kiểm soát dữ liệu đầu vào
- Giảm phụ thuộc

**Phỏng vấn**
- Getter/Setter có phải Encapsulation? → Không.
- Vì sao field nên `private`? → Bảo vệ dữ liệu.

---

## 2. Inheritance
Cho phép class con kế thừa class cha.

```java
class Dog extends Animal {}
```

**Phỏng vấn**
- Java có Multiple Inheritance? → Không với class, Có với interface.
- `Object` có phải cha của mọi class? → Có.

---

## 3. Polymorphism
Một object có nhiều hình thái.

- Compile-time → Overloading
- Runtime → Overriding

```java
Animal a = new Dog();
a.sound();
```

→ Gọi `Dog.sound()`.

---

## 4. Abstraction
Ẩn chi tiết cài đặt, chỉ lộ chức năng cần thiết.

Thực hiện bằng:
- Interface
- Abstract Class

---

## 5. Constructor

- Dùng để khởi tạo object.
- Không có kiểu trả về.
- Overload được.
- Override được? → Không.
- Có thể `private` (Singleton).

---

## 6. this

Đại diện object hiện tại.

```java
this.name = name;
this(...);
```

**Bẫy**
- `this()` phải là dòng đầu tiên của constructor.

---

## 7. super

Đại diện class cha.

```java
super();
super.method();
```

**Bẫy**
- `super()` phải là dòng đầu tiên.
- Không viết thì compiler tự thêm (nếu có constructor không tham số).

---

## 8. Overloading

Điều kiện:
- Khác số lượng tham số
- Khác kiểu tham số
- Khác thứ tự tham số

Không được khác mỗi kiểu trả về.

→ Compile Time.

---

## 9. Overriding

Điều kiện:
- Cùng tên
- Cùng tham số
- Return type tương thích
- Access modifier không hẹp hơn

Không override được:
- private
- static (method hiding)
- final
- constructor

→ Runtime.

---

## 10. Interface

Dùng để định nghĩa hành vi.

- Không có constructor.
- Field mặc định: `public static final`.
- Java 8: `default`, `static`.
- Java 9: `private`.
- Một class implement nhiều interface.

---

## 11. Abstract Class

- Không tạo object được.
- Có constructor.
- Có field.
- Có method thường.
- Có abstract method.
- Không bắt buộc phải có abstract method.

---

## 12. Interface vs Abstract Class

| Interface | Abstract Class |
|-----------|----------------|
| implements | extends |
| Không có constructor | Có constructor |
| Đa kế thừa | Chỉ kế thừa 1 class |
| Định nghĩa hành vi | Chia sẻ code chung |

---

## 13. Access Modifier

| Modifier | Class | Package | Subclass | Other |
|----------|:-----:|:-------:|:--------:|:-----:|
| private | ✓ | ✗ | ✗ | ✗ |
| default | ✓ | ✓ | ✗ | ✗ |
| protected | ✓ | ✓ | ✓ | ✗ |
| public | ✓ | ✓ | ✓ | ✓ |

Thứ tự phạm vi:

`private < default < protected < public`

---

# Bộ câu hỏi bẫy

1. OOP có mấy tính chất? → 4.
2. Getter/Setter có phải Encapsulation? → Không.
3. Java có đa kế thừa? → Không với class, Có với interface.
4. Constructor override được? → Không.
5. Constructor overload được? → Có.
6. Constructor có return type? → Không.
7. Interface có constructor? → Không.
8. Abstract class có constructor? → Có.
9. Interface có field? → Có, luôn là `public static final`.
10. Interface có method có body? → Java 8 (`default`, `static`), Java 9 (`private`).
11. Abstract class bắt buộc có abstract method? → Không.
12. `this()` và `super()` phải ở đâu? → Dòng đầu tiên constructor.
13. Overloading khác mỗi return type? → Không.
14. Override được `private`? → Không.
15. Override được `static`? → Không (method hiding).
16. Override được `final`? → Không.
17. Có `new Interface()`? → Không.
18. Có `new AbstractClass()`? → Không.
19. `Object` là cha của mọi class? → Đúng.
20. `instanceof` dùng làm gì? → Kiểm tra kiểu thực tế của object.
