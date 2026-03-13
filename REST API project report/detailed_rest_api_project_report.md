# REST API Project Report

## 1. Introduction

Application Programming Interfaces (APIs) are a fundamental component of
modern software systems. They allow different software applications to
communicate with each other using a standardized set of rules and
protocols. One of the most widely used API architectures today is REST
(Representational State Transfer). REST APIs use standard HTTP methods
such as GET, POST, PUT, and DELETE to perform operations on resources.

In this project, a secure REST API was developed using the Flask
framework in Python. The API allows management of user resources and
demonstrates important backend development concepts such as
authentication, authorization, secure password storage, and API security
best practices.

The API was designed with a focus on security, testing, and proper
documentation. Several common API vulnerabilities were identified and
mitigation techniques were implemented to protect the system from
potential attacks.

------------------------------------------------------------------------

## 2. Project Objectives

The main objectives of this project were:

-   To understand the fundamentals of REST API architecture.
-   To build a RESTful API using the Flask framework.
-   To implement CRUD (Create, Read, Update, Delete) operations for user
    management.
-   To implement secure authentication using JSON Web Tokens (JWT).
-   To implement Role-Based Access Control (RBAC).
-   To perform API testing using curl commands.
-   To identify and mitigate common API vulnerabilities such as
    injection attacks, broken authentication, and XSS.
-   To document the entire API development and testing process.

------------------------------------------------------------------------

## 3. Tools, Frameworks, and Libraries Used

## Tools, Frameworks, and Libraries Used

The following tools and libraries were used to develop and test the project.

| Tool / Library | Description |
|----------------|-------------|
| Python | Primary programming language used to develop the backend API |
| Flask | Lightweight Python web framework used to create the REST API |
| Flask-JWT-Extended | Library used to implement JWT-based authentication |
| bcrypt | Library used for secure password hashing |
| curl | Command-line tool used to test API endpoints |
| VS Code | Code editor used for development |
| Git | Version control system used to manage project code |

These tools were selected because they are widely used in modern backend
development and provide reliable functionality for building secure APIs.

------------------------------------------------------------------------

## 4. Project Architecture

The project follows a modular folder structure to maintain readability, scalability, and separation of concerns. Each component of the application is organized into specific directories responsible for handling different functionalities of the REST API.

### Project Structure

```
flask_rest_api_project/
│
├── app.py
├── config.py
├── requirements.txt
│
├── routes/
│   ├── auth_routes.py
│   └── user_routes.py
│
├── models/
│   └── user_model.py
│
├── utils/
│   └── security.py
│
├── venv/

```

### Module Responsibilities

Each file and directory in the project has a specific role:

- **app.py**  
  The main entry point of the Flask application. It initializes the Flask app, configures JWT authentication, registers blueprints (routes), and starts the server.

- **config.py**  
  Contains configuration settings such as secret keys, JWT configuration, and token expiration settings used by the application.

- **requirements.txt**  
  Lists all the Python dependencies required to run the project. This allows the project environment to be easily recreated.

