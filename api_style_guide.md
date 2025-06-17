# JuaJobs API – Style Guide

This document outlines the official **API Style Guide** for the JuaJobs platform. It ensures consistency, clarity, and performance across all endpoints.

---

## 1. Design Principles

- **RESTful**: Uses standard HTTP methods and status codes.
- **Resource-Centric**: Focuses on real-world entities like users, jobs, applications, and transactions.
- **Consistent & Predictable**: Follows uniform endpoint structures, naming conventions, and data formats.
- **Mobile-First**: Optimized for low-bandwidth, high-latency environments.
- **Secure by Design**: Implements OAuth2.0 with JWT-based authentication and role-based access.
- **Localisation-Ready**: Supports multiple languages for content and error messages.

---

## 2. URL Structure

- **Use plural nouns** for resource collections: `/users`, `/jobs`, `/applications`
- **Avoid verbs** in paths: use `/jobs`, not `/createJob`
- **Nest resources** to reflect logical hierarchy: `/users/:id/profile`, `/jobs/:jobId/applications`
- **Use camelCase** for query parameters and JSON fields

**Examples**:
- `/users/:id/profile`
- `/jobs/:jobId/applications`
- `/reviews?revieweeId=123`

---

## 3. HTTP Methods

Use standard HTTP verbs to reflect resource operations:

| Method | Purpose                    |
|--------|----------------------------|
| GET    | Retrieve data              |
| POST   | Create new resource        |
| PUT    | Replace full resource      |
| PATCH  | Update part of a resource  |
| DELETE | Remove resource            |

---

## 4. JSON Format

- **Content Type**: All requests and responses use `application/json`
- **Timestamps**: Use ISO 8601 format in UTC, e.g., `2025-06-16T18:00:00Z`
- **Field Naming**: All keys should use `camelCase`

**Example**:
```json
{
  "id": "user_123",
  "fullName": "Ajang Deng",
  "createdAt": "2025-06-16T18:00:00Z"
}


---

## 5. Query Parameters

The API supports flexible querying via URL parameters.

| Feature         | Example                                      |
|-----------------|----------------------------------------------|
| Filtering       | `/jobs?status=open&location=Nairobi`         |
| Sorting         | `/jobs?sort=-createdAt`                      |
| Pagination      | `/jobs?page=2&pageSize=10`                   |
| Sparse Fields   | `/jobs?fields=title,budget`                  |
| Text Search     | `/jobs/search?q=plumber`                     |

- Use minus (`-`) for descending sort: `sort=-createdAt`
- Avoid overfetching by using `fields=`

---

## 6. HTTP Status Codes

Use standard HTTP status codes that align with the response type.

| Code | Meaning                  | When to Use                              |
|------|---------------------------|-------------------------------------------|
| 200  | OK                        | Successful GET, PUT, or PATCH             |
| 201  | Created                   | Resource successfully created via POST   |
| 204  | No Content                | Successful DELETE or PATCH with no body  |
| 400  | Bad Request               | Invalid request format or data           |
| 401  | Unauthorized              | Missing or invalid authentication        |
| 403  | Forbidden                 | Authenticated but not authorized         |
| 404  | Not Found                 | Resource does not exist                  |
| 409  | Conflict                  | Resource conflict (e.g. duplicate entry) |
| 422  | Unprocessable Entity      | Valid JSON but violates business rules   |
| 500  | Internal Server Error     | Unexpected server failure                |

---

## 7. Standard Error Format

All error responses should follow a consistent structure.

**Example**:
```json
{
  "code": 422,
  "message": "Invalid input",
  "details": ["Budget must be greater than 0"]
}

**code**: HTTP status code
**message**: High-level summary
**details**: Field-specific validation or logic issues

---

## 8. Authentication

8.1 **Authentication Method**

- **OAuth 2.0 with JWT (JSON Web Tokens)**

- **Uses access tokens (short-lived) and refresh tokens (long-lived)**

- **Tokens signed using HS256**

- **HTTPS enforced on all endpoints**

---

## 8.2 Authentication Endpoints

**Endpoint	Method	Description**

/auth/login	POST	Authenticate user and get token
/auth/refresh	POST	Refresh access token
/auth/logout	POST	Invalidate refresh token

Clients must store tokens securely and implement auto-refresh logic.

---

## 9. Versioning Strategy

9.1 **Approach**

URL-based versioning is used.

Easy to cache, trace, and debug.

## 9.2 Deprecation Policy

- **Change Type	Notification Method	Notice Period**
- **Minor	Dev Portal + Patch Notes	None**
- **Major	Headers + Email Alerts	90 days**

**Sample Header:**

vbnet
Copy
Edit
Deprecation: true
Sunset: 2025-09-30
Link: </v2/jobs>; rel="alternate"

10. Field Classifications
Type	Meaning
Required	Must be submitted by client
Optional	Enhances functionality, not mandatory
System-generated	Auto-populated by backend (e.g. createdAt)
Computed	Derived from other values (e.g. ratingAvg)

11. Naming Conventions

Element	Convention
Resource Names	Plural nouns
JSON Fields	camelCase
Query Params	camelCase

Avoid abbreviations like usr, jp, or unclear field names.

Correct:

jobTitle, budgetMin, createdAt

Avoid:

jobtitle, budget_min, create_time



