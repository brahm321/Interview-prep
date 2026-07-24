
### 1. HTTP Methods and Idempotency

An operation is **Idempotent** if performing it multiple times has the same effect as performing it once.

| **Method** | **Action**                          | **Safe?** | **Idempotent?**  |
| ---------- | ----------------------------------- | --------- | ---------------- |
| **GET**    | Retrieve a resource.                | **Yes**   | **Yes**          |
| **POST**   | Create a new resource.              | No        | **No**           |
| **PUT**    | Update/Replace a resource entirely. | No        | **Yes**          |
| **PATCH**  | Partial update to a resource.       | No        | **No** (usually) |
| **DELETE** | Remove a resource.                  | No        | **Yes**          |

- **Safe Methods:** Methods that do not modify the server state (read-only).
    
- **POST is NOT idempotent:** If you click "Order" twice, you might be charged twice (two different records created).
    
- **PUT is idempotent:** If you update your profile name to "Brahmesh" ten times, the result is the same as doing it once.
    

---

### 2. Common HTTP Status Codes

These are the server's "status reports" back to the client.

- **2xx (Success)**
    
    - **200 OK:** Request succeeded.
        
    - **201 Created:** Request succeeded and a new resource was created (common for POST).
        
    - **204 No Content:** Request succeeded, but there is no data to send back (common for DELETE).
        
- **4xx (Client Error)**
    
    - **400 Bad Request:** The server cannot process the request due to client error (e.g., malformed JSON).
        
    - **404 Not Found:** The requested resource does not exist.
        
- **5xx (Server Error)**
    
    - **500 Internal Server Error:** The server encountered a situation it doesn't know how to handle (usually an unhandled exception in your Java code).
        

---

### 3. ResponseEntity in Spring Boot

In your Controllers, instead of returning a raw object, you should use `ResponseEntity<T>`. This allows you to control the **Status Code**, **Headers**, and **Body**.

Java

```java
@PostMapping("/orders")
public ResponseEntity<Order> createOrder(@RequestBody Order order) {
    Order savedOrder = orderService.save(order);
    
    // Returning 201 Created with the object in body
    return new ResponseEntity<>(savedOrder, HttpStatus.CREATED);
}
```

**Why use it?**

1. **Flexibility:** You can return different status codes based on logic (e.g., return `404` if an ID is missing or `200`if found).
    
2. **Custom Headers:** You can add metadata like pagination info or security tokens.
    

---

An interviewer might ask: _"If I want to change only the 'status' of a shipping container, should I use PUT or PATCH?"_

> **Your Answer:** "I should use **PATCH**. **PUT** is used for a full replacement of the resource; if I send a PUT request with only the 'status' field, I might accidentally nullify all other fields (like origin and destination). **PATCH** is specifically designed for partial updates, changing only the fields provided in the request body."

---

### Summary for your Interview

