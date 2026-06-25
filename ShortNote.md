| Rule                                       | Meaning                                        | Example                     |
| ------------------------------------------ | ---------------------------------------------- | --------------------------- |
| `permitAll()`                              | Anyone can access, even without login          | Login page, public APIs     |
| `authenticated()`                          | User must be logged in                         | Profile page                |
| `hasRole("ADMIN")`                         | User must have a specific role                 | Admin dashboard             |
| `hasAnyRole("ADMIN", "MANAGER")`           | User can have any one of multiple roles        | Admin or Manager APIs       |
| `hasAuthority("READ_EMP")`                 | User must have a specific permission/authority | Read employee data          |
| `hasAnyAuthority("READ_EMP", "WRITE_EMP")` | User can have any one of multiple permissions  | Read or write APIs          |
| `denyAll()`                                | Nobody can access                              | Disable an API temporarily  |
| `anonymous()`                              | Only users who are NOT logged in can access    | Registration or login page  |
| `access()`                                 | Custom access rules using expressions          | Complex business conditions |
