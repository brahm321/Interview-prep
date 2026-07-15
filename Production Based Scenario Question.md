
### **Category 1: Debugging & Monitoring (The "Firefighter" Scenarios)**

**1. The CPU utilization of your Java application suddenly spikes to 100% in production. How do you troubleshoot it?**

- **Step 1:** SSH into the server and run the `top` command (or `top -H -p <pid>`) to find the exact Process ID (PID) and the specific Thread ID that is consuming the CPU.
    
- **Step 2:** Convert that Thread ID from decimal to hexadecimal.
    
- **Step 3:** Generate a thread dump using `jstack <pid> > dump.txt`.
    
- **Step 4:** Open the thread dump and search for that hexadecimal Thread ID. This will point you to the exact line of Java code causing the spike (usually an infinite loop, heavy garbage collection, or an unoptimized regex).
    

**2. Production is throwing `java.lang.OutOfMemoryError: Java heap space`. What is your step-by-step approach?**

- I would ensure the JVM is configured with the `-XX:+HeapDumpOnOutOfMemoryError` flag so it automatically generates a `.hprof` file when it crashes.
    
- I would pull that heap dump file from the server and open it in a profiling tool like **Eclipse MAT (Memory Analyzer Tool)**.
    
- I would look at the "Dominator Tree" or "Leak Suspects" report to see exactly which objects are hogging the memory (e.g., a massive `List` that was never cleared, or an open database connection loop).
    

**3. A user reports a bug saying "I clicked checkout and it failed," but there are no errors in your console logs. How do you trace it?**

- First, I would search our centralized logging tool (like Splunk, ELK, or Datadog) using the user's ID or email.
    
- To trace the exact request across multiple microservices, I would rely on **Correlation IDs** (using Spring Cloud Sleuth / Micrometer Tracing). Every request gets a unique trace ID attached to its header. I would search that Trace ID to see exactly where the request dropped.
    

### **Category 2: Performance & Scalability**

**4. One specific microservice is suddenly responding very slowly (latency went from 100ms to 5 seconds). How do you find the bottleneck?**

- I would check the APM (Application Performance Monitoring) tool, like New Relic or AppDynamics, to see the breakdown of the request time.
    
- **If the time is spent in the DB:** I would check for slow queries, missing indexes, or database locks.
    
- **If the time is spent in the Code:** I would look for thread contention or memory issues.
    
- **If the time is spent waiting on a 3rd party API:** I would implement a timeout and a Circuit Breaker to prevent it from backing up our system.
    

**5. How do you handle a sudden, massive spike in traffic that is overwhelming your backend?**

- **Short term (immediate mitigation):** Enable rate limiting (e.g., via API Gateway) to block abusive IPs, and heavily rely on caching (Redis) to serve read-heavy requests without hitting the database.
    
- **Long term:** Configure Kubernetes Horizontal Pod Autoscaling (HPA) to automatically spin up new instances of the service when CPU/Memory hits 70%.
    

**6. Your application is making too many database calls and slowing down. How do you optimize it?**

- **The N+1 Problem:** I would check Hibernate logs to see if fetching a parent object is triggering 100 separate queries for its children. I would fix this using `JOIN FETCH` or `@EntityGraph`.
    
- **Batching:** Instead of doing 1,000 separate `INSERT` statements in a loop, I would enable JDBC batching (`spring.jpa.properties.hibernate.jdbc.batch_size=50`).
    
- **Caching:** Put frequently accessed, rarely changing data (like country lists or product categories) into a Redis Cache.
    

### **Category 3: Database & Transactions**

**7. How do you configure a Spring Boot application to connect to two different databases (e.g., MySQL for users, Postgres for products)?**

- I cannot rely on Spring Boot's default auto-configuration.
    
- I would create two separate `@Configuration` classes.
    
- In each class, I would manually define three beans: a `DataSource`, a `LocalContainerEntityManagerFactoryBean` (for Hibernate), and a `PlatformTransactionManager`.
    
