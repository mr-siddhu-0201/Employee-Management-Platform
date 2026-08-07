# Security Guide - Employee Management System

## Table of Contents

1. [Authentication & Authorization](#authentication--authorization)
2. [Data Protection](#data-protection)
3. [API Security](#api-security)
4. [Infrastructure Security](#infrastructure-security)
5. [Dependency Management](#dependency-management)
6. [Best Practices](#best-practices)
7. [Security Checklist](#security-checklist)
8. [Incident Response](#incident-response)

---

## Authentication & Authorization

### JWT (JSON Web Tokens) Implementation

**Token Generation**
```java
// AuthService.java
@Service
public class AuthService {
    
    @Value("${jwt.secret}")
    private String jwtSecret;
    
    @Value("${jwt.expiration}")
    private long jwtExpiration; // 24 hours
    
    public String generateToken(UserDetails userDetails) {
        return Jwts.builder()
            .setSubject(userDetails.getUsername())
            .claim("authorities", userDetails.getAuthorities())
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + jwtExpiration))
            .signWith(SignatureAlgorithm.HS512, jwtSecret)
            .compact();
    }
    
    public String validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(jwtSecret).parseClaimsJws(token);
            return true;
        } catch (JwtException | IllegalArgumentException e) {
            return false;
        }
    }
}
```

**Backend Security Configuration**
```java
// SecurityConfig.java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf()
                .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())
            .and()
            .authorizeRequests()
                .antMatchers("/api/auth/**").permitAll()
                .antMatchers("/api/employees/**").hasRole("ADMIN")
                .antMatchers("/api/departments/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            .and()
            .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            .and()
            .addFilterBefore(jwtAuthenticationFilter(), UsernamePasswordAuthenticationFilter.class);
        
        return http.build();
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

**Frontend Token Storage**
```javascript
// authService.js
const authService = {
    login: async (email, password) => {
        const response = await axios.post('/api/auth/login', { email, password });
        const token = response.data.token;
        
        // Store token in memory (NOT localStorage for sensitive data)
        sessionStorage.setItem('authToken', token);
        
        return token;
    },
    
    logout: () => {
        sessionStorage.removeItem('authToken');
    },
    
    getToken: () => {
        return sessionStorage.getItem('authToken');
    }
};

// Setup axios interceptor
axios.interceptors.request.use((config) => {
    const token = authService.getToken();
    if (token) {
        config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
});
```

### Role-Based Access Control (RBAC)

```java
// User.java - Entity with roles
@Entity
@Table(name = "user")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;
    
    private String username;
    private String email;
    private String password;
    
    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
        name = "user_role",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles = new HashSet<>();
}

// Authorization annotations
@Component
@Aspect
public class AuthorizationAspect {
    
    @Around("@annotation(authorize)")
    public Object authorize(ProceedingJoinPoint joinPoint, Authorize authorize) 
            throws Throwable {
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        
        boolean hasRole = authorize.roles().length == 0 ||
                Stream.of(authorize.roles())
                    .anyMatch(role -> authentication.getAuthorities().stream()
                        .anyMatch(auth -> auth.getAuthority().equals("ROLE_" + role)));
        
        if (!hasRole) {
            throw new AccessDeniedException("User does not have required role");
        }
        
        return joinPoint.proceed();
    }
}

// Controller usage
@RestController
@RequestMapping("/api/employees")
public class EmployeeController {
    
    @GetMapping
    @Authorize(roles = {"ADMIN", "HR"})
    public List<Employee> getAllEmployees() {
        return employeeService.getAllEmployees();
    }
    
    @DeleteMapping("/{id}")
    @Authorize(roles = {"ADMIN"})
    public void deleteEmployee(@PathVariable Integer id) {
        employeeService.deleteEmployee(id);
    }
}
```

---

## Data Protection

### Password Security

**Password Hashing**
```properties
# application.properties
# Use BCrypt with at least 12 rounds
spring.security.user.password={bcrypt}$2a$12$...
```

```java
// Password validation example
@Component
public class PasswordValidator {
    
    private static final String PASSWORD_REGEX = 
        "^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d)(?=.*[@$!%*?&])[A-Za-z\\d@$!%*?&]{12,}$";
    
    public boolean isValid(String password) {
        return password.matches(PASSWORD_REGEX);
    }
    
    public String getRequirements() {
        return "Password must be at least 12 characters and contain: " +
               "uppercase, lowercase, number, and special character";
    }
}
```

**Password Reset Flow**
```java
@Service
public class PasswordResetService {
    
    public PasswordResetToken createResetToken(User user) {
        String token = UUID.randomUUID().toString();
        
        PasswordResetToken resetToken = new PasswordResetToken();
        resetToken.setUser(user);
        resetToken.setToken(token);
        resetToken.setExpiryDate(System.currentTimeMillis() + 3600000); // 1 hour
        
        return passwordResetTokenRepository.save(resetToken);
    }
    
    public boolean validateResetToken(String token) {
        PasswordResetToken resetToken = passwordResetTokenRepository.findByToken(token);
        
        if (resetToken == null) {
            return false;
        }
        
        if (resetToken.getExpiryDate() < System.currentTimeMillis()) {
            passwordResetTokenRepository.delete(resetToken);
            return false;
        }
        
        return true;
    }
    
    public void resetPassword(String token, String newPassword) {
        PasswordResetToken resetToken = passwordResetTokenRepository.findByToken(token);
        User user = resetToken.getUser();
        user.setPassword(new BCryptPasswordEncoder().encode(newPassword));
        userRepository.save(user);
        passwordResetTokenRepository.delete(resetToken);
    }
}
```

### Data Encryption

**Sensitive Field Encryption**
```java
// Entity with encrypted field
@Entity
@Table(name = "employee")
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;
    
    private String name;
    private String email;
    
    @Column(columnDefinition = "VARBINARY(255)")
    @Convert(converter = EncryptedFieldConverter.class)
    private String ssn; // Encrypted
    
    @Column(columnDefinition = "VARBINARY(255)")
    @Convert(converter = EncryptedFieldFieldConverter.class)
    private String bankAccount; // Encrypted
}

// Converter for encryption/decryption
@Converter
public class EncryptedFieldConverter implements AttributeConverter<String, String> {
    
    private static final String ENCRYPTION_KEY = System.getenv("ENCRYPTION_KEY");
    
    @Override
    public String convertToDatabaseColumn(String attribute) {
        if (attribute == null) return null;
        return encrypt(attribute);
    }
    
    @Override
    public String convertToEntityAttribute(String dbData) {
        if (dbData == null) return null;
        return decrypt(dbData);
    }
    
    private String encrypt(String data) {
        // Use AES encryption
        Cipher cipher = Cipher.getInstance("AES");
        cipher.init(Cipher.ENCRYPT_MODE, new SecretKeySpec(ENCRYPTION_KEY.getBytes(), 0, 16, "AES"));
        return Base64.getEncoder().encodeToString(cipher.doFinal(data.getBytes()));
    }
    
    private String decrypt(String encryptedData) {
        // Use AES decryption
        Cipher cipher = Cipher.getInstance("AES");
        cipher.init(Cipher.DECRYPT_MODE, new SecretKeySpec(ENCRYPTION_KEY.getBytes(), 0, 16, "AES"));
        return new String(cipher.doFinal(Base64.getDecoder().decode(encryptedData)));
    }
}
```

**Database Connection Security**
```properties
# application.properties - Secure credentials
spring.datasource.url=jdbc:mysql://localhost:3306/employee_management?useSSL=true&requireSSL=true&serverTimezone=UTC
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.maximum-pool-size=20
spring.datasource.hikari.connection-timeout=20000
spring.datasource.hikari.idle-timeout=300000
spring.datasource.hikari.max-lifetime=1200000
```

---

## API Security

### CORS Configuration

```java
@Configuration
public class CorsConfig {
    
    @Bean
    public CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();
        configuration.setAllowedOrigins(Arrays.asList(System.getenv("ALLOWED_ORIGINS").split(",")));
        configuration.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "DELETE", "OPTIONS"));
        configuration.setAllowedHeaders(Arrays.asList("Content-Type", "Authorization"));
        configuration.setExposedHeaders(Arrays.asList("Authorization"));
        configuration.setAllowCredentials(true);
        configuration.setMaxAge(3600L);
        
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration);
        return source;
    }
}
```

### Rate Limiting

```java
@RestController
@RequestMapping("/api")
public class RateLimitedController {
    
    private final RateLimiter rateLimiter = RateLimiter.create(10.0); // 10 requests per second
    
    @GetMapping("/employees")
    public ResponseEntity<?> getEmployees() {
        if (!rateLimiter.tryAcquire()) {
            return ResponseEntity.status(429).body("Too many requests");
        }
        // Process request
        return ResponseEntity.ok(employeeService.getAllEmployees());
    }
}
```

### Request Validation

```java
// Input validation
@RestController
@RequestMapping("/api/employees")
public class EmployeeController {
    
    @PostMapping
    public ResponseEntity<?> createEmployee(@Valid @RequestBody EmployeeDTO employee) {
        // @Valid triggers validation
        return ResponseEntity.created(uri).body(employeeService.createEmployee(employee));
    }
}

// DTO with validation
@Data
public class EmployeeDTO {
    
    @NotBlank(message = "Name is required")
    @Size(min = 2, max = 100, message = "Name must be 2-100 characters")
    private String name;
    
    @NotBlank(message = "Email is required")
    @Email(message = "Email must be valid")
    private String email;
    
    @NotBlank(message = "Phone is required")
    @Pattern(regexp = "^[+]?[0-9]{10,13}$", message = "Phone must be valid")
    private String phone;
    
    @Min(value = 0, message = "Salary cannot be negative")
    @Max(value = 1000000, message = "Salary exceeds maximum")
    private Double salary;
}
```

---

## Infrastructure Security

### SSL/TLS Configuration

```properties
# application.properties - HTTPS
server.port=8443
server.ssl.key-store=classpath:keystore.p12
server.ssl.key-store-password=${SSL_KEYSTORE_PASSWORD}
server.ssl.key-store-type=PKCS12
server.ssl.key-alias=tomcat
```

### HTTP Security Headers

```java
@Configuration
public class SecurityHeadersConfig implements WebMvcConfigurer {
    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new HandlerInterceptor() {
            @Override
            public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
                response.setHeader("X-Content-Type-Options", "nosniff");
                response.setHeader("X-Frame-Options", "DENY");
                response.setHeader("X-XSS-Protection", "1; mode=block");
                response.setHeader("Strict-Transport-Security", "max-age=31536000; includeSubDomains");
                response.setHeader("Content-Security-Policy", "default-src 'self'");
                return true;
            }
        });
    }
}
```

### Secrets Management

```properties
# DO NOT commit secrets to repository!
# Use environment variables instead

