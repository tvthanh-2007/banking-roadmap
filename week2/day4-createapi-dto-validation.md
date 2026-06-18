# Day 9 - Create API, DTO, Validation

## Mục tiêu

Xây dựng API tạo Customer đầu tiên bằng Spring Boot và hiểu cách tách biệt API Contract với Database Model.

---

# 1. POST API

Tạo endpoint:

```java
@PostMapping("/customers")
public Customer createCustomer(
        @RequestBody CreateCustomerRequest request) {

    return customerService.create(
            request.getName());
}
```

Khác với:

```java
@GetMapping("/customers")
```

* GET dùng để lấy dữ liệu.
* POST dùng để tạo dữ liệu.

---

# 2. @RequestBody

Spring sử dụng Jackson để chuyển JSON thành Java Object.

Request:

```json
{
  "name": "Thanh"
}
```

↓

```java
CreateCustomerRequest request
```

Ví dụ:

```java
request.getName();
```

sẽ nhận giá trị:

```text
Thanh
```

---

# 3. Request DTO

Tạo DTO:

```java
public class CreateCustomerRequest {

    private String name;

    public CreateCustomerRequest() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

Không nên dùng:

```java
@RequestBody Customer customer
```

Vì API Contract không nên phụ thuộc trực tiếp vào Entity.

---

# 4. Service Layer

Interface:

```java
public interface CustomerService {

    List<Customer> findAll();

    Customer create(String name);
}
```

Implementation:

```java
@Service
public class CustomerServiceImpl
        implements CustomerService {

    private final CustomerRepository customerRepository;

    public CustomerServiceImpl(
            CustomerRepository customerRepository) {
        this.customerRepository = customerRepository;
    }

    @Override
    public Customer create(String name) {

        Customer customer =
                new Customer(name);

        return customerRepository.save(customer);
    }
}
```

---

# 5. JpaRepository.save()

Ví dụ:

```java
Customer customer =
        new Customer("Thanh");

Customer savedCustomer =
        customerRepository.save(customer);
```

`save()` trả về:

```java
Customer
```

đã được persist xuống database.

Ví dụ:

```java
savedCustomer.getId();
```

đã có giá trị sau khi lưu.

---

# 6. Vì sao không nhận Entity trực tiếp?

Ví dụ:

```java
@PostMapping("/customers")
public Customer createCustomer(
        @RequestBody Customer customer) {

    return customerRepository.save(customer);
}
```

Client gửi:

```json
{
  "id": 1,
  "name": "Thanh Hacker"
}
```

JPA có thể hiểu đây là bản ghi đã tồn tại và thực hiện update.

Điều này rất nguy hiểm.

Vì vậy nên dùng:

```text
Request DTO
↓
Entity
↓
Repository
```

---

# 7. Validation

Thêm dependency:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

DTO:

```java
import jakarta.validation.constraints.NotBlank;

public class CreateCustomerRequest {

    @NotBlank
    private String name;

    // getter/setter
}
```

Controller:

```java
@PostMapping("/customers")
public Customer createCustomer(
        @Valid @RequestBody CreateCustomerRequest request) {

    return customerService.create(
            request.getName());
}
```

---

# 8. @NotBlank

```java
@NotBlank
private String name;
```

Sẽ reject:

```json
{}
```

```json
{
  "name": ""
}
```

```json
{
  "name": "   "
}
```

Kết quả:

```http
400 Bad Request
```

---

# 9. So sánh Validation Annotation

## @NotNull

Reject:

```text
null
```

Cho phép:

```text
""
"   "
```

---

## @NotEmpty

Reject:

```text
null
""
```

Cho phép:

```text
"   "
```

---

## @NotBlank

Reject:

```text
null
""
"   "
```

Đây là annotation thường dùng nhất cho String.

---

# 10. Response DTO

Tạo Response DTO:

```java
public class CustomerResponse {

    private Long id;
    private String name;

    public CustomerResponse(
            Long id,
            String name) {
        this.id = id;
        this.name = name;
    }

    public Long getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}
```

Controller:

```java
@PostMapping("/customers")
public CustomerResponse createCustomer(
        @Valid @RequestBody CreateCustomerRequest request) {

    Customer customer =
            customerService.create(
                    request.getName());

    return new CustomerResponse(
            customer.getId(),
            customer.getName()
    );
}
```

---

# 11. Tại sao cần Response DTO?

Entity:

```java
@Entity
public class Customer {

    private Long id;
    private String name;
    private String citizenId;
    private String phoneNumber;
}
```

Nếu trả trực tiếp Entity:

```java
return customer;
```

API có thể expose dữ liệu nhạy cảm.

Response DTO giúp kiểm soát dữ liệu trả về.

Ví dụ:

```json
{
  "id": 1,
  "name": "Thanh"
}
```

---

# 12. Constructor rỗng và Reflection

## Entity

```java
@Entity
public class Customer {

    protected Customer() {
    }
}
```

JPA/Hibernate cần constructor rỗng để tạo object bằng Reflection.

Ví dụ:

```java
Customer customer =
    Customer.class
        .getDeclaredConstructor()
        .newInstance();
```

---

## Request DTO

```java
public CreateCustomerRequest() {
}
```

Jackson cần constructor rỗng để deserialize JSON.

Ví dụ:

```json
{
  "name": "Thanh"
}
```

↓

```java
CreateCustomerRequest request
```

---

## Response DTO

Không bắt buộc constructor rỗng.

Vì chúng ta tự tạo object:

```java
new CustomerResponse(
        customer.getId(),
        customer.getName());
```

Jackson chỉ cần đọc getter để chuyển thành JSON.

---

# 13. Record và DTO

Có thể dùng:

```java
public record CustomerResponse(
        Long id,
        String name
) {
}
```

Ưu điểm:

* Immutable
* Ngắn gọn
* Phù hợp với Response DTO

---

# 14. Vì sao Entity không nên dùng Record?

Record:

```java
public record Customer(
        Long id,
        String name
) {
}
```

Có các hạn chế:

* final
* immutable
* không phù hợp với cơ chế proxy của Hibernate
* không phù hợp với JPA Entity

Vì vậy:

```java
@Entity
public class Customer {
}
```

vẫn là lựa chọn phổ biến.

---

# Kiến trúc sau Day 9

```text
POST /customers
        ↓
@RequestBody
        ↓
CreateCustomerRequest
        ↓
Validation
        ↓
Service
        ↓
Repository
        ↓
PostgreSQL
        ↓
CustomerResponse
```

---

# Kết quả đạt được

✅ POST API

✅ @RequestBody

✅ Request DTO

✅ Response DTO

✅ Validation

✅ @Valid

✅ @NotBlank

✅ JpaRepository.save()

✅ Hiểu Reflection và Constructor rỗng

✅ Hiểu Record vs Entity

---

# Chuẩn bị cho Day 10

* Exception Handling
* @RestControllerAdvice
* Custom Error Response
* HTTP Status Code
* Validation Error Handling
