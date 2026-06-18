# 🧪 BÀI 11 – SPRING BOOT TESTING (CUSTOMER API)

---

# 1. OVERVIEW

Spring Boot Testing chia làm 3 tầng:

| Layer | Mục tiêu | Tool |
|------|---------|------|
| Controller | Test API | MockMvc + @WebMvcTest |
| Service | Test logic | Mockito + JUnit 5 |
| Repository | Test DB | @DataJpaTest |

---

# 2. JUNIT 5 (TEST ENGINE)

JUnit 5 là framework chạy test:

```java
@Test
void sampleTest() {
    assertEquals(1, 1);
}
```

Không phụ thuộc Spring
Chỉ là test runner

---

# 3. CONTROLLER TEST

## Annotation

@WebMvcTest(CustomerController.class)

---

## Mock dependency (Spring Boot 4)

```java
@MockitoBean
private CustomerService customerService;
```
---

## MockMvc

```java
@Autowired
private MockMvc mockMvc;
```

---

## GET /customers

```java
@Test
void getCustomers_success() throws Exception {

    when(customerService.findAll())
            .thenReturn(List.of(
                    new Customer(1L, "A"),
                    new Customer(2L, "B")
            ));

    mockMvc.perform(get("/customers"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.length()").value(2))
            .andExpect(jsonPath("$[0].name").value("A"));
}
```
---

## POST /customers

```java
@Test
void createCustomer_success() throws Exception {

    CreateCustomerRequest req = new CreateCustomerRequest();
    req.setName("A");

    Customer saved = new Customer(1L, "A");

    when(customerService.create("A"))
            .thenReturn(saved);

    mockMvc.perform(post("/customers")
            .contentType(MediaType.APPLICATION_JSON)
            .content(objectMapper.writeValueAsString(req)))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.id").value(1))
            .andExpect(jsonPath("$.name").value("A"));
}
```
---

# 4. SERVICE TEST (MOCKITO PURE)

## Setup

```java
@ExtendWith(MockitoExtension.class)

@Mock
CustomerRepository customerRepository;

@InjectMocks
CustomerService customerService;
```
---

## findAll()

```java
@Test
void findAll_success() {

    when(customerRepository.findAll())
            .thenReturn(List.of(
                    new Customer(1L, "A"),
                    new Customer(2L, "B")
            ));

    List<Customer> result = customerService.findAll();

    assertEquals(2, result.size());

    verify(customerRepository).findAll();
}
```
---

```java
## create()

@Test
void create_success() {

    when(customerRepository.save(any(Customer.class)))
            .thenReturn(new Customer(1L, "A"));

    Customer result = customerService.create("A");

    assertEquals("A", result.getName());

    verify(customerRepository).save(any());
}
```
---

# 5. VERIFY() TRONG MOCKITO

verify() dùng để kiểm tra method có được gọi hay không.

---

## Ví dụ
```java
verify(customerRepository).save(any());
```
---

## Các kiểu verify

### 1 lần
```java
verify(customerRepository).save(any());
verify(customerRepository, times(1)).save(any());
```
---

### nhiều lần
verify(customerRepository, times(2)).findAll();

---

### không được gọi
verify(customerRepository, never()).delete(any());

---

### ít nhất / nhiều nhất
verify(customerRepository, atLeastOnce()).save(any());
verify(customerRepository, atMost(3)).save(any());

---

## Khi dùng verify

- check service gọi repo
- check flow logic
- check side effect

---

# 6. ASSERT VS VERIFY

| Mục tiêu | assert | verify |
|----------|--------|--------|
| Check output | OK | NO |
| Check method call | NO | OK |
| Check flow | NO | OK |

---

# 7. TEST FLOW

Controller test:
MockMvc → Controller → Service (mock) → JSON response

Service test:
Service → Repository (mock) → return data

---

# 8. COMMON MISTAKES

Sai:
when(repo.save(new Customer("A")))

Đúng:
when(repo.save(any(Customer.class)))

---

Quên ObjectMapper:
@Autowired
ObjectMapper objectMapper

---

# 9. SUMMARY

Bạn đã học:

- JUnit 5
- Controller test
- Service test
- MockMvc
- MockitoBean
- verify()

---

# 10. KEY TAKEAWAY

assert = check output

verify = check behavior
