# Spring_Security
TOPICS:

# Phase 1: Security Basics (Must Know First)

### 1. What is Spring Security?

* Why do we need it?
* Problems it solves
* Authentication vs Authorization

### 2. Authentication

* What is authentication?
* How login works
* Username & Password authentication

### 3. Authorization

* What is authorization?
* Roles vs Authorities
* ROLE_ADMIN vs ADMIN

### 4. SecurityFilterChain

* What is it?
* Why do all requests pass through it?
* Request flow

### 5. Filters in Spring Security

* What is a Filter?
* Why use Filters?
* Filter Chain concept

Interview Question:

> Explain request flow from browser to controller through Spring Security.

---

# Phase 2: User Authentication Internals

### 6. UserDetails

Understand:

```java
public interface UserDetails
```

Know:

* getUsername()
* getPassword()
* getAuthorities()

### 7. UserDetailsService

```java
loadUserByUsername()
```

Understand:

* Why Spring calls it
* When it is executed
* How user is loaded from DB

### 8. User Entity → UserDetails Mapping

Example:

```java
User
 ↓
CustomUserDetails
 ↓
Spring Security
```

Interview Question:

> Why can't Spring Security directly use your User entity?

---

### 9. PasswordEncoder

Know:

* Why passwords are never stored in plain text
* BCryptPasswordEncoder

Example:

```java
passwordEncoder.encode("admin123");
```

### 10. Hashing

Must know:

* What is hashing
* Why irreversible
* BCrypt advantages
* Salt

Interview Favorite:

> Difference between Encryption and Hashing?

---

# Phase 3: Authentication Process Internals

### 11. AuthenticationManager

Know:

* What it does
* Who calls it

### 12. AuthenticationProvider

Understand:

```java
DaoAuthenticationProvider
```

Responsibilities:

* Fetch user
* Compare password

### 13. Authentication Object

Know:

```java
Authentication
```

Contains:

* Principal
* Credentials
* Authorities

### 14. SecurityContext

Know:

* What is SecurityContext?
* Where logged-in user is stored?

### 15. SecurityContextHolder

Interview Favorite:

```java
SecurityContextHolder
```

Get current user:

```java
SecurityContextHolder
.getContext()
.getAuthentication();
```

---

# Phase 4: Session-Based Security

### 16. Session Management

Understand:

* What is session?
* JSESSIONID

### 17. Stateful Authentication

Flow:

```
Login
 ↓
Session Created
 ↓
Session ID stored
 ↓
User recognized
```

### 18. Session Fixation Attack

Know:

* What it is
* How Spring prevents it

### 19. Concurrent Session Control

Example:

* One user only one login

---

# Phase 5: CSRF (Very Important)

### 20. CSRF

Know:

* What is CSRF?
* Why dangerous?

### 21. CSRF Token

Understand:

* Token generation
* Validation

### 22. When CSRF is Required?

Know:

| Authentication Type | CSRF Needed |
| ------------------- | ----------- |
| Session Based       | Yes         |
| JWT                 | No          |

Interview Question:

> Why is CSRF disabled in JWT?

---

# Phase 6: JWT Security (Most Asked in Interviews)

### 23. JWT Introduction

Structure:

```
Header.Payload.Signature
```

### 24. JWT Authentication Flow

```
Login
 ↓
Generate Token
 ↓
Send Token
 ↓
Client Stores Token
 ↓
Every Request Sends Token
```

### 25. JWT Claims

Understand:

* sub
* exp
* iat
* roles

### 26. JWT Filter

Custom Filter:

```java
OncePerRequestFilter
```

### 27. JWT Validation

Know:

* Expiration
* Signature Verification

### 28. Stateless Authentication

Interview Favorite:

> What is Stateless Authentication?

---

# Phase 7: Advanced Authorization

### 29. Role Based Authorization

```java
.hasRole("ADMIN")
```

### 30. Authority Based Authorization

```java
.hasAuthority("CREATE_USER")
```

### 31. Method Level Security

```java
@PreAuthorize
@PostAuthorize
```

### 32. Enable Method Security

```java
@EnableMethodSecurity
```

---

# Phase 8: Exception Handling

### 33. AuthenticationException

Examples:

* BadCredentialsException

### 34. AccessDeniedException

Difference:

| Error | Meaning                     |
| ----- | --------------------------- |
| 401   | Not Authenticated           |
| 403   | Authenticated but No Access |

Interview Question:

> Difference between 401 and 403?

---

# Phase 9: OAuth2 (4+ Years Level)

### 35. OAuth2 Basics

Know:

* Why OAuth exists

### 36. OAuth2 Login

Examples:

* Google Login
* GitHub Login

### 37. Resource Server

### 38. Authorization Server

Interview Question:

> Difference between Authentication and Authorization Server?

---

# Phase 10: Real Project Security (Must Know)

### 39. Custom Login

### 40. Custom Success Handler

### 41. Custom Failure Handler

### 42. CORS

Know:

* Why browser blocks requests

### 43. Security Headers

Examples:

* XSS Protection
* CSP

### 44. Remember Me Authentication

### 45. Logout Handling

### 46. Refresh Token

### 47. Access Token vs Refresh Token

Very Common Interview Question.

---

# Final Interview Revision Topics (Most Asked)

1. Authentication vs Authorization
2. UserDetails
3. UserDetailsService
4. PasswordEncoder
5. BCrypt Hashing
6. SecurityFilterChain
7. Filters
8. AuthenticationManager
9. AuthenticationProvider
10. SecurityContextHolder
11. Session Management
12. CSRF
13. JWT
14. OncePerRequestFilter
15. Role vs Authority
16. @PreAuthorize
17. 401 vs 403
18. CORS
19. OAuth2
20. Access Token vs Refresh Token

### Suggested Order 


✅ Authentication vs Authorization
✅ SecurityFilterChain
✅ Filters
✅ UserDetails
✅ UserDetailsService
✅ PasswordEncoder & BCrypt
✅ AuthenticationManager
✅ AuthenticationProvider
✅ SecurityContextHolder
✅ Session Management
✅ CSRF
➡️ JWT (Next Focus)
➡️ Role/Authority
➡️ Method Security
➡️ Exception Handling
➡️ OAuth2

