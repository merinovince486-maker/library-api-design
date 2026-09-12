# Library Management System API Design

## 1. Overview

The Library Management System API is a RESTful API designed to manage library members, books, loans, and book categories.

The API allows users to register and manage library members, manage books, borrow and return books, track active and overdue loans, and organize books into categories.

The API follows REST principles by using resource-oriented URLs, HTTP methods, appropriate status codes, JSON representations, and stateless communication.

---

## 2. Base URL

### Development

```text
http://localhost:3000/api
```

### Production

```text
https://api.library.com/v1
```

---

# 3. Resources

The main resources in the system are:

1. Members
2. Books
3. Loans
4. Categories

---

## 3.1 Members Resource

Properties:

```text
id              integer, auto-generated, unique
first_name      string, required
last_name       string, required
email           string, unique, required
phone           string
address         string
membership_date date
created_at      timestamp
updated_at      timestamp
```

Example:

```json
{
  "id": 1,
  "first_name": "Juan",
  "last_name": "Dela Cruz",
  "email": "juan@example.com",
  "phone": "09123456789",
  "address": "Cebu City, Philippines",
  "membership_date": "2026-09-11",
  "created_at": "2026-09-11T08:00:00Z",
  "updated_at": "2026-09-11T08:00:00Z"
}
```

---

## 3.2 Books Resource

Properties:

```text
id              integer, auto-generated, unique
title           string, required
author          string, required
isbn            string, unique, required
published_year  integer
category_id     integer, foreign key
total_copies    integer
available_copies integer
created_at      timestamp
updated_at      timestamp
```

Example:

```json
{
  "id": 1,
  "title": "Clean Code",
  "author": "Robert C. Martin",
  "isbn": "978-0132350884",
  "published_year": 2008,
  "category_id": 3,
  "total_copies": 5,
  "available_copies": 3,
  "created_at": "2026-09-11T08:00:00Z",
  "updated_at": "2026-09-11T08:00:00Z"
}
```

---

## 3.3 Loans Resource

Properties:

```text
id              integer, auto-generated, unique
member_id       integer, foreign key
book_id         integer, foreign key
borrowed_at     timestamp
due_date        date
returned_at     timestamp, nullable
status          string
created_at      timestamp
updated_at      timestamp
```

Possible status values:

```text
active
returned
overdue
```

Example:

```json
{
  "id": 1,
  "member_id": 1,
  "book_id": 5,
  "borrowed_at": "2026-09-11T08:30:00Z",
  "due_date": "2026-09-18",
  "returned_at": null,
  "status": "active",
  "created_at": "2026-09-11T08:30:00Z",
  "updated_at": "2026-09-11T08:30:00Z"
}
```

---

## 3.4 Categories Resource

Properties:

```text
id          integer, auto-generated, unique
name        string, unique, required
description string
created_at  timestamp
updated_at  timestamp
```

Example:

```json
{
  "id": 3,
  "name": "Programming",
  "description": "Books about programming and software development.",
  "created_at": "2026-09-11T08:00:00Z",
  "updated_at": "2026-09-11T08:00:00Z"
}
```

---

# 4. Endpoints

## 4.1 Members Endpoints

| Method | Endpoint           | Description                |
| ------ | ------------------ | -------------------------- |
| GET    | `/api/members`     | Get all members            |
| GET    | `/api/members/:id` | Get one member             |
| POST   | `/api/members`     | Create a member            |
| PUT    | `/api/members/:id` | Replace member information |
| PATCH  | `/api/members/:id` | Partially update member    |
| DELETE | `/api/members/:id` | Delete member              |

---

## 4.2 Books Endpoints

| Method | Endpoint         | Description              |
| ------ | ---------------- | ------------------------ |
| GET    | `/api/books`     | Get all books            |
| GET    | `/api/books/:id` | Get one book             |
| POST   | `/api/books`     | Create a book            |
| PUT    | `/api/books/:id` | Replace book information |
| PATCH  | `/api/books/:id` | Partially update book    |
| DELETE | `/api/books/:id` | Delete book              |

---

## 4.3 Loans Endpoints

| Method | Endpoint         | Description              |
| ------ | ---------------- | ------------------------ |
| GET    | `/api/loans`     | Get all loans            |
| GET    | `/api/loans/:id` | Get one loan             |
| POST   | `/api/loans`     | Borrow a book            |
| PUT    | `/api/loans/:id` | Replace loan information |
| PATCH  | `/api/loans/:id` | Update loan information  |
| DELETE | `/api/loans/:id` | Delete loan              |

