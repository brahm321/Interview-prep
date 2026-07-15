

### 1. Authentication vs. Authorization

This is the ultimate warm-up question. Don't mix these up.

- **Authentication (AuthN):** "Who are you?" (Checking your ID at the door. Validating username/password or a JWT).
    
- **Authorization (AuthZ):** "What are you allowed to do?" (You are inside the club, but are you allowed in the VIP room? Checking roles like `ROLE_USER` vs `ROLE_ADMIN`).
    

---

### 2. The Filter Chain (The Front Door)

Before an HTTP request ever reaches your `@RestController`, it has to pass through a series of standard Java Servlets called **Filters**.

Spring Security does not reinvent the wheel; it just inserts a massive, highly complex filter into the standard Java Servlet flow.

Here are the three components of that front door you must know:

1. **`DelegatingFilterProxy`:** The web server (like Tomcat) doesn't know what Spring is. It only knows standard Java Filters. Spring provides the `DelegatingFilterProxy` as a bridge. Tomcat calls this proxy, and the proxy says, _"Hold on, let me hand this over to Spring."_
    
2. **`FilterChainProxy`:** This is the actual starting point of Spring Security. It looks at the incoming HTTP request and decides which `SecurityFilterChain` to send it through.
    
3. **`SecurityFilterChain`:** This is a list of specialized Spring Security filters (usually 10 to 15 of them).
    
    - _Example:_ If a request comes in, it hits the `UsernamePasswordAuthenticationFilter` first. If it passes, it moves to the `BasicAuthenticationFilter`, then maybe a `BearerTokenAuthenticationFilter` (for JWTs), and finally the `AuthorizationFilter` before finally hitting your Controller.
        

> **Interview Tip:** If an interviewer asks, "How do you add a custom JWT filter to Spring Security?", your answer should be: _"I extend `OncePerRequestFilter` and insert it into the `SecurityFilterChain`specifically **before** the `UsernamePasswordAuthenticationFilter`."_

---

### 3. The Authentication Architecture (The VIP Check)

This is the absolute meat of Spring Security. When the `UsernamePasswordAuthenticationFilter` intercepts a login request, it doesn't check the database itself. It delegates the work to a very specific chain of command.

You must memorize these three interfaces:

#### **A. `AuthenticationManager` (The Boss)**

The filter extracts the username and password and hands it to the `AuthenticationManager`. The Manager doesn't actually know _how_ to verify the password. Instead, it manages a list of "Providers" and asks: _"Which one of you knows how to handle a Username/Password login?"_

#### **B. `AuthenticationProvider` (The Specialist)**

The `AuthenticationProvider` (specifically the `DaoAuthenticationProvider`) steps up and says, _"I can do that."_The Provider holds the actual logic to verify the credentials. However, it doesn't talk to the database directly either!

#### **C. `UserDetailsService` (The Database Clerk)**

The Provider asks the `UserDetailsService` to go fetch the user record from the database.

- You implement this interface by overriding one single method: `loadUserByUsername(String username)`.
    
- It returns a `UserDetails` object containing the username, the **hashed** password from the database, and the user's roles.
    

#### **D. The Password Encoder (`BCryptPasswordEncoder`)**

Once the `UserDetailsService` brings back the hashed password from the database, the `AuthenticationProvider`uses the `PasswordEncoder` to hash the raw password the user just typed in and compares the two hashes.

- If they match, you are Authenticated! The manager creates an `Authentication` object (like a VIP wristband) and stores it in the `SecurityContextHolder`.
    
![[Screenshot 2026-05-07 at 09.51.43.png]]
---
### The Mid-Level Interview Summary

If an interviewer asks you to explain the Spring Security login flow, you say:

> _"The request is intercepted by an **Authentication Filter**, which creates an unauthenticated token and passes it to the **AuthenticationManager**. The Manager delegates to the correct **AuthenticationProvider**. The Provider calls the **UserDetailsService** to load the user's record from the database, and then uses a **PasswordEncoder** to verify the credentials. If successful, an authenticated token is stored in the Security Context."_


### 1. Stateful Security (The Traditional Way)