# .env (not committed to git)
JWT_SECRET=your-super-secret-key-min-256-characters
DATABASE_PASSWORD=your-database-password
ENCRYPTION_KEY=your-encryption-key
API_KEY=your-api-key

# application.properties
jwt.secret=${JWT_SECRET}
spring.datasource.password=${DATABASE_PASSWORD}
encryption.key=${ENCRYPTION_KEY}
```

```bash
# Docker-compose with secrets
version: '3.8'
services:
  backend:
    environment:
      - JWT_SECRET_FILE=/run/secrets/jwt_secret
      - DB_PASSWORD_FILE=/run/secrets/db_password
    secrets:
      - jwt_secret
      - db_password

secrets:
  jwt_secret:
    file: ./secrets/jwt_secret.txt
  db_password:
    file: ./secrets/db_password.txt
```

---

## Dependency Management

### Vulnerability Scanning

```bash
# Check for vulnerable dependencies
mvn org.owasp:dependency-check-maven:check

# NPM audit
npm audit

# Fix vulnerabilities
npm audit fix
mvn dependency:update
```

### Dependency Policy

**Maven pom.xml best practices**
```xml
<dependencyManagement>
    <dependencies>
        <!-- Always specify versions explicitly -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>3.3.5</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<build>
    <plugins>
        <!-- Check for security vulnerabilities -->
        <plugin>
            <groupId>org.owasp</groupId>
            <artifactId>dependency-check-maven</artifactId>
            <version>9.0.0</version>
            <configuration>
                <failBuildOnCVSS>7</failBuildOnCVSS>
            </configuration>
            <executions>
                <execution>
                    <goals>
                        <goal>check</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

