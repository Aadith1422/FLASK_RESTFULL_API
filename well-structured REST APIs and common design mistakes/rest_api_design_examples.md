
## Well-Structured REST APIs and Common Design Mistakes

### Examples of Well-Structured REST APIs

A well-designed REST API follows clear conventions for naming endpoints, using HTTP methods correctly, and returning meaningful responses. This improves readability, maintainability, and usability for developers interacting with the API.

Below are examples of well-structured REST API endpoints.

#### Example: User Resource API

| HTTP Method | Endpoint | Description |
|-------------|----------|-------------|
| GET | /users | Retrieve a list of all users |
| GET | /users/{id} | Retrieve details of a specific user |
| POST | /users | Create a new user |
| PUT | /users/{id} | Update user information |
| DELETE | /users/{id} | Delete a user |

Example request:

```bash
curl http://127.0.0.1:5000/users
```

Example response:

```json
[
  {
    "id": 1,
    "username": "testuser",
    "email": "test@mail.com"
  }
]
```

#### Example: Authentication API

| HTTP Method | Endpoint | Description |
|-------------|----------|-------------|
| POST | /register | Register a new user |
| POST | /login | Authenticate a user and generate JWT tokens |
| POST | /refresh | Generate a new access token using a refresh token |

Example login request:

```bash
curl -X POST http://127.0.0.1:5000/login \
-H "Content-Type: application/json" \
-d '{"username":"testuser","password":"123456"}'
```

Example response:

```json
{
  "access_token": "JWT_ACCESS_TOKEN",
  "refresh_token": "JWT_REFRESH_TOKEN"
}
```

These examples demonstrate good REST API practices such as clear resource naming, correct HTTP methods, and consistent JSON responses.

---

## Common REST API Design Mistakes

### 1. Poor Endpoint Naming

Bad example:

```
/getUsers
/createUser
/deleteUser
```

Good example:

```
GET /users
POST /users
DELETE /users/{id}
```

REST APIs should use **resource-based naming instead of action-based naming**.

---

### 2. Using Incorrect HTTP Methods

Bad example:

```
GET /deleteUser/1
```

Correct example:

```
DELETE /users/1
```

HTTP methods should match the intended operation.

---

### 3. Not Using Proper HTTP Status Codes

Bad example:

```
200 OK
```

Even when an error occurs.

Correct examples:

| Status Code | Meaning |
|-------------|--------|
| 200 | Successful request |
| 201 | Resource created |
| 400 | Bad request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Resource not found |

Proper status codes improve API clarity and debugging.

---

### 4. Returning Sensitive Data

Bad example response:

```json
{
  "username": "admin",
  "password": "123456"
}
```

Correct approach:

Passwords should **never be returned or stored in plain text**. Instead they should be hashed using secure algorithms such as bcrypt.

---

### 5. Lack of Authentication and Authorization

APIs that expose sensitive endpoints without authentication are vulnerable to attacks.

Bad example:

```
GET /users
```

Accessible without authentication.

Correct approach:

Protected endpoints should require authentication using mechanisms such as **JWT tokens**.

Example:

```
Authorization: Bearer ACCESS_TOKEN
```

---

### Conclusion

A well-structured REST API improves maintainability, usability, and security. By following best practices such as proper endpoint naming, correct HTTP methods, meaningful status codes, and secure authentication mechanisms, developers can design APIs that are reliable and easy to integrate with modern applications.
