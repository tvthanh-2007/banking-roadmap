# Banking Backend Roadmap - Week 5 (Day 1-5)

## Day 1-3: Transaction, Locking, Idempotency

### Mục tiêu
Xây dựng transfer service an toàn cho hệ thống banking.

Flow:

Request
|
Idempotency check
|
Lock accounts
|
Withdraw / Deposit
|
Save transaction
|
Save idempotency
|
Commit

## Transaction

`@Transactional`

- Đảm bảo nhiều thao tác DB thành một đơn vị.
- Thành công -> COMMIT.
- RuntimeException đi ra ngoài transaction -> ROLLBACK.

## Pessimistic Lock

Dùng:

findByIdForUpdate()

Mục tiêu:
- tránh race condition
- tránh double spending

Ví dụ:
Request A đọc balance 1000
Request B đọc balance 1000

Lock giúp chỉ một request xử lý tại một thời điểm.

## Lock order

Luôn lock theo thứ tự:

firstId = min(fromId, toId)
secondId = max(fromId, toId)

Tránh deadlock.

## Idempotency

Dùng key để đảm bảo request retry không tạo giao dịch lần 2.

Flow:

Request 1:
key abc -> tạo transaction

Request 2:
key abc -> trả transaction cũ


---

# Day 4: Exception và Rollback

## RuntimeException rollback

Spring rollback khi:

- Exception là RuntimeException
- Exception thoát khỏi method @Transactional

Ví dụ:

withdraw()

throw InsufficientBalanceException

↓

bubble lên transfer()

↓

rollback


## Không được nuốt exception

Sai:

try {
    transfer();
}
catch(Exception e) {
    log.error(e);
}

Vì method kết thúc bình thường:

COMMIT


Đúng:

catch(Exception e){
    log.error(e);
    throw e;
}


## Domain Exception

Business rule nên nằm trong Entity.

Ví dụ:

Account.withdraw():

- amount phải > 0
- balance đủ tiền

throw:

InsufficientBalanceException


## Resource Exception

Ví dụ:

Account không tồn tại:

AccountNotFoundException


---

# Day 5: Transaction Propagation

## Ý tưởng

Khi transaction gọi transaction khác:

Method con dùng transaction nào?


## REQUIRED (default)

Có transaction:
-> dùng lại

Không có:
-> tạo mới


Ví dụ:

transfer()
|
transaction save
|
idempotency save

Cùng TX1.


## REQUIRES_NEW

Luôn tạo transaction mới.

Ví dụ Audit log:

TX1:
transfer money

Suspend TX1

TX2:
save audit

Commit TX2

Resume TX1


Nếu TX1 rollback:

Money rollback
Audit vẫn còn


## Banking usage

REQUIRED:

- transfer
- transaction record
- idempotency key


REQUIRES_NEW:

- audit log


---

# Spring AOP Proxy

@Transactional hoạt động nhờ proxy.

Flow:

Controller

↓

Service Proxy

↓

Service thật


Proxy làm:

- begin transaction
- commit
- rollback


## Self invocation issue

Không hoạt động:

this.saveAudit()


Vì:

Service thật gọi trực tiếp method.

Không đi qua proxy.

Không tạo transaction mới.


## Muốn REQUIRES_NEW hoạt động

Tách service:

TransferService

↓

AuditService


Transfer gọi:

auditService.saveAudit()

=> đi qua AuditServiceProxy

=> tạo transaction mới


---

# Week 5 Progress

Completed:

[x] Transaction basics
[x] Pessimistic locking
[x] Idempotency
[x] Exception rollback
[x] Custom exceptions
[x] Transaction propagation
[x] Spring AOP proxy concept
