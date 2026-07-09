# 5. HashMap ⭐⭐⭐⭐⭐

---

# 1. HashMap là gì?

HashMap lưu dữ liệu theo cặp **Key → Value**.

Đặc điểm:

- Không giữ thứ tự.
- Key không được trùng.
- Value được phép trùng.
- Cho phép **1 key = null**.
- Cho phép nhiều value = null.
- Không thread-safe.
- Triển khai bằng **bảng băm (Hash Table)**.

Ví dụ:

```java
Map<Integer, String> map = new HashMap<>();

map.put(1, "Java");
map.put(2, "Spring");
map.put(3, "Docker");
```

---

# 2. HashMap hoạt động như thế nào?

Ví dụ:

```java
map.put("Java", 100);
```

Quy trình:

```
Key
 ↓
hashCode()
 ↓
hash
 ↓
index = hash & (capacity - 1)
 ↓
Bucket[index]
 ↓
equals()
 ↓
Lưu hoặc cập nhật value
```

Khi gọi `get(key)`:

```
key
 ↓
hashCode()
 ↓
bucket
 ↓
equals()
 ↓
value
```

---

# 3. Bucket là gì?

Bucket là từng ô trong mảng nội bộ của HashMap.

Ví dụ:

```
bucket[]

0
1
2
3
4
5 -> Java
6
7
...
15
```

HashMap mặc định có:

```
capacity = 16
```

---

# 4. hashCode() dùng để làm gì?

HashMap gọi:

```java
key.hashCode();
```

để tính vị trí lưu dữ liệu.

Ví dụ:

```java
String s = "Java";

System.out.println(s.hashCode());
```

Giả sử:

```
2301506
```

HashMap tính:

```
index = hash & (capacity - 1)
```

(Java 8 không dùng phép `%`, mà dùng phép AND bit để nhanh hơn.)

Ví dụ:

```
capacity = 16

index = hash & 15
```

---

# 5. equals() dùng để làm gì?

Sau khi xác định bucket, HashMap phải kiểm tra đúng key.

Ví dụ:

```java
map.put("Java", 1);

map.get("Java");
```

Quy trình:

```
hashCode()

↓

bucket

↓

equals()

↓

value
```

Nếu không có `equals()` thì không thể phân biệt các key nằm cùng bucket.

---

# 6. hashCode() và equals()

Nếu override `equals()` thì bắt buộc override luôn `hashCode()`.

Sai:

```java
class User {

    int id;

    @Override
    public boolean equals(Object o) {
        return true;
    }
}
```

Đúng:

```java
class User {

    int id;

    @Override
    public boolean equals(Object o) {
        ...
    }

    @Override
    public int hashCode() {
        ...
    }
}
```

Nếu không, HashMap sẽ hoạt động sai.

---

# 7. Collision là gì?

Collision là hiện tượng nhiều key rơi vào cùng một bucket.

Ví dụ:

```
Bucket 5

Java

Spring

Docker
```

Có thể xảy ra vì:

- Hai object có cùng hashCode().
- Hai hash khác nhau nhưng sau khi tính index lại trùng bucket.

---

# 8. Collision được xử lý như thế nào?

## Java 7

```
Bucket

↓

LinkedList
```

```
Java
 ↓
Spring
 ↓
Docker
```

## Java 8

Nếu ít node:

```
LinkedList
```

Nếu nhiều node:

```
Red Black Tree
```

để tăng tốc độ tìm kiếm.

---

# 9. Treeify (Java 8)

Nếu:

- Bucket có **>= 8 node**
- Và table có **>= 64 bucket**

thì:

```
LinkedList

↓

Red Black Tree
```

Giúp:

```
O(n)

↓

O(log n)
```

---

# 10. Untreeify

Nếu số node giảm xuống:

```
< 6
```

thì:

```
Red Black Tree

↓

LinkedList
```

---

# 11. Load Factor

Load Factor là ngưỡng để HashMap resize.

Mặc định:

```
0.75
```

Ví dụ:

```
capacity = 16

16 × 0.75 = 12
```

Khi thêm phần tử thứ **13**, HashMap sẽ resize.

---

# 12. Resize

Resize là tăng số bucket.

Ví dụ:

```
16

↓

32

↓

64

↓

128
```

Sau đó toàn bộ key sẽ được:

```
Rehash
```

---

# 13. Rehash

Sau khi resize:

```
Bucket cũ

↓

Không còn đúng

↓

Tính lại bucket mới
```

Toàn bộ key sẽ được phân phối lại.

---

# 14. Độ phức tạp

