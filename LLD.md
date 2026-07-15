
OOPS - Done

SOLID

S

The **Single Responsibility Principle (SRP)** is the most fundamental of the five. It states:

> **"A class should have one, and only one, reason to change."**

In simple terms, a class should do **one thing**. If a class handles database logic, UI formatting, and business calculations all at once, it’s a "God Object." When you change the database schema, you risk breaking the calculation logic.

---

## 1. The "Violation" (Bad Code)

Imagine you are building a simple system for your **MindBin** journaling app to manage entries.

Java

```java
public class JournalEntry {
    private String content;
    private String date;

    // Responsibility 1: Manage Entry Data
    public JournalEntry(String content, String date) {
        this.content = content;
        this.date = date;
    }

    public String getContent() { return content; }

    // Responsibility 2: Persistence (Saving to a file)
    // VIOLATION: If the saving logic changes (e.g., saving to DB instead of File), 
    // this class must change.
    public void saveToFile(String filename) {
        System.out.println("Saving entry to " + filename);
        // logic to write to disk...
    }

    // Responsibility 3: Notification
    // VIOLATION: If you change your email provider, this class must change.
    public void sendEmailNotification(String email) {
        System.out.println("Sending entry to " + email);
    }
}
```

### Why is this bad?

- **Fragility:** If you decide to use **Kafka** for notifications instead of email, you have to modify the `JournalEntry`class.

- **Testing:** To test the "content" logic, you might accidentally trigger a "save" or "email" action.

- **Coupling:** The business logic is tightly coupled with technical infrastructure (File I/O, SMTP).

---

## 2. The "Correction" (SOLID Code)

We break the class into three distinct pieces. Each has exactly **one** reason to change.

### A. The Domain Model

Only cares about the journal entry itself.

Java

```java
public class JournalEntry {
    private String content;
    private String date;

    public JournalEntry(String content, String date) {
        this.content = content;
        this.date = date;
    }
    // Getters and logic related to content only...
}
```

### B. The Repository (Persistence)

Only cares about **where** and **how** data is stored.

Java

```java
public class JournalRepository {
    public void save(JournalEntry entry) {
        System.out.println("Saving entry to Database...");
        // Database logic here
    }
}
```

### C. The Notification Service

Only cares about **alerting** the user.

Java

```java
public class NotificationService {
    public void notify(JournalEntry entry, String recipient) {
        System.out.println("Sending notification to " + recipient);
        // Email/Kafka/SMS logic here
    }
}
```

---

## 3. How to Identify a Violation

During an LLD interview, look for these "Code Smells":

1. **The "And" Description:** If you describe a class as "It holds user data **and** saves it **and** validates it," it's violating SRP.
2. **Giant Imports:** If a simple POJO class is importing `java.io.*`, `java.sql.*`, and `javax.mail.*`, it’s doing too much.
3. **Frequent Merges:** If two different developers are constantly hitting merge conflicts on the same class for unrelated features, the class has too many responsibilities.


---

## **O - Open/Closed Principle (OCP)**

Now that you've mastered **S**, let’s move to the second letter: **O**.

> **"Software entities (classes, modules, functions) should be open for extension, but closed for modification."**

- **Open for extension:** You should be able to add new functionality.
    
- **Closed for modification:** You should not have to change the existing, tested code to add that new functionality.
    

---

### **1. The Violation (Bad Code)**

Imagine you are building a **Payment Module** for a food delivery app. At first, you only support **Credit Cards**.

Java

```java
public class PaymentProcessor {
    public void processPayment(String type) {
        if (type.equals("CREDIT_CARD")) {
            System.out.println("Processing credit card payment...");
        } else if (type.equals("UPI")) {
            // VIOLATION: To add UPI, I had to come back and modify this existing class.
            System.out.println("Processing UPI payment...");
        }
    }
}
```

**Why is this bad?**

1. **Risk:** Every time you add a new payment method (like Crypto or NetBanking), you modify the `PaymentProcessor` class. You might accidentally break the Credit Card logic that was already working fine.


2. **Maintenance:** The `if-else` or `switch` block will eventually become massive and hard to read.
---

### **2. The Correction (Using Interfaces)**

To follow OCP, we use **Abstractions** (Interfaces or Abstract Classes). We "close" the processor and "open" it for new types via implementation.

#### **Step A: Create an Interface**

Java

```java
public interface PaymentMethod {
    void pay();
}
```