---

## Best Practices

### Secure Coding Guidelines

1. **Input Validation**
   - Always validate user input
   - Use whitelisting, not blacklisting
   - Sanitize special characters

2. **SQL Injection Prevention**
   ```java
   // ✅ GOOD - Use parameterized queries
   Employee emp = employeeRepository.findByEmail(email);
   
   // ❌ BAD - Vulnerable to SQL injection
   String query = "SELECT * FROM employee WHERE email = '" + email + "'";
   ```

3. **XSS Prevention**
   ```javascript
   // ✅ GOOD - React auto-escapes by default
   <div>{userData}</div>
   
   // ❌ BAD - Can cause XSS
   <div dangerouslySetInnerHTML={{__html: userData}}></div>
   ```

4. **CSRF Protection**
   - Use CSRF tokens in forms
   - Spring Boot provides automatic CSRF protection

5. **Logging Security**
   ```java
   // ✅ GOOD - Don't log sensitive data
   logger.info("User login attempt for user: {}", userId);
   
   // ❌ BAD - Logs password
   logger.info("User login with password: {}", password);
   ```

### Audit Logging

```java
@Entity
@Table(name = "audit_log")
public class AuditLog {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;
    
    private String username;
    private String action;
    private String resourceType;
    private Integer resourceId;
    private String changes;
    private LocalDateTime timestamp;
    private String ipAddress;
}

@Component
@Aspect
public class AuditAspect {
    
    @Around("@annotation(audit)")
    public Object audit(ProceedingJoinPoint joinPoint, Audit audit) throws Throwable {
        String username = SecurityContextHolder.getContext().getAuthentication().getName();
        String action = audit.action();
        Object result = joinPoint.proceed();
        
        AuditLog log = new AuditLog();
        log.setUsername(username);
        log.setAction(action);
        log.setTimestamp(LocalDateTime.now());
        auditLogRepository.save(log);
        
        return result;
    }
}
```

