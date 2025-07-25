# Secure Notes Application

This is a secure, full-stack notes application built with Spring Boot. It features user authentication, a robust security model, and a clean architecture for managing user notes.

## Table of Contents
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Running the Application](#running-the-application)
- [Configuration](#configuration)
- [Database Schema](#database-schema)
- [API Endpoints](#api-endpoints)
- [Security Features](#security-features)

## Features
- **User Authentication**: Secure user registration, login, and password reset.
- **JWT-based Authentication**: Implements JSON Web Tokens for stateless authentication.
- **Role-Based Access Control (RBAC)**: Supports different user roles (ROLE_USER, ROLE_ADMIN).
- **Notes Management**: Users can create, read, update, and delete their own notes.
- **Password Management**: Secure password hashing (BCrypt) and a password reset mechanism.
- **Audit Logging**: Tracks user actions for security and compliance.
- **TOTP (Two-Factor Authentication)**: Supports Time-Based One-Time Password for enhanced security.
- **OAuth2 Integration**: Handles successful OAuth2 logins (e.g., Google, GitHub).
- **CORS Configuration**: Properly configured Cross-Origin Resource Sharing for frontend integration.
- **Admin Management**: Endpoints for managing users, roles, and account statuses.

## Technologies Used
**Backend**:
- Java 21
- Spring Boot 3.5.3
- Spring Data JPA, Spring Web, Spring Security, Spring Mail
- Spring Boot Starter Validation, OAuth2 Client

**Security**:
- JWT (Json Web Tokens) - `io.jsonwebtoken:jjwt-*` (v0.12.6)
- BCrypt for password encoding
- Google Authenticator (TOTP) - `com.warrenstrange:googleauth:1.4.0`

**Database**: MySQL (`com.mysql:mysql-connector-j`)

**Build Tool**: Maven

**Utilities**: Lombok, Java Dotenv (`io.github.cdimascio:java-dotenv:5.2.2`)

## Project Structure
```text
notes/
├── src/
│   └── main/
│       ├── java/
│       │   └── com/secure/notes/
│       │       ├── config/
│       │       ├── controllers/
│       │       ├── dtos/
│       │       ├── models/
│       │       ├── repositories/
│       │       ├── security/
│       │       ├── services/
│       │       ├── services/impl/
│       │       └── util/
│       └── resources/
│           └── application.properties
└── pom.xml
```

Descriptions of directories and responsibilities follow in detailed sections.

## Prerequisites
- Java JDK 21+
- Maven 3.6+
- MySQL Database instance
- IDE (Optional): IntelliJ IDEA, Eclipse, or VS Code

## Installation
```bash
git clone https://github.com/your-username/secure-notes.git
cd secure-notes
mvn clean install
```

## Running the Application
```bash
mvn spring-boot:run
```
Default URL: `http://localhost:8080`

## Configuration
Edit the `application.properties` (or `.yml`) in `src/main/resources`:

### MySQL Configuration
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/notes_db?useSSL=false&serverTimezone=UTC
spring.datasource.username=your_mysql_username
spring.datasource.password=your_mysql_password
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
```

### JWT Configuration
```properties
notes.app.jwtSecret=YourSuperSecretKeyThatIsAtLeast256BitsLongAndSecure
notes.app.jwtExpirationMs=86400000
```

### Email Service
```properties
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=your_email@gmail.com
spring.mail.password=your_app_password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

### Frontend URL (CORS)
```properties
frontend.url={Hosted URL}
```

### OAuth2 (Google Example)
```properties
spring.security.oauth2.client.registration.google.client-id=YOUR_GOOGLE_CLIENT_ID
spring.security.oauth2.client.registration.google.client-secret=YOUR_GOOGLE_CLIENT_SECRET
spring.security.oauth2.client.registration.google.redirect-uri={baseUrl}/oauth2/callback/{registrationId}
spring.security.oauth2.client.registration.google.scope=openid,profile,email
```

## Database Schema
**Entities**:
- `User`
- `Role`
- `Note`
- `AuditLog`
- `PasswordResetToken`

Roles initialized at startup: `ROLE_USER`, `ROLE_ADMIN`. Default users: `user1/password1`, `admin/adminPass`.

## API Endpoints
Refer to detailed `README` above. Grouped into:
- `/api/auth` (Authentication & 2FA)
- `/api/notes` (User notes)
- `/api/admin` (Admin tasks)
- `/api/audit` (Audit logs)
- `/api/csrf-token` (CSRF)
- `/oauth2/*` (OAuth2 flow)

## Security Features
- JWT-based stateless auth
- BCrypt password hashing
- CSRF protection
- Role-based access control
- Method-level security
- Custom JWT and OAuth2 filters