---

## 4.4 Categories Endpoints

| Method | Endpoint              | Description                  |
| ------ | --------------------- | ---------------------------- |
| GET    | `/api/categories`     | Get all categories           |
| GET    | `/api/categories/:id` | Get one category             |
| POST   | `/api/categories`     | Create a category            |
| PUT    | `/api/categories/:id` | Replace category information |
| PATCH  | `/api/categories/:id` | Partially update category    |
| DELETE | `/api/categories/:id` | Delete category              |

---

# 5. Nested Resources and Relationships

## Relationship 1: Members have many Loans

```http
GET /api/members/:id/loans
```

Description:

Gets all loans belonging to a specific member.

```http
POST /api/members/:id/loans
```

Description:

Creates a new loan for a specific member.

---

## Relationship 2: Categories have many Books

```http
GET /api/categories/:id/books
```

Description:

Gets all books belonging to a specific category.

---

## Relationship 3: Books have many Loans

```http
GET /api/books/:id/loans
```

Description:

Gets all borrowing records for a specific book.

---

# 6. Request Body Examples

## 6.1 POST /api/members

Request:

```json
{
  "first_name": "Juan",
  "last_name": "Dela Cruz",
  "email": "juan@example.com",
  "phone": "09123456789",
  "address": "Cebu City, Philippines"
}
```

---

## 6.2 POST /api/loans

Request:

```json
{
  "member_id": 1,
  "book_id": 5,
  "due_date": "2026-09-18"
}
```

---

## 6.3 PATCH /api/books/:id

Request:

```json
{
  "available_copies": 2
}
```

---

# 7. Response Body Examples

## 7.1 GET /api/members/:id

### Success — 200 OK

```json
{
  "id": 1,
  "first_name": "Juan",
  "last_name": "Dela Cruz",
  "email": "juan@example.com",
  "phone": "09123456789",
  "address": "Cebu City, Philippines",
  "membership_date": "2026-09-11",
  "created_at": "2026-09-11T08:00:00Z",
  "updated_at": "2026-09-11T08:00:00Z"
}
```

### Error — 404 Not Found

```json
{
  "error": {
    "code": "MEMBER_NOT_FOUND",
    "message": "Member with ID 1 does not exist"
  }
}
```

---

## 7.2 POST /api/loans

### Success — 201 Created

```json
{
  "id": 10,
  "member_id": 1,
  "book_id": 5,
  "borrowed_at": "2026-09-11T08:30:00Z",
  "due_date": "2026-09-18",
  "returned_at": null,
  "status": "active"
}
```

### Error — 409 Conflict

```json
{
  "error": {
    "code": "BOOK_NOT_AVAILABLE",
    "message": "This book has no available copies for borrowing"
  }
}
```

---

## 7.3 DELETE /api/books/:id

### Success — 204 No Content

The book is successfully deleted and the response contains no body.

---

# 8. Status Code Mapping

## 8.1 GET /api/books/:id

```text
Success:      200 OK
Not Found:    404 Not Found
Server Error: 500 Internal Server Error
```

Example:

```json
{
  "error": {
    "code": "BOOK_NOT_FOUND",
    "message": "Book with the specified ID does not exist"
  }
}
```

---

## 8.2 POST /api/members

```text
Created:      201 Created
Bad Request:  400 Bad Request
Conflict:     409 Conflict
Server Error: 500 Internal Server Error
```

Examples:

```text
400 - Required member information is missing
409 - A member with this email already exists
500 - Database connection failed
```

---

## 8.3 POST /api/loans

```text
Created:      201 Created
Bad Request:  400 Bad Request
Not Found:    404 Not Found
Conflict:     409 Conflict
Server Error: 500 Internal Server Error
```

Examples:

```text
400 - Invalid loan information
404 - Member or book does not exist
409 - Book has no available copies
500 - Database connection failed
```

---

## 8.4 DELETE /api/books/:id

```text
Success:      204 No Content
Not Found:    404 Not Found
Conflict:     409 Conflict
Server Error: 500 Internal Server Error
```

---

# 9. Special Scenarios

## Scenario 1: Borrowing an unavailable book

```text
Status Code: 409
Error Code: "BOOK_NOT_AVAILABLE"
Error Message: "This book has no available copies for borrowing."
```

---

## Scenario 2: Returning an already-returned loan

```text
Status Code: 409
Error Code: "LOAN_ALREADY_RETURNED"
Error Message: "This loan has already been returned."
```

---

