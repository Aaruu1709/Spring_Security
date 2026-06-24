

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

