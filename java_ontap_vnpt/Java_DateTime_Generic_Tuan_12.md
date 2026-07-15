# 12. Date Time ⭐⭐⭐

## LocalDate

-   Chỉ lưu ngày.
-   `LocalDate.now()`, `LocalDate.of(...)`
-   `plusDays()`, `minusMonths()`, `isAfter()`, `isBefore()`.

## LocalTime

-   Chỉ lưu giờ.
-   `LocalTime.now()`, `LocalTime.of(...)`
-   `plusHours()`, `minusMinutes()`.

## LocalDateTime

-   Kết hợp ngày và giờ.
-   `LocalDateTime.now()`, `LocalDateTime.of(...)`
-   `toLocalDate()`, `toLocalTime()`.

## Duration

-   Đo khoảng thời gian theo giờ/phút/giây.
-   Dùng với `LocalTime`, `LocalDateTime`, `Instant`.

## Period

-   Đo khoảng thời gian theo năm/tháng/ngày.
-   Dùng với `LocalDate`.

## DateTimeFormatter

-   Format/Parse ngày giờ.
-   Ví dụ: `dd/MM/yyyy`, `yyyy-MM-dd`, `HH:mm:ss`.

------------------------------------------------------------------------

# 13. Generic ⭐⭐⭐⭐

## Generic Class

``` java
class Box<T>{}
```

## Generic Method

``` java
public static <T> void print(T value){}
```

## Wildcard (?)

``` java
List<?>
```

Nhận mọi kiểu `List<T>`.

## extends

``` java
List<? extends Number>
```

-   Chỉ đọc (Producer).
-   Không thêm phần tử.

## super

``` java
List<? super Integer>
```

-   Thêm được `Integer`.
-   Đọc ra kiểu `Object`.

## PECS

-   Producer → `extends`
-   Consumer → `super`

## Type Erasure

-   Generic chỉ tồn tại khi compile.
-   Runtime: `List<String>` và `List<Integer>` đều là `List`.

## Diamond Operator

``` java
List<String> list = new ArrayList<>();
```
