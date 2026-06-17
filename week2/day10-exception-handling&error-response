# Day 10 - Exception Handling & Error Response

## Mục tiêu

Xử lý lỗi theo chuẩn Spring Boot thay vì để hệ thống trả về 500 Internal Server Error mặc định.

---

# 1. HTTP Status Code

## 200 OK

Request thành công.

Ví dụ:

```http
GET /customers/1
```

---

## 201 Created

Tạo dữ liệu thành công.

Ví dụ:

```http
POST /customers
```

---

## 400 Bad Request

Request không hợp lệ.

Ví dụ:

```json
{
  "name": ""
}
```

---

## 404 Not Found

Không tìm thấy dữ liệu.

Ví dụ:

```http
GET /customers/999
```

---

## 500 Internal Server Error

Lỗi hệ thống.

Ví dụ:

* Database down
* NullPointerException
* Kafka unavailable

---

# 2. GET Customer By ID

Service:

```java
Customer findById(Long id);
```

Implementation:

```java
@Override
public Customer findById(Long id) {

    return customerRepository.findById(id)
            .orElseThrow();
}
```

Controller:

```java
@GetMapping("/customers/{id}")
public CustomerResponse getCustomer(
        @PathVariable Long id) {

    Customer customer =
            customerService.findById(id);

    return new CustomerResponse(
            customer.getId(),
            customer.getName()
    );
}
```

---

# 3. Vấn đề

Nếu customer không tồn tại:

```http
GET /customers/999
```

Spring trả:

```http
500 Internal Server Error
```

Điều này không đúng.

Thực tế phải là:

```http
404 Not Found
```

---

# 4. Custom Exception

Tạo exception:

```java
package com.banking.bankinglab.exception;

public class CustomerNotFoundException
        extends RuntimeException {

    public CustomerNotFoundException(Long id) {
        super("Customer not found: " + id);
    }
}
```

Service:

```java
@Override
public Customer findById(Long id) {

    return customerRepository.findById(id)
            .orElseThrow(
                    () -> new CustomerNotFoundException(id)
            );
}
```

---

# 5. Global Exception Handler

Tạo:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
}
```

Ý nghĩa:

```text
Controller
    ↓
Exception
    ↓
GlobalExceptionHandler
```

Toàn bộ exception sẽ được xử lý tập trung.

---

# 6. Handle CustomerNotFoundException

```java
@ExceptionHandler(CustomerNotFoundException.class)
@ResponseStatus(HttpStatus.NOT_FOUND)
public String handleCustomerNotFound(
        CustomerNotFoundException ex) {

    return ex.getMessage();
}
```

---

# 7. Kết quả

Request:

```http
GET /customers/999
```

Response:

```http
404 Not Found
```

Body:

```text
Customer not found: 999
```

---

# 8. ErrorResponse DTO

Không nên trả String đơn thuần.

Tạo DTO:

```java
package com.banking.bankinglab.dto;

import java.time.LocalDateTime;

public class ErrorResponse {

    private LocalDateTime timestamp;
    private int status;
    private String message;

    public ErrorResponse(
            LocalDateTime timestamp,
            int status,
            String message) {

        this.timestamp = timestamp;
        this.status = status;
        this.message = message;
    }

    public LocalDateTime getTimestamp() {
        return timestamp;
    }

    public int getStatus() {
        return status;
    }

    public String getMessage() {
        return message;
    }
}
```

---

# 9. Trả về ErrorResponse

```java
@ExceptionHandler(CustomerNotFoundException.class)
@ResponseStatus(HttpStatus.NOT_FOUND)
public ErrorResponse handleCustomerNotFound(
        CustomerNotFoundException ex) {

    return new ErrorResponse(
            LocalDateTime.now(),
            HttpStatus.NOT_FOUND.value(),
            ex.getMessage()
    );
}
```

---

# 10. Response Chuẩn Hơn

```json
{
  "timestamp": "2026-06-18T19:30:00",
  "status": 404,
  "message": "Customer not found: 999"
}
```

---

# 11. Validation Exception

Request:

```json
{
  "name": ""
}
```

Spring ném:

```java
MethodArgumentNotValidException
```

---

# 12. Handle Validation Exception

```java
@ExceptionHandler(MethodArgumentNotValidException.class)
@ResponseStatus(HttpStatus.BAD_REQUEST)
public ErrorResponse handleValidationException(
        MethodArgumentNotValidException ex) {

    String message = ex.getBindingResult()
            .getFieldError()
            .getDefaultMessage();

    return new ErrorResponse(
            LocalDateTime.now(),
            HttpStatus.BAD_REQUEST.value(),
            message
    );
}
```

---

# 13. Custom Validation Message

DTO:

```java
@NotBlank(message = "Customer name is required")
private String name;
```

Request:

```json
{
  "name": ""
}
```

Response:

```json
{
  "timestamp": "...",
  "status": 400,
  "message": "Customer name is required"
}
```

---

# 14. Khám Phá Exception Object

Không cần học thuộc API.

Có thể dùng:

## IntelliJ Auto Complete

```java
ex.
```

↓

```java
getBindingResult()
getStatusCode()
...
```

---

## Ctrl + Click

Ví dụ:

```java
ex.getBindingResult()
```

↓

```java
BindingResult
```

↓

```java
getFieldError()
```

↓

```java
FieldError
```

↓

```java
getDefaultMessage()
```

---

## Debug

Đặt breakpoint:

```java
handleValidationException(...)
```

Xem:

```text
ex
 └─ bindingResult
      └─ fieldErrors
           └─ defaultMessage
```

---

# Phân Loại Lỗi

## Business Error

Ví dụ:

```text
Customer not found
Account not found
Insufficient balance
```

Trả:

```text
404
400
```

Log:

```text
WARN
```

---

## System Error

Ví dụ:

```text
NullPointerException
Database down
Kafka unavailable
```

Trả:

```text
500
```

Log:

```text
ERROR
```

---

# Kiến Trúc Sau Day 10

```text
Controller
    ↓
Validation
    ↓
Service
    ↓
Repository
    ↓
Database
```

và

```text
Exception
    ↓
GlobalExceptionHandler
    ↓
ErrorResponse
```

---

# Kết Quả Đạt Được

✅ HTTP Status Code

✅ RuntimeException

✅ CustomerNotFoundException

✅ @RestControllerAdvice

✅ @ExceptionHandler

✅ ErrorResponse DTO

✅ 404 Not Found

✅ Validation Exception Handling

✅ Custom Validation Message

✅ Debug & Exception Inspection

---

# Chuẩn Bị Cho Day 11

* Unit Test
* Mockito
* MockMvc
* Service Test
* Controller Test
* @Mock
* @InjectMocks
* Mock Repository
