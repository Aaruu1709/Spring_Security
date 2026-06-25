| No. | Filter Name                            | Purpose                                                    |
| --- | -------------------------------------- | ---------------------------------------------------------- |
| 1   | `CorsFilter`                           | Handles CORS requests                                      |
| 2   | `CsrfFilter`                           | Protects against CSRF attacks                              |
| 3   | `LogoutFilter`                         | Handles logout requests                                    |
| 4   | `UsernamePasswordAuthenticationFilter` | Processes form login username/password                     |
| 5   | `BasicAuthenticationFilter`            | Processes HTTP Basic Authentication                        |
| 6   | `BearerTokenAuthenticationFilter`      | Processes JWT Bearer tokens (OAuth2 Resource Server)       |
| 7   | `AnonymousAuthenticationFilter`        | Creates an anonymous user if no one is logged in           |
| 8   | `SessionManagementFilter`              | Manages HTTP sessions                                      |
| 9   | `ExceptionTranslationFilter`           | Handles 401 and 403 exceptions                             |
| 10  | `AuthorizationFilter`                  | Checks authorization rules like roles and permissions      |
| 11  | `SecurityContextHolderFilter`          | Loads and stores authentication in `SecurityContextHolder` |
| 12  | `RememberMeAuthenticationFilter`       | Handles Remember Me functionality                          |


# Spring Security Filters You Must Know (4+ Years Experience)

You don't need all 30+ filters. These **10 filters** are enough for most interviews.

```text
1. SecurityContextHolderFilter
2. CorsFilter
3. CsrfFilter
4. UsernamePasswordAuthenticationFilter
5. BasicAuthenticationFilter
6. BearerTokenAuthenticationFilter (JWT)
7. AnonymousAuthenticationFilter
8. SessionManagementFilter
9. ExceptionTranslationFilter
10. AuthorizationFilter
```

```
Based on Authentication Type
Form Login
Request
  ↓
CsrfFilter
  ↓
UsernamePasswordAuthenticationFilter
  ↓
SessionManagementFilter
  ↓
AuthorizationFilter
  ↓
Controller
```

```
HTTP Basic Authentication
Request
  ↓
CsrfFilter
  ↓
BasicAuthenticationFilter
  ↓
SessionManagementFilter
  ↓
AuthorizationFilter
  ↓
Controller
```

```
JWT Authentication
Request
  ↓
CorsFilter
  ↓
CsrfFilter
  ↓
JwtAuthenticationFilter (Custom)
or
BearerTokenAuthenticationFilter
  ↓
SecurityContextHolderFilter
  ↓
AuthorizationFilter
  ↓
Controller
```
---

# 1. SecurityContextHolderFilter

## What does it do?

Loads the logged-in user's information into `SecurityContextHolder` at the beginning of the request and saves/clears it at the end.

---

## Internal Flow

```text
HTTP Request
     ↓
SecurityContextHolderFilter
     ↓
Load Authentication from Session/JWT
     ↓
Store Authentication in SecurityContextHolder
     ↓
Next Filter
```

---

## Interview Script

> "SecurityContextHolderFilter is responsible for loading the authenticated user's information into SecurityContextHolder so that other filters and controllers can access the current user during request processing."

---

# 2. CorsFilter

## What does it do?

Checks whether the frontend origin is allowed to access the backend.

Example:

```text
Frontend:
http://localhost:3000

Backend:
http://localhost:8080
```

---

## Internal Flow

```text
Request
  ↓
Read Origin Header
  ↓
Origin Allowed?
 ↓         ↓
YES        NO
 ↓          ↓
Next      CORS Error
Filter
```

---

## Interview Script

> "CorsFilter handles Cross-Origin Resource Sharing. It validates the Origin header and decides whether the request from another domain is allowed or should be blocked."

---

# 3. CsrfFilter

## What does it do?

Protects against fake requests from malicious websites.

---

## Internal Flow

