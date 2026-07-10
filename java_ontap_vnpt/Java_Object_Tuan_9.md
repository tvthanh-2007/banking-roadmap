# 7. Object ⭐⭐⭐⭐

## Mục tiêu

Sau bài này cần hiểu:

- Object là class gì?
- toString()
- equals()
- hashCode()
- clone()
- Khi nào cần override equals/hashCode
- HashMap sử dụng equals và hashCode như thế nào

---

# 1. Object là gì?

`Object` là class cha của tất cả class trong Java.

```java
class Person {}
```

Thực tế Java sẽ hiểu là:

```java
class Person extends Object {}
```

Do đó mọi object đều có sẵn các method:

- toString()
- equals()
- hashCode()
- clone()
- getClass()
- wait()
- notify()
- notifyAll()

---

# 2. toString()

## Mục đích

Trả về chuỗi biểu diễn object.

Ví dụ:

```java
Person p = new Person();

System.out.println(p);
```

Thực chất Java gọi:

```java
System.out.println(p.toString());
```

Nếu không override:

```
Person@6d06d69c
```

---

## Override

```java
class Person {

    String name;

    @Override
    public String toString() {
        return name;
    }
}
```

```java
Person p = new Person();
p.name = "John";

System.out.println(p);
```

Kết quả

```
John
```

---

## Khi nào dùng?

- Logging
- Debug
- In thông tin object

---

# 3. equals()

## Mặc định

Nhiều người nghĩ equals() luôn so sánh value.

Đây là sai.

Object đã có equals():

```java
public boolean equals(Object obj) {
    return this == obj;
}
```

Nghĩa là mặc định equals() cũng chỉ so sánh reference.

Ví dụ:

```java
Object a = new Object();
Object b = new Object();

System.out.println(a == b);      // false
System.out.println(a.equals(b)); // false
```

---

## Override equals()

```java
class Person {

    String name;

    @Override
    public boolean equals(Object obj) {

        if (this == obj)
            return true;

        if (!(obj instanceof Person))
            return false;

        Person other = (Person) obj;

        return Objects.equals(name, other.name);
    }
}
```

Lúc này:

```java
Person a = new Person();
a.name = "John";

Person b = new Person();
b.name = "John";

System.out.println(a.equals(b));
```

Kết quả

```
true
```

---

# == và equals()

## Primitive

```java
int a = 10;
int b = 10;

a == b
```

So sánh giá trị.

---

## Object

```java
Person p1 = new Person();
Person p2 = new Person();
```

```
p1 == p2
```

↓

So sánh reference.

---

```
p1.equals(p2)
```

↓

- Nếu chưa override → so sánh reference.
- Nếu đã override → so sánh logic/value.

---

# String

String đã override equals()

```java
String a = new String("Java");
String b = new String("Java");

a == b
```

↓

false

```
a.equals(b)
```

↓

true

---

# Integer

Integer cũng override equals()

```java
Integer a = 100;
Integer b = 100;

a.equals(b)
```

↓

true

---

Wrapper class hầu hết đều override:

- String
- Integer
- Long
- Double
- Float
- Character
- Boolean

---

# Class tự tạo

```java
class Student {

    int id;

}
```

```java
Student s1 = new Student();
Student s2 = new Student();

System.out.println(s1.equals(s2));
```

↓

false

Vì Student chưa override equals().

---

# Contract của equals()

Nếu override phải đảm bảo:

### Reflexive

```
a.equals(a)
```

luôn true.

---

### Symmetric

```
a.equals(b)

↓

b.equals(a)
```

---

### Transitive

Nếu

```
a.equals(b)

b.equals(c)
```

thì

```
a.equals(c)
```

---

### Consistent

Object không đổi thì kết quả không đổi.

---

### Null

```
a.equals(null)
```

↓

false

---

# 4. hashCode()

hashCode() trả về số nguyên đại diện cho object.

Ví dụ

```java
Person p = new Person();

System.out.println(p.hashCode());
```

---

## Dùng để làm gì?

Các collection dạng hash:

- HashMap
- HashSet
- Hashtable
- ConcurrentHashMap

đều dùng hashCode().

---

## Contract

Nếu

```
a.equals(b)
```

thì

```
a.hashCode()==b.hashCode()
```

phải đúng.

---

Ngược lại

```
hashCode giống nhau
```

không có nghĩa

```
equals()
```

true.

Đây gọi là Collision.

---

## Override

```java
@Override
public int hashCode() {
    return Objects.hash(name);
}
```

