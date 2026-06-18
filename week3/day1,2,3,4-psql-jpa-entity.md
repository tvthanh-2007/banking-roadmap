# 🏦 BANKING LAB – WEEK 3
**PostgreSQL + JPA + CRUD + QUERY**

---

## 📌 WEEK 3 OVERVIEW

| Day | Nội dung |
|-----|----------|
| Day 1 | Connect PostgreSQL |
| Day 2 | Entity Mapping |
| Day 3 | CRUD Real DB |
| Day 4 | Query Methods |

---

## 📘 DAY 1 – POSTGRESQL SETUP + SPRING BOOT CONNECT

### 🎯 Mục tiêu
- Setup PostgreSQL (Docker)
- Connect Spring Boot với database
- Verify Hibernate connection

### 🐳 Run PostgreSQL (Docker)
```bash
docker run --name banking-postgres \
  -e POSTGRES_USER=banking_user \
  -e POSTGRES_PASSWORD=123456 \
  -e POSTGRES_DB=banking \
  -p 5432:5432 \
  -d postgres:16
```

### ⚙️ application.yml
```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/banking
    username: banking_user
    password: 123456
    driver-class-name: org.postgresql.Driver
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```

### 📦 Output
- PostgreSQL running
- Spring Boot connect success
- Hibernate logs active

---

## 📘 DAY 2 – JPA ENTITY MAPPING

### 🎯 Mục tiêu
- Map Java class → DB table
- Auto create schema using Hibernate

### 🧩 Customer Entity
```java
@Entity
@Table(name = "customers")
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    public Customer() {}

    public Customer(String name) {
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

### 🧠 Key Concept
- `@Entity` → table
- field → column
- Hibernate auto generates schema

### 📦 Output
- Table `customers` created
- Columns: `id`, `name`

---

## 📘 DAY 3 – CRUD WITH REAL DATABASE

### 🎯 Mục tiêu
- CRUD using PostgreSQL
- No mock data anymore

### 🧩 Repository
```java
public interface CustomerRepository
        extends JpaRepository<Customer, Long> {
}
```

### ⚙️ Service
```java
@Service
public class CustomerService {

    private final CustomerRepository customerRepository;

    public CustomerService(CustomerRepository customerRepository) {
        this.customerRepository = customerRepository;
    }

    public List<Customer> findAll() {
        return customerRepository.findAll();
    }

    public Customer create(String name) {
        return customerRepository.save(new Customer(name));
    }

    public Customer findById(Long id) {
        return customerRepository.findById(id)
                .orElseThrow();
    }

    public void delete(Long id) {
        customerRepository.deleteById(id);
    }
}
```

### 🌐 Controller
```java
@RestController
@RequestMapping("/customers")
public class CustomerController {

    private final CustomerService customerService;

    public CustomerController(CustomerService customerService) {
        this.customerService = customerService;
    }

    @GetMapping
    public List<Customer> getAll() {
        return customerService.findAll();
    }

    @PostMapping
    public Customer create(@RequestBody CreateCustomerRequest req) {
        return customerService.create(req.getName());
    }
}
```

### 📦 Output
- Full CRUD working with PostgreSQL
- Save / Get / Delete real data

---

## 📘 DAY 4 – JPA QUERY METHODS

### 🎯 Mục tiêu
- Derived query methods
- JPQL query
- Native SQL query

### 🧩 Repository
```java
public interface CustomerRepository extends JpaRepository<Customer, Long> {

    List<Customer> findByName(String name);

    List<Customer> findByNameContaining(String keyword);

    List<Customer> findByNameStartingWith(String prefix);

    List<Customer> findByNameEndingWith(String suffix);

    @Query("SELECT c FROM Customer c WHERE c.name = :name")
    List<Customer> searchByName(String name);

    @Query(value = "SELECT * FROM customers WHERE name = :name", nativeQuery = true)
    List<Customer> searchNative(String name);
}
```

### ⚙️ Service
```java
@Service
public class CustomerService {

    private final CustomerRepository customerRepository;

    public CustomerService(CustomerRepository customerRepository) {
        this.customerRepository = customerRepository;
    }

    public List<Customer> searchContaining(String keyword) {
        return customerRepository.findByNameContaining(keyword);
    }

    public List<Customer> searchExact(String name) {
        return customerRepository.findByName(name);
    }

    public List<Customer> searchStart(String prefix) {
        return customerRepository.findByNameStartingWith(prefix);
    }

    public List<Customer> searchEnd(String suffix) {
        return customerRepository.findByNameEndingWith(suffix);
    }

    public List<Customer> searchJPQL(String name) {
        return customerRepository.searchByName(name);
    }

    public List<Customer> searchNative(String name) {
        return customerRepository.searchNative(name);
    }
}
```

### 🌐 Controller
```java
@RestController
@RequestMapping("/customers")
public class CustomerController {

    private final CustomerService customerService;

    public CustomerController(CustomerService customerService) {
        this.customerService = customerService;
    }

    @GetMapping("/search")
    public List<Customer> search(@RequestParam String keyword) {
        return customerService.searchContaining(keyword);
    }
}
```

### 📦 Output
- Search API working
- Derived query understood
- JPQL + Native SQL working

---

## 🧠 WEEK 3 SUMMARY

| Day | Kết quả |
|-----|---------|
| Day 1 | PostgreSQL connection |
| Day 2 | Entity mapping |
| Day 3 | CRUD real DB |
| Day 4 | Query methods |