Before modern frontend frameworks (like React or Angular) existed, web applications were "Monoliths." The server generated the HTML pages and sent them to the browser.

In a Stateful architecture, the server remembers exactly who you are between requests.

- **How it works:** When you log in, Spring Security creates a Session and stores your `Authentication` object **in the server's RAM** (or in a database like Redis). It then hands your browser a tiny coat-check ticket—a cookie called `JSESSIONID`.
    
- **The Flow:** Every time you click a link, your browser automatically sends that `JSESSIONID` cookie. The server looks at the ticket, checks its RAM, finds your VIP wristband, and lets you in.
    
- **The Problem (Scaling):** Imagine your app gets popular and you add a second server (Server B). If you logged into Server A, your session is in Server A's memory. If the load balancer sends your next request to Server B, Server B has no idea who you are, and you get logged out!
    

### 2. Stateless Security (The Modern REST API Way)

Today, we build separate frontend apps that communicate with backend REST APIs. REST APIs, by definition, should be **Stateless**. The server should not remember _anything_ about previous requests.

- **How it works:** When you log in, the server does not store anything in its RAM. Instead, it packs all your user details, roles, and a cryptographic signature into a **Token** (usually a JWT - JSON Web Token). It hands this token back to the client.
    
- **The Flow:** The client stores the token (usually in LocalStorage or memory). For every subsequent request, the client must manually attach the token to the HTTP `Authorization` header as a "Bearer" token. The server reads the token, verifies the math behind the signature, and knows who you are without ever looking at its own memory.
    
- **The Benefit:** Massive scalability. You can have 10,000 servers. Because the token contains all the necessary data, _any_ server can verify the request instantly.
    

**How you configure this in Spring Security:**

Java

```java
// Telling Spring Security NOT to create sessions in memory
http.sessionManagement(session -> session
    .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
);
```

---

### 3. Cross-Site Request Forgery (CSRF)

This is a guaranteed interview question. If you understand State Management, you can answer the CSRF question perfectly.

**What is CSRF?**

Imagine you log into your bank (`bank.com`), and you get a Session Cookie. Then, in another tab, you visit a malicious website (`evil.com`). `evil.com` has a hidden button that sends a transfer request to `[bank.com/transfer](https://bank.com/transfer)`. Because cookies are attached _automatically_ by your browser to any request going to that domain, your browser attaches your bank cookie to the evil request. The bank thinks YOU authorized it, and your money is gone.

**How Spring stops it:**

Spring enables CSRF protection by default. It sends a unique, randomized "CSRF Token" to your browser. Whenever you submit a form, you have to include that token. `evil.com` can't read that token, so their fake requests fail.

> **The Mid-Level Interview Question:** _"If we are building a REST API, why do we almost always disable CSRF protection in our Spring Security config (`http.csrf(csrf -> csrf.disable())`)?"_
> 
> **Your Answer:** _"Because REST APIs are Stateless and use Bearer Tokens (JWTs) instead of Cookies. Browsers do not automatically attach Authorization headers to cross-site requests like they do with Cookies. Without automatically attached cookies, the CSRF vulnerability simply doesn't exist, so we can safely disable the protection."_

---

### 4. CORS (Cross-Origin Resource Sharing)

While CSRF is an attack, CORS is actually a browser security feature that often gets in the way of frontend developers.

By default, web browsers have a "Same-Origin Policy." If your React app is running on `http://localhost:3000`and tries to call your Spring Boot API on `http://localhost:8080`, the browser will block the response because the ports (origins) are different.

**How to fix it:**

You have to configure Spring Security to tell the browser: _"It's okay, I recognize `localhost:3000`, let them see the data."_

Java

```java
@Bean
public CorsConfigurationSource corsConfigurationSource() {
    CorsConfiguration configuration = new CorsConfiguration();
    configuration.setAllowedOrigins(Arrays.asList("http://localhost:3000")); // Allow React
    configuration.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "DELETE"));
    configuration.setAllowedHeaders(Arrays.asList("Authorization", "Content-Type"));
    
    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    source.registerCorsConfiguration("/**", configuration);
    return source;
}
```