#### **Step B: Create Specific Implementations**

Java

```java
public class CreditCardPayment implements PaymentMethod {
    public void pay() {
        System.out.println("Paying via Credit Card");
    }
}

public class UpiPayment implements PaymentMethod {
    public void pay() {
        System.out.println("Paying via UPI");
    }
}
```

#### **Step C: The "Closed" Processor**

Java

```java
public class PaymentProcessor {
    // This class is now CLOSED for modification. 
    // It doesn't care if you add 100 new payment types.
    public void process(PaymentMethod method) {
        method.pay();
    }
}
```

---

## **3. Why this matters for your LLD Prep**

In an interview for a system like **Uber** or **Swiggy**:

- **The Scenario:** "How would you handle different pricing strategies (Surge pricing, Holiday pricing, Rain pricing)?"
    
- **The SOLID Answer:** "I would use the **Open/Closed Principle**. I'll create a `PricingStrategy` interface. For every new scenario, I'll just add a new class (e.g., `RainyDayPricing`) without touching the core `BookingService` code."


You’ve hit the nail on the head! It is **very** closely related to polymorphism. Specifically, the Open/Closed Principle (OCP) is the "architectural version" of **Runtime Polymorphism**.

However, there is a small correction to make regarding the "return type" part. Let’s break it down in "normal language."

---

## **L - Liskov Substitution Principle (LSP)**

Since you mentioned polymorphism, this is the perfect time for the third letter: **L**. This principle is the "Rulebook" for Polymorphism.

> **"Objects of a superclass should be replaceable with objects of its subclasses without breaking the application."**

In simple words: **A subclass should not "downgrade" or "break" the behavior of the parent class.**

### **The Classic Violation: The Square-Rectangle Problem**

In math, a Square is a Rectangle. But in OOPS, this can break code.

Java

```java
public class Rectangle {
    protected int width, height;
    public void setWidth(int w) { this.width = w; }
    public void setHeight(int h) { this.height = h; }
    public int getArea() { return width * height; }
}

public class Square extends Rectangle {
    // VIOLATION: In a square, width and height must be equal.
    // If a user sets width, we have to change height too.
    @Override
    public void setWidth(int w) {
        this.width = w;
        this.height = w; 
    }
}
```

**Why it breaks:**

If a developer writes a test case that expects a `Rectangle` to have an area of 20 ($5 \times 4$), and you pass a `Square` instead, the code will fail because setting the width to 5 will also make the height 5 (Area = 25).

**The Fix:** Don't force a relationship if the behavior is different. Square and Rectangle should likely both implement a `Shape` interface instead.

---

### **Summary of S, O, L so far:**

- **S:** One class, one job.

- **O:** Use Interfaces so you don't have to edit old code to add new features.

- **L:** If you extend a class, don't change its basic "expected" behavior.

## **I - Interface Segregation Principle (ISP)**

This one is very simple: **"No client should be forced to depend on methods it does not use."**

In "normal language": Don't create a "Fat Interface" that has 20 different methods. If a class only needs 2 of them, it shouldn't be forced to implement the other 18 with empty bodies or `null` returns.

### **The Violation (Bad Code)**

Imagine a `Worker` interface for a food delivery system like **Swiggy**.

Java

```java
public interface SmartDevice {
    void print();
    void scan();
    void fax();
}

public class BasicPrinter implements SmartDevice {
    public void print() {
        System.out.println("Printing...");
    }

    // VIOLATION: This printer can't scan, but it's forced to implement it.
    public void scan() {
        throw new UnsupportedOperationException("I can't scan!");
    }

    public void fax() {
        throw new UnsupportedOperationException("I can't fax!");
    }
}
```

### **The Correction (Split it up)**

Break the big interface into smaller, specific ones.

Java

```java
public interface Printer { void print(); }
public interface Scanner { void scan(); }

public class BasicPrinter implements Printer {
    public void print() { System.out.println("Printing..."); }
}

public class AllInOneScannerPrinter implements Printer, Scanner {
    public void print() { /* logic */ }
    public void scan() { /* logic */ }
}
```

---

## **D - Dependency Inversion Principle (DIP)**

This is the "heart" of **Spring Boot** (Dependency Injection). It states:

1. **High-level modules should not depend on low-level modules. Both should depend on abstractions.**
    
2. **Abstractions should not depend on details. Details should depend on abstractions.**
    

### **The Violation (Bad Code)**

