# Week 4 - Banking Lab: Account Transfer & Transaction Safety

## Goal

Implement core banking transfer flow:

- Account management
- Money transfer between accounts
- Transaction history
- ACID transaction handling
- Concurrency control
- Duplicate request handling

---

# 1. Domain Design

## Account

Responsibilities:
- Store balance
- Hold account information
- Update balance safely

Fields:
- id
- accountNumber
- balance

## Transaction

Represent money movement:

Fields:
- id
- fromAccount
- toAccount
- amount
- type
- status
- createdAt

---

# 2. Transfer Flow

Controller -> TransferService -> Repository -> Database

Flow:
- Validate account
- Check balance
- Decrease sender balance
- Increase receiver balance
- Create transaction history

---

# 3. ACID Transaction

Transfer requires atomic operation.

Use:

```java
@Transactional
public TransferResponse transfer(){

}
```

Rollback if one step fails.

---

# 4. Concurrency Problem

Example:

Balance: 1000

Two requests withdraw 800 at the same time.

Without lock:
- Both read 1000
- Both update incorrectly

---

# 5. Locking

## Pessimistic Lock

Lock database row:

```java
@Lock(LockModeType.PESSIMISTIC_WRITE)
Optional<Account> findById(Long id);
```

SQL:

```sql
select *
from account
where id = ?
for update;
```

## Optimistic Lock

Use:

```java
@Version
private Long version;
```

Detect conflicting updates.

---

# 6. Idempotency

Prevent duplicate transfer requests.

Example:

Header:

```
Idempotency-Key: abc123
```

Flow:

- Check key exists
- Return old transaction if duplicated
- Otherwise process transfer

---

# 7. Pagination & Sorting

Using Pageable:

```java
Pageable pageable =
 PageRequest.of(
    0,
    20,
    Sort.by("createdAt").descending()
 );
```

Support:
- Multiple fields sorting
- Join field sorting

Example:

```java
Sort.by(
 Order.desc("createdAt"),
 Order.desc("amount")
)
```

---

# 8. Testing Strategy

TransferServiceTest focuses on:
- Business flow
- Duplicate handling
- Validation

Mock:
- Repository
- Mapper

Do not test mapper detail inside service test.

---

# Week 4 Completed

Implemented:

- Account domain
- Transaction domain
- Transfer service
- Balance validation
- ACID transaction
- Locking strategy
- Idempotency handling
- Pagination + sorting
- Unit tests

Commit:

```
feat: implement account transfer with transaction safety
```
