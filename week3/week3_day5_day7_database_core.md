# WEEK 3 - DATABASE CORE (PostgreSQL + JPA)

## Day 5 - Transaction + Persistence Context

## @Transactional

`@Transactional` tạo transaction boundary:

    BEGIN

    business logic

    COMMIT

Nếu có RuntimeException:

    BEGIN

    business logic

    ROLLBACK

Spring transaction thường dùng:

``` java
import org.springframework.transaction.annotation.Transactional;
```

------------------------------------------------------------------------

## Persistence Context

Hibernate quản lý entity trong Persistence Context.

Ví dụ:

``` java
@Transactional
public void updateCustomerName(Long id, String name){

    Customer customer = repository.findById(id)
        .orElseThrow();

    customer.setName(name);

}
```

Không cần gọi:

``` java
repository.save(customer);
```

vì Hibernate dùng Dirty Checking.

------------------------------------------------------------------------

## Dirty Checking

Flow:

    Load entity
        |
    Change field
        |
    Commit
        |
    Hibernate tạo UPDATE SQL

------------------------------------------------------------------------

# Day 6 - Pagination + Sorting

## Vì sao cần Pagination?

Không nên:

``` java
repository.findAll();
```

với dữ liệu lớn.

Ví dụ:

    10 triệu customers

sẽ load toàn bộ vào RAM.

------------------------------------------------------------------------

## Pageable

JpaRepository có sẵn:

``` java
Page<T> findAll(Pageable pageable)
```

------------------------------------------------------------------------

## Service

``` java
public Page<CustomerResponse> findAll(Pageable pageable){

    return customerRepository
        .findAll(pageable)
        .map(customer ->
            new CustomerResponse(
                customer.getId(),
                customer.getName()
            )
        );
}
```

------------------------------------------------------------------------

## Controller

``` java
@GetMapping("/customers")
public Page<CustomerResponse> getCustomers(
        Pageable pageable
){

    return customerService.findAll(pageable);
}
```

------------------------------------------------------------------------

## Test API

Pagination:

    GET /customers?page=0&size=5

Sorting:

    GET /customers?page=0&size=5&sort=name,desc

------------------------------------------------------------------------

# Day 7 - Locking + Concurrency

## Concurrency problem

Ví dụ:

    balance = 1000

A withdraw 800

B withdraw 800

Không có lock:

    A update 200

    B update 200

Sai dữ liệu.

------------------------------------------------------------------------

# Pessimistic Lock

Ý tưởng:

Khóa trước khi sửa.

Repository:

``` java
@Lock(LockModeType.PESSIMISTIC_WRITE)
@Query("""
select a from Account a
where a.id = :id
""")
Account findByIdForUpdate(Long id);
```

Hibernate tạo SQL:

``` sql
SELECT *
FROM account
FOR UPDATE
```

Flow:

    A lock row

    B wait

    A commit

    B continue

Dùng cho:

-   Transfer money
-   Balance
-   Payment

------------------------------------------------------------------------

# Optimistic Lock

Không khóa khi đọc.

Entity:

``` java
@Version
private Long version;
```

Ví dụ:

    id | balance | version

    1  | 1000    | 0

A đọc version 0

B đọc version 0

A update thành công:

    version 0 -> 1

B update:

    WHERE version = 0

Fail vì DB đã version 1.

Hibernate throw:

    OptimisticLockException

------------------------------------------------------------------------

# So sánh

                Pessimistic          Optimistic
  ------------- -------------------- ---------------
  Lock          trước                sau
  Cơ chế        DB lock              version
  Người khác    chờ                  fail
  Performance   thấp hơn             cao hơn
  Dùng          dữ liệu quan trọng   conflict thấp

------------------------------------------------------------------------

# Week 3 Progress

✅ PostgreSQL connection\
✅ Entity Mapping\
✅ CRUD PostgreSQL\
✅ Query Methods\
✅ Transaction\
✅ Persistence Context\
✅ Dirty Checking\
✅ Pagination\
✅ Sorting\
✅ Pessimistic Lock\
✅ Optimistic Lock
