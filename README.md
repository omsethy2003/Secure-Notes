Notes Application
This is a secure Notes application designed to manage user notes with robust authentication, authorization, and auditing features. It incorporates JWT for security, OAuth2 for social logins, and offers functionalities like user management, note creation, and audit logging.

Table of Contents
Features

Technologies Used

Project Structure

Getting Started

Prerequisites

Installation

Running the Application

API Endpoints

Security Features

Contributing

License

Features
User Authentication & Authorization: Secure user login/signup using JWT (JSON Web Tokens) and Spring Security. Implements role-based access control (RBAC) with ROLE_USER and ROLE_ADMIN.

OAuth2 Integration: Supports social login (e.g., Google, GitHub) for seamless user experience, with a custom success handler for post-login processing.

Note Management: Create, view, update, and delete personal notes.

User Management: Admin functionalities for managing users and roles, accessible only to ROLE_ADMIN.

Audit Logging: Tracks significant user actions and system events for security and compliance.

Password Reset: Secure mechanism for users to reset forgotten passwords.

Email Service Integration: For features like password resets or user notifications.

TOTP (Time-based One-Time Password): Enhances authentication security (if fully implemented).

CORS Configuration: Properly configured Cross-Origin Resource Sharing to allow frontend applications from specified origins.

Technologies Used
Backend: Java, Spring Boot

Security: Spring Security, JWT (using io.jsonwebtoken), OAuth2

Database: (Assumed, e.g., H2, PostgreSQL, MySQL)

Build Tool: Maven

Password Hashing: BCryptPasswordEncoder

Project Structure
The project follows a standard Spring Boot application structure, separating concerns into distinct packages for better organization and maintainability.

.
├── src
│   └── main
│       └── java
│           └── com
│               └── secure
│                   └── notes
│                       ├── config               // Application configuration (e.g., security, OAuth2)
│                       │   ├── EnvLoader.java
│                       │   ├── OAuth2LoginSuccessHandler.java
│                       │   └── WebConfig.java
│                       ├── controllers          // REST API endpoints
│                       │   ├── AdminController.java
│                       │   ├── AuditLogController.java
│                       │   ├── AuthController.java
│                       │   ├── CsrfController.java
│                       │   └── NoteController.java
│                       ├── dtos                   // Data Transfer Objects for API requests/responses
│                       │   └── UserDTO.java
│                       ├── models               // JPA entities representing database tables
│                       │   ├── AppRole.java
│                       │   ├── AuditLog.java
│                       │   ├── Note.java
│                       │   ├── PasswordResetToken.java
│                       │   ├── Role.java
│                       │   └── User.java
│                       ├── repositories         // Spring Data JPA repositories for data access
│                       │   ├── AuditLogRepository.java
│                       │   ├── NoteRepository.java
│                       │   ├── PasswordResetTokenRepository.java
│                       │   ├── RoleRepository.java
│                       │   └── UserRepository.java
│                       ├── request              // DTOs for incoming API requests
│                       │   ├── LoginRequest.java
│                       │   └── SignupRequest.java
│                       ├── response             // DTOs for outgoing API responses
│                       │   ├── LoginResponse.java
│                       │   ├── MessageResponse.java
│                       │   └── UserInfoResponse.java
│                       ├── security             // Security-related components (JWT, filters)
│                       │   ├── AuthEntryPointJwt.java
│                       │   ├── AuthTokenFilter.java
│                       │   ├── JwtUtils.java
│                       │   └── SecurityConfig.java // Main security configuration
│                       │   └── WebConfig.java      // CORS configuration
│                       ├── services             // Business logic and service interfaces
│                       │   ├── UserDetailsImpl.java
│                       │   ├── UserDetailsServiceImpl.java
│                       │   └── impl             // Implementations of service interfaces
│                       │       ├── AuditLogService.java
│                       │       ├── NoteService.java
│                       │       ├── TotpService.java
│                       │       └── UserService.java
│                       └── util                 // Utility classes (e.g., email, authentication helpers)
│                           ├── AuthUtil.java
│                           └── EmailService.java
│                       └── NotesApplication.java  // Main Spring Boot application class
└── pom.xml                    // Maven project configuration


Getting Started
Follow these instructions to set up and run the project on your local machine.

Prerequisites
Java Development Kit (JDK) 17 or higher

Maven 3.8.x or higher

(Optional) Your preferred IDE (e.g., IntelliJ IDEA, Eclipse)

(Optional) Database server (e.g., PostgreSQL, MySQL) if not using an in-memory database like H2.

Installation
Clone the repository:

git clone https://github.com/your-username/notes.git
cd notes

Configure your database (if applicable):
Edit src/main/resources/application.properties (or application.yml) to configure your database connection.

spring.datasource.url=jdbc:your_database_url
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.jpa.hibernate.ddl-auto=update # or create, none

Configure application properties:
Create or modify src/main/resources/application.properties (or application.yml) to include:

Frontend URL for CORS:

frontend.url=http://localhost:3000 # Replace with your frontend URL

JWT Secret and Expiration:

spring.app.jwtSecret=YourSuperSecretJWTKeyThatIsAtLeast256BitsLong # Replace with a strong, unique secret
spring.app.jwtExpirationMs=86400000 # 24 hours in milliseconds

OAuth2 Client Credentials:
If using OAuth2, you'll need to configure client IDs and secrets for providers like Google, GitHub, etc., in your application.properties or as environment variables. (e.g., spring.security.oauth2.client.registration.google.client-id=...)

Running the Application
Build the project using Maven:

mvn clean install

Run the Spring Boot application:

mvn spring-boot:run

Alternatively, you can run the NotesApplication.java file directly from your IDE.