### 1. The Death of `WebSecurityConfigurerAdapter`

In older applications, configuring security meant extending a base class and overriding its methods. Spring explicitly deprecated and removed this because it violated the principle of composition over inheritance.

Today, Spring Security is completely **Component-Based**. Instead of extending a class, you simply declare a `@Bean` of type `SecurityFilterChain`.

**The Old Way (Do NOT write this in an interview):**

Java

```java
// Spring Security 5 (Deprecated/Removed)
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // config...
    }
}
```

**The Modern Way (Spring Security 6):**

Java

```java
// Spring Security 6
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        // config...
        return http.build();
    }
}
```

---

### 2. Lambda DSL (The End of `.and()`)

If you look at older tutorials, you will see a method called `.and()` chained everywhere. It was used to jump back out of a configuration block. It was notoriously confusing because it made it hard to tell which object you were currently configuring.

Spring Security 6 enforces **Lambda DSL** (Domain Specific Language). You pass a lambda function to configure a specific feature, and when the lambda block ends, that configuration is cleanly closed.

Java

```
// ❌ THE OLD WAY (Chaining)
http
    .csrf().disable()
    .cors().and()
    .authorizeRequests() ...

// ✅ THE NEW WAY (Lambda DSL)
http
    .csrf(csrf -> csrf.disable())
    .cors(Customizer.withDefaults())
    .authorizeHttpRequests(auth -> {
         // authorization rules...
    });
```

> **Interview Tip:** If an interviewer asks _why_ Spring moved to Lambda DSL, tell them: _"It improves readability, prevents configuration scope confusion, and plays much better with IDE auto-formatting and indentation."_

---

### 3. Securing Endpoints (`requestMatchers`)

This is where you actually define the rules for your "Nightclub."

> **Crucial Update:** In Spring Security 5, you used `antMatchers()` or `mvcMatchers()`. In Spring Security 6, these have been unified and replaced by **`requestMatchers()`**.

Here is a complete, production-grade `SecurityFilterChain` bean demonstrating how to lock down an API:

