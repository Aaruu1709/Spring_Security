

# Q1. What is SecurityFilterChain?

### Interview Ready Answer

> SecurityFilterChain is the central configuration component in Spring Security that defines how incoming HTTP requests should be secured. It contains a chain of security filters responsible for authentication, authorization, CSRF protection, session management, and other security-related operations before the request reaches the controller.

---

# Q2. Why Do We Need SecurityFilterChain?

### Interview Ready Answer

> Every request entering the application must be validated for security. Instead of writing security checks in every controller, Spring Security processes requests through SecurityFilterChain, where authentication and authorization are handled centrally.

---

# Problem It Solves

Without SecurityFilterChain:

```java
@GetMapping("/employees")
public List<Employee> getEmployees() {

    // check login
    // check role
    // check permissions
    // check session

    return employees;
}
```

Every controller would contain security code ❌

---

With SecurityFilterChain:

```text
Request
   ↓
SecurityFilterChain
   ↓
Security Checks
   ↓
Controller
```

Controller remains clean ✅

---

# Most Important Practical Understanding

Suppose user calls:

```http
GET /employees
```

Request flow:

```text
Client
  ↓
SecurityFilterChain
  ↓
Authentication Filter
  ↓
Authorization Filter
  ↓
Controller
```

Only after passing security checks:

```java
@GetMapping("/employees")
```

is executed.

---

# Real Project Example

Suppose:

```text
ADMIN
```

can delete employees.

```text
USER
```

cannot.

Configuration:

```java
@Bean
public SecurityFilterChain securityFilterChain(
        HttpSecurity http) throws Exception {

    http
      .authorizeHttpRequests(auth -> auth
          .requestMatchers("/admin/**")
          .hasRole("ADMIN")

          .requestMatchers("/user/**")
          .hasRole("USER")

          .anyRequest()
          .authenticated());

    return http.build();
}
```

---

When request comes:

```http
DELETE /admin/employee/1
```

SecurityFilterChain checks:

```text
Is User Authenticated?
       ↓
Does User Have ROLE_ADMIN?
```

If yes:

```text
Controller Executes
```

If no:

```text
403 Forbidden
```

---

# What Happens Internally?

This is a favorite interview question.

### Flow

```text
Request
  ↓
DelegatingFilterProxy
  ↓
Spring Security Filter Chain
  ↓
Authentication Filters
  ↓
Authorization Filters
  ↓
Controller
```

---

# Common Filters You Should Know

Don't memorize 20 filters.

Remember these 4:

### 1. UsernamePasswordAuthenticationFilter

Handles login authentication.

```text
Username
Password
```

verification.

---

### 2. BasicAuthenticationFilter

Handles Basic Authentication.

```http
Authorization: Basic xxx
```

---

### 3. CsrfFilter

Validates CSRF tokens.

```text
POST
PUT
DELETE
```

requests.

---

### 4. AuthorizationFilter

Checks:

```text
Roles
Authorities
Permissions
```

---

# Interview Question

### How Does SecurityFilterChain Work?

### Answer

> Every incoming request passes through SecurityFilterChain before reaching the controller. The chain contains multiple security filters that perform authentication, authorization, CSRF validation, and session checks. If all validations pass, the request proceeds to the controller; otherwise, access is denied.

---

# Modern Spring Security Question

### Why Do We Create a SecurityFilterChain Bean?

### Answer

> Since Spring Security 5.7, WebSecurityConfigurerAdapter has been deprecated. Security configuration is now defined using a SecurityFilterChain bean, which provides a more flexible and component-based configuration approach.

---

# Very Important Question

### Difference Between WebSecurityConfigurerAdapter and SecurityFilterChain?

### Answer

> Earlier Spring Security used WebSecurityConfigurerAdapter for configuration. It has been deprecated, and modern Spring Security uses SecurityFilterChain beans because they provide clearer, more flexible, and component-based security configuration.

---


SecurityFilterChain is the central security configuration mechanism in Spring Security. Every incoming request passes through this filter chain before reaching the controller. The filters perform tasks such as authentication, authorization, CSRF validation, and session management. In my projects, I configure SecurityFilterChain using HttpSecurity to define role-based access rules, authentication mechanisms, and endpoint security. If a request passes all security checks, it reaches the controller; otherwise, Spring Security blocks it.

---


After SecurityFilterChain, be ready for:

```text
1. What is HttpSecurity?
2. What is AuthenticationManager?
3. What is UserDetailsService?
4. What is UsernamePasswordAuthenticationFilter?
5. What is BasicAuthenticationFilter?
6. What is CsrfFilter?
7. What is SecurityContextHolder?
8. What is JWT Authentication?
9. What is Session Authentication?
10. How does request flow through Spring Security?
```