The application will typically start on http://localhost:8080 (or another port if configured).

Initial Users (for testing):
Upon first run, the CommandLineRunner in SecurityConfig.java will create two default users if they don't already exist:

Username: user1

Password: password1

Role: ROLE_USER

Username: admin

Password: adminPass

Role: ROLE_ADMIN

API Endpoints
This section details the REST API endpoints available in the application.

Authentication Endpoints (AuthController - assumed, not provided but implied by SecurityConfig)
POST /api/auth/signup

Description: Registers a new user.

Request Body: User registration details (e.g., username, email, password).

Access: Publicly accessible.

POST /api/auth/login

Description: Authenticates a user and returns a JWT for subsequent requests.

Request Body: User login credentials (username/email, password).

Access: Publicly accessible.

GET /oauth2/authorization/{provider}

Description: Initiates the OAuth2 login flow for a specified provider (e.g., google, github).

Access: Publicly accessible.

CSRF Token Endpoint (CsrfController)
GET /api/csrf-token

Description: Retrieves the CSRF token for frontend applications that require it for secure form submissions.

Access: Publicly accessible.

Note Management Endpoints (NoteController)
All Note Management endpoints require authenticated access.

POST /api/notes

Description: Creates a new note for the authenticated user.

Request Body: Raw string content of the note.

Access: Authenticated users.

GET /api/notes

Description: Retrieves all notes belonging to the authenticated user.

Access: Authenticated users.

PUT /api/notes/{noteId}

Description: Updates the content of a specific note by its ID for the authenticated user.

Path Variable: noteId (ID of the note to update).

Request Body: Raw string content for the updated note.

Access: Authenticated users (only for their own notes).

DELETE /api/notes/{noteId}

Description: Deletes a specific note by its ID for the authenticated user.

Path Variable: noteId (ID of the note to delete).

Access: Authenticated users (only for their own notes).

Admin Endpoints (AdminController)
All Admin endpoints require users with the ROLE_ADMIN role.

GET /api/admin/getusers

Description: Retrieves a list of all registered users.

Access: ROLE_ADMIN.

PUT /api/admin/update-role

Description: Updates the role of a specified user.

Request Params: userId (ID of the user), roleName (new role name, e.g., "ROLE_USER", "ROLE_ADMIN").

Access: ROLE_ADMIN.

GET /api/admin/user/{id}

Description: Retrieves details of a specific user by their ID.

Path Variable: id (ID of the user).

Access: ROLE_ADMIN.

PUT /api/admin/update-lock-status

Description: Updates the account lock status of a user.

Request Params: userId (ID of the user), lock (boolean: true to lock, false to unlock).

Access: ROLE_ADMIN.

GET /api/admin/roles

Description: Retrieves a list of all available roles in the system.

Access: ROLE_ADMIN.

PUT /api/admin/update-expiry-status

Description: Updates the account expiry status of a user.

Request Params: userId (ID of the user), expire (boolean: true to expire, false to unexpire).

Access: ROLE_ADMIN.

PUT /api/admin/update-enabled-status

Description: Updates the enabled status of a user's account.

Request Params: userId (ID of the user), enabled (boolean: true to enable, false to disable).

Access: ROLE_ADMIN.

PUT /api/admin/update-credentials-expiry-status

Description: Updates the credentials expiry status of a user.

Request Params: userId (ID of the user), expire (boolean: true to expire, false to unexpire).

Access: ROLE_ADMIN.

PUT /api/admin/update-password

Description: Updates the password for a specified user.

Request Params: userId (ID of the user), password (new password).

Access: ROLE_ADMIN.

Audit Log Endpoints (AuditLogController)
All Audit Log endpoints require users with the ROLE_ADMIN role.

GET /api/audit

Description: Retrieves a list of all audit logs.

Access: ROLE_ADMIN.

GET /api/audit/note/{id}

Description: Retrieves audit logs specifically related to a particular note.

Path Variable: id (ID of the note).

Access: ROLE_ADMIN.

Security Features
JWT (JSON Web Token) Authentication:

JwtUtils: Utility for generating, parsing, and validating JWTs.

AuthTokenFilter: Intercepts requests, extracts JWT from Authorization header, validates it, and sets user authentication in Spring Security Context.

Spring Security: Comprehensive security framework handling authentication, authorization, and various security concerns.

OAuth2 Client: Configured for external identity providers, with OAuth2LoginSuccessHandler to manage post-login redirects and user provisioning.

CSRF Protection: Enabled by default with CookieCsrfTokenRepository.withHttpOnlyFalse() and configured to ignore public authentication endpoints (/api/auth/public/**).

Role-Based Access Control (RBAC):

@EnableMethodSecurity: Enables method-level security annotations (@PreAuthorize, @Secured, @RolesAllowed).

SecurityConfig: Defines URL-based authorization rules (e.g., /api/admin/** requires ROLE_ADMIN).

Authentication Entry Point: AuthEntryPointJwt handles unauthorized access attempts, returning a 401 Unauthorized JSON response.

Password Hashing: Passwords are securely stored using BCryptPasswordEncoder.

Password Reset Mechanism: Token-based password reset (managed by PasswordResetToken and related services).

TOTP Support: TotpService indicates potential for Time-based One-Time Password (2FA) implementation.

CORS Configuration: WebConfig sets up CORS rules, allowing specific origins, methods, and headers, crucial for frontend integration.

Contributing
Contributions are welcome! Please follow these steps:

Fork the repository.

Create a new branch (git checkout -b feature/your-feature-name).

Make your changes.

Commit your changes (git commit -m 'Add new feature').

Push to the branch (git push origin feature/your-feature-name).

Open a Pull Request.

License
This project is licensed under the MIT License - see the LICENSE file for details.




