
# Q1. What is Spring Security?

### Interview Ready Answer

> Spring Security is a powerful authentication and authorization framework for Java applications. It helps secure applications by handling user authentication, role-based authorization, session management, CSRF protection, and protection against common security vulnerabilities.

---

# Q2. Why Do We Need Spring Security?

### Interview Ready Answer

> Without Spring Security, we would have to implement authentication, authorization, password encryption, session handling, and security checks manually. Spring Security provides these features out of the box, reducing development effort and improving application security.

---

# Q3. What Problems Does Spring Security Solve?

### Interview Ready Answer

> Spring Security solves several security challenges such as user authentication, access control, password encryption, session management, CSRF attacks, session fixation attacks, and unauthorized API access. It provides a centralized and standardized security mechanism.

---

# Q4. Explain Spring Security with a Project Example

### Interview Ready Answer

> In my employee management project, Spring Security is used to authenticate users through email and password. After successful login, authorization is performed using roles such as ADMIN and USER. ADMIN users can create, update, and delete employee records, while USERs have only read access.

---

# Q5. What Features Does Spring Security Provide?

### Interview Ready Answer

> Spring Security provides authentication, authorization, password encoding, session management, CSRF protection, CORS support, role-based access control, OAuth2 integration, JWT authentication support, and protection against common web security attacks.

---

# Q6. What Happens Internally When a Request Comes?

### Interview Ready Answer

> Every request passes through Spring Security's filter chain. The filters validate authentication, check security rules, verify permissions, and decide whether the request should be allowed or rejected before it reaches the controller.

---

# Q7. What Is SecurityFilterChain?

### Interview Ready Answer

> SecurityFilterChain is the central configuration component in modern Spring Security. It defines authentication mechanisms, authorization rules, CSRF settings, session management, and other security-related configurations for incoming requests.

---

# Q8. Why Is Spring Security Based on Filters?

### Interview Ready Answer

> Spring Security uses servlet filters because every HTTP request passes through the filter chain before reaching controllers. This allows security checks to happen early in the request lifecycle.

---

# Q9. What Is Authentication in Spring Security?

### Interview Ready Answer

> Authentication is the process of verifying a user's identity. Spring Security validates the submitted credentials and creates an Authentication object when the user is successfully authenticated.

---

# Q10. What Is Authorization in Spring Security?

### Interview Ready Answer

> Authorization determines whether an authenticated user has permission to access a specific resource or perform a specific operation based on assigned roles and authorities.

---

# Q11. How Does Spring Security Authenticate a User?

### Interview Ready Answer

> Spring Security calls UserDetailsService to load user information, compares passwords using PasswordEncoder, and if validation succeeds, creates an Authentication object and stores it in SecurityContextHolder.

---

# Q12. What Is SecurityContextHolder?

### Interview Ready Answer

> SecurityContextHolder stores the Authentication object of the currently logged-in user and allows access to user details throughout the application.

---

# Q13. What Is PasswordEncoder?

### Interview Ready Answer

> PasswordEncoder is responsible for hashing passwords before storing them and validating passwords during login using methods such as matches().

---

# Q14. Why Do We Use BCrypt?

### Interview Ready Answer

> BCrypt is a one-way password hashing algorithm that provides strong password protection. It includes salting and is resistant to brute-force attacks, making it a recommended choice for password storage.

---

# Q15. What Is a Security Filter Chain?

### Interview Ready Answer

> A Security Filter Chain is a collection of filters that process incoming requests. Each filter performs a specific security task such as authentication, authorization, CSRF validation, or session management.

---

# Q16. What Is the Flow of Spring Security?

### Interview Ready Answer

```text
Request
   ↓
Security Filter Chain
   ↓
Authentication
   ↓
Authorization
   ↓
Controller
   ↓
Response
```

Then say:

> Every request first goes through the filter chain. Authentication verifies the user, authorization checks permissions, and only then is the request allowed to reach the controller.

---

# Q17. Why Is Spring Security Better Than Writing Custom Security?

### Interview Ready Answer

> Spring Security is highly tested, standardized, and provides industry-standard security mechanisms. Implementing security manually increases complexity and introduces security risks.

---

# Most Important 30-Second Answer

Practice this daily:

Spring Security is a framework used to secure Java applications. It provides authentication, authorization, password encryption, session management, and protection against common security attacks. In my projects, I use Spring Security to authenticate users using email and password and authorize access based on roles such as ADMIN and USER. Internally, requests pass through a security filter chain where authentication and authorization checks are performed before the request reaches the controller.

---

 Hidden Expectation

When they ask:

```text
What is Spring Security?
```

They are actually checking:

```text
✓ Do you know Authentication?
✓ Do you know Authorization?
✓ Do you know Filter Chain?
✓ Do you know UserDetailsService?
✓ Do you know PasswordEncoder?
✓ Have you used it in a project?
```
