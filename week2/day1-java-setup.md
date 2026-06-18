# Week 2 - Day 6: Java & Spring Boot Environment Setup

## Goal

Set up a complete Java/Spring Boot development environment for the Banking Lab project.

---

## Installed Components

### JDK 21

Verify installation:

```bash
java -version
javac -version
```

Purpose:

* Java runtime
* Java compiler

---

### Maven

Verify installation:

```bash
mvn -version
```

Purpose:

* Dependency management
* Project build tool
* Package management

Rails equivalent:

| Rails          | Java        |
| -------------- | ----------- |
| Gemfile        | pom.xml     |
| bundle install | mvn install |

---

### IDE

Recommended:

* IntelliJ IDEA Community

Reason:

* Best support for Spring Boot
* Better navigation and refactoring tools
* Industry standard for Java development

---

## Create Spring Boot Project

Spring Initializr configuration:

```text
Project: Maven
Language: Java
Group: com.banking
Artifact: banking-lab
Packaging: Jar
Java: 21
```

Dependencies:

```text
Spring Web
Spring Data JPA
PostgreSQL Driver
Validation
Lombok
```

---

## PostgreSQL with Docker

docker-compose.yml

```yaml
services:
  postgres:
    image: postgres:17
    container_name: banking-postgres
    environment:
      POSTGRES_DB: banking_lab
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: bankinglab
    ports:
      - "15570:5432"
```

Start database:

```bash
docker compose up -d
```

---

## Spring Boot Database Configuration

application.properties

```properties
spring.datasource.url=jdbc:postgresql://localhost:15570/banking_lab
spring.datasource.username=postgres
spring.datasource.password=bankinglab

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

---

## Troubleshooting Learned

### Issue

```text
FATAL: password authentication failed for user "postgres"
```

Cause:

Docker container was created previously with a different password.

Changing:

```yaml
POSTGRES_PASSWORD=new_password
```

does not automatically update PostgreSQL credentials stored inside the volume.

---

### Fix

Reset container and volume:

```bash
docker compose down -v
docker compose up -d
```

Important:

```text
Config change ≠ Database change
```

PostgreSQL reads environment variables only during initial database creation.

---

## Run Application

```bash
mvn spring-boot:run
```

Expected result:

```text
Started BankingLabApplication
```

---

## Result

Completed:

* JDK 21 setup
* Maven setup
* Spring Boot setup
* PostgreSQL Docker setup
* JPA configuration
* Successful application startup

Day 6 Status: COMPLETE