Java

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    
    http
        // 1. Disable CSRF (Standard for Stateless REST APIs using JWTs)
        .csrf(csrf -> csrf.disable())
        
        // 2. Define Endpoint Authorizations
        .authorizeHttpRequests(auth -> auth
            // Public endpoints (Login, Signup, Swagger docs)
            .requestMatchers("/api/auth/**", "/v3/api-docs/**").permitAll()
            
            // Role-based endpoints
            .requestMatchers("/api/admin/**").hasRole("ADMIN")
            .requestMatchers("/api/manager/**").hasAnyRole("ADMIN", "MANAGER")
            
            // Secure a specific HTTP method
            .requestMatchers(HttpMethod.DELETE, "/api/users/**").hasAuthority("user:delete")
            
            // The Catch-All: Anything else MUST be authenticated
            .anyRequest().authenticated()
        )
        
        // 3. Make the session stateless (Since we will use JWTs later)
        .sessionManagement(session -> session
            .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
        );

    return http.build();
}
```

#### The Golden Rule of Routing: "Order Matters"

If an interviewer gives you a block of routing config and asks if there is a bug, **always check the order**.

Spring Security evaluates rules from **Top to Bottom**. The moment it finds a match, it stops looking.

- If you put `.anyRequest().authenticated()` at the very top, it will lock down every single endpoint, and your `.permitAll()` rules underneath it will be completely ignored!
    
- **Rule of Thumb:** Always go from _Most Specific_ at the top, to _Least Specific_ (the catch-all) at the bottom.
    

### 1. What is a JWT? (The Anatomy)

A JWT is a string that looks like gibberish, but it is actually highly structured. It always consists of three parts, separated by periods (`.`): `xxxxx.yyyyy.zzzzz`.

**Part 1: The Header (`xxxxx`)**

This is a Base64-encoded JSON object that tells the server _how_ the token was created. It usually just contains two things: the token type ("JWT") and the hashing algorithm used (like "HS256").

**Part 2: The Payload (`yyyyy`)**

This is the meat of the token. It is a Base64-encoded JSON object containing **Claims** (statements about the user).

- **Standard Claims:** `sub` (subject/username), `exp` (expiration time), `iat` (issued at).
    
- **Custom Claims:** You can add anything here, like `role: "ADMIN"` or `department: "SALES"`.
    

> **Crucial Interview Note:** The Payload is _encoded_, not _encrypted_. Anyone who intercepts a JWT can easily decode it and read the data inside. **Never put passwords or sensitive personal data in a JWT payload.**

**Part 3: The Signature (`zzzzz`)**

This is the security layer. The server takes the encoded Header, the encoded Payload, and a **Secret Key** (that only the server knows), and runs them all through a hashing algorithm (like HMAC SHA256).

- If a hacker intercepts the token and tries to change their role from "USER" to "ADMIN" in the payload, the math of the signature breaks. When the server receives it, it re-calculates the signature. If it doesn't match the one on the token, the server immediately rejects the request.
    

---

### 2. The JWT Authentication Flow

You need to be able to explain the end-to-end lifecycle of a JWT during an interview.

1. **Login:** The user sends a POST request with `{ "username": "john", "password": "123" }`.
    
2. **Generation:** The `AuthenticationManager` verifies the credentials against the database. If correct, a utility class generates a JWT containing the username and roles, signs it with the server's Secret Key, and sends it back to the client.
    
3. **Storage:** The client (e.g., React frontend) receives the token and stores it (usually in `localStorage` or a secure `HttpOnly` cookie).
    
4. **Subsequent Requests:** On every future request, the client attaches the token to the HTTP Header:
    
    - `Authorization: Bearer eyJhbGciOiJIUzI1...`
        
5. **Validation:** The server intercepts the request, grabs the token, uses its Secret Key to verify the signature, checks that it hasn't expired, and extracts the user data to fulfill the request.
    

---

### 3. Implementing a Custom JWT Filter (`OncePerRequestFilter`)

Because Spring Security doesn't know what a JWT is out-of-the-box, you have to build a custom filter to intercept requests and perform Step 5 from above.

You do this by extending **`OncePerRequestFilter`**, which guarantees Spring will only execute this filter one single time per HTTP request.

Here is the core logic you must understand for an interview:

Java

```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request, 
                                    HttpServletResponse response, 
                                    FilterChain filterChain) throws ServletException, IOException {
        
        // 1. Extract the token from the Header
        String authHeader = request.getHeader("Authorization");
        if (authHeader == null || !authHeader.startsWith("Bearer ")) {
            filterChain.doFilter(request, response); // No token? Move along.
            return;
        }
        
        String jwt = authHeader.substring(7); // Remove "Bearer " prefix
        
        // 2. Validate the token and extract the username (usually using a JwtService utility)
        String username = jwtService.extractUsername(jwt);
        
        // 3. If the token is valid AND the user isn't already authenticated in this context
        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            
            // 4. Load the user's roles/authorities
            UserDetails userDetails = userDetailsService.loadUserByUsername(username);
            
            if (jwtService.isTokenValid(jwt, userDetails)) {
                // 5. The Magic Step: Create the "VIP Wristband"
                UsernamePasswordAuthenticationToken authToken = new UsernamePasswordAuthenticationToken(
                        userDetails, null, userDetails.getAuthorities()
                );
                
                // 6. Put the wristband into the Security Context!
                SecurityContextHolder.getContext().setAuthentication(authToken);
            }
        }
        
        // 7. Pass the request down the chain to the next filter (or the Controller)
        filterChain.doFilter(request, response);
    }
}
```

---

### 4. Injecting the JWT Filter into the Chain

Writing the filter isn't enough; you have to tell Spring Security exactly _where_ to put it in the pipeline.

**The Interview Question:** _"Where do you register your custom JWT filter?"_

**The Answer:** _"I add it to the `SecurityFilterChain` bean, specifically using `addFilterBefore()`, placing it directly in front of the `UsernamePasswordAuthenticationFilter`."_

Why? Because if a user provides a valid JWT, you want to intercept it and authenticate them _before_ Spring Security gets confused and tries to look for a username and password in the request.

Java

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .csrf(csrf -> csrf.disable())
        .authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
        .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
        
        // THIS IS THE CRITICAL LINE
        .addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);

    return http.build();
}
```