## Scenario 3: Deleting a book with active loans

```text
Status Code: 409
Error Code: "BOOK_HAS_ACTIVE_LOANS"
Error Message: "The book cannot be deleted because it has active loans."
```

---

## Scenario 4: Creating a member with duplicate email

```text
Status Code: 409
Error Code: "EMAIL_ALREADY_EXISTS"
Error Message: "A member with this email address already exists."
```

---

# 10. Query Parameters

## 10.1 GET /api/books

Search and filtering:

```text
?search=programming
```

Search by title, author, or ISBN.

```text
?category_id=3
```

Filter books by category.

```text
?sort=published_year
```

Sort by publication year.

```text
?order=desc
```

Sort from newest to oldest.

```text
?page=1&limit=10
```

Pagination.

### Complete Example

```http
GET /api/books?search=programming&category_id=3&sort=published_year&order=desc&page=1&limit=10
```

---

## 10.2 GET /api/loans

Filter loans:

```text
?status=active
```

Get active loans.

```text
?status=returned
```

Get returned loans.

```text
?status=overdue
```

Get overdue loans.

```text
?member_id=1
```

Get loans belonging to a specific member.

### Complete Example

```http
GET /api/loans?status=active&member_id=1
```

---

# 11. API Versioning

Current Version:

```text
v1
```

New Version:

```text
v2
```

Breaking Change:

```text
The books response will replace available_copies
with total_copies and borrowed_copies.
```

Migration Strategy:

```text
Support both /api/v1/books and /api/v2/books for 6 months.
Clients will be given documentation and instructions for
migrating to v2.
```

Deprecation Timeline:

```text
Announce v1 deprecation: January 1, 2027
End of v1 support: July 1, 2027
```

---

# 12. Authentication

The API will use JWT (JSON Web Tokens) for authentication in a future implementation.

Authenticated requests will include:

```http
Authorization: Bearer <token>
```

Authentication will be implemented in a future phase.

---

# 13. Error Handling

The API will use a consistent error response format.

Example:

```json
{
  "error": {
    "code": "BOOK_NOT_FOUND",
    "message": "Book with ID 5 does not exist"
  }
}
```

Common status codes:

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
422 Unprocessable Entity
500 Internal Server Error
```

---

# 14. Resource Relationships

The relationships between the resources are:

```text
MEMBERS
   |
   | has many
   v
 LOANS
   ^
   |
   | belongs to
   |
 BOOKS
   |
   | belongs to
   v
CATEGORIES
```

More specifically:

```text
Members 1 ---- * Loans
Books   1 ---- * Loans
Categories 1 ---- * Books
```

---

# 15. API Borrowing Flow

The borrowing process follows these steps:

```text
Client
  |
  | POST /api/loans
  | member_id + book_id
  v
API Server
  |
  | Check member
  v
Member Database
  |
  | Member exists
  v
API Server
  |
  | Check book availability
  v
Book Database
  |
  | available_copies > 0
  v
API Server
  |
  | Create loan
  | Decrease available copies
  v
Database
  |
  | Success
  v
Client
  |
  | 201 Created
  v
Loan Response
```

If the book is unavailable:

```text
Client
  |
  | POST /api/loans
  v
API Server
  |
  | Check availability
  v
Book Database
  |
  | available_copies = 0
  v
API Server
  |
  | 409 Conflict
  v
Client
```

---

# 16. Future Enhancements

Possible future features include:

1. JWT authentication
2. Admin and member roles
3. Email notifications for overdue books
4. Automatic overdue calculation
5. Book reservation system
6. Fines and payment tracking
7. Library reports and analytics
8. Book cover image support
9. Barcode or QR code scanning
10. Mobile application integration

---

# 17. REST Principles Used

This API follows the major REST principles:

### Client-Server

The frontend and backend are separated.

### Stateless

Each request contains the information needed by the server.

### Cacheable

GET responses can be cached when appropriate.

### Uniform Interface

The API uses standard HTTP methods and resource-oriented URLs.

### Layered System

The API can work with proxies, load balancers, and other intermediaries.

### Code on Demand

This optional REST constraint may be used by web clients through JavaScript.

---

# 18. Conclusion

The Library Management System API provides a structured and RESTful approach to managing library resources.

The design uses nouns for URLs and HTTP methods as actions. It also uses appropriate status codes, JSON request and response formats, nested resources, query parameters, API versioning, and consistent error handling.

This design can serve as the blueprint for implementing the actual Library Management System backend in Node.js, Express, or another server-side technology.
