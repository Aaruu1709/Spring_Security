

---

# Q1. What is Authentication?

### Answer

> Authentication is the process of verifying a user's identity. In Spring Security, a user provides credentials such as username and password, and the framework validates them against stored user details. If the credentials are valid, the user is authenticated.

---

# Q2. What is Authorization?

### Answer

> Authorization determines what an authenticated user is allowed to access. In Spring Security, authorization is implemented using roles and authorities. Based on the user's permissions, access to APIs or resources is granted or denied.

---

# Q3. Difference Between Authentication and Authorization?

### Answer

> Authentication verifies who the user is, whereas authorization determines what the user can access. Authentication always happens first, and once the user is authenticated, authorization checks the user's roles and permissions.

---

# Q4. Explain Authentication and Authorization with Project Example

### Answer

> In my employee management project, users log in using email and password. Spring Security authenticates the user by validating the credentials. Once authenticated, authorization is handled through roles such as ADMIN and USER. ADMIN users can create, update, and delete employees, while USERs have only read access.

---

# Q5. What Happens Internally When a User Logs In?

### Answer

> When a login request comes, Spring Security calls UserDetailsService to load user information from the database. It then uses PasswordEncoder to compare the entered password with the stored encoded password. If validation succeeds, an Authentication object is created and stored in SecurityContextHolder.

---

# Q6. What is UserDetailsService?

### Answer

> UserDetailsService is a Spring Security interface responsible for loading user information during authentication. It contains a single method, loadUserByUsername(), which fetches user details from the database or another source.

---

# Q7. Why Do We Implement UserDetailsService?

### Answer

> We implement UserDetailsService so Spring Security can load user information from our custom database tables instead of using in-memory users.

---

# Q8. What is UserDetails?

### Answer

> UserDetails is an interface that represents authenticated user information. It contains username, password, roles, and account status details that Spring Security uses during authentication and authorization.

---

# Q9. What is GrantedAuthority?

### Answer

> GrantedAuthority represents a permission or role assigned to a user. Spring Security uses GrantedAuthority objects during authorization to determine whether a user can access a specific resource.

---

# Q10. Why Do We Use SimpleGrantedAuthority?

### Answer

> SimpleGrantedAuthority is the default implementation of GrantedAuthority. It is used to convert roles or permissions stored in the database into authority objects that Spring Security can understand.

---

# Q11. What is SecurityContextHolder?

### Answer

> SecurityContextHolder stores the currently authenticated user's Authentication object. It allows us to access logged-in user information anywhere within the application.

---

# Q12. How Can You Get Logged-In User Information?

### Answer

```java
Authentication auth =
SecurityContextHolder.getContext().getAuthentication();
```

Interview Answer:

> We can retrieve the currently authenticated user through SecurityContextHolder, which stores authentication details for the current request.

---

# Q13. What is PasswordEncoder?

### Answer

> PasswordEncoder is used to securely store passwords by hashing them. During login, it compares the raw password entered by the user with the encoded password stored in the database.

---

# Q14. Why Can't We Store Passwords Directly?

### Answer

> Storing plain-text passwords is a security risk. If the database is compromised, attackers can immediately see all passwords. Therefore passwords should always be stored in encoded form using algorithms such as BCrypt.

---

# Q15. What is BCrypt?

### Answer

> BCrypt is a password hashing algorithm commonly used in Spring Security. It is a one-way hashing algorithm, meaning the original password cannot be retrieved from the hash.

---

# Q16. Does Spring Security Decode BCrypt Passwords?

### Answer

> No. BCrypt hashes cannot be decoded. Spring Security uses PasswordEncoder.matches() to verify whether the entered password matches the stored hash.

---

# Q17. What Happens After Successful Authentication?

### Answer

> After successful authentication, Spring Security creates an Authentication object and stores it in SecurityContextHolder. Subsequent requests use this authentication information for authorization decisions.

---

# Q18. Can Authorization Happen Without Authentication?

### Answer

> No. A user must first be authenticated before authorization can be performed because the system needs to know the user's identity and permissions.

---

# Q19. What is Role-Based Access Control (RBAC)?

### Answer

> RBAC is an authorization mechanism where access is controlled based on user roles. For example, ADMIN users may have additional privileges compared to USER roles.

---

# Q20. Difference Between Role and Authority?

### Answer

> A role is a collection of permissions, while an authority represents a specific permission. For example, ROLE_ADMIN is a role, whereas EMPLOYEE_DELETE or EMPLOYEE_UPDATE can be individual authorities.

---

# Q21. How Do You Restrict APIs Based on Roles?

### Answer

> In Spring Security, APIs can be restricted using hasRole(), hasAuthority(), or annotations such as @PreAuthorize.

Example:

```java
@PreAuthorize("hasRole('ADMIN')")
```

---

# Q22. What Is the Authentication Object?

### Answer

> Authentication is an interface that represents the authenticated user. It contains principal, credentials, authorities, and authentication status.

---

# Q23. What Is Principal?

### Answer

> Principal represents the currently logged-in user. It typically contains the UserDetails object of the authenticated user.

---

# Q24. How Does Spring Security Know Which User Is Logged In?

### Answer

> Spring Security stores the Authentication object in SecurityContextHolder after successful authentication. It uses this object to identify the logged-in user.

---

# 30-Second Answer (Most Important)

Practice this daily:

Authentication is the process of verifying a user's identity, typically through username and password. In Spring Security, UserDetailsService loads user information and PasswordEncoder validates credentials. Once authenticated, Spring creates an Authentication object and stores it in SecurityContextHolder.

Authorization determines what the authenticated user can access. It is usually implemented using roles and authorities. In my employee management project, ADMIN users can perform create, update, and delete operations, while USERs have read-only access.