### 1. Roles vs. Authorities (The Granularity Trap)

Interviewers love to ask about the difference between a Role and an Authority. In Spring Security, they are technically the same underlying object (`GrantedAuthority`), but they represent different levels of access.

- **Authority (Fine-Grained):** A specific action a user is allowed to take.
    
    - _Examples:_ `user:read`, `user:write`, `report:delete`.
        
- **Role (Coarse-Grained):** A logical container grouping multiple authorities together. By convention in Spring, Roles must be prefixed with `ROLE_`.
    
    - _Examples:_ `ROLE_ADMIN` (contains `user:read`, `user:write`, `report:delete`), `ROLE_USER` (contains only `user:read`).
        

> **Interview Tip:** Always recommend **Authority-Based Access Control** over strict Role-Based Access Control for enterprise apps. If a manager suddenly decides a "User" should be allowed to delete reports, changing a hardcoded `hasRole('ADMIN')` everywhere in your code is a nightmare. Checking for `hasAuthority('report:delete')` is much more flexible.

---

### 2. Method-Level Security (`@PreAuthorize`)

In Section 3, we secured URLs in the `SecurityFilterChain` (e.g., `.requestMatchers("/api/admin/")`). But what if you want to secure a specific _Service method_, regardless of which URL called it?

You use Method-Level Security.

**How to enable it:**

You must add `@EnableMethodSecurity` to your configuration class.

**How to use it:**

You place annotations directly above your methods. Spring creates a proxy (just like `@Transactional`) that intercepts the method call and checks the Security Context.

Java

```java
// Checks if the user has the ADMIN role before the method even runs
@PreAuthorize("hasRole('ADMIN')")
public void deleteUser(Long userId) {
    userRepository.deleteById(userId);
}

// Advanced SpEL (Spring Expression Language): 
// Checks if the user is an ADMIN, OR if the logged-in user is trying to edit their own profile.
@PreAuthorize("hasRole('ADMIN') or #username == authentication.principal.username")
public void updateProfile(String username, ProfileDto dto) { ... }
```

---

### 3. OAuth2 Architecture (The "Sign in with Google" Engine)

OAuth2 is not an authentication protocol; it is an **Authorization Framework**. It allows a user to grant a third-party application access to their data without giving away their password.

To explain OAuth2 in an interview, you must define the **4 Actors**:

1. **Resource Owner:** The human being holding the phone or sitting at the keyboard (You).
    
2. **Client:** The application trying to get access (Your Spring Boot / React App).
    
3. **Authorization Server:** The server that verifies identity and issues tokens (Google, Okta, Keycloak, Auth0).
    
4. **Resource Server:** The API that holds the actual data, which requires a token to access (Google Contacts API, or your own backend API).
    

---

### 4. The OAuth2 Authorization Code Flow

If an interviewer asks how OAuth2 works, they are almost always asking about the **Authorization Code Flow**. This is the safest and most common flow used by backend applications.

Here is exactly how you explain the steps:

1. **The Redirect:** The user clicks "Login with Google". Your Spring Boot app redirects the user's browser to Google's login page.
    
2. **The Consent:** The user logs into Google and sees a screen saying, _"MyApp wants to view your email profile. Allow?"_ The user clicks Yes.
    
3. **The Code:** Google redirects the user's browser _back_ to your Spring Boot app. Attached to the URL is a temporary, short-lived **Authorization Code** (e.g., `?code=xyz123`).
    
4. **The Exchange (The Secure Step):** Your Spring Boot backend takes that Code, combines it with your secret `client_id` and `client_secret` (which safely live in your `application.yml`), and makes a silent, back-channel HTTP request directly to Google's server.
    
5. **The Token:** Google verifies the Code and Secret, and responds to your backend with an **Access Token**(usually a JWT).
    
6. **The Fetch:** Your backend uses that Access Token to call the Google API, fetch the user's email and profile picture, and logs them into your system!
    

