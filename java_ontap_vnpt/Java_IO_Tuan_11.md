# 11. IO (Input / Output) ⭐⭐⭐

Đây là phần Java Core khá hay gặp trong phỏng vấn Junior/Mid.

---

# 1. IO là gì?

IO (Input/Output) là quá trình **đọc (Input)** hoặc **ghi (Output)** dữ liệu.

Ví dụ:

- Đọc file txt
- Ghi file csv
- Đọc ảnh
- Gửi dữ liệu qua socket
- Đọc keyboard

```
File --> Java Program --> Console
```

hoặc

```
Console --> Java Program --> File
```

---

## Câu hỏi

### IO là gì?

**Đáp án**

IO (Input/Output) là cơ chế giúp chương trình đọc và ghi dữ liệu từ nhiều nguồn khác nhau như:

- File
- Network
- Database
- Keyboard
- Memory

---

# 2. File

`File` đại diện cho đường dẫn của file hoặc thư mục.

Lưu ý:

**File KHÔNG chứa dữ liệu.**

Nó chỉ đại diện cho:

```
C:\abc\data.txt
```

Ví dụ:

```java
File file = new File("data.txt");

System.out.println(file.exists());
System.out.println(file.length());
```

Có thể:

- tạo file
- xóa file
- đổi tên
- lấy kích thước
- kiểm tra tồn tại

---

## Câu hỏi

### File có dùng để đọc dữ liệu không?

**Đáp án**

Không.

`File` chỉ đại diện cho đường dẫn.

Muốn đọc dữ liệu phải dùng:

- FileInputStream
- FileReader
- BufferedReader

---

# 3. InputStream

Dùng để đọc **dữ liệu dạng byte**.

```
File
 ↓
InputStream
 ↓
byte[]
```

Ví dụ:

```java
FileInputStream fis = new FileInputStream("a.jpg");

int data;

while ((data = fis.read()) != -1) {
    System.out.print(data);
}

fis.close();
```

---

## Khi nào dùng?

Khi đọc:

- ảnh
- pdf
- zip
- video
- file nhị phân

---

## Câu hỏi

### InputStream đọc kiểu dữ liệu gì?

**Đáp án**

Đọc theo **byte**.

---

### Vì sao InputStream phù hợp với ảnh?

**Đáp án**

Ảnh là dữ liệu nhị phân.

InputStream đọc byte nên không làm hỏng dữ liệu.

---

# 4. OutputStream

Ngược lại InputStream.

Dùng để ghi byte.

```
Program

↓

OutputStream

↓

File
```

Ví dụ:

```java
FileOutputStream fos =
        new FileOutputStream("hello.txt");

fos.write("Hello".getBytes());

fos.close();
```

---

## Câu hỏi

### OutputStream dùng để làm gì?

**Đáp án**

Ghi dữ liệu dạng byte.

---

# 5. Reader

Reader dùng để đọc **ký tự (character)**.

Java sử dụng Unicode.

```
File

↓

Reader

↓

char
```

Ví dụ:

```java
FileReader reader =
        new FileReader("test.txt");

int c;

while ((c = reader.read()) != -1) {
    System.out.print((char) c);
}

reader.close();
```

---

## Khi nào dùng?

Đọc:

- txt
- csv
- json
- xml
- html

---

## Câu hỏi

### Reader khác InputStream như thế nào?

**Đáp án**

- Reader đọc character.
- InputStream đọc byte.

---

# 6. Writer

Writer dùng để ghi ký tự.

Ví dụ:

```java
FileWriter writer =
        new FileWriter("a.txt");

writer.write("Hello Java");

writer.close();
```

---

## Câu hỏi

### Writer dùng khi nào?

**Đáp án**

Khi ghi dữ liệu văn bản:

- txt
- json
- xml
- html

---

# 7. Byte vs Character

### InputStream

```
byte

↓

01010100
```

### Reader

```
char

↓

A
B
C
```

Ví dụ:

```
Xin chào
```

Reader sẽ đọc từng ký tự.

InputStream sẽ đọc từng byte UTF-8.

---

## Câu hỏi

### Khi đọc file txt nên dùng gì?

**Đáp án**

Reader.

---

### Khi đọc file PDF?

**Đáp án**

InputStream.

---

# 8. BufferedReader

BufferedReader giúp đọc nhanh hơn.

Nó sử dụng bộ nhớ đệm (Buffer).

### Không dùng Buffer

```
Disk

↓

Java đọc 1 ký tự

↓

Disk

↓

Java đọc tiếp...
```

Rất nhiều lần truy cập ổ đĩa nên chậm.

---

### Có Buffer

```
Disk

↓

Đọc 8KB một lần

↓

Java lấy dần trong RAM
```

Nhanh hơn rất nhiều.

Ví dụ:

```java
BufferedReader br =
    new BufferedReader(
        new FileReader("a.txt"));

String line;

while ((line = br.readLine()) != null) {
    System.out.println(line);
}
```

---

## Câu hỏi

### Vì sao BufferedReader nhanh hơn?

**Đáp án**

Vì đọc nhiều dữ liệu một lần vào bộ nhớ đệm rồi mới xử lý.

Giảm số lần truy cập ổ đĩa.

---

# 9. BufferedWriter

Ghi dữ liệu có Buffer.

Ví dụ:

```java
BufferedWriter bw =
    new BufferedWriter(
        new FileWriter("a.txt"));

bw.write("Hello");

bw.close();
```

---

# 10. Serialization

Serialization là quá trình biến object thành chuỗi byte.

```
Object

↓

byte[]

↓

File
```

Ví dụ:

```java
class User implements Serializable {

    String name;
}
```

```java
ObjectOutputStream out =
    new ObjectOutputStream(
        new FileOutputStream("user.dat"));

out.writeObject(user);

out.close();
```

---

## Deserialization

Ngược lại.

```
byte[]

↓

Object
```

Ví dụ:

```java
ObjectInputStream in =
    new ObjectInputStream(
        new FileInputStream("user.dat"));

User user = (User) in.readObject();

in.close();
```

---

## Câu hỏi

### Serialization là gì?

**Đáp án**

Biến object thành chuỗi byte để:

- lưu file
- gửi network
- cache
- truyền qua message queue

---

### Deserialization là gì?

**Đáp án**

Khôi phục object từ chuỗi byte.

---

### Muốn object Serializable cần làm gì?

**Đáp án**

Implement interface:

```java
Serializable
```

---

### Serializable có phương thức không?

**Đáp án**

Không.

Đây là **Marker Interface**, không có phương thức nào.

---

### Tác dụng của transient?

**Đáp án**

Đánh dấu field **không được serialize**.

Ví dụ:

```java
class User implements Serializable {

    String username;

    transient String password;
}
```

Sau khi deserialize:

```java
username = "admin"
password = null
```

Thường dùng cho:

- password
- token
- cache
- dữ liệu tạm

---

# 11. Try-with-resources

Từ Java 7 nên dùng:

```java
try (BufferedReader br =
         new BufferedReader(
             new FileReader("a.txt"))) {

    String line;

    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
}
```

Không cần gọi:

```java
br.close();
```

Java sẽ tự động đóng tài nguyên.

---

## Câu hỏi

### Vì sao nên dùng try-with-resources?

**Đáp án**

- Tự động đóng tài nguyên.
- Tránh resource leak.
- Code ngắn gọn hơn.
- Vẫn đóng tài nguyên nếu xảy ra Exception.

---

# 12. Tổng kết

```
                IO
                 │
      ┌──────────┴──────────┐
      │                     │
   Byte Stream          Character Stream
      │                     │
 InputStream            Reader
 OutputStream           Writer
      │                     │
 FileInputStream      FileReader
 FileOutputStream     FileWriter
      │                     │
 BufferedInputStream  BufferedReader
 BufferedOutputStream BufferedWriter
```

---

# Tổng hợp câu hỏi phỏng vấn

## ⭐ Dễ

- IO là gì?
- File dùng để làm gì?
- InputStream là gì?
- OutputStream là gì?
- Reader là gì?
- Writer là gì?
- Byte khác Character như thế nào?
- Khi nào dùng Reader?
- Khi nào dùng InputStream?

---

## ⭐⭐ Trung bình

- BufferedReader hoạt động như thế nào?
- Vì sao BufferedReader nhanh hơn FileReader?
- Serialization là gì?
- Deserialization là gì?
- Serializable là gì?
- transient dùng để làm gì?
- Marker Interface là gì?

---

## ⭐⭐⭐ Khó

### Nếu muốn đọc một file ảnh thì dùng Reader hay InputStream? Vì sao?

**Đáp án**

Dùng InputStream vì ảnh là dữ liệu nhị phân.

Reader sẽ chuyển byte thành ký tự nên có thể làm hỏng dữ liệu.

---

### Vì sao không nên dùng FileReader để đọc file UTF-8 trong mọi trường hợp?

**Đáp án**

FileReader sử dụng charset mặc định của hệ điều hành.

Nếu file UTF-8 nhưng hệ điều hành dùng charset khác thì dữ liệu có thể bị lỗi.

Nên dùng:

```java
BufferedReader br = new BufferedReader(
    new InputStreamReader(
        new FileInputStream("a.txt"),
        StandardCharsets.UTF_8
    )
);
```

để chỉ định rõ charset.

---

### Serialization có nhược điểm gì?

**Đáp án**

- Hiệu năng không cao.
- Khó tương thích khi thay đổi cấu trúc class (`serialVersionUID`).
- Có thể gây lỗ hổng bảo mật nếu deserialize dữ liệu từ nguồn không tin cậy.
- Hiện nay nhiều hệ thống ưu tiên JSON, Protocol Buffers hoặc Avro để trao đổi dữ liệu.
