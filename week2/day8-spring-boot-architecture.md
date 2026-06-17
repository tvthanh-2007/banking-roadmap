# Day 8 - Spring Boot Architecture

## Mục tiêu

Hiểu kiến trúc cơ bản của Spring Boot:

```text
HTTP Request
    ↓
Controller
    ↓
Service
    ↓
Repository
    ↓
PostgreSQL
```

Đây là kiến trúc sẽ được sử dụng cho các module banking sau này như:

* Customer
* Account
* Transaction
* Transfer

---

## 1. JPA Entity

Tạo entity Customer:

```java
@Entity
@Table(name = "customers")
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    public Customer() {
    }

    public Customer(String name) {
        this.name = name;
    }
}
```

### Annotation

* `@Entity`: ánh xạ class với bảng database.
* `@Table`: chỉ định tên bảng.
* `@Id`: khóa chính.
* `@GeneratedValue`: tự động tăng ID.

### Jakarta Persistence

```java
import jakarta.persistence.*;
```

Dùng để import các annotation JPA như:

* `@Entity`
* `@Table`
* `@Id`
* `@GeneratedValue`

---

## 2. Constructor rỗng

```java
public Customer() {
}
```

JPA/Hibernate cần constructor rỗng để tạo object bằng Reflection khi đọc dữ liệu từ database.

Ví dụ:

```java
Customer customer = new Customer();
```

sau đó Hibernate sẽ gán giá trị cho các field.

---

## 3. Hibernate Auto DDL

Cấu hình:

```properties
spring.jpa.hibernate.ddl-auto=update
```

Khi chạy ứng dụng:

```bash
mvn spring-boot:run
```

Hibernate tự động tạo bảng:

```sql
customers
---------
id
name
```

---

## 4. Repository Layer

```java
public interface CustomerRepository
        extends JpaRepository<Customer, Long> {
}
```

### Ý nghĩa

* `Customer`: Entity.
* `Long`: kiểu của Primary Key.

Tự động có sẵn:

```java
save()
findById()
findAll()
deleteById()
count()
```

### Ghi chú

Repository kế thừa `JpaRepository` không cần thêm `@Repository`.

Ví dụ:

```java
public interface CustomerRepository
        extends JpaRepository<Customer, Long> {
}
```

Spring Data JPA sẽ tự động tạo implementation và đăng ký Bean trong IoC Container.

Do đó vẫn có thể inject:

```java
private final CustomerRepository customerRepository;
```

mà không cần tự viết implementation.

---

## 5. Service Layer

Interface:

```java
public interface CustomerService {
    List<Customer> findAll();
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
    public List<Customer> findAll() {
        return customerRepository.findAll();
    }
}
```

### Vai trò

Service chứa business logic.

Kiến trúc chuẩn:

```text
Controller
    ↓
Service
    ↓
Repository
```

---

## 6. Controller Layer

```java
@RestController
public class CustomerController {

    private final CustomerService customerService;

    public CustomerController(
            CustomerService customerService) {
        this.customerService = customerService;
    }

    @GetMapping("/customers")
    public List<Customer> getCustomers() {
        return customerService.findAll();
    }
}
```

API:

```http
GET /customers
```

---

## 7. DataLoader

```java
@Component
public class DataLoader
        implements CommandLineRunner {

    @Override
    public void run(String... args) {
    }
}
```

### String... args

Là varargs của Java.

Được Spring truyền vào khi ứng dụng khởi động.

Trong Banking Lab hiện tại chưa sử dụng.

---

## 8. Dependency Injection (DI)

Ví dụ:

```java
public CustomerController(
        CustomerService customerService) {
    this.customerService = customerService;
}
```

Spring tự động truyền dependency vào constructor.

Không cần:

```java
new CustomerServiceImpl(...)
```

### Multiple Implementations

Ví dụ:

```java
@Service
public class CustomerServiceImpl
        implements CustomerService {
}
```

và:

```java
@Service
public class MockCustomerService
        implements CustomerService {
}
```

Khi inject:

```java
CustomerService customerService
```

Spring sẽ báo lỗi:

```text
NoUniqueBeanDefinitionException
```

Vì không biết chọn implementation nào.

Giải pháp:

* `@Primary`
* `@Qualifier`

---

## 9. IoC Container

### Viết tắt

IoC = Inversion of Control

### Định nghĩa

Spring IoC Container chịu trách nhiệm:

* Tạo Bean
* Quản lý Bean
* Inject Dependency
* Quản lý vòng đời Bean

Ví dụ:

```text
CustomerController
        ↓
CustomerServiceImpl
        ↓
CustomerRepository
```

Đều được Spring tạo và quản lý.

---

## 10. Bean

Bean là object được Spring quản lý.

Ví dụ:

```java
@Service
public class CustomerServiceImpl {
}
```

Spring sẽ tạo object này và lưu trong IoC Container.

---

## Mối quan hệ giữa IoC và DI

```text
IoC
 └── DI
```

Hoặc:

```text
IoC = Ý tưởng

DI = Cách Spring triển khai ý tưởng đó
```

* IoC: chuyển quyền tạo và quản lý object từ Developer sang Spring.
* DI: Spring tự động inject dependency thay vì phải dùng từ khóa `new`.

---

## Kiến thức đạt được

✅ Spring Boot Architecture

✅ Entity

✅ JPA

✅ Hibernate

✅ Repository

✅ Service

✅ Controller

✅ Dependency Injection

✅ IoC Container

✅ Bean

✅ Constructor Injection

✅ Spring Data JPA

---

## Kết quả

API đầu tiên:

```http
GET /customers
```

Luồng xử lý:

```text
HTTP Request
    ↓
CustomerController
    ↓
CustomerService
    ↓
CustomerRepository
    ↓
PostgreSQL
```