```text
POST/PUT/DELETE Request
          ↓
CsrfFilter
          ↓
Check CSRF Token
          ↓
Token Valid?
 ↓            ↓
YES           NO
 ↓             ↓
Next Filter   403 Forbidden
```

---

## Interview Script

> "CsrfFilter protects the application from Cross-Site Request Forgery attacks by validating the CSRF token for state-changing requests such as POST, PUT, and DELETE. In REST APIs using JWT, we generally disable CSRF because the application is stateless."

---

# 4. UsernamePasswordAuthenticationFilter

(Form Login Authentication)

---

## What does it do?

Processes login forms.

```text
Username = arti
Password = 12345
```

---

## Internal Flow

```text
POST /login
      ↓
UsernamePasswordAuthenticationFilter
      ↓
Read Username & Password
      ↓
Create UsernamePasswordAuthenticationToken
      ↓
AuthenticationManager.authenticate()
      ↓
UserDetailsService
      ↓
loadUserByUsername()
      ↓
UserDetails
      ↓
PasswordEncoder.matches()
      ↓
Authentication Success
      ↓
Store Authentication in SecurityContextHolder
```

---

## Interview Script

> "UsernamePasswordAuthenticationFilter handles form-based login requests. It extracts the username and password, creates an authentication token, delegates authentication to AuthenticationManager, and stores the authenticated user in SecurityContextHolder."

---

# 5. BasicAuthenticationFilter

(HTTP Basic Authentication)

---

## What does it do?

Processes Basic Authentication headers.

Example:

```http
Authorization: Basic YXJ0aToxMjM0NQ==
```

(Base64 of `arti:12345`)

---

## Internal Flow

```text
Request
   ↓
BasicAuthenticationFilter
   ↓
Read Authorization Header
   ↓
Decode Base64
   ↓
username = arti
password = 12345
   ↓
AuthenticationManager
   ↓
UserDetailsService
   ↓
PasswordEncoder.matches()
   ↓
Authentication Object Created
   ↓
SecurityContextHolder
```

---

## Interview Script

> "BasicAuthenticationFilter processes HTTP Basic Authentication requests. It decodes the Authorization header, authenticates the user through AuthenticationManager, and stores the authenticated user in SecurityContextHolder."

---

# 6. BearerTokenAuthenticationFilter (JWT)

---

## What does it do?

Processes JWT tokens.

Example:

```http
Authorization: Bearer eyJhbGciOiJIUzI1Ni...
```

---

## Internal Flow

```text
Request
   ↓
BearerTokenAuthenticationFilter
   ↓
Extract JWT Token
   ↓
Validate Signature
   ↓
Check Expiration
   ↓
Extract Username
   ↓
Load UserDetails
   ↓
Create Authentication Object
   ↓
Store in SecurityContextHolder
```

---

## Interview Script

> "BearerTokenAuthenticationFilter processes JWT tokens. It extracts the token from the Authorization header, validates it, loads the user details, creates an authentication object, and stores it in SecurityContextHolder."

---

# 7. AnonymousAuthenticationFilter

---

## What does it do?

Creates an anonymous user when nobody is logged in.

---

## Internal Flow

```text
Authentication Exists?
 ↓             ↓
YES            NO
 ↓              ↓
Skip           Create
               AnonymousAuthenticationToken
               ↓
               Store in SecurityContextHolder
```

---

## Interview Script

> "AnonymousAuthenticationFilter creates an AnonymousAuthenticationToken when no user is authenticated. This ensures that Spring Security always has an authentication object, even for anonymous users."

---

# 8. SessionManagementFilter

---

## What does it do?

Manages user sessions.

---

# Stateful Authentication

```text
Login
  ↓
Create Session
  ↓
Store JSESSIONID
  ↓
Future Requests
  ↓
Load Authentication from Session
```

---

# Stateless Authentication (JWT)

```java
SessionCreationPolicy.STATELESS
```

---

## Internal Flow

```text
Request
   ↓
SessionManagementFilter
   ↓
STATELESS?
 ↓            ↓
YES           NO
 ↓             ↓
Use JWT      Use HTTP Session
Only         (JSESSIONID)
```