> **Why the middleman "Code"?** Why doesn't Google just send the Access Token in Step 3? Because Step 3 happens in the user's browser, where tokens can be stolen by malicious browser extensions or JavaScript. By sending a useless "Code", your secure backend server is the only thing that actually handles the sensitive Access Token.


### **Core Spring Security Architecture**

**1. Difference between Authentication and Authorization?**

- **Authentication:** Verifying _who you are_ (e.g., checking a username/password or validating a JWT).
    
- **Authorization:** Verifying _what you can do_ (e.g., checking if the authenticated user has the `ADMIN` role to delete a record).
    

**2. Explain the Filter Chain and `DelegatingFilterProxy`.**

- `DelegatingFilterProxy` is a standard Servlet filter that acts as a bridge between the web server (Tomcat) and the Spring Application Context.
    
- It intercepts incoming requests and hands them off to a Spring bean called `FilterChainProxy`. This proxy manages the actual **SecurityFilterChain**, passing the request through a sequence of security filters (like JWT verification, CORS checking, etc.) before it reaches your controller.
    

**3. What is `UserDetailsService` and how is it used?**

- It is an interface with a single method: `loadUserByUsername()`.
    
- You implement it to query your database (via a Repository) for a user. If found, you map your database user object into a Spring Security `UserDetails` object, which Spring uses to perform password validation and role assignment.
    

**4. How do you bypass security for public endpoints?**

- Inside your `SecurityFilterChain` bean, use the `authorizeHttpRequests` method.
    
- _Snippet:_ `auth.requestMatchers("/api/login", "/api/signup").permitAll().anyRequest().authenticated();`
    

**5. How does `PasswordEncoder` (BCrypt) work?**

- **Never store plain text passwords.** `BCryptPasswordEncoder` hashes the password using a mathematical algorithm and adds a random "salt" to defeat rainbow-table attacks.
    
- When logging in, Spring hashes the raw password provided by the user and compares it to the hashed string in the database. You cannot reverse-engineer a BCrypt hash.
    

**6. How do you implement Method-Level Security?**

- Enable it by placing `@EnableMethodSecurity` on your configuration class.
    
- Use `@PreAuthorize("hasRole('ADMIN')")` above controller or service methods to enforce role checks _before_ the method executes. Use `@PostAuthorize` if the check depends on the returned object.
    

**7. How do you handle Custom Error Messages?**

- Spring returns default HTML error pages. For a REST API, you implement two interfaces to return JSON:
    
    - **`AuthenticationEntryPoint`:** Triggers a 401 response when an _unauthenticated_ user tries to access a secure endpoint.
        
    - **`AccessDeniedHandler`:** Triggers a 403 response when an _authenticated_ user lacks the required role.
        

**8. What is CSRF and why disable it?**

- **CSRF (Cross-Site Request Forgery):** An attack that exploits session cookies to force a user to execute unwanted actions on an authenticated site.
    
- **Why disable it:** Modern REST APIs use JWTs and are completely stateless. Since there is no server-side session or `JSESSIONID` cookie to hijack, CSRF attacks are impossible, making the protection unnecessary.

**9. How do you handle CORS?**

- **CORS (Cross-Origin Resource Sharing):** Browsers block frontend apps (e.g., `localhost:3000`) from calling backend APIs (e.g., `localhost:8080`) for security.

- You fix it by creating a `CorsConfigurationSource` bean, explicitly setting `setAllowedOrigins()` to your frontend URL and specifying allowed HTTP methods and headers.


**10. How do you make Spring Security Stateless?**

- Inside the `SecurityFilterChain`, set the session creation policy to `STATELESS`:

- _Snippet:_ `http.sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS));`

- This stops Spring from saving sessions in memory, forcing it to authenticate _every_ request via the JWT, which saves massive amounts of RAM.


### **JWT (JSON Web Token)**

**11. Explain the complete JWT Flow.**

- **Login:** User sends credentials -> `AuthenticationManager` verifies via DB -> Server signs a JWT with a secret key -> Server returns JWT to client.
    
