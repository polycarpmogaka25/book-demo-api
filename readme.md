# Books API Demo

## Overview

`books-api-demo` is a **Spring Boot 3+ REST API** that demonstrates the usage of the reusable **`p11-masking-spring-boot-starter`**.  
It provides CRUD operations for `Book` entities while masking sensitive fields in logs (`email` and `phoneNumber`) without modifying database values.

---

## Architecture


**Flow:**

1. Client sends requests to `BookController`.
2. `BookService` handles business logic and interacts with H2 database via `BookRepository`.
3. DTOs are serialized to JSON using **MaskingSerializer** from the masking starter.
4. Logs show **masked sensitive fields**; database remains unmasked.

---

## Dependencies

- Spring Boot 3+
- Spring Data JPA
- H2 in-memory database
- `p11-masking-spring-boot-starter` (local module dependency)

---

## Configuration Example

`src/main/resources/application.yaml`:

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:h2:mem:testdb
    driverClassName: org.h2.Driver
    username: sa
    password: 
    platform: h2
  h2:
    console:
      enabled: true
      path: /h2-console
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true

p11:
  masking:
    enabled: true
    fields:
      - email
      - phoneNumber
    mask-style: PARTIAL
    mask-character: "*"