| Thao tác | Average | Worst |
|----------|----------|--------|
| put | O(1) | O(n) |
| get | O(1) | O(n) |
| containsKey | O(1) | O(n) |
| remove | O(1) | O(n) |

Java 8:

Nếu Treeify:

```
O(log n)
```

---

# 15. HashMap có thread-safe không?

Không.

Muốn thread-safe:

```java
ConcurrentHashMap
```

hoặc

```java
Collections.synchronizedMap(...)
```

---

# Câu hỏi phỏng vấn

## 1. HashMap hoạt động như thế nào?

**Đáp án**

```
Key

↓

hashCode()

↓

Bucket

↓

equals()

↓

Value
```

---

## 2. hashCode() dùng để làm gì?

Để xác định bucket lưu object.

---

## 3. equals() dùng để làm gì?

Để xác định đúng key trong bucket.

---

## 4. Nếu override equals() mà không override hashCode()?

HashMap sẽ hoạt động sai vì hai object bằng nhau có thể nằm ở hai bucket khác nhau.

---

## 5. Collision là gì?

Nhiều key cùng nằm trong một bucket.

---

## 6. Java 8 xử lý Collision như thế nào?

- Ít node → LinkedList.
- Nhiều node → Red Black Tree.

---

## 7. Load Factor mặc định?

```
0.75
```

---

## 8. Khi nào HashMap resize?

Khi:

```
size > capacity × loadFactor
```

Ví dụ:

```
16 × 0.75 = 12

Thêm phần tử thứ 13 → Resize.
```

---

## 9. Resize có làm gì ngoài tăng mảng?

Có.

HashMap còn:

```
Rehash toàn bộ key.
```

---

## 10. HashMap có giữ thứ tự không?

Không.

Muốn giữ thứ tự:

- LinkedHashMap

Muốn sắp xếp theo key:

- TreeMap

---

# Các câu bẫy

## Bẫy 1

### HashMap chỉ dùng hashCode(), không dùng equals().

❌ Sai.

**Giải thích**

- hashCode() xác định bucket.
- equals() xác định đúng key.

---

## Bẫy 2

### Hai object có cùng hashCode() thì equals() luôn true.

❌ Sai.

Nhiều object khác nhau vẫn có cùng hashCode().

---

## Bẫy 3

### Hai object equals() thì hashCode() có thể khác nhau.

❌ Sai.

Nếu:

```java
a.equals(b)
```

thì:

```
a.hashCode() == b.hashCode()
```

là bắt buộc.

---

## Bẫy 4

### HashMap luôn O(1).

❌ Sai.

- Average: O(1)
- Worst Java 7: O(n)
- Worst Java 8 (Treeify): O(log n)

---

## Bẫy 5

### Collision chỉ xảy ra khi hashCode() giống nhau.

❌ Sai.

Điều HashMap quan tâm là **cùng bucket**.

Hai hash khác nhau vẫn có thể rơi vào cùng bucket sau khi tính index.

---

## Bẫy 6

### HashMap resize khi đầy 75%.

❌ Sai.

Chính xác là:

```
size > capacity × loadFactor
```

---

## Bẫy 7

### Có 8 node trong bucket thì chắc chắn Treeify.

❌ Sai.

Điều kiện đầy đủ:

- Bucket >= 8 node.
- Table >= 64 bucket.

Nếu table < 64, HashMap sẽ ưu tiên Resize.

---

# Tổng kết cần nhớ

```
put(key, value)

        │
        ▼
   hashCode()
        │
        ▼
Tính bucket (index)
        │
        ▼
 Có bucket?
        │
        ▼
   equals() kiểm tra key
        │
        ├── Có → cập nhật value
        └── Không → thêm node
                │
                ▼
      Collision?
                │
                ▼
LinkedList → (>=8 node & table>=64) → Red Black Tree
                │
                ▼
 Vượt capacity × loadFactor?
                │
                ▼
      Resize → Rehash toàn bộ
```

# Checklist trước phỏng vấn

- [ ] HashMap là gì?
- [ ] Bucket là gì?
- [ ] hashCode() dùng để làm gì?
- [ ] equals() dùng để làm gì?
- [ ] Vì sao phải override cả equals() và hashCode()?
- [ ] Collision là gì?
- [ ] Java 7 xử lý Collision?
- [ ] Java 8 xử lý Collision?
- [ ] Treeify xảy ra khi nào?
- [ ] Untreeify xảy ra khi nào?
- [ ] Load Factor là gì?
- [ ] Resize là gì?
- [ ] Rehash là gì?
- [ ] HashMap có thread-safe không?
- [ ] HashMap khác LinkedHashMap và TreeMap ở điểm nào?
