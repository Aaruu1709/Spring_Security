 interviewers usually don't ask:

> "What is UserDetails?"

They ask:

> "User enters username and password. Explain what happens internally until authentication succeeds."

or

> "Why do we need UserDetailsService if we already have a User entity?"

or

> "How does Spring Security know the currently logged-in user?"

or

> "Why is CSRF required for Session Authentication but not for JWT?"

---
You should be able to explain these 10 flows end-to-end:

### 1. Login Flow

```text
Request
 ↓
UsernamePasswordAuthenticationFilter
 ↓
AuthenticationManager
 ↓
AuthenticationProvider
 ↓
UserDetailsService
 ↓
Database
 ↓
PasswordEncoder
 ↓
Authentication Success
```

---

### 2. UserDetails Flow

```text
User Entity
 ↓
UserDetailsService
 ↓
UserDetails
 ↓
Spring Security
```

Explain:

* Why UserDetails exists
* Why User entity is not directly used

---

### 3. Password Verification Flow

```text
Raw Password
 ↓
BCrypt
 ↓
Hash
 ↓
Compare Stored Hash
```

Interviewers love:

* Hashing
* Salting
* BCrypt

---

### 4. SecurityContext Flow

```text
Login Success
 ↓
Authentication Object
 ↓
SecurityContext
 ↓
SecurityContextHolder
```

Explain where Spring stores the logged-in user.

---

### 5. Session Authentication Flow

```text
Login
 ↓
Session Created
 ↓
JSESSIONID
 ↓
Browser Stores Cookie
 ↓
Subsequent Requests
```

---

### 6. CSRF Flow

```text
Server Generates Token
 ↓
Browser Receives Token
 ↓
Request Sends Token
 ↓
Server Validates Token
```

Explain:

* Why GET is not protected
* Why POST/PUT/DELETE are protected

---

### 7. JWT Flow

```text
Login
 ↓
Generate JWT
 ↓
Send JWT
 ↓
Client Stores JWT
 ↓
Authorization Header
 ↓
JWT Filter
 ↓
Validation
```

---

### 8. Authorization Flow

```text
Authenticated User
 ↓
Role Check
 ↓
Permission Check
 ↓
Access Granted/Denied
```

---

### 9. Method Security Flow

```java
@PreAuthorize("hasRole('ADMIN')")
```

Know:

* How Spring intercepts method calls
* When access is denied

---

### 10. Exception Flow

Know the difference:

```text
Unauthenticated → 401

Authenticated but no permission → 403
```

This question appears very frequently.

---

# Additional Topics I would add for a strong 4+ year profile

### CORS

Very common in microservices.

### OncePerRequestFilter

Almost every JWT implementation uses it.

### Custom Authentication Filter

Good for experienced-level discussions.

### OAuth2 Basics

Google Login, GitHub Login concepts.

### Refresh Token

Asked in JWT-heavy projects.

### Security Best Practices

* Password hashing
* Principle of least privilege
* Disable unnecessary endpoints
* Secure Actuator endpoints

---


I would prioritize:

1. Authentication vs Authorization
2. SecurityFilterChain
3. Filters
4. UserDetails
5. UserDetailsService
6. PasswordEncoder + BCrypt
7. AuthenticationManager
8. AuthenticationProvider
9. SecurityContextHolder
10. Session Management
11. CSRF
12. JWT
13. OncePerRequestFilter
14. Role vs Authority
15. @PreAuthorize
16. 401 vs 403
17. CORS
18. Refresh Token
19. OAuth2 Basics


My suggestion for your learning path now:
👉 Finish **CSRF completely today**, then tomorrow start **AuthenticationManager → AuthenticationProvider → SecurityContextHolder**, because those are the topics where many candidates struggle during interviews.