---

# Vì sao phải override hashCode cùng equals?

Ví dụ:

```java
HashSet<Person> set = new HashSet<>();
```

Nếu chỉ override equals

```
equals()

↓

true
```

nhưng

```
hashCode

↓

khác nhau
```

HashSet vẫn lưu thành hai object khác nhau.

---

# HashMap hoạt động

```
Key

↓

hashCode()

↓

Bucket

↓

equals()

↓

Tìm đúng key
```

HashMap luôn:

1. hashCode()
2. equals()

---

# 5. clone()

clone() dùng để tạo bản sao object.

```java
Person p2 = (Person) p1.clone();
```

---

Muốn clone phải:

```java
implements Cloneable
```

và

```java
@Override
protected Object clone() throws CloneNotSupportedException {
    return super.clone();
}
```

---

Nếu không

```
CloneNotSupportedException
```

---

## Shallow Copy

```
Person

↓

Address
```

Clone xong

```
Person

↓

Address (dùng chung)
```

Sửa Address

↓

Cả hai object cùng thay đổi.

---

## Deep Copy

Clone luôn object bên trong.

```
Person

↓

Address mới
```

Hai object hoàn toàn độc lập.

---

## Thực tế

clone() hiện nay khá ít dùng.

Thường thay bằng:

- Copy Constructor
- Builder
- Factory Method

---

# Câu hỏi phỏng vấn

## 1. Object là gì?

Đáp:

Class cha của mọi class trong Java.

---

## 2. == khác equals() thế nào?

Đáp:

Primitive

- == so sánh giá trị.

Object

- == so sánh reference.
- equals() so sánh logic nếu đã override.
- Nếu chưa override thì equals() cũng so sánh reference.

---

## 3. equals() mặc định so sánh gì?

Đáp:

Reference.

Vì Object.equals()

```java
return this == obj;
```

---

## 4. equals() có luôn so sánh value không?

Không.

Chỉ các class đã override mới so sánh value.

Ví dụ:

- String
- Integer
- Long
- Double
- hoặc class tự viết.

---

## 5. Vì sao override equals() phải override hashCode()?

Đáp:

Để đảm bảo contract của Java.

Nếu không HashMap, HashSet hoạt động sai.

---

## 6. HashMap dùng hashCode hay equals trước?

Đáp:

```
hashCode

↓

equals
```

---

## 7. hashCode giống nhau thì equals có luôn true?

Không.

Có thể xảy ra Collision.

---

## 8. equals true nhưng hashCode khác được không?

Không.

Vi phạm contract.

---

## 9. clone() là deep copy?

Không.

Mặc định là Shallow Copy.

---

## 10. clone() có gọi constructor không?

Không.

---

# Câu hỏi bẫy

### Câu 1

```java
Object a = new Object();
Object b = new Object();

a.equals(b)
```

Đáp:

```
false
```

Vì Object.equals() dùng ==.

---

### Câu 2

```java
String a = new String("Java");
String b = new String("Java");

a.equals(b)
```

Đáp:

```
true
```

Vì String override equals().

---

### Câu 3

```java
Integer a = 100;
Integer b = 100;

a == b
```

Đáp:

```
true
```

Do Integer Cache (-128 đến 127).

---

### Câu 4

```java
Integer a = 200;
Integer b = 200;

a == b
```

Đáp:

```
false
```

Vì ngoài vùng cache.

---

### Câu 5

```java
Integer a = 200;
Integer b = 200;

a.equals(b)
```

Đáp:

```
true
```

Vì Integer đã override equals().

---

# Ghi nhớ nhanh

| Method | Mục đích | Mặc định | Thường override |
|----------|----------|----------|----------------|
| toString() | Biểu diễn object | ClassName@hashcode | ✅ |
| equals() | So sánh object | Reference | ✅ |
| hashCode() | HashMap/HashSet | Hash theo object | ✅ (khi override equals) |
| clone() | Tạo bản sao | Shallow Copy | ❌ Ít dùng |

---

# Mẹo phỏng vấn

- `Object.equals()` **không** so sánh value, mà chỉ gọi `==`.
- `==` với object luôn so sánh **reference**.
- `equals()` chỉ so sánh **value** khi class đã override.
- Override `equals()` thì luôn override `hashCode()`.
- `HashMap` tìm key bằng **hashCode() trước, equals() sau**.
- `clone()` mặc định là **shallow copy** và hiện nay ít được dùng trong các dự án thực tế.
