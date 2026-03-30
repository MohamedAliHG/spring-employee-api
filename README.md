# 👥 spring-employee-api

A **secured RESTful API** for employee/personnel management, built with **Spring Boot 3** and **Java 17**. Features JWT-based authentication via **OAuth2 Resource Server**, fine-grained scope-based authorization with **Spring Security**, and data persistence using **Spring Data JPA** with a **MySQL** backend.


---

## 📋 Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Database Setup](#database-setup)
- [Running the Application](#running-the-application)
- [Security & Authentication](#security--authentication)
- [API Endpoints](#api-endpoints)
- [Contributing](#contributing)

---

## Overview

**spring-employee-api** is a secure backend REST API for managing employee records. It exposes endpoints to create, read, update, and delete employees, with rich query capabilities (filter by name, position, salary range). All endpoints are protected by JWT scopes — each operation requires a specific OAuth2 scope in the token.

---

## Tech Stack

| Component | Technology |
|---|---|
| Language | Java 17 |
| Framework | Spring Boot 3.4.4 |
| ORM | Spring Data JPA (Hibernate) |
| Database | MySQL |
| Security | Spring Security + OAuth2 Resource Server (JWT) |
| Build Tool | Maven (Maven Wrapper included) |
| Testing | Spring Boot Test + Spring Security Test |

---

## Project Structure

```
spring-employee-api/
│
├── src/
│   ├── main/
│   │   ├── java/gl2/example/personnel/
│   │   │   ├── controller/
│   │   │   │   └── EmployeeController.java   # REST endpoints
│   │   │   ├── model/
│   │   │   │   └── Employee.java             # Employee entity
│   │   │   └── service/
│   │   │       └── EmployeeService.java      # Business logic
│   │   └── resources/
│   │       └── application.properties        # App & security configuration
│   └── test/                                 # Unit & integration tests
│
├── .mvn/wrapper/          # Maven wrapper config
├── mvnw / mvnw.cmd        # Maven wrapper scripts
├── pom.xml                # Project dependencies
└── .gitignore
```

---

## Getting Started

### Prerequisites

- [Java 17+](https://adoptium.net/)
- [Maven 3.8+](https://maven.apache.org/) *(or use the included `mvnw` wrapper)*
- [MySQL 8+](https://www.mysql.com/)

### Clone

```bash
git clone https://github.com/MohamedAliHG/spring-employee-api.git
cd spring-employee-api
```

---

## Configuration

Edit `src/main/resources/application.properties`:

```properties
# Database
spring.datasource.url=jdbc:mysql://localhost:3306/personnel_db
spring.datasource.username=your_username
spring.datasource.password=your_password

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

# JWT / OAuth2 Resource Server
spring.security.oauth2.resourceserver.jwt.issuer-uri=https://your-auth-server.com
```

> **Tip:** Use environment variables or Spring profiles to avoid committing credentials to source control.

---

## Database Setup

Create the database in MySQL before running the app:

```sql
CREATE DATABASE personnel_db;
```

Hibernate will automatically create/update the schema on first run.

---

## Running the Application

```bash
# Linux / macOS
./mvnw spring-boot:run

# Windows
mvnw.cmd spring-boot:run
```

The API will be available at `http://localhost:8080`.

---

## Security & Authentication

All endpoints require a valid **JWT Bearer token** in the `Authorization` header. Access is controlled by **OAuth2 scopes** — each operation requires a specific scope:

| Scope | Grants Access To |
|---|---|
| `SCOPE_readpersonnel` | All `GET` endpoints |
| `SCOPE_addpersonnel` | `POST /api/employees` |
| `SCOPE_updatepersonnel` | `PUT /api/employees/{id}` |
| `SCOPE_deletepersonnel` | `DELETE /api/employees/{id}` |

Example request with a token:

```http
GET /api/employees HTTP/1.1
Authorization: Bearer <your_jwt_token>
```

---

## API Endpoints

Base path: `/api/employees`

### Employee Queries — requires `SCOPE_readpersonnel`

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/employees` | Get all employees |
| `GET` | `/api/employees/id/{id}` | Get employee by ID |
| `GET` | `/api/employees/name/{name}` | Get employees by name |
| `GET` | `/api/employees/position/{position}` | Get employees by position |
| `GET` | `/api/employees/salary/{salary}` | Get employees with exact salary |
| `GET` | `/api/employees/salarybetween/{salary1}/{salary2}` | Get employees with salary in range |
| `GET` | `/api/employees/salarygreaterthan/{salary}` | Get employees with salary greater than value |
| `GET` | `/api/employees/salarylessthan/{salary}` | Get employees with salary less than value |

### Employee Mutations

| Method | Endpoint | Required Scope | Description |
|---|---|---|---|
| `POST` | `/api/employees` | `SCOPE_addpersonnel` | Create a new employee |
| `PUT` | `/api/employees/{id}` | `SCOPE_updatepersonnel` | Update an existing employee |
| `DELETE` | `/api/employees/{id}` | `SCOPE_deletepersonnel` | Delete an employee |

### Example Request Body (POST / PUT)

```json
{
  "name": "John Doe",
  "position": "Software Engineer",
  "salary": 3500.00
}
```

### Example Responses

```json
// GET /api/employees/id/1 → 200 OK
{
  "id": 1,
  "name": "John Doe",
  "position": "Software Engineer",
  "salary": 3500.00
}

// GET /api/employees/id/99 → 404 Not Found
// DELETE /api/employees/1 → 204 No Content
```

---

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push: `git push origin feature/your-feature`
5. Open a Pull Request

---

## Author

**Mohamed Ali HG** — [@MohamedAliHG](https://github.com/MohamedAliHG)

---

*Built with ☕ using Spring Boot 3, Spring Security, and OAuth2/JWT.*
