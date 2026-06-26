# System Design - Notification System

## 1. Monolith (cùng source)

Flow:

User
 |
API
 |
Business Service
 |
Database
 |
Background Job
 |
Notification Worker
 |
Email / SMS / Push


Ý tưởng:
- Notification là một module trong application
- Gửi mail/push không chạy trong request chính
- Dùng background job để tránh block request


Ví dụ:
Transfer thành công
    |
    |
enqueue SendNotificationJob
    |
    |
Worker gửi email


---

## 2. Microservice

Flow:

Service A
    |
    |
  Kafka
    |
    |
Service B


Ví dụ:

Transaction Service
        |
        |
TransactionCompleted Event
        |
        |
      Kafka
        |
        |
Notification Service
        |
        |
 Email / SMS / Push


---

## 3. Kafka hoạt động

Service A:
- xử lý nghiệp vụ
- publish event vào Kafka

Kafka:
- lưu message
- không xử lý business

Service B:
- consume event
- xử lý


Nếu Service B lỗi:
- Kafka giữ message
- Service B restart đọc tiếp
- retry hoặc DLQ


---

## 4. Background Job vs Kafka

Background Job:

App
 |
Queue
 |
Worker


Dùng khi:
- cùng source
- xử lý async


Kafka:

Service A
 |
Kafka
 |
Service B


Dùng khi:
- nhiều service
- event driven
- scale độc lập


---

## 5. Trả lời phỏng vấn

"Em sẽ bắt đầu với notification module trong cùng application và xử lý gửi thông báo bằng background job để tránh block request.

Khi hệ thống lớn hơn, cần nhiều service giao tiếp và scale độc lập thì tách Notification Service và dùng Kafka để publish/consume event.

Kafka giúp giảm coupling, có retry và xử lý bất đồng bộ."
