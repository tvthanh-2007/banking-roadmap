# Java Core - Collections Framework ⭐⭐⭐⭐⭐

> Mục tiêu: Ôn phỏng vấn Junior/Mid Backend (VNPT, Ngân hàng, FPT, NashTech...)
>
> Chỉ giữ những kiến thức quan trọng và hay hỏi nhất.

---

# 1. Collection Framework

## Collection Framework là gì?

Là tập hợp các interface và class trong Java dùng để lưu trữ và thao tác dữ liệu.

Bao gồm:

- List
- Set
- Queue
- Map

---

## Câu hỏi

### Collection Framework là gì?

**Đáp án**

Là tập hợp các cấu trúc dữ liệu và thuật toán được Java cung cấp sẵn.

---

### Map có phải Collection không? ⭐⭐⭐

**Đáp án**

Không.

Map là interface riêng, không kế thừa Collection.

```
Iterable
    ↑
Collection
├── List
├── Set
└── Queue

Map (riêng)
```

---

# 2. List

## Đặc điểm

- Có thứ tự
- Có index
- Cho phép duplicate

Ví dụ

```java
A
B
A
```

---

## Câu hỏi

### List khác Set?

| List | Set |
|-------|------|
| Có index | Không có index |
| Có duplicate | Không duplicate |
| Có thứ tự | Không đảm bảo thứ tự |

---

### List khác ArrayList? ⭐⭐⭐⭐

Đây là câu hỏi rất hay.

**Đáp án**

List là **interface**.

ArrayList là **class implement List**.

```java
List<String> list = new ArrayList<>();
```

không phải

```java
new List<>();
```

vì interface không tạo object được.

---

### Vì sao nên khai báo bằng List?

```java
List<String> list = new ArrayList<>();
```

Thay vì

```java
ArrayList<String> list = new ArrayList<>();
```

Để dễ thay đổi implementation.

Ví dụ

```java
List<String> list = new LinkedList<>();
```

không cần sửa code phía dưới.

---

# 3. ArrayList ⭐⭐⭐⭐⭐

## Đặc điểm

- Dynamic Array
- get(index) nhanh
- add giữa chậm
- Không thread-safe

---

## Câu hỏi

### ArrayList được xây dựng trên gì?

**Đáp án**

Dynamic Array (mảng động).

---

### ArrayList khác Array?

| Array | ArrayList |
|--------|-----------|
| Kích thước cố định | Tự mở rộng |
| Primitive được | Chỉ Object (Wrapper) |
| Ít API | Nhiều API |

---

### Capacity khác Size?

Ví dụ

```java
ArrayList<Integer> list = new ArrayList<>(10);
```

```
capacity = 10
size = 0
```

Sau khi

```java
list.add(1);
list.add(2);
```

```
capacity = 10
size = 2
```

**Size** = số phần tử hiện có.

**Capacity** = sức chứa của mảng.

---

### ArrayList get() là O(1) vì sao? ⭐⭐⭐⭐⭐

Ví dụ

```java
list.get(5000);
```

Java tính trực tiếp vị trí trong mảng.

Không cần duyệt.

=> O(1)

---

### ArrayList search() là O(n) vì sao? ⭐⭐⭐⭐⭐

Ví dụ

```java
list.contains("Java");
```

Java không biết "Java" ở đâu.

Phải duyệt từng phần tử.

```
Java?

↓

index 0

↓

index 1

↓

index 2

...
```

=> O(n)

---

### Câu bẫy

**ArrayList get O(1) sao search O(n)?**

**Giải thích**

- get() truy cập theo index.
- search() tìm theo value.

Đây là hai thao tác hoàn toàn khác nhau.

---

### Tại sao add giữa ArrayList là O(n)?

Ví dụ

```
A B C D

↓

A X B C D
```

Java phải dịch toàn bộ phần tử phía sau.

=> O(n)

---

### ArrayList thread-safe không?

Không.

---

# 4. LinkedList ⭐⭐⭐⭐

## Đặc điểm

- Doubly Linked List
- get() O(n)
- add/remove đầu cuối O(1)

---

## Câu hỏi

### LinkedList khác ArrayList?

| ArrayList | LinkedList |
|------------|------------|
| Dynamic Array | Doubly Linked List |
| get O(1) | get O(n) |
| Thêm giữa O(n) | Thêm/xóa khi đã có node O(1) |
| Ít tốn RAM | Tốn RAM hơn |

---

### Khi nào dùng LinkedList?

Khi thêm/xóa nhiều.

Nếu đọc nhiều

→ ArrayList.

---

### Câu bẫy

LinkedList luôn nhanh hơn ArrayList?

**Đáp án**

Không.

get(index) rất chậm.

---

# 5. HashSet ⭐⭐⭐⭐⭐

## Đặc điểm

- Không duplicate
- Không đảm bảo thứ tự
- Dựa trên HashMap

