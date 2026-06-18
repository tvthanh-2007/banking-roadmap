# Week 2 - Day 7: Java Core for Rails Developer

## Goal

Learn the fundamental Java concepts required for Spring Boot development.

Topics:

* Class
* Object
* Constructor
* Interface

---

## 1. Class

Rails:

```ruby
class Customer
end
```

Java:

```java
public class Customer {
}
```

A class is a blueprint used to create objects.

---

## 2. Object

Rails:

```ruby
customer = Customer.new
```

Java:

```java
Customer customer = new Customer();
```

An object is an instance of a class.

---

## 3. Constructor

Rails:

```ruby
class Customer
  def initialize(name)
    @name = name
  end
end
```

Java:

```java
public class Customer {

    private String name;

    public Customer(String name) {
        this.name = name;
    }
}
```

Create object:

```java
Customer customer = new Customer("Thanh");
```

Characteristics:

* Same name as class
* No return type
* Called automatically when creating an object

---

## Rails vs Java

| Rails                 | Java                  |
| --------------------- | --------------------- |
| initialize            | Constructor           |
| @name = name          | this.name = name      |
| Customer.new("Thanh") | new Customer("Thanh") |

---

## 4. Interface

Interface defines a contract.

Example:

```java
public interface CustomerService {
    Customer findById(Long id);
}
```

Implementation:

```java
public class CustomerServiceImpl implements CustomerService {

    @Override
    public Customer findById(Long id) {
        return new Customer("Thanh");
    }
}
```

---

## Why Interfaces Matter

Spring Boot heavily uses interfaces.

Examples:

```java
CustomerService
AccountService
TransferService
PaymentService
```

Benefits:

* Loose coupling
* Easier testing
* Multiple implementations
* Dependency Injection support

---

## Key Mental Mapping

| Rails Concept  | Java Concept               |
| -------------- | -------------------------- |
| Class          | Class                      |
| Object         | Object                     |
| initialize     | Constructor                |
| Service Object | Interface + Implementation |

---

## Knowledge Check

### Constructor

```java
public Customer(String name) {
    this.name = name;
}
```

Answer:

```text
Constructor
```

### Interface Purpose

```java
public interface CustomerService {
    Customer findById(Long id);
}
```

Answer:

```text
Define a contract that implementing classes must follow.
```

---

## Result

Completed:

* Class
* Object
* Constructor
* Interface

Day 7 Status: COMPLETE

Next:

Week 2 - Day 8
Spring Boot Architecture

Controller
↓
Service
↓
Repository
↓
PostgreSQL