### Easy Memory Diagram

```text
Request
   ↓
SecurityFilterChain
   ↓
Authentication
   ↓
Authorization
   ↓
Controller
   ↓
Response
```

# 1. What is HttpSecurity?

### Answer

> HttpSecurity is used to configure security rules in Spring Security. We use it to define which APIs are public, which require authentication, login configuration, CSRF settings, session management, and authorization rules.

### Project Example

> In my project, I use HttpSecurity to allow public access to login APIs and restrict employee management APIs to authenticated users.

---

# 2. What is AuthenticationManager?

### Answer

> AuthenticationManager is responsible for authenticating users. When a user submits username and password, Spring Security passes those credentials to AuthenticationManager, which validates them and returns an authenticated user if the credentials are correct.

### Easy Memory

```text
Username + Password
         ↓
AuthenticationManager
         ↓
Valid or Invalid
```

---

# 3. What is UserDetailsService?

### Answer

> UserDetailsService is an interface used to load user information during authentication. It contains the loadUserByUsername() method, which fetches user details from the database.

### Project Example

> In my project, I implemented UserDetailsService to load user data from the users table.

---

# 4. What is UsernamePasswordAuthenticationFilter?

### Answer

> UsernamePasswordAuthenticationFilter handles login requests containing username and password. It extracts the credentials from the request and starts the authentication process.

### Internally

```text
Login Request
      ↓
UsernamePasswordAuthenticationFilter
      ↓
AuthenticationManager
```

---

# 5. What is BasicAuthenticationFilter?

### Answer

> BasicAuthenticationFilter processes HTTP Basic Authentication requests. It reads the Authorization header, extracts username and password, and authenticates the user.

Example:

```http
Authorization: Basic YWFydXU6MTEx
```

---

# 6. What is CsrfFilter?

### Answer

> CsrfFilter protects the application from CSRF attacks. It validates the CSRF token for requests that modify data, such as POST, PUT, PATCH, and DELETE requests.

### Example

> When a user submits a form, Spring verifies the CSRF token before allowing the request.

---

# 7. What is SecurityContextHolder?

### Answer

> SecurityContextHolder stores the authentication information of the currently logged-in user. After successful login, Spring Security stores the Authentication object here so it can be accessed anywhere in the application.

### Example

```java
Authentication auth =
SecurityContextHolder.getContext().getAuthentication();
```

---

# 8. What is JWT Authentication?

### Answer

> JWT Authentication is a stateless authentication mechanism where, after successful login, the server generates a JWT token and sends it to the client. The client includes the token in every request, and the server validates it before granting access.

### Flow

```text
Login
  ↓
JWT Generated
  ↓
Client Stores Token
  ↓
Send Token In Every Request
  ↓
Server Validates
```

### Interview Point

> JWT is commonly used in microservices and REST APIs because it is stateless.

---

# 9. What is Session Authentication?

### Answer

> Session Authentication is a stateful authentication mechanism. After successful login, the server creates a session and stores user information. The browser sends the session ID with every request, and the server uses it to identify the user.

### Flow

```text
Login
  ↓
Session Created
  ↓
JSESSIONID Generated
  ↓
Browser Sends Session ID
```

---

# 10. How Does Request Flow Through Spring Security?

### Answer

> Every request first passes through Spring Security's filter chain. Authentication filters verify the user's identity, authorization filters check permissions, and if all validations succeed, the request reaches the controller.

### Flow

```text
Client Request
      ↓
SecurityFilterChain
      ↓
Authentication
      ↓
Authorization
      ↓
Controller
      ↓
Response
```

---

# Most Important Interview Question

### Session Authentication vs JWT Authentication

### Answer

> Session Authentication is stateful because user information is stored on the server side in a session. JWT Authentication is stateless because the server does not store session data; instead, the client sends the JWT token with each request.

---

# 1-Minute Spring Security Summary (Practice Daily)

> Spring Security secures applications using authentication and authorization. Authentication verifies the user's identity, while authorization determines access permissions. HttpSecurity is used to configure security rules. UserDetailsService loads user data, AuthenticationManager validates credentials, and SecurityContextHolder stores authenticated user information. Requests pass through SecurityFilterChain, where filters such as UsernamePasswordAuthenticationFilter, BasicAuthenticationFilter, and CsrfFilter perform security checks. In modern applications, authentication can be implemented using either Session-based Authentication or JWT Authentication depending on the project requirements.

---