- **routes/**  
  Contains all API endpoint implementations.

  - **auth_routes.py**  
    Implements authentication-related endpoints such as:
    - User registration (`/register`)
    - User login (`/login`)
    - Token refresh (`/refresh`)

  - **user_routes.py**  
    Implements CRUD operations for user resources:
    - Create user
    - Retrieve users
    - Update user details
    - Delete users (with RBAC restrictions)

- **models/**  
  Contains data models used by the application.

  - **user_model.py**  
    Defines the `User` class and handles password hashing using bcrypt.

- **utils/**  
  Contains helper functions and security utilities that support the API logic.

- **venv/**  
  Python virtual environment containing installed dependencies required to run the project.


## 5. API Endpoints and Functionalities

The REST API provides several endpoints that allow interaction with user
resources.

### 5.1 User Registration

#### Endpoint:

```POST /register```

This endpoint allows new users to register in the system.

#### Example request:
```
curl -X POST http://127.0.0.1:5000/register\
-H "Content-Type: application/json"\
-d '{"username":"testuser","email":"test@mail.com","password":"123456"}'
```
#### Example response:

``` { "message": "User registered successfully" }```

------------------------------------------------------------------------

### 5.2 User Login

#### Endpoint:

``` POST /login```

This endpoint authenticates the user and generates JWT tokens.

#### Example response:

```{ "access_token": "...", "refresh_token": "..." }```

The access token is used to access protected API endpoints.

------------------------------------------------------------------------

### 5.3 Create User

#### Endpoint:

```POST /users```

This endpoint creates a new user resource in the system.

------------------------------------------------------------------------

### 5.4 Retrieve Users

#### Endpoint:

```GET /users```

This endpoint returns the list of all registered users.

------------------------------------------------------------------------

### 5.5 Update User

#### Endpoint:

```PUT /users/`<id>`{=html}```

This endpoint updates the information of an existing user.

------------------------------------------------------------------------

### 5.6 Delete User

#### Endpoint:

``` DELETE /users/`<id>`{=html}```

This endpoint deletes a user resource. Only administrators are allowed
to perform this operation.

------------------------------------------------------------------------

## 6. Authentication and Authorization (JWT Workflow)

Authentication and authorization in this project are implemented using **JSON Web Tokens (JWT)** through the `Flask-JWT-Extended` library. JWT is a compact and secure method for transmitting information between a client and a server as a JSON object. The token is digitally signed, which ensures that it cannot be tampered with during transmission.

In this API, JWT is used to **authenticate users and control access to protected endpoints**. Once a user successfully logs in, the server generates an **Access Token** and a **Refresh Token**. These tokens are then used to maintain secure communication between the client and the server.

### JWT Authentication Workflow

The authentication process follows these steps:

1. **User Registration**

   A new user registers through the `/register` endpoint by providing a username, email, and password.  
   The password is securely hashed using **bcrypt** before being stored.

2. **User Login**

   The user sends login credentials to the `/login` endpoint.

   #### Example request:

   ```bash
   curl -X POST http://127.0.0.1:5000/login \
   -H "Content-Type: application/json" \
   -d '{"username":"testuser","password":"123456"}'
   ```

3. **Credential Verification**

   The server verifies the submitted credentials by comparing the provided password with the stored hashed password using bcrypt.

4. **Token Generation**

   If authentication is successful, the server generates two tokens:

   - **Access Token** – Used to access protected API endpoints.
   - **Refresh Token** – Used to generate a new access token when the current one expires.

   Example response:

   ```json
   {
     "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
     "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
   }
   ```

5. **Using the Access Token**

   The client includes the access token in the **Authorization header** when making requests to protected endpoints.

   Example:

   ```
   Authorization: Bearer ACCESS_TOKEN
   ```

6. **Accessing Protected Routes**

   Protected routes require authentication using the `@jwt_required()` decorator.

   Example implementation:

   ```python
   from flask_jwt_extended import jwt_required

   @user_bp.route("/users", methods=["GET"])
   @jwt_required()
   def get_users():
       return jsonify([user.to_dict() for user in users])
   ```

7. **Token Refresh Mechanism**

   When the access token expires, the client can use the **refresh token** to obtain a new access token without logging in again.

   Example endpoint:

   ```
   POST /refresh
   ```

### Benefits of Using JWT in This Project

- Eliminates the need for server-side session storage.
- Enables secure authentication using signed tokens.
- Allows stateless communication between client and server.
- Improves API scalability and performance.

Through this mechanism, the API ensures that **only authenticated users can access protected resources**, while maintaining secure and efficient communication between the client and server.

### JWT Workflow Diagram

```
User → Login Request → Server
Server → Validate Credentials
Server → Generate Access Token + Refresh Token
Client → Sends Access Token in Authorization Header
Server → Verify Token
Server → Allow Access to Protected Endpoint
```

------------------------------------------------------------------------

## 7. Security Vulnerabilities and Mitigations

Several common API vulnerabilities were tested and mitigation mechanisms
were implemented.

### 7.1 Injection Attacks

Injection attacks occur when malicious input is sent to manipulate application logic.

**Test payload:**

  'OR 1=1 --

**Result:**
The API returned "Invalid credentials".

**Mitigation:**

The API safely parses JSON input using:

data = request.get_json()

No direct query execution is performed.

------------------------------------------------------------------------

### 7.2 Broken Authentication

Broken authentication occurs when APIs fail to properly enforce
authentication.

**Test:**

```curl http://127.0.0.1:5000/users```

**Result:**

{ "msg": "Missing Authorization Header" }

**Mitigation:**

Protected endpoints require JWT authentication.

------------------------------------------------------------------------

### 7.3 Broken Access Control (RBAC)

Broken access control occurs when users access resources without proper
permissions.

**Test:**

A normal user attempted to delete another user.

**Result:**

```{ "error": "Admin access required" }```

**Mitigation:**

Role validation was implemented using:

current_user = get_jwt_identity()

------------------------------------------------------------------------

### 7.4 Cross-Site Scripting (XSS)

XSS attacks attempt to inject malicious scripts into the application.

**Test payload:**

```{=html}
<script>alert(1)</script>
```
**Result:**

The payload is returned as plain text in JSON.

Mitigation:

The API returns JSON responses rather than rendering HTML.

------------------------------------------------------------------------

### 7.5 Sensitive Data Exposure

Sensitive data exposure occurs when confidential information is stored
insecurely.

**Mitigation:**

Passwords are hashed using bcrypt before being stored.

**Example:**

### Password Hashing Implementation

Passwords are securely stored using **bcrypt hashing**.

```python
import bcrypt

class User:
    def __init__(self, id, username, email, password):
        self.id = id
        self.username = username
        self.email = email
        self.password_hash = bcrypt.hashpw(password.encode(), bcrypt.gensalt())


```       

------------------------------------------------------------------------

### 7.6 Security Headers

Security headers were implemented to improve API protection.

**Headers implemented:**

```
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Content-Security-Policy: default-src 'self' 
```

**Verification command:**

```curl -I http://127.0.0.1:5000/```

------------------------------------------------------------------------

## 8. Testing Process

The API endpoints were tested using curl commands in the terminal.

The following tests were performed:

-   User registration test
-   Login and JWT token generation
-   CRUD operations
-   Authorization validation
-   RBAC testing
-   Injection attack testing
-   XSS payload testing
-   Security header verification

Screenshots were recorded for each test case as visual evidence.

------------------------------------------------------------------------

## 9. Summary and Key Learnings

This project provided practical experience in developing secure REST
APIs.

Key learnings include:

-   Understanding REST API architecture
-   Implementing CRUD operations in Flask
-   Implementing secure authentication using JWT
-   Implementing RBAC authorization mechanisms
-   Testing APIs using command-line tools
-   Identifying and mitigating common security vulnerabilities
-   Applying secure coding practices

------------------------------------------------------------------------

## 10. Conclusion

The REST API developed in this project successfully demonstrates the
implementation of a secure backend system using Flask. The API supports
user management with authentication and authorization features while
protecting against common vulnerabilities such as injection attacks,
XSS, and broken authentication.

The project highlights best practices in secure API design, development,
testing, and documentation.