- **Validation:** Client attaches JWT to the `Authorization: Bearer` header -> Custom `JwtFilter` intercepts request -> Verifies signature and expiration -> Extracts username -> Loads `UserDetails` -> Places it in `SecurityContextHolder` -> Controller processes the request.

### 1. The Absolute Shield: HTTPS (TLS/SSL)

This is the mandatory first line of defense. You **never** send a JWT over plain HTTP.

When you use HTTPS, the entire connection between the user's browser and your server is encrypted _before_any data is sent.

- If a hacker is sitting "in between" (like on the same public Wi-Fi network) and intercepts the network traffic, they will not see the JWT. They will only see an unbreakable string of encrypted gibberish.
    
- **The Rule:** A JWT is only as secure as the channel it travels on.
    

### 2. The Backup Shield: Short Expiration Times

We always assume the worst. What if the hacker didn't intercept it in transit, but instead infected the user's computer with malware and stole the token directly from their browser?

- This is why Access Tokens (JWTs) must have a **very short lifespan** (usually 15 minutes).
    
- If a hacker steals the token, they only have a tiny window of time to use it before it becomes mathematically invalid. Once it expires, the hacker is locked out, and the real user's application will silently use a Refresh Token to get a new session.
    

### 3. The Front-End Shield: `HttpOnly` Cookies

If someone steals the token, it is usually not intercepted in mid-air; it is stolen from the browser using an **XSS (Cross-Site Scripting)** attack.

- Many developers mistakenly store JWTs in the browser's `localStorage`. If a hacker injects malicious JavaScript into your site, that script can read `localStorage` and steal the token.
    
- **The Fix:** The backend server should attach the JWT to an `HttpOnly` cookie. This tells the browser: _"Do not let any JavaScript touch this cookie."_ The browser will then automatically attach that cookie to every future backend request safely.
    

### 4. Advanced Shield: IP Binding (Optional)

In highly secure applications (like banking), the server might log the IP address or User-Agent (browser type) of the person who originally requested the JWT.

- When the JWT is used later, the server checks: _"Is this request coming from the same IP address that I issued the token to?"_ * If a hacker steals the token and tries to use it from a different country, the server notices the mismatch and rejects the token.

**12. What is the structure of a JWT and how is it secure?**

- **Structure:** Header (algorithm type), Payload (user claims/data), and Signature.
    
- **Security:** The signature is generated by hashing the Header + Payload using the server's private Secret Key. If a user tries to alter the payload (e.g., changing their role to ADMIN), the signature will no longer match, and the server will reject the token.
    

**13. How do you write a custom `JwtAuthenticationFilter`?**

- Create a class extending `OncePerRequestFilter`.
    
- Override `doFilterInternal()`. Inside, extract the token from the header, validate the signature using a utility class, and if valid, set the `UsernamePasswordAuthenticationToken` in the `SecurityContext`. Finally, register this filter using `addFilterBefore()` in your config.
    

**14. How do you handle JWT expiration?**

- Set an expiration claim (`exp`) when generating the token (usually 15-60 minutes).
    
- When verifying, the JWT library automatically throws an `ExpiredJwtException` if the token is past its time. Your filter catches this and rejects the request with a 401.
    

**15. What is a Refresh Token flow?**

- Access tokens (JWTs) have short lifespans for security. A Refresh Token is a long-lived token (e.g., 7 days) stored securely (often in an HttpOnly cookie).
    
- When the access token expires, the client sends the Refresh Token to a specific `/refresh` endpoint. The server validates it and issues a _new_ short-lived Access Token, allowing the user to stay logged in without re-typing their password.
    

### **OAuth2.0 & OIDC**

**16. OAuth2.0 vs OpenID Connect (OIDC)?**

- **OAuth2.0:** A framework strictly for _Delegated Authorization_ (granting an app access to your data, like "letting a printing app read your Google Drive").
    
- **OIDC:** An identity layer built on _top_ of OAuth2.0 that provides _Authentication_ (verifying who you are using an ID Token).
    

**17. Explain the OAuth2 Authorization Code Grant Flow.**

