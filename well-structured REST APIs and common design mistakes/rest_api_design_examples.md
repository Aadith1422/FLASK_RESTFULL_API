# Well-Structured REST APIs and Common Design Mistakes 

## 🔹 What is a Well-Structured REST API?

A well-designed REST API follows standardized conventions that make it:

-    Easy to understand
-    Consistent across endpoints
-    Secure and scalable
-    Developer-friendly

It follows REST principles such as:

-   Statelessness
-   Resource-based architecture
-   Uniform interface
-   Client-server separation

------------------------------------------------------------------------

#  Examples of Well-Structured REST APIs

##  1. User Resource API

A good REST API treats users as resources, not actions.

## Endpoints

| HTTP Method | Endpoint      | Description            |
|-------------|--------------|------------------------|
| GET         | /users       | Fetch all users        |
| GET         | /users/{id}  | Fetch a specific user  |
| POST        | /users       | Create a new user      |
| PUT         | /users/{id}  | Update user (full)     |
| PATCH       | /users/{id}  | Partial update         |
| DELETE      | /users/{id}  | Delete user            |

###  Example Request

``` bash
curl http://127.0.0.1:5000/users
```

###  Example Response

``` json
[
  {
    "id": 1,
    "username": "testuser",
    "email": "test@mail.com"
  }
]
```

###  Best Practices Applied Here

-   Use plural nouns (`/users`)
-   Use path parameters (`{id}`)
-   Use correct HTTP methods
-   Return structured JSON

------------------------------------------------------------------------

##  2. Authentication API (JWT-Based)

Authentication APIs manage identity and access control.

## Endpoints

| HTTP Method | Endpoint   | Description                              |
|-------------|-----------|------------------------------------------|
| POST        | /register | Create account                           |
| POST        | /login    | Authenticate & generate tokens           |
| POST        | /refresh  | Generate new access token                |
| POST        | /logout   | Invalidate token (optional)              |

###  Example Login Request

``` bash
curl -X POST http://127.0.0.1:5000/login \
-H "Content-Type: application/json" \
-d '{"username":"testuser","password":"123456"}'
```

###  Example Response

``` json
{
  "access_token": "JWT_ACCESS_TOKEN",
  "refresh_token": "JWT_REFRESH_TOKEN"
}
```

###  JWT Flow 

1.  User logs in → server verifies credentials\
2.  Server returns:
    -   Access Token (short-lived)
    -   Refresh Token (long-lived)
3.  Client sends access token in headers:

``` http
Authorization: Bearer <access_token>
```

4.  If expired → use refresh token

------------------------------------------------------------------------

#  Common REST API Design Mistakes (Detailed)

##  1. Poor Endpoint Naming

### Bad

    /getUsers
    /createUser
    /deleteUser

### Why it's wrong:

-   Uses verbs instead of resources
-   Not RESTful

### Good

    GET /users
    POST /users
    DELETE /users/{id}

 Rule: Use nouns (resources), not verbs

------------------------------------------------------------------------

##  2. Incorrect HTTP Methods

### Bad

    GET /deleteUser/1

### Why it's wrong:

-   GET should be read-only
-   Violates REST semantics

### Good

    DELETE /users/1

## ✔ HTTP Methods Summary

| Method | Purpose        |
|--------|----------------|
| GET    | Read           |
| POST   | Create         |
| PUT    | Full update    |
| PATCH  | Partial update |
| DELETE | Remove         |

------------------------------------------------------------------------

##  3. Improper Use of HTTP Status Codes

### Bad

    200 OK (even on failure)

### Why it's wrong:

-   Misleading to client
-   Hard to debug

## Good

| Code | Meaning        |
|------|----------------|
| 200  | Success        |
| 201  | Created        |
| 204  | No content     |
| 400  | Bad request    |
| 401  | Unauthorized   |
| 403  | Forbidden      |
| 404  | Not found      |
| 500  | Server error   |

###  Example Error Response

``` json
{
  "error": "User not found"
}
```

------------------------------------------------------------------------

##  4. Returning Sensitive Data

### Bad

``` json
{
  "username": "admin",
  "password": "123456"
}
```

###  Risks

-   Data breach
-   Credential theft

### Good Practices

-   Store passwords using:
    -   bcrypt
    -   argon2
-   Never expose:
    -   Passwords
    -   Tokens
    -   Internal IDs

------------------------------------------------------------------------

##  5. No Authentication / Authorization

### Bad

    GET /users

Accessible by anyone 

### Good

``` http
Authorization: Bearer <token>
```

## Types of Protection

| Type           | Description              |
|----------------|--------------------------|
| Authentication | Who are you?             |
| Authorization  | What can you access?     |

------------------------------------------------------------------------

##  6. No Input Validation

### Problem

``` json
{
  "email": "invalid"
}
```

### Solution

Validate:

-   Email format
-   Password strength
-   Required fields

------------------------------------------------------------------------

##  7. Inconsistent Response Format

### Bad

``` json
"User created"
```

### Good

``` json
{
  "status": "success",
  "message": "User created",
  "data": {
    "id": 1
  }
}
```

------------------------------------------------------------------------

##  8. Ignoring Versioning

### Problem

Changes break existing clients

### Good

    /api/v1/users
    /api/v2/users

------------------------------------------------------------------------

##  9. No Rate Limiting

### Risk

-   DDoS attacks
-   Brute force login

### Solution

-   Limit requests per user/IP

------------------------------------------------------------------------

##  10. Missing Security Headers

## Important Headers

| Header                    | Purpose                      |
|--------------------------|------------------------------|
| Content-Security-Policy  | Prevent XSS                  |
| X-Frame-Options          | Prevent clickjacking         |
| Strict-Transport-Security| Force HTTPS                  |
| X-Content-Type-Options   | Prevent MIME attacks         |

------------------------------------------------------------------------

#  Advanced REST API Best Practices

##  Pagination

    GET /users?page=1&limit=10

##  Filtering

    GET /users?role=admin

##  Sorting

    GET /users?sort=created_at

##  HATEOAS

``` json
{
  "id": 1,
  "links": {
    "self": "/users/1",
    "orders": "/users/1/orders"
  }
}
```

##  Idempotency

-   PUT & DELETE should be idempotent
-   Calling multiple times → same result

------------------------------------------------------------------------

#  Real-World API Design Tips 

-   Always think: **What is the resource?**

-   Keep URLs:

    -   Short
    -   Predictable

-   Use:

        /users/1/orders

    instead of:

        /getUserOrders

-   Separate:

    -   Auth logic
    -   Business logic

-   Log all API activity for monitoring

------------------------------------------------------------------------

#  Conclusion

A well-structured REST API is:

-   ✔ Resource-based
-   ✔ Secure (JWT, hashing, headers)
-   ✔ Consistent (naming, responses)
-   ✔ Scalable (pagination, versioning)

Avoiding common mistakes ensures:

-   Better performance
-   Easier debugging
-   Stronger security
-   Cleaner architecture