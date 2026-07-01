# System Design - Customer Management System

## 1. Bài toán

Thiết kế hệ thống quản lý khách hàng với các chức năng:

- Tạo khách hàng
- Cập nhật thông tin khách hàng
- Tìm kiếm khách hàng
- Xem thông tin khách hàng
- Quản lý lịch sử liên quan

Ví dụ trong banking, entity `Customer` gồm:

- `id`
- `name`
- `phone`
- `email`
- `address`
- `status`
- `created_at`

---

## Level 1: Ít user (10k khách)

Kiến trúc đơn giản:

```
Frontend
   ↓
Spring Boot
   ↓
PostgreSQL
```

**Database** — bảng `customers`:

- `id`
- `name`
- `phone`
- `email`

**API:**

```
POST /customers
GET  /customers/{id}
PUT  /customers/{id}
```

**Mục tiêu:**

- Code đơn giản
- Dễ maintain
- Chưa cần scale phức tạp

---

## Level 2: 1 triệu khách

Bắt đầu có vấn đề về hiệu năng truy vấn.

Ví dụ query:

```sql
SELECT * FROM customers WHERE phone = '090xxxx';
```

Cần thêm index:

```sql
CREATE INDEX idx_customer_phone ON customers(phone);
```

**Lợi ích:**

- Tìm kiếm nhanh hơn
- Giảm full table scan

### Pagination

Không lấy toàn bộ dữ liệu một lúc.

Sai:

```sql
SELECT * FROM customers;
```

Đúng:

```sql
SELECT * FROM customers LIMIT 100 OFFSET 0;
```

API:

```
GET /customers?page=1&size=100
```

### Search nâng cao

Khi dữ liệu lớn:

- **Không nên dùng:** `LIKE '%abc%'`
- **Có thể dùng:**
  - PostgreSQL Full Text Search
  - Elasticsearch

Flow đồng bộ dữ liệu sang search engine:

```
Customer DB
   ↓
Sync data
   ↓
Elasticsearch
   ↓
Search nhanh
```

---

## Level 3: 10-100 triệu khách

### Database Partition

Chia bảng lớn thành nhiều phần:

```
      customers
          |
   ---------------
   |      |      |
   P1     P2     P3
```

Có thể partition theo:

- `customer_id`
- `region`
- Thời gian

**Mục đích:**

- Query nhanh hơn
- Quản lý dữ liệu lớn dễ hơn

### Redis Cache

**Case:** User xem profile nhiều lần.

Flow:

```
Request
   ↓
Redis
   ↓
Có cache?
  ├─ Có   → trả về
  └─ Không → query DB
```

Ví dụ cache key/value:

```
customer:100
{ "id": 100, "name": "A" }
```

---

## Level 4: Hệ thống lớn

Tách thành các service riêng:

```
        API Gateway
             |
   -------------------------
   |          |            |
Customer   Account    Transaction
Service    Service      Service
```

**Customer Service** chịu trách nhiệm:

- Thông tin khách hàng
- Profile
- KYC
- Customer lifecycle

### Database trong microservice

Mỗi service có database riêng:

```
Customer Service → Customer DB
Account Service  → Account DB
```

Không chia sẻ database trực tiếp giữa các service.

---

## Các vấn đề cần quan tâm

### 1. Duplicate data

Ví dụ: Transaction cần `customer_name`.

Cách xử lý:

- Gọi Customer Service để lấy dữ liệu, hoặc
- Lưu snapshot dữ liệu cần thiết

### 2. Đồng bộ dữ liệu

Có thể dùng event-driven:

```
CustomerUpdated
   ↓
Kafka
   ↓
Các service cập nhật dữ liệu liên quan
```

---

## Trả lời phỏng vấn VNPT

> "Với hệ thống quản lý khách hàng, ban đầu em sẽ thiết kế monolith với Spring Boot và PostgreSQL.
>
> Khi dữ liệu tăng, em tối ưu bằng index, pagination, cache Redis.
>
> Nếu hệ thống lớn hơn có thể dùng Elasticsearch cho search, partition database để xử lý dữ liệu lớn.
>
> Khi cần scale độc lập theo nghiệp vụ thì tách Customer Service thành microservice."