---

## Security Checklist

- [ ] All passwords use BCrypt with 12+ rounds
- [ ] JWT tokens have expiration (24 hours max)
- [ ] HTTPS/SSL enabled in production
- [ ] CORS properly configured
- [ ] CSRF protection enabled
- [ ] Input validation on all endpoints
- [ ] SQL injection prevention via parameterized queries
- [ ] XSS protection enabled
- [ ] Security headers configured
- [ ] Secrets stored in environment variables
- [ ] Audit logging implemented
- [ ] Rate limiting configured
- [ ] Dependency vulnerabilities scanned
- [ ] Database encryption enabled for sensitive fields
- [ ] API authentication required
- [ ] Role-based access control implemented
- [ ] Error messages don't expose sensitive info
- [ ] File upload validation implemented
- [ ] Regular security audits scheduled

---

## Incident Response

### Response Plan

1. **Identify** - Detect the security issue
2. **Contain** - Prevent further damage
3. **Investigate** - Understand root cause
4. **Remediate** - Fix the vulnerability
5. **Communicate** - Notify affected users
6. **Review** - Implement preventative measures

### Contact Information

- Security Team Email: security@example.com
- Emergency Hotline: +1-800-555-SECURITY
- GitHub Security Advisory: https://github.com/TEJA6777/WorkHub-Modern-Employee-Management-Platform/security/advisories

---

**Last Updated:** November 17, 2025
**Security Level:** Production
**Author:** Kodati Sai Teja

For security issues, please email security@example.com instead of using public issue tracker.
