# 6. Exception ⭐⭐⭐⭐

---

# 1. Exception là gì?

Exception là sự kiện xảy ra trong quá trình chạy chương trình (runtime) làm gián đoạn luồng thực thi bình thường.

Ví dụ:

```java
int a = 10 / 0;
```

Kết quả:

```
ArithmeticException
```

Nếu không xử lý, chương trình sẽ dừng.

---

# 2. Cấu trúc phân cấp Exception

```
                 Object
                    │
               Throwable
               /        \
          Error      Exception
                          │
                RuntimeException
```

Java chia thành:

- Error
- Checked Exception
- Unchecked Exception

---

# 3. Checked Exception

Là các Exception mà **compiler bắt buộc phải xử lý**.

Nếu không:

- try-catch
- hoặc throws

thì sẽ compile error.

Ví dụ:

```java
FileReader file = new FileReader("abc.txt");
```

Có thể phát sinh:

```
FileNotFoundException
```

Bắt buộc:

```java
try{
    ...
}catch(FileNotFoundException e){

}
```

hoặc

```java
public void readFile() throws FileNotFoundException{

}
```

Một số Checked Exception phổ biến:

- IOException
- SQLException
- FileNotFoundException
- ParseException

---

# 4. Unchecked Exception

Là Runtime Exception.

Compiler không bắt buộc xử lý.

Ví dụ:

```java
int a = 10 / 0;
```

```
ArithmeticException
```

Hoặc

```java
String s = null;

s.length();
```

```
NullPointerException
```

Một số Runtime Exception:

- NullPointerException
- ArithmeticException
- IndexOutOfBoundsException
- NumberFormatException
- ClassCastException

---

# 5. Error

Error không phải Exception.

Đây là lỗi nghiêm trọng của JVM.

Thông thường không xử lý.

Ví dụ:

```
OutOfMemoryError
StackOverflowError
VirtualMachineError
```

---

# 6. Checked vs Unchecked

| Checked Exception | Unchecked Exception |
|-------------------|---------------------|
| Kiểm tra lúc compile | Không kiểm tra |
| Bắt buộc xử lý | Không bắt buộc |
| Kế thừa Exception | Kế thừa RuntimeException |
| Thường do tài nguyên ngoài | Thường do bug code |

Ví dụ:

Checked

```java
IOException
```

Unchecked

```java
NullPointerException
```

---

# 7. try-catch

Dùng để bắt Exception.

Ví dụ:

```java
try{
    int a = 10 / 0;
}catch(ArithmeticException e){
    System.out.println("Lỗi chia cho 0");
}
```

---

# 8. finally

Khối finally luôn được chạy dù có Exception hay không.

Ví dụ:

```java
try{

}catch(Exception e){

}finally{
    System.out.println("Close connection");
}
```

Thường dùng để:

- close file
- close database
- release resource

---

# 9. throw

throw dùng để **ném ra một Exception**.

Ví dụ:

```java
public void withdraw(int money){

    if(money < 0){
        throw new IllegalArgumentException("Money invalid");
    }

}
```

---

# 10. throws

throws khai báo phương thức có thể phát sinh Exception.

Ví dụ:

```java
public void readFile() throws IOException{

}
```

Người gọi sẽ phải xử lý.

---

# 11. throw vs throws

| throw | throws |
|--------|---------|
| Ném Exception | Khai báo Exception |
| Trong method | Sau tên method |
| Một Exception | Có thể nhiều Exception |

Ví dụ:

```java
throw new RuntimeException();
```

```java
public void test() throws IOException{

}
```

---

# 12. Custom Exception

Có thể tự tạo Exception.

Ví dụ:

```java
public class AgeException extends Exception{

    public AgeException(String message){
        super(message);
    }

}
```

Sử dụng

```java
if(age < 18){
    throw new AgeException("Age must >=18");
}
```

Nếu muốn Runtime Exception

```java
public class AgeException extends RuntimeException{

}
```

---

# 13. Exception Propagation

Exception sẽ truyền ngược lên stack.

Ví dụ:

```java
A()

↓

B()

↓

C()
```

Nếu C throw Exception

```
↓

B

↓

A

↓

main
```

Nếu không ai bắt

```
Program terminated
```

---