- **Safe:** GET (Doesn't change anything).
    
- **Idempotent:** GET, PUT, DELETE (Repeated calls = same result).
    
- **Not Idempotent:** POST (Repeated calls = multiple creations).



sample code

```java
import java.util.List;

@RestController
public class JobRESTcontroller {

    @Autowired
    private JobRepo repo;


    @RequestMapping("AllPosts")
    public List<Job> getAllJobs()
    {
        return repo.getAllJobs();
    }
}
```
Bhai, tu ne jo `@RequestMapping("AllPosts")` likha hai na — wo **by default** dono `GET` aur `POST` methods ke liye kaam karta hai. Isiliye ye dono requests handle ho rahi hain.

method = {RequestMethod.GET, RequestMethod.POST, RequestMethod.PUT, RequestMethod.DELETE}

It's ideal to use mappings like

| Annotation        | Accepts Methods |
| ----------------- | --------------- |
| `@RequestMapping` | All by default  |
| `@GetMapping`     | Only GET        |
| `@PostMapping`    | Only POST       |

Here's a short note for your latest learning:

---

**PathVariable in Spring Boot**

- `@PathVariable` is used to extract values from the URI path.
- Useful when the endpoint contains dynamic values.
- Example:
  ```java
  @GetMapping("/posts/{id}")
  public Job getJobById(@PathVariable("id") int id) {
      return service.getJobById(id);
  }
  ```
- Hitting `/posts/5` will pass `5` as the `id` to the method.

---


for multiple paths

```java
@GetMapping("/AllPosts/{id}/{name}")
public Job getJobByIdAndName(@PathVariable("id") int id, @PathVariable("name") String name) {
    // your logic
    return service.getJobByIdAndName(id, name);
}

```

In **Spring Boot**, `@PathVariable` and `@RequestParam` both take values from the URL, but from **different parts**.

### `@PathVariable`

Value is part of the **URL path**.

```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.getUser(id);
}
```

Request:

```text
GET /users/101
```

Here `101` → `id`.

**Use when the value identifies a resource.**

```text
/users/101
/orders/500
/products/20
```

---

### `@RequestParam`

Value comes from the **query parameter**.

```java
@GetMapping("/users")
public List<User> getUsers(
        @RequestParam String city) {
    return userService.getUsers(city);
}
```

Request:

```text
GET /users?city=Delhi
```

Here `Delhi` → `city`.

Useful for **filtering, sorting, pagination, optional inputs**.

```text
/users?page=1&size=10
/products?category=mobile
/users?sort=name
```

### Interview difference

|PathVariable|RequestParam|
|---|---|
|URL path|Query string|
|Identifies resource|Filters/configures request|
|`/users/101`|`/users?id=101`|
|Usually required|Can easily be optional|
|`@PathVariable`|`@RequestParam`|

**Best interview answer:**

> `@PathVariable` extracts a value from the URI path and is generally used to identify a specific resource, whereas `@RequestParam` extracts query parameters and is generally used for filtering, sorting, pagination, or optional request inputs.

For your interview prep, I can also explain the tricky question: **`/users/{id}` vs `/users?id=1` — both work, so why is PathVariable RESTful?**

## `/users/{id}` vs `/users?id=1`

Technically, **both can work**:

```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return service.getUser(id);
}
```

```text
GET /users/101
```

And:

```java
@GetMapping("/users")
public User getUser(@RequestParam Long id) {
    return service.getUser(id);
}
```

```text
GET /users?id=101
```

### Why is `/users/101` more RESTful?

Because in REST, the URL should represent a **resource**.

```text
/users        → User collection
/users/101    → Specific user resource
```

`101` is the **identity of the resource**, so it naturally belongs in the path.

Think:

```text
/orders/500
/products/20
/employees/123
```

### Then when should I use RequestParam?

When you're **modifying/filtering the result of a resource query**.

```text
/users?city=Delhi
/users?age=25
/users?page=0&size=10
/users?sort=name
```

Example:

```java
@GetMapping("/users")
public List<User> getUsers(
        @RequestParam(required = false) String city,
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size) {

    return service.getUsers(city, page, size);
}
```

## 🔥 Interview trap

**Interviewer:** Can I use `@RequestParam` instead of `@PathVariable` for ID?

**Answer:**

> Yes, technically we can use `@RequestParam` and the API will work. However, if the ID uniquely identifies a resource, `@PathVariable` is preferred because the URI clearly represents that specific resource and follows RESTful resource-oriented design. `@RequestParam` is more suitable for filtering, sorting, pagination, and optional parameters.

**One rule to remember:**

> **Identity → PathVariable**  
> **Filter / Options → RequestParam**

Next interview trap worth knowing: **`@RequestParam` vs `@RequestBody` vs `@ModelAttribute`** — Spring Boot interviewers ask this a lot.


Good question — **this will NOT work properly as-is**, because:

You're saying:

```java
@DeleteMapping("products/{ProductID}")
public void deleteProduct(@PathVariable int pid)
```

But your **path variable name is `ProductID`**, and your method parameter is `pid`, and you **didn't explicitly map them**.

---

### ✅ Correct Ways:

#### Option 1: Match names
```java
@DeleteMapping("products/{pid}")
public void deleteProduct(@PathVariable int pid)
```

#### Option 2: Explicit mapping
```java
@DeleteMapping("products/{ProductID}")
public void deleteProduct(@PathVariable("ProductID") int pid)
```

---

So short answer: **yes it can work**, but **only if you explicitly tell Spring which path variable maps to which method argument**, *when names differ*.

Want me to summarize other REST mappings (GET with query, POST with body, etc.)?

---

### ✅ Learned: Sending Data with `@RequestBody` in Spring Boot

To send data (like from Postman or a frontend) as **JSON** to your backend, use:

```java
@PostMapping("AllPosts")
public void AddJob(@RequestBody Job job) {
    service.addData(job);
}
```

- `@RequestBody` binds the incoming **JSON payload** to your Java object (`Job` here).
- Use `void` when you're not returning anything.
- Commonly used for **POST** or **PUT** operations where you're creating or updating data.
