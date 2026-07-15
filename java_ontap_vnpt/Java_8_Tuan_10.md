# Java 8 ⭐⭐⭐⭐

## 1. Lambda Expression

### Khái niệm

Lambda là cách viết ngắn gọn của Anonymous Class để implement Functional
Interface.

``` java
Runnable r = () -> System.out.println("Hello");
```

**Cú pháp**

``` java
(parameters) -> expression
(parameters) -> { statements; }
```

**Lưu ý** - Chỉ dùng với Functional Interface. - Biến local sử dụng
trong lambda phải *effectively final*. - Lambda được JVM triển khai bằng
`invokedynamic`, không tạo anonymous class theo cách truyền thống.

### Câu hỏi gài

-   Lambda có phải Object? → Không, nó là implementation của Functional
    Interface.
-   Lambda có tạo class mới? → Không theo cách anonymous class.
-   Có sửa biến local bên ngoài trong lambda được không? → Không.

------------------------------------------------------------------------

## 2. Functional Interface

Interface chỉ có **1 abstract method**.

``` java
@FunctionalInterface
interface Calculator{
    int add(int a,int b);
}
```

Có thể có nhiều `default` và `static` method.

``` java
interface A{
    void test();              // abstract
    default void hello(){}    // không phải abstract
    static void hi(){}        // không phải abstract
}
```

### Lưu ý

-   `@FunctionalInterface` không bắt buộc nhưng nên dùng.
-   Các method của `Object` (`equals`, `hashCode`, `toString`) **không
    được tính** là abstract method.

### Câu hỏi gài

-   Có 2 abstract method? → Không còn là Functional Interface.
-   Có nhiều default/static method? → Vẫn hợp lệ.
-   `equals(Object)` có được tính không? → Không.

------------------------------------------------------------------------

## 3. Method Reference

Viết tắt của Lambda.

``` java
list.forEach(System.out::println);
```

### 4 loại

``` java
Math::max          // static
obj::method        // instance
System.out::println
Student::new       // constructor
```

### Câu hỏi gài

-   Có nhanh hơn Lambda không? → Không đáng kể, chủ yếu giúp code dễ
    đọc.

------------------------------------------------------------------------

## 4. Stream API

Dùng để xử lý Collection theo kiểu Functional.

``` java
list.stream()
    .filter(x -> x > 5)
    .map(x -> x * 2)
    .toList();
```

### Intermediate

-   filter
-   map
-   flatMap
-   sorted
-   distinct
-   peek

### Terminal

-   collect
-   toList
-   count
-   reduce
-   forEach
-   findFirst
-   anyMatch

### Lazy Evaluation

Stream chỉ chạy khi gặp Terminal Operation.

### Lưu ý

-   Stream chỉ dùng **1 lần**.
-   Không làm thay đổi Collection gốc.

### map vs flatMap

-   map: 1 → 1
-   flatMap: 1 → nhiều rồi làm phẳng.

### Câu hỏi gài

-   Stream có lưu dữ liệu? → Không.
-   parallelStream lúc nào cũng nhanh? → Không.
-   peek dùng để làm gì? → Debug, không nên sửa dữ liệu.

------------------------------------------------------------------------

## 5. Optional

Giúp hạn chế NullPointerException.

``` java
Optional.of(value);
Optional.ofNullable(value);
Optional.empty();
```

### Thường dùng

-   get()
-   ifPresent()
-   orElse()
-   orElseGet()
-   orElseThrow()
-   map()
-   flatMap()
-   filter()

### orElse vs orElseGet

``` java
optional.orElse(create());
```

`create()` luôn chạy.

``` java
optional.orElseGet(() -> create());
```

Chỉ chạy khi Optional rỗng.

### Câu hỏi gài

-   Optional.of(null)? → NPE.
-   Có nên dùng Optional trong Entity? → Không.

------------------------------------------------------------------------

## 6. Predicate

Đại diện cho:

    T -> boolean

``` java
Predicate<Integer> p = x -> x > 10;
p.test(15);
```

Method: - test() - and() - or() - negate()

------------------------------------------------------------------------

## 7. Consumer

Đại diện cho:

    T -> void

``` java
Consumer<String> c = System.out::println;
c.accept("Hello");
```

Method: - accept() - andThen()

Consumer **không trả về giá trị**.

------------------------------------------------------------------------

## 8. Supplier

Đại diện cho:

    () -> T

``` java
Supplier<String> s = () -> "Hello";
s.get();
```

Không nhận tham số, chỉ sinh dữ liệu.

------------------------------------------------------------------------

## 9. Function

Đại diện:

    T -> R

``` java
Function<String,Integer> f = String::length;
f.apply("Hello");
```

Method: - apply() - compose() - andThen() - identity()

### compose vs andThen

``` text
compose:  B -> A
andThen:  A -> B
```

------------------------------------------------------------------------

## Bảng so sánh

  Interface   Input   Output    Method
  ----------- ------- --------- ----------
  Predicate   T       boolean   test()
  Consumer    T       void      accept()
  Supplier    \-      T         get()
  Function    T       R         apply()

------------------------------------------------------------------------

# 20 câu phỏng vấn hay gặp

1.  Lambda khác Anonymous Class?
2.  Functional Interface là gì?
3.  Vì sao dùng @FunctionalInterface?
4.  Default method có phải abstract không?
5.  Method của Object có được tính không?
6.  Method Reference là gì?
7.  Stream khác Collection?
8.  Intermediate và Terminal Operation?
9.  Stream dùng lại được không?
10. Lazy Evaluation là gì?
11. map và flatMap khác nhau?
12. reduce dùng để làm gì?
13. peek có nên sửa dữ liệu?
14. parallelStream khi nào dùng?
15. Optional.of và ofNullable?
16. orElse và orElseGet?
17. Có nên dùng Optional trong Entity?
18. Predicate dùng khi nào?
19. Consumer khác Function?
20. Supplier khác Consumer?