If your High-Level service directly creates an instance of a Low-Level tool, they are "tightly coupled."

Java

```java
public class NotificationService {
    // VIOLATION: High-level service depends on a concrete "Email" class.
    private EmailClient emailClient = new EmailClient(); 

    public void send(String msg) {
        emailClient.sendEmail(msg);
    }
}
```

**Problem:** If you want to switch to **SMS** or **Kafka**, you have to change the `NotificationService` code.

### **The Correction (Invert the Dependency)**

Introduce an interface. Now, the Service doesn't care _who_ sends the message, as long as they follow the rules.

Java

```java
public interface MessageSender {
    void send(String msg);
}

public class NotificationService {
    private MessageSender sender;

    // We "Inject" the dependency via constructor
    public NotificationService(MessageSender sender) {
        this.sender = sender;
    }

    public void send(String msg) {
        sender.send(msg);
    }
}
```

---

### 1. The Bad Code (Tightly Coupled)

The `OrderService` is "married" to `ConsoleLogger`. If you want to log to a **File** or **CloudWatch** later, you have to rewrite the `OrderService`.

Java

```java
// Low-level tool
class ConsoleLogger {
    public void log(String msg) {
        System.out.println("Console: " + msg);
    }
}

// High-level Business Logic
class OrderService {
    // VIOLATION: Directly creating the concrete class
    private ConsoleLogger logger = new ConsoleLogger();

    public void placeOrder() {
        logger.log("Order placed successfully!");
    }
}
```

---

### 2. The Good Code (DIP applied)

We create a "Socket" (Interface). Now `OrderService` doesn't care _how_ the logging happens.

Java

```java
// 1. The Interface (The "Socket")
interface Logger {
    void log(String msg);
}

// 2. The Implementations (The "Plugs")
class ConsoleLogger implements Logger {
    public void log(String msg) { System.out.println("Console: " + msg); }
}

class FileLogger implements Logger {
    public void log(String msg) { System.out.println("Writing to file: " + msg); }
}

// 3. The High-level Logic (Independent)
class OrderService {
    private final Logger logger;

    // DIP: We pass the Interface. 
    // This is exactly what Spring Boot's Constructor Injection does!
    public OrderService(Logger logger) {
        this.logger = logger;
    }

    public void placeOrder() {
        logger.log("Order placed successfully!");
    }
}
```

---

### Why this is the "Simplest" explanation:

- **The Problem:** In the bad code, `OrderService` **controls** the logger (it creates it).
    
- **The Inversion:** In the good code, the **caller** (like Spring) controls the logger. `OrderService` just says, "I need _any_ logger that follows the rules."
    

**Key takeaway for your interview:** Whenever you see the `new` keyword inside a high-level class to create a low-level tool (like `new EmailClient()`, `new KafkaProducer()`), it is a **DIP violation**. Use an Interface and pass it through the constructor instead.

Ready for **Design Patterns**? We can start with the **Factory Pattern**!
## **Summary Table for your LLD Interview**