- I would mark the primary database's beans with the `@Primary` annotation so Spring knows which one to default to if unspecified.
    
- I would use `@EnableJpaRepositories` in each config class to point to the specific package containing the repositories for that database.
    

**8. Two users try to book the last available flight ticket at the exact same millisecond. How do you prevent a double-booking?**

- I would use **Optimistic Locking**.
    
- I would add an `@Version` integer field to the `Ticket` entity.
    
- When both users read the ticket, the version is `1`. When User A buys it, Hibernate updates the DB and increments the version to `2`.
    
- When User B's transaction tries to save, Hibernate sees the DB version is `2` but User B's object version is still `1`. It throws an `OptimisticLockException`, which I catch to tell User B the ticket is sold out.
    

**9. A database query that was blazing fast yesterday is suddenly taking 10 seconds today. No code was deployed. What happened?**

- The database likely experienced "Index Fragmentation" or stale statistics. As thousands of rows are inserted/deleted, the database's internal query planner gets confused and decides to do a "Full Table Scan" instead of using the index.
    
- _Solution:_ I would ask the DBA to run an `ANALYZE` or rebuild the indexes on that table. Alternatively, a newly inserted massive transaction might be holding a row-level lock.
    

### **Category 4: Microservices Resilience**

**10. Service A calls Service B. Service B crashes and goes offline. How do you prevent Service A from running out of threads and crashing too?**

- I would wrap the HTTP call in a **Circuit Breaker** (using Resilience4j).
    
- If Service B fails a certain percentage of times (e.g., 50% of requests fail), the circuit "opens."
    
- Once open, Service A stops calling Service B entirely and immediately returns a predefined **Fallback response** (like cached data or a default error message). This saves Service A's threads from hanging indefinitely.
    

**11. How do you ensure data consistency across multiple microservices (e.g., Order Service creates an order, but Payment Service fails)?**

- We cannot use traditional ACID database transactions across two different APIs.
    
- I would use the **Saga Pattern** (specifically, Choreography using Kafka).
    
- If the Order is created, it emits an `OrderCreated` event. If the Payment fails, the Payment service emits a `PaymentFailed` event. The Order Service listens for that failure and executes a **Compensating Transaction** (e.g., changing the order status to `CANCELLED`).
    

**12. Your service consumes messages from Kafka, but your primary Database suddenly goes down. What do you do with the Kafka messages?**

- I must **not** commit the Kafka offset. If I commit it, the message is lost.
    
- I would pause the Kafka consumer or throw an exception so the message goes to a **Dead Letter Queue (DLQ)**. Once the database is back online, we can replay the messages from the DLQ to process them safely.
    

### **Category 5: Deployments & DevOps**

**13. You merged a PR, it deployed to production, and it immediately broke critical functionality. What is your immediate action?**

- **Do not try to fix the bug in production.** The immediate action is always a **Rollback**.
    
- I would go to our CI/CD pipeline (Jenkins/GitHub Actions) or Kubernetes dashboard and revert the deployment to the previous stable Docker image tag. Once the system is healthy, I will debug the broken code locally.
    

**14. Passwords and API keys were accidentally pushed to GitHub. How should you actually manage secrets in production?**

- First, I would immediately revoke and regenerate the compromised keys.
    
- In a production environment, secrets should never be in `application.properties`.
    
- They should be injected at runtime using environment variables securely stored in a vault system, such as **HashiCorp Vault**, **AWS Secrets Manager**, or Kubernetes Secrets.
    

**15. How do you perform a Zero-Downtime Deployment for a Spring Boot application?**

- I would use a **Rolling Update** or a **Blue-Green Deployment**.
    
- **Blue-Green:** We have two identical production environments. Blue is currently live. We deploy the new code to Green, run our automated tests on it, and if it passes, we simply switch the Load Balancer to point traffic to Green. Blue is kept around as an instant rollback option.