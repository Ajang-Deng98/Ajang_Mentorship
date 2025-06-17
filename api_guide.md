# JuaJobs API – README

This is the official documentation and API style guide for the JuaJobs platform. JuaJobs is a RESTful API designed to power a gig economy platform tailored for African markets, connecting clients and skilled workers across the continent. The API is built with scalability, security, localisation, and mobile-first performance in mind.

---

## 1. Design Principles

* **RESTful**: Follows REST architecture with standard HTTP methods.
* **Resource-Centric**: API revolves around real-world entities like jobs, users, applications, and transactions.
* **Consistency**: Uniform endpoint structures, naming conventions, and data formats.
* **Mobile-First**: Designed for low-bandwidth and high-latency environments.
* **Secure by Design**: Includes role-based access, OAuth2.0, and JWT-based authentication.
* **Localisation-Ready**: Supports multi-language content and error messaging.

---

## 2. URL Structure

* Use **plural nouns**: `/users`, `/jobs`, `/applications`
* Nest resources when it reflects a clear hierarchy.
* Avoid verbs in paths.
* Use `camelCase` for query parameters and JSON fields.

**Examples**:

* `/users/:id/profile`
* `/jobs/:jobId/applications`
* `/reviews?revieweeId=123`

---

## 3. HTTP Methods

| Method | Use Case                |
| ------ | ----------------------- |
| GET    | Retrieve data           |
| POST   | Create new resource     |
| PUT    | Replace full resource   |
| PATCH  | Update partial resource |
| DELETE | Remove resource         |

---

## 4. JSON Request and Response Format

* All payloads use `application/json`.
* All timestamps follow **ISO 8601** in UTC (e.g., `2025-06-16T18:00:00Z`).
* Keys are written in `camelCase`.

**Example**:

```json
{
  "id": "user_123",
  "fullName": "Ajang Deng",
  "createdAt": "2025-06-16T18:00:00Z"
}
```

---

## 5. Pagination, Filtering & Sorting

JuaJobs supports flexible querying via URL parameters:

| Feature       | Example                              |
| ------------- | ------------------------------------ |
| Filtering     | `/jobs?status=open&location=Nairobi` |
| Sorting       | `/jobs?sort=-createdAt`              |
| Pagination    | `/jobs?page=2&pageSize=10`           |
| Sparse Fields | `/jobs?fields=title,budget`          |
| Text Search   | `/jobs/search?q=plumber`             |

---

## 6. HTTP Status Codes

| Code | Description           | Use Case                        |
| ---- | --------------------- | ------------------------------- |
| 200  | OK                    | Successful GET, PUT, PATCH      |
| 201  | Created               | Resource successfully created   |
| 204  | No Content            | DELETE or PATCH success         |
| 400  | Bad Request           | Malformed request or validation |
| 401  | Unauthorized          | Invalid/missing token           |
| 403  | Forbidden             | Authenticated but unauthorized  |
| 404  | Not Found             | Resource doesn’t exist          |
| 409  | Conflict              | Resource logic conflict         |
| 422  | Unprocessable Entity  | Valid JSON but business error   |
| 500  | Internal Server Error | Unexpected backend failure      |

---

## 7. Standard Error Format

All error responses return a consistent JSON structure:

```json
{
  "code": 422,
  "message": "Invalid input",
  "details": ["Budget must be greater than 0"]
}
```

* `code`: HTTP status code
* `message`: High-level summary
* `details`: Field-specific validation or logic issues

---

## 8. Authentication

### 8.1 Authentication Method

* **OAuth 2.0** with **JWT (JSON Web Tokens)**
* Uses **access tokens** (short-lived) and **refresh tokens** (long-lived)
* Tokens signed using **HS256**
* HTTPS enforced on all endpoints

### 8.2 Authentication Endpoints

| Endpoint        | Method | Description                     |
| --------------- | ------ | ------------------------------- |
| `/auth/login`   | POST   | Authenticate user and get token |
| `/auth/refresh` | POST   | Refresh access token            |
| `/auth/logout`  | POST   | Invalidate refresh token        |

Clients must store tokens securely and implement auto-refresh logic.

---

## 9. Versioning Strategy

### 9.1 Approach

* **URL-based versioning** is used: `/v1/`, `/v2/`, etc.
* Easy to cache, trace, and debug.

### 9.2 Deprecation Policy

| Change Type | Notification Method      | Notice Period |
| ----------- | ------------------------ | ------------- |
| Minor       | Dev Portal + Patch Notes | None          |
| Major       | Headers + Email Alerts   | 90 days       |

**Sample Header**:

```
Deprecation: true
Sunset: 2025-09-30
Link: </v2/jobs>; rel="alternate"
```

---

## 10. Field Classifications

| Type             | Meaning                                      |
| ---------------- | -------------------------------------------- |
| Required         | Must be submitted by client                  |
| Optional         | Enhances functionality, not mandatory        |
| System-generated | Auto-populated by backend (e.g. `createdAt`) |
| Computed         | Derived from other values (e.g. ratingAvg)   |

---

## 11. Naming Conventions

| Element        | Convention   |
| -------------- | ------------ |
| Resource Names | Plural nouns |
| JSON Fields    | `camelCase`  |
| Query Params   | `camelCase`  |

Avoid abbreviations like `usr`, `jp`, or unclear field names.

**Correct**:

* `jobTitle`, `budgetMin`, `createdAt`

**Avoid**:

* `jobtitle`, `budget_min`, `create_time`

---

## 12. Additional Best Practices

* No verbs in URLs: Use `/jobs`, not `/createJob`
* No trailing slashes: Use `/users`, not `/users/`
* Prefer clarity over brevity
* Use proper HTTP status codes for every scenario

---