| **Letter** | **Principle**         | **Summary in 1 Sentence**                                     |
| ---------- | --------------------- | ------------------------------------------------------------- |
| **S**      | Single Responsibility | A class should do one thing only.                             |
| **O**      | Open/Closed           | Add new features by adding new classes, not editing old ones. |
| **L**      | Liskov Substitution   | Subclasses must be able to stand in for their parents.        |
| **I**      | Interface Segregation | Keep interfaces small and specific.                           |
| **D**      | Dependency Inversion  | Depend on Interfaces, not concrete Classes (Spring's core).   |

## Practice Code: The Notification Factory

Imagine you are building a notification system. Depending on the user's preference, you create different senders.

Java

```java
// 1. Product Interface
interface Notification {
    void notifyUser();
}

// 2. Concrete Products
class EmailNotification implements Notification {
    public void notifyUser() { System.out.println("Sending an Email..."); }
}

class SMSNotification implements Notification {
    public void notifyUser() { System.out.println("Sending an SMS..."); }
}

// 3. The Factory
class NotificationFactory {
    // This is the "Creation Logic"
    public Notification createNotification(String channel) {
        if (channel == null || channel.isEmpty()) return null;
        
        // You can use your Map-based "No If-Else" logic here!
        switch (channel.toUpperCase()) {
            case "SMS": return new SMSNotification();
            case "EMAIL": return new EmailNotification();
            default: throw new IllegalArgumentException("Unknown channel");
        }
    }
}

// 4. Usage
public class FactoryDemo {
    public static void main(String[] args) {
        NotificationFactory factory = new NotificationFactory();
        
        Notification note = factory.createNotification("EMAIL");
        note.notifyUser();
    }
}
```

---

## 4. Why do we need it? (Interview Perspective)

If the interviewer asks: **"Brahmesh, why not just use `new EmailNotification()`?"**

You give these 3 reasons:

1. **Loose Coupling:** The client code doesn't need to know the name of the concrete class (`EmailNotification`). It only knows the Interface (`Notification`).
2. **Abstraction of Complexity:** If creating an object requires 5 steps (setting headers, authenticating, etc.), the Factory hides all that mess. The client just says "Give me an Email object."
3. **Single Point of Change:** If you rename `EmailNotification` to `GmailNotification`, you only change the Factory. The rest of the app remains untouched.

---
## Real-World Example in Spring Boot

You see this everywhere in Java:

- `Calendar.getInstance()`
- `LoggerFactory.getLogger()`
- `BeanPostProcessor` (Spring uses factories to create your beans!)


## Step 0: You already understand these correctly ✅

Let me confirm first (this is important for confidence):

### ✅ Eager singleton

```java
static User obj = new User();
```

✔ Object created **when class loads**
✔ Even if `getInstance()` is never called
✔ Wastes memory sometimes

You are 100% right.

---

### ✅ Lazy singleton (non-thread-safe)

```java
if (obj == null) {
    obj = new User();
}
```

✔ Object created **only when needed**
❌ Two threads can create **two objects**

You are 100% right.

---

### ✅ Synchronized method (thread-safe but slow)

```java
public static synchronized User getInstance()
```

✔ Only one thread enters at a time
✔ No duplicate objects
❌ Every call is synchronized → slow

You are 100% right.

---

So far, **perfect understanding** 👌
Now comes the **only confusing part**.

---

## Now the REAL question

> **Why double-checked locking?
> What exactly is happening?
> Why is it faster?
> Why do we need two `if (obj == null)` checks?**

Let’s answer ONLY this.

---

## Step 1: Why synchronized method is slow

Your synchronized version:

```java
public static synchronized User getInstance()
```

### What JVM does:

* Thread enters method → acquires lock
* Even **after object is created**
* Every thread still waits for lock

So after this:

```java
User s1 = User.getInstance();
```

Even later calls like:

```java
User s2 = User.getInstance();
```

👉 **still synchronized**, even though object already exists
👉 This is unnecessary locking

---

## Step 2: Key observation (THIS IS THE CORE IDEA)

> **We only need synchronization when the object is NOT created.**

Once `obj` is created:

* No more object creation
* No race condition
* No need to lock

💡 This observation leads to **double-checked locking**.

---

## Step 3: First `if (obj == null)` — FAST PATH

Your code:

```java
if (obj == null)
```

### What this does:

* 99% of the time → `obj` is NOT null
* Thread **returns immediately**
* NO synchronization
* Very fast

So for already-created object:

```java
return obj;
```

No lock, no waiting.

---

## Step 4: Why synchronized block INSIDE?

```java
if (obj == null) {
    synchronized (User.class) {
        if (obj == null) {
            obj = new User();
        }
    }
}
```

This part only runs **ONCE**, when object is first created.

### Scenario with two threads

Let’s walk it like a movie 🎬

---

### 🧵 Thread T1

```java
if (obj == null) → TRUE
```

Enters synchronized block.

---

### 🧵 Thread T2 (at same time)

```java
if (obj == null) → TRUE
```

But cannot enter synchronized block (T1 has lock).

---

### 🧵 Thread T1

```java
if (obj == null) → TRUE
obj = new User();   // object created
```

Releases lock.

---

### 🧵 Thread T2 enters synchronized block

```java
if (obj == null) → FALSE
```

So **no new object created**.

✅ Only one object created
✅ Thread-safe
✅ Synchronization happens only once

---

## Step 5: WHY second `if (obj == null)` is REQUIRED

This is the most common confusion.

### ❌ WRONG version (very dangerous)

```java
if (obj == null) {
    synchronized (User.class) {
        obj = new User(); // ❌
    }
}
```

### Why this is wrong?

Thread T1 creates object.
Thread T2 was waiting.

When T2 enters synchronized:

* `obj` is **already created**
* But T2 still creates **another object**

💥 TWO objects created
💥 Singleton broken

That’s why the **second null check is mandatory**.

---

## Step 6: One FINAL but CRITICAL thing — `volatile`

Your code is **almost correct**, but **missing one keyword**.

### Correct double-checked locking

```java
class User {

    private static volatile User obj;

    private User() {
        System.out.println("Creating");
    }

    public static User getInstance() {
        if (obj == null) {
            synchronized (User.class) {
                if (obj == null) {
                    obj = new User();
                }
            }
        }
        return obj;
    }
}
```

---

## Why `volatile` is REQUIRED (simple explanation)

Without `volatile`, JVM can reorder steps:

```text
1. Allocate memory
2. Assign obj reference
3. Run constructor
```

Another thread may see:

```java
obj != null
```

BUT constructor not finished 😱

This leads to:

* Half-constructed object
* Very rare but deadly bug

🔥 `volatile` prevents this reordering.

When you run `instance = new DatabaseConnection();`, the CPU/JVM performs three steps:

1. **Memory:** Squeeze out some space in memory for the object.
    
2. **Initialize:** Run the constructor (set the variables, connect to DB, etc.).
    
3. **Publish:** Point the `instance` variable to that memory address.
    

**The Problem:** The JVM might reorder these to **1 -> 3 -> 2** for optimization.

---

## Final mental picture (PLEASE remember this)

```
First if      → avoids locking (fast)
synchronized  → ensures only one creator
Second if     → prevents duplicate creation
volatile      → prevents broken object visibility
```

---

## Now compare all versions in ONE table

| Version             | Lazy | Thread-Safe | Fast |
| ------------------- | ---- | ----------- | ---- |
| Eager               | ❌    | ✅           | ❌    |
| Lazy (no sync)      | ✅    | ❌           | ✅    |
| Synchronized method | ✅    | ✅           | ❌    |
| Double-checked      | ✅    | ✅           | ✅    |
| Enum                | ✅    | ✅           | ✅✅   |

---

## About Enum (why it’s best)

Your enum example is correct ✔
Why it wins:

* JVM guarantees single instance
* Thread-safe
* Serialization-safe
* Reflection-safe
* No `volatile`, no `synchronized`, no headache

That’s why **real projects prefer enum**.

```java
public enum DatabaseConnection {
    INSTANCE; // This is the single instance

    // You can add fields just like a normal class
    private String connectionUrl = "jdbc:mysql://localhost:3306/nftp-prod";

    public void connect() {
        System.out.println("Connecting to: " + connectionUrl);
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        // You don't use 'new'. You just access the INSTANCE.
        DatabaseConnection connection = DatabaseConnection.INSTANCE;
        connection.connect();
    }
}
```
---

## **Builder Pattern**

Perfect 👍
Here is **ONE clean Java program** that **demonstrates EVERYTHING** you just explained:

* ❌ Problem with constructor
* ✅ How Builder fixes it
* Clear, readable, interview-ready

No extra patterns, no noise.

---

## ❌ Normal Constructor Approach (Problem)

```java
class House {

    private int doors;
    private int windows;
    private String shape;
    private boolean hasGarage;
    private boolean hasGarden;

    // constructor becomes confusing
    public House(int doors, int windows, String shape,
                 boolean hasGarage, boolean hasGarden) {
        this.doors = doors;
        this.windows = windows;
        this.shape = shape;
        this.hasGarage = hasGarage;
        this.hasGarden = hasGarden;
    }

    public void show() {
        System.out.println(
            "doors=" + doors +
            ", windows=" + windows +
            ", shape=" + shape +
            ", garage=" + hasGarage +
            ", garden=" + hasGarden
        );
    }
}
```

### ❌ Usage (confusing)

```java
House h1 = new House(1, 2, "Cone", true, false);
```

👉 You **don’t know** what `true, false` means
👉 Order matters
👉 Adding new fields breaks constructor

---

## ✅ Builder Pattern (Solution)

```java
class House {

    private int doors;
    private int windows;
    private String shape;
    private boolean hasGarage;
    private boolean hasGarden;

    // private constructor
    private House(Builder builder) {
        this.doors = builder.doors;
        this.windows = builder.windows;
        this.shape = builder.shape;
        this.hasGarage = builder.hasGarage;
        this.hasGarden = builder.hasGarden;
    }

    // static inner Builder class
    public static class Builder {

        private int doors;
        private int windows;
        private String shape;
        private boolean hasGarage;
        private boolean hasGarden;

        public Builder doors(int doors) {
            this.doors = doors;
            return this;
        }

        public Builder windows(int windows) {
            this.windows = windows;
            return this;
        }

        public Builder shape(String shape) {
            this.shape = shape;
            return this;
        }

        public Builder garage(boolean hasGarage) {
            this.hasGarage = hasGarage;
            return this;
        }

        public Builder garden(boolean hasGarden) {
            this.hasGarden = hasGarden;
            return this;
        }

        public House build() {
            return new House(this);
        }
    }

    public void show() {
        System.out.println(
            "doors=" + doors +
            ", windows=" + windows +
            ", shape=" + shape +
            ", garage=" + hasGarage +
            ", garden=" + hasGarden
        );
    }
}
```

---

## ✅ Usage (CLEAR & READABLE)

```java
public class Main {
    public static void main(String[] args) {

        House house = new House.Builder()
                .doors(1)
                .windows(2)
                .shape("Cone")
                .garage(true)
                .build();

        house.show();
    }
}
```

---

## 🧠 What this program shows (IMPORTANT)

### ❌ Constructor problems

* Too many parameters
* Hard to read
* Order-dependent
* Breaks easily when fields increase

### ✅ Builder advantages

* Step-by-step creation
* Self-documenting code
* Optional fields supported
* Works great with immutability

---

## 🔑 One-line you should say in interview

> **Builder pattern solves constructor complexity by providing readable, step-by-step object creation.**

---

## ✅ Final confidence check

You now have:

* **Singleton** → object control
* **Builder** → clean object construction

```java
package com.brahmesh;  
import lombok.Builder;  
import lombok.Getter;  
import lombok.ToString;  
  
@Getter  
@Builder  
@ToString  
class House {  
  
    private final int doors;  
  
    @Builder.Default  
    private final int windows = 2;  
  
    @Builder.Default  
    private final String shape = "Flat";  
  
    @Builder.Default  
    private final boolean hasGarage = false;  
  
    @Builder.Default  
    private final boolean hasGarden = false;  
}  
  
  
public class hello {  
    public static void main(String[] args) {  
  
        House house = House.builder()  
                .doors(1).build();  
  
        System.out.println(house);    }  
}
```



To visualize the Observer Pattern and implement it in Java, we will build upon the **Weather Station** concept introduced in the previous example. This example remains the most straightforward way to understand the relationships between the Subject and its Observers.

Here is a visual breakdown and the corresponding Java implementation.

### Visualization of the Observer Pattern - 

We must understand that there are two roles: the **Subject** (the one with the data) and the **Observers** (the ones who want the data).

1. The Subject maintains a list of Observers (subscribers).
    
2. Observers use a method (like `register()`) to add themselves to that list.
    
3. When the Subject's data changes, it "broadcasts" the update to everyone on its list.
    

### Weather Station Example in Java

In this example, the `WeatherStation` is the Subject (the source of truth for temperature). We will create different "Display" devices as the Observers.

We will use interfaces to define the "contracts" (the behavior) that the Subject and Observers must adhere to. This ensures that the WeatherStation does not care which _specific_ display it is talking to, as long as that display knows how to `update()`.

#### Step 1: Define the Interfaces (Contracts)

Java

```java
// Observer Interface: Anything that wants to be notified must implement this.
interface Observer {
    // The Subject calls this method to push the new data.
    void update(float temperature);
}

// Subject Interface: The central data source that is being 'observed'.
interface Subject {
    // Methods to manage subscribers
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    
    // Method to send notifications when data changes
    void notifyObservers();
}
```

#### Step 2: Implement the Concrete Subject

The `WeatherStation` implements the `Subject` interface. It holds the data (temperature) and the list of subscribers.

Java

```java
import java.util.ArrayList;
import java.util.List;

// Concrete Subject
class WeatherStation implements Subject {
    // The list of interested observers
    private List<Observer> observers;
    private float temperature;

    public WeatherStation() {
        // We use an ArrayList to store the registered displays
        observers = new ArrayList<>();
    }

    @Override
    public void registerObserver(Observer o) {
        observers.add(o);
        System.out.println("LOG: Display Added.");
    }

    @Override
    public void removeObserver(Observer o) {
        observers.remove(o);
        System.out.println("LOG: Display Removed.");
    }

    // The core of the pattern: Loop through the list and call update() on each.
    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature);
        }
    }

    // This method simulates the station getting new data (e.g., from a sensor).
    public void setMeasurements(float temperature) {
        System.out.println("\n--- Weather Station: New Measurement Received: " + temperature + "C ---");
        this.temperature = temperature;
        // The data changed! We must tell our observers immediately.
        notifyObservers();
    }
}
```

#### Step 3: Implement the Concrete Observers (The Displays)

Each display (Phone, Window) implements the `Observer` interface. They define how they handle the new `temperature` data.

Java

```java
// Concrete Observer A
class PhoneDisplay implements Observer {
    private float lastTemperature;

    // When update() is called, this display simply shows the data.
    @Override
    public void update(float temperature) {
        this.lastTemperature = temperature;
        display();
    }

    public void display() {
        System.out.println(">>> PHONE DISPLAY (Current Conditions): " + lastTemperature + "C");
    }
}

// Concrete Observer B
class WindowDisplay implements Observer {
    private float currentTemperature;

    @Override
    public void update(float temperature) {
        this.currentTemperature = temperature;
        // This display might do something slightly different,
        // like rendering it visually on smart glass.
        displayVisual();
    }

    public void displayVisual() {
        System.out.println(">>> WINDOW DISPLAY (Rendering): " + currentTemperature + "C");
    }
}
```

#### Step 4: Run the Simulation

Java

```java
public class ObserverPatternExample {
    public static void main(String[] args) {
        // 1. Create the central Subject (Weather Station)
        WeatherStation weatherStation = new WeatherStation();

        // 2. Create the Observers (Displays)
        PhoneDisplay phone = new PhoneDisplay();
        WindowDisplay window = new WindowDisplay();

        // 3. Subscribing: Register the displays with the station
        // The station doesn't know *what* these displays are, only that they
        // are 'Observers'.
        weatherStation.registerObserver(phone);
        weatherStation.registerObserver(window);

        // 4. Simulate weather changes. This triggers notifyObservers()
        // both phone and window are updated automatically.
        weatherStation.setMeasurements(25.5f);
        weatherStation.setMeasurements(28.0f);

        // 5. Unsubscribing: The window display is turned off/removed.
        weatherStation.removeObserver(window);

        // 6. Simulate another change. Only the phone gets updated.
        weatherStation.setMeasurements(22.1f);
    }
}
```

### Explanation of the Output

If you run this code, the output will clearly demonstrate the automatic notifications:

Plaintext

```js
LOG: Display Added.
LOG: Display Added.

--- Weather Station: New Measurement Received: 25.5C ---
>>> PHONE DISPLAY (Current Conditions): 25.5C
>>> WINDOW DISPLAY (Rendering): 25.5C

--- Weather Station: New Measurement Received: 28.0C ---
>>> PHONE DISPLAY (Current Conditions): 28.0C
>>> WINDOW DISPLAY (Rendering): 28.0C
LOG: Display Removed.

--- Weather Station: New Measurement Received: 22.1C ---
>>> PHONE DISPLAY (Current Conditions): 22.1C
```


The **Decorator Pattern** is a structural design pattern that allows you to dynamically attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors.

Think of it like **clothing**. You are a person (the Base Object). If you’re cold, you put on a sweater (Decorator 1). If it starts raining, you put on a raincoat (Decorator 2). You haven't changed your identity, but you've added "warmth" and "waterproofing" behaviors dynamically.

---

## Why Should We Use It?

In traditional programming, you might use **Inheritance** to extend functionality. However, inheritance is static and leads to "Class Explosion."

- **Inheritance Problem:** If you have a `Coffee` class and want `CoffeeWithMilk`, `CoffeeWithSugar`, and `CoffeeWithMilkAndSugar`, you end up creating dozens of subclasses for every possible combination.

- **Decorator Solution:** You create a base `SimpleCoffee` and "wrap" it with a `MilkDecorator` and a `SugarDecorator` at runtime.
---

## Real-Life Example: The Pizza Order

Imagine a Pizza shop. You start with a **Plain Pizza**.

1. You want **Extra Cheese**? We wrap the pizza in a cheese layer.

2. You want **Mushrooms**? We wrap that "Cheese-Pizza" in a mushroom layer.

3. The final `getPrice()` method calls the mushroom price + the cheese price + the base pizza price.
---

## Sample Code (Java)

Here is how you would implement that dynamic Coffee example in Java.

### 1. The Component Interface

Java

```java
public interface Coffee {
    String getDescription();
    double getCost();
}
```

### 2. The Concrete Component (The "Base")

Java

```java
public class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }

    @Override
    public double getCost() {
        return 5.0; // Base price
    }
}
```

### 3. The Abstract Decorator

This class implements the interface and **holds a reference** to a Coffee object.

Java

```java
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }

    public String getDescription() {
        return decoratedCoffee.getDescription();
    }

    public double getCost() {
        return decoratedCoffee.getCost();
    }
}
```

### 4. Concrete Decorators

Java

```java
class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) { super(coffee); }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost() + 1.5;
    }
}

class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) { super(coffee); }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Sugar";
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost() + 0.5;
    }
}
```

### 5. Running the Code

Java

```java
public class Main {
    public static void main(String[] args) {
        // Start with simple coffee
        Coffee myCoffee = new SimpleCoffee();
        
        // Add milk
        myCoffee = new MilkDecorator(myCoffee);
        
        // Add sugar
        myCoffee = new SugarDecorator(myCoffee);

        System.out.println("Order: " + myCoffee.getDescription());
        System.out.println("Total Cost: $" + myCoffee.getCost());
    }
}
```

---

## How to Visualize the Execution

When you call `getCost()` on the final object, it triggers a "recursive" call through the layers:

1. **SugarDecorator** asks: "What is the cost of everything inside me? (Inner is Milk + Simple)" then adds $0.5.
2. **MilkDecorator** asks: "What is the cost of the inner coffee? (Simple)" ... then adds $1.5.
3. **SimpleCoffee** returns $5.0.


Strategy

Converting a **Factory Pattern** to a **Strategy Pattern** is a shift in intent.

In your current **Factory** code, the focus is on _creating_ the object. In the **Strategy** pattern, the focus is on _passing_ the behavior to a "Context" class that uses it. Instead of asking a factory for an object, you provide a specific strategy to a manager that executes the work.

### The Key Difference

- **Factory**: "Give me the right tool based on this string."
    
- **Strategy**: "Here is the tool I want you to use to perform this specific task."
    

---

### Refactoring to Strategy Pattern

To implement this, we introduce a **Context** class (e.g., `NotificationService`). This class doesn't care if it's using Email or Kafka; it just knows how to trigger a `Notification` strategy.

Java

```java
package com.brahmesh;

// 1. The Strategy Interface (Remains the same)
interface Notification {
    void getNotify();
}

// 2. Concrete Strategies
class Email implements Notification {
    public void getNotify() {
        System.out.println("Executing Email Strategy");
    }
}

class Kafka implements Notification {
    public void getNotify() {
        System.out.println("Executing Kafka Strategy");
    }
}

// 3. The Context Class
// This is what makes it a Strategy Pattern. It "has-a" strategy.
class NotificationContext {
    private Notification strategy;

    // You can set the strategy via Constructor
    public NotificationContext(Notification strategy) {
        this.strategy = strategy;
    }

    // Or you can change it at runtime via a Setter
    public void setStrategy(Notification strategy) {
        this.strategy = strategy;
    }

    public void executeNotification() {
        if (strategy == null) {
            System.out.println("No strategy set!");
            return;
        }
        strategy.getNotify();
    }
}

// 4. Client Code
public class hello {
    public static void main(String args[]) {
        // We decide the strategy here
        NotificationContext context = new NotificationContext(new Email());
        context.executeNotification(); // Output: Executing Email Strategy

        // We can swap the behavior at runtime without creating a new Context
        context.setStrategy(new Kafka());
        context.executeNotification(); // Output: Executing Kafka Strategy
    }
}
```

---

### Comparison Table

|**Feature**|**Factory Pattern**|**Strategy Pattern**|
|---|---|---|
|**Primary Goal**|**Creational**: To instantiate objects without exposing logic.|**Behavioral**: To make algorithms interchangeable within a class.|
|**Decision Maker**|The Factory decides which class to instantiate.|The Client decides which strategy to give to the Context.|
|**Usage**|`factory.create("type")`|`context.setStrategy(new SpecificStrategy())`|
|**Structure**|Returns an instance of an interface.|Holds a reference to an interface.|

### When to use which?

You use **Factory** when you have many subclasses and you want to centralize the `if/else` or `switch` logic for creation. You use **Strategy** when you want to change **how** an object behaves at runtime (e.g., switching between different payment methods or different compression algorithms).