- User clicks "Login with Google" -> App redirects user to Google's Auth page -> User logs in and grants consent -> Google redirects back to your App with an **Authorization Code** -> Your backend securely swaps that Code with Google for an **Access Token** -> Your backend uses the token to fetch user details.


### Phase 1: The Login Trigger & Password Check

- **Standard JWT Flow:** The user types their password into your frontend. Your backend (`AuthenticationManager`) takes that password, hashes it using `BCrypt`, and compares it to the hash in your database.
    
- **OAuth2 Flow:** Your backend **never sees a password**.
    
    1. The user clicks "Login with Google".
        
    2. Spring Security intercepts this and redirects the user's browser to Google's actual login page.
        
    3. The user enters their credentials on Google's server. **Google** does the password hashing and database checking.
        
    4. If successful, Google asks the user: _"Do you want to share your email and name with this App?"_ (This is the Consent phase).
        

### Phase 2: The "Code" and the Swap

- **Standard JWT Flow:** (This step doesn't exist. You already have the credentials).
    
- **OAuth2 Flow:** Google doesn't just hand over the user's data instantly. It is highly secure.
    
    1. Google redirects the user back to your backend with a temporary **Authorization Code** attached to the URL.
        
    2. Behind the scenes, Spring Security's `OAuth2LoginAuthenticationFilter` catches this code.
        
    3. Your server then makes a direct, hidden backend-to-backend call to Google. It says: _"Here is the Code, and here is my super-secret `client-secret` to prove I am the real backend."_
        
    4. Google verifies this and hands your backend an **Access Token** (This is Google's token, not yours).
        

### Phase 3: Fetching the User Details (The `UserDetailsService` Replacement)

- **Standard JWT Flow:** You use `UserDetailsService.loadUserByUsername()` to query your local Postgres/MySQL database to find the user's details.
    
- **OAuth2 Flow:** Spring Security uses the **Access Token** it just got to make a secure API call to Google's `/userinfo` endpoint. Google responds with a JSON object containing the user's email, name, and profile picture.
    
    - **The Code Equivalent:** Instead of implementing `UserDetailsService`, you extend **`DefaultOAuth2UserService`**. This is where you intercept the data coming back from Google.
        

### Phase 4: The Database Sync (Crucial Step)

- **Standard JWT Flow:** The user is already in your database, so you just assign them their roles (`GrantedAuthorities`) and put them in the `SecurityContext`.
    
- **OAuth2 Flow:** Just because they logged in with Google doesn't mean they exist in _your_ database yet.
    
    - Inside your custom `DefaultOAuth2UserService`, you take the email Google gave you and write logic like this:
        
        - _"Does this email exist in my database?"_
            
        - **If YES:** Great, grab their internal user ID and roles.
            
        - **If NO:** Automatically create a new row in your `users` table with their Google email and a default `USER` role. (This acts as a seamless auto-registration).
            

### Phase 5: Generating YOUR Token

- **Standard JWT Flow:** Once the password check passes, you generate a JWT using your app's secret key and send it to the frontend.
    
- **OAuth2 Flow:** Exactly the same!
    
    - Even though the user logged in via Google, you do **not** use Google's token to secure your own APIs.
        
    - You use an `AuthenticationSuccessHandler`. Once the Google flow is completely finished and the user is synced in your database, your backend generates its **own standard JWT**, just like normal.
        
    - You send _your_ JWT back to the React/Angular frontend. From this point forward, the frontend calls your APIs using your JWT, and your app behaves exactly as if they had logged in with a password.





**18. What are the 4 Roles in OAuth2?**

- **Resource Owner:** The user.
    
- **Client:** Your application.
    
- **Authorization Server:** The server verifying identity (Google, Okta, Keycloak).
    
- **Resource Server:** The API that holds the protected data.
    

**19. How do you implement SSO in Spring Boot?**

- Add the `spring-boot-starter-oauth2-client` dependency.
    
- Add the provider's `client-id` and `client-secret` to your `application.yml`.
    
- Call `.oauth2Login()` inside your `SecurityFilterChain`. Spring Boot handles the entire redirect and code-swap flow automatically.