# 14. try-with-resources

Java 7 giới thiệu.

Tự động đóng resource.

Ví dụ:

```java
try(
    BufferedReader br =
        new BufferedReader(new FileReader("a.txt"))
){

}
```

Không cần:

```java
finally{
    br.close();
}
```

---

# Câu hỏi phỏng vấn

## 1. Exception là gì?

Lỗi xảy ra trong quá trình chạy chương trình làm gián đoạn luồng thực thi.

---

## 2. Checked Exception là gì?

Exception mà compiler bắt buộc phải xử lý.

---

## 3. Runtime Exception là gì?

Exception xảy ra lúc runtime, compiler không bắt buộc xử lý.

---

## 4. Error là gì?

Lỗi nghiêm trọng của JVM, thường không xử lý.

---

## 5. throw khác throws?

**throw**

- Ném Exception.

**throws**

- Khai báo method có thể phát sinh Exception.

---

## 6. finally có luôn chạy không?

Thông thường **Có**.

Ngoại lệ:

- System.exit()
- JVM crash
- Máy mất điện

---

## 7. Có nên catch Exception không?

Không nên.

Nên bắt Exception cụ thể.

Ví dụ:

```java
catch(IOException e)
```

tốt hơn

```java
catch(Exception e)
```

---

## 8. Khi nào dùng Custom Exception?

Khi muốn biểu diễn lỗi nghiệp vụ.

Ví dụ:

- InvalidAccountException
- InsufficientBalanceException
- DuplicateEmailException

---

## 9. Vì sao RuntimeException không bắt buộc xử lý?

Vì thường là lỗi lập trình (bug), cần sửa code thay vì ép người dùng bắt lỗi.

---

## 10. try-with-resources dùng khi nào?

Khi làm việc với resource như:

- File
- Socket
- Database Connection
- InputStream
- OutputStream

---

# Các câu bẫy

## Bẫy 1

### Exception và Error là một.

❌ Sai.

- Exception có thể xử lý.
- Error thường không xử lý.

---

## Bẫy 2

### RuntimeException là Checked Exception.

❌ Sai.

RuntimeException là **Unchecked Exception**.

---

## Bẫy 3

### throws sẽ ném Exception.

❌ Sai.

`throws` chỉ khai báo.

`throw` mới thực sự ném Exception.

---

## Bẫy 4

### finally luôn chạy 100%.

❌ Sai.

Không chạy nếu:

- System.exit()
- JVM crash
- Mất điện

---

## Bẫy 5

### Có thể throw Error.

✅ Đúng.

Nhưng gần như không bao giờ nên làm.

---

## Bẫy 6

### Có thể catch Error.

✅ Đúng.

Nhưng hầu như không nên.

---

## Bẫy 7

### Override method có thể throws Exception lớn hơn.

❌ Sai.

Override chỉ được:

- throws cùng Exception
- hoặc Exception nhỏ hơn (subclass)

Không được throws Exception cha rộng hơn.

Ví dụ:

```java
class A{
    void test() throws IOException{}
}

class B extends A{

    @Override
    void test() throws FileNotFoundException{

    }
}
```

Được vì:

```
FileNotFoundException

↓

IOException
```

---

# Tổng kết

```
Throwable
     │
 ┌───┴────────┐
 │            │
Error     Exception
                │
       RuntimeException
```

```
Checked

↓

Compiler bắt xử lý

↓

try-catch hoặc throws
```

```
Unchecked

↓

RuntimeException

↓

Không bắt buộc xử lý
```

```
throw

↓

Ném Exception
```

```
throws

↓

Khai báo Exception
```

```
finally

↓

Đóng resource
```

```
try-with-resources

↓

Tự động close resource
```

# Checklist trước phỏng vấn

- [ ] Exception là gì?
- [ ] Error là gì?
- [ ] Checked Exception là gì?
- [ ] Unchecked Exception là gì?
- [ ] RuntimeException là gì?
- [ ] throw khác throws?
- [ ] finally dùng để làm gì?
- [ ] try-with-resources là gì?
- [ ] Khi nào dùng Custom Exception?
- [ ] Exception propagation là gì?
- [ ] Override method với throws có quy tắc gì?
- [ ] Có nên catch Exception không?