---

## Interview Script

> "SessionManagementFilter controls how user sessions are managed. In traditional applications it uses HTTP sessions, while in JWT-based applications we configure it as stateless so that no session is created on the server."

---

# 9. ExceptionTranslationFilter

---

## What does it do?

Handles security exceptions.

---

## Internal Flow

### User Not Logged In

```text
Protected API
      ↓
AuthenticationException
      ↓
ExceptionTranslationFilter
      ↓
401 Unauthorized
```

---

### User Has Wrong Role

```text
Admin API
    ↓
User has USER role
    ↓
AccessDeniedException
    ↓
ExceptionTranslationFilter
    ↓
403 Forbidden
```

---

## Interview Script

> "ExceptionTranslationFilter handles security-related exceptions. It converts AuthenticationException into 401 Unauthorized responses and AccessDeniedException into 403 Forbidden responses."

---

# 10. AuthorizationFilter

---

## What does it do?

Checks authorization rules.

Example:

```java
.requestMatchers("/admin/**")
.hasRole("ADMIN")
```

---

## Internal Flow

```text
Request: /admin/dashboard
        ↓
AuthorizationFilter
        ↓
Find Matching Rule
        ↓
Need ROLE_ADMIN
        ↓
Authentication.getAuthorities()
        ↓
ROLE_ADMIN Present?
 ↓                ↓
YES               NO
 ↓                 ↓
Controller        403 Forbidden
```

---

## Interview Script

> "AuthorizationFilter is responsible for checking whether the authenticated user has the required roles or permissions to access a particular resource based on the configured authorization rules."

---

# COMPLETE FLOW (FORM LOGIN)

```text
POST /login
    ↓
CorsFilter
    ↓
CsrfFilter
    ↓
UsernamePasswordAuthenticationFilter
    ↓
AuthenticationManager
    ↓
UserDetailsService
    ↓
UserDetails
    ↓
PasswordEncoder.matches()
    ↓
SecurityContextHolder
    ↓
SessionManagementFilter
    ↓
Response
```

---

# COMPLETE FLOW (HTTP BASIC)

```text
Request
   ↓
SecurityContextHolderFilter
   ↓
CorsFilter
   ↓
CsrfFilter
   ↓
BasicAuthenticationFilter
   ↓
AuthenticationManager
   ↓
UserDetailsService
   ↓
PasswordEncoder.matches()
   ↓
AnonymousAuthenticationFilter
   ↓
SessionManagementFilter
   ↓
AuthorizationFilter
   ↓
ExceptionTranslationFilter
   ↓
Controller
```

---

# COMPLETE FLOW (JWT)

```text
Request
   ↓
SecurityContextHolderFilter
   ↓
CorsFilter
   ↓
CsrfFilter
   ↓
BearerTokenAuthenticationFilter
   ↓
Validate JWT
   ↓
Load UserDetails
   ↓
Create Authentication Object
   ↓
Store in SecurityContextHolder
   ↓
SessionManagementFilter
   ↓
AuthorizationFilter
   ↓
ExceptionTranslationFilter
   ↓
Controller
```

---

# One-Line Revision Sheet (Interview Day)

```text
SecurityContextHolderFilter
→ Loads current user's Authentication.

CorsFilter
→ Validates cross-origin requests.

CsrfFilter
→ Prevents CSRF attacks.

UsernamePasswordAuthenticationFilter
→ Handles form login.

BasicAuthenticationFilter
→ Handles HTTP Basic authentication.

BearerTokenAuthenticationFilter
→ Handles JWT authentication.

AnonymousAuthenticationFilter
→ Creates anonymous user authentication.

SessionManagementFilter
→ Manages sessions or stateless JWT mode.

ExceptionTranslationFilter
→ Converts security exceptions to 401/403.

AuthorizationFilter
→ Checks roles and permissions.
```

These are the filters and explanations that are sufficient and practical for a **4+ years Spring Boot/Spring Security interview**.