---

## Câu hỏi

### HashSet kiểm tra duplicate như thế nào?

```
hashCode()

↓

bucket

↓

equals()
```

---

### Giải thích

Java

B1

Tính hashCode

↓

Tìm bucket

↓

Nếu bucket đã có object

↓

Gọi equals()

↓

Nếu equals == true

↓

Không thêm.

---

### Câu bẫy

Java gọi equals trước hay hashCode trước?

**Đáp án**

Luôn hashCode trước.

---

### Override equals mà không override hashCode có sao không?

Có.

HashSet sẽ hoạt động sai.

---

# 6. HashMap ⭐⭐⭐⭐⭐

## Đặc điểm

- Key - Value
- O(1) trung bình
- Không thread-safe
- Cho phép 1 null key

---

## Câu hỏi

### HashMap hoạt động như thế nào?

```
hashCode()

↓

bucket

↓

equals()

↓

value
```

---

### Collision là gì?

Hai key khác nhau

↓

cùng hashCode

↓

rơi vào cùng bucket.

---

### Java 8 xử lý Collision thế nào?

Ban đầu

LinkedList

↓

Nếu bucket quá dài

↓

Red Black Tree

↓

Tìm kiếm nhanh hơn.

---

### HashMap có cho null không?

Có.

- 1 null key
- nhiều null value

---

### HashMap thread-safe không?

Không.

---

### HashMap khác HashSet?

HashMap

```
key -> value
```

HashSet

```
chỉ value
```

HashSet thực chất dùng HashMap bên trong.

---

### HashMap khác Hashtable?

| HashMap | Hashtable |
|----------|-----------|
| Không thread-safe | Thread-safe |
| Cho null | Không cho null |

---

# 7. Comparable vs Comparator ⭐⭐⭐⭐

## Comparable

- compareTo()
- Sort mặc định

## Comparator

- compare()
- Sort tùy chỉnh

---

## Câu hỏi

### Comparable và Comparator khác nhau?

| Comparable | Comparator |
|------------|------------|
| compareTo() | compare() |
| 1 cách sort | Nhiều cách sort |
| Sort mặc định | Sort tùy chỉnh |

---

### Khi nào dùng Comparable?

Khi class chỉ có một cách sắp xếp tự nhiên.

Ví dụ

Student mặc định sort theo ID.

---

### Khi nào dùng Comparator?

Muốn sort theo

- GPA
- Name
- Age

Có nhiều cách sắp xếp.

---

### Câu bẫy

Một class implement nhiều Comparable được không?

Không.

Chỉ implement một Comparable.

Nếu cần nhiều kiểu sort

→ Comparator.

---

# 8. TreeMap ⭐⭐⭐

## Đặc điểm

- Red Black Tree
- O(log n)
- Tự sort theo key

---

## Câu hỏi

### TreeMap khác HashMap?

| HashMap | TreeMap |
|----------|----------|
| Không sort | Sort theo key |
| O(1) | O(log n) |

---

### TreeMap cho null key không?

Không.

---

# 9. Big O cần nhớ ⭐⭐⭐⭐⭐

| Collection | get | add cuối | search |
|------------|------|----------|---------|
| ArrayList | O(1) | O(1)* | O(n) |
| LinkedList | O(n) | O(1) | O(n) |
| HashSet | - | O(1) | O(1) |
| HashMap | - | O(1) | O(1) |
| TreeMap | - | O(log n) | O(log n) |

> *O(1) trung bình (amortized), có thể O(n) khi resize.

---

# 10. 15 câu hỏi phải thuộc

1. Collection Framework là gì?
2. Map có phải Collection không?
3. List khác ArrayList?
4. ArrayList hoạt động như thế nào?
5. Capacity khác Size?
6. ArrayList get() vì sao O(1)?
7. Search() vì sao O(n)?
8. ArrayList khác LinkedList?
9. Khi nào dùng LinkedList?
10. HashSet kiểm tra duplicate bằng gì?
11. HashMap hoạt động như thế nào?
12. Collision là gì?
13. Override equals có cần override hashCode không?
14. Comparable khác Comparator?
15. TreeMap khác HashMap?

---

# Ghi nhớ nhanh trước phỏng vấn

- **List** → Interface
- **ArrayList** → Dynamic Array
- **LinkedList** → Doubly Linked List
- **HashSet** → hashCode() → equals()
- **HashMap** → hashCode() → bucket → equals()
- **Collision** → Nhiều key cùng bucket
- **Java 8** → Bucket dài chuyển thành Red-Black Tree
- **Comparable** → compareTo() → Sort mặc định
- **Comparator** → compare() → Sort tùy chỉnh
- **TreeMap** → Tự sort theo key
- **ArrayList get()** → O(1)
- **ArrayList contains()** → O(n)
- **HashMap/HashSet** → O(1) trung bình
