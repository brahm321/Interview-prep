  **24/09/2024**

![[Screenshot 2024-09-24 at 23.17.46.png]]
So basically i will write a java code (.java file) then it gets compiled by java compiler (javac) then it will get converted to byte code that .class file, then JVM will run the byte code for only one and will run only that file that has a signature in our case it is public static void main ( String a [ ]) then it will run it with the libraries installed within . JDK ke ander JRE ke and uske ander JVM and Java can run only in those environment that JRE and JDK so it’s not totally platform depenedent. but if you write code it can run everywhere that’s why java called write once run everywhere.

So, while Java is not fully platform-independent (because of the need for a platform-specific JVM), its bytecode is portable, allowing your code to run on any machine with the appropriate JVM, which is why Java has the slogan "write once, run everywhere.”

```java
class hello 
    { 
        public static void main(String a[]) 
           { 
                System.out.println("MOTU"); 
           }
    }
```

for the file [hello.java](http://hello.java/)

- `javac hello.java` → Generates `hello.class` (bytecode).
- `java hello` → Executes the `hello.class` file using the JVM, and prints `"MOTU"` to the console.

When you are processing or manipulating data in your program, you need to store the data somewhere temporarily. **Variables** provide a way to hold that data in memory while the program is running.

Data type 
-Primitive
- Integer
- Float
- Character
- Boolean

-Integer
- **`byte`**: **Range:** −27−27 to 27−127−1 (i.e., -128 to 127)
- **`short`**: **Range:** −215−215 to 215−1215−1 (i.e., -32,768 to 32,767)
- **`int`**: **Range:** −231−231 to 231−1231−1 (i.e., -2,147,483,648 to 2,147,483,647)
- **`long`**: **Range:** −263−263 to 263−1263−1 (i.e., -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807)

Java usually works with double 8 bits because it gives more precision and for float you have to directly say like 

```java
double x = 9.213123213;
float f = 9.21f;
long l = 3243242342l; // same with long
char k = 'd'; //char take 2 bytes // java works on unicode that's why 2 bytes
boolean b = true;
```

**25/09/2024**

```java
class hello {

public static void main(String a[]) {

//literals

int x = 0b101;
System.out.println(x); // gives 5 because 101 is binary for 5

int y = 0x7e;
System.out.println(y); // hexadecimal thing gives 126

int num = 10000_000_00;
System.out.println(num); // gives real num that is 1000000000, - to diffrentiate between zeros

}
}
```
n mathematical terms:

12e10=12×1010=12,000,000,000,00012e10=12×1010=12,000,000,000,000

Here's how it works:

- `12` is the base.
- `e10` means "times 10 to the power of 10" (where "e" stands for exponent).
- The result is 12 followed by 10 zeros: `12,000,000,000,000`.
```java
double largeNumber = 12e10;
System.out.println(largeNumber);// Output: 1.2E12 or 12000000000000.0
char k = 'd';
k++; // will gives e
```

```java
//expicit type casting

byte a = 25;
int x;
x = a;
System.out.println(a); // this will work because we are assigning a byte to int , since int has a longer raneg no issue

int b = 90; //it's in the byte range right
// a = b;
System.out.println(a); // it will give error due to conversion we can't directly assign a integer to a byte
a = (byte)b;
System.out.println(a); // this will work

// suppose value exceeding in int for a byte then it will do modulo with given range
int c = 890;
a = (byte)c;
System.out.println(a); // gives 122

//type promotion
byte z = 20;
byte y = 60;
// so the output will be byte but promotes to int
int res = z*y;
System.out.println(res);

```

```java
class calc
{
public int add(int num1 , int num2)
{
return num1+num2;
}
}

public class hello 
{
public static void main(String args[])
{
int num1 = 2; int num2 = 6;
calc ab = new calc();
int x = ab.add(num1,num2);
System.out.println(x);
}
}
```

### Differences Between Stack and Heap:

| Feature           | Stack                                         | Heap                                                    |
| ----------------- | --------------------------------------------- | ------------------------------------------------------- |
| **Memory Type**   | Local variables, method calls                 | Objects, instance variables                             |
| **Access Speed**  | Very fast (LIFO structure)                    | Slower (due to dynamic memory)                          |
| **Size**          | Limited, smaller than the heap                | Larger, but slower to access                            |
| **Lifetime**      | Exists for the duration of method calls       | Exists until garbage collection or explicitly nullified |
| **Thread Safety** | Each thread has its own stack                 | Shared across all threads (needs synchronization)       |
| **Managed By**    | JVM automatically cleans up after method ends | Garbage collection handles unused objects               |

**ARRAY**

```java
import java.util.*;
public class hello {

public static void main(String args[]) {
Scanner sc = new Scanner(System.in);
int arr[] = new int[6];
for (int i = 0; i < 6; i++) {
arr[i] = sc.nextInt();
sc.nextLine();
System.out.println(arr[i]);
}
}
}
```

```java
public class MainOverload {

    // Original main method, the program entry point
    public static void main(String[] args) {
        System.out.println("This is the main entry point");
        
        // Calling the overloaded main methods
        main(10);
        main("Hello");
    }

    // Overloaded main method with an int parameter
    public static void main(int num) {
        System.out.println("Overloaded main method with int: " + num);
    }

    // Overloaded main method with a String parameter
    public static void main(String str) {
        System.out.println("Overloaded main method with String: " + str);
    }
}```


Yes, that's absolutely right! No matter how many **overloaded `main()` methods** you define, the Java program will always start from the specific method signature
`public static void main(String[] args)`

```java
class Person {
    // Instance variables
    String name;
    int age;

    // Method that uses local variables
    public void setDetails(String newName, int newAge) {
        // Local variables
        String tempName = newName;  // Local variable tempName
        int tempAge = newAge;       // Local variable tempAge

        // Assigning local variables to instance variables
        name = tempName;
        age = tempAge;
    }

    public void displayDetails() {
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
    }
}

public class Main {
    public static void main(String[] args) {
        Person p1 = new Person();
        p1.setDetails("John", 25);  // Local variables used in this method call
        p1.displayDetails();        // Instance variables are used here
    }
}

```

Jagged Array
```java
import java.util.*;

public class hello {
    public static void main(String args[]) {
        int arr[3][] = new int[3][];
        arr[0] = new int[3];
        arr[1] = new int[4];
        arr[2] = new int[2];

        for ( int a[]:arr)
        {
            for( int val :row)
            {
                
            }
        }
        
    }
}
```

### Java `String` Characteristics and Behavior:

| **Concept**                           | **Explanation**                                                                                                                                                                                         |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **String is Immutable**               | Once a `String` is created, its value cannot be changed. Modifying a `String` like concatenation creates a new `String` object. The original object remains unchanged.                                  |
| **String Pool**                       | Java uses a special memory pool called the **string constant pool**. If a string literal already exists in the pool, a new reference is created to that literal. No new object is created in this case. |
| **String Declaration**                | Declaring a `String` like `String str = "hello";` will either refer to an existing literal in the pool or create a new one if it doesn't exist.                                                         |
| **String Object Creation with `new`** | Using `new String("hello")` will always create a new object in heap memory, even if `"hello"`already exists in the string pool.                                                                         |
| **Concatenation and Immutability**    | Concatenating `Strings` (e.g., `"hello" + " world"`) creates a **new object** in memory. The original `String` remains unchanged. Java's garbage collector will eventually remove unreferenced objects. |
| **Example - Same Literal**            | `java String str1 = "hello"; String str2 = "hello";` Both `str1` and `str2` refer to the same object in the **string pool**. Only **one object** is created.                                            |
| **Example - Using `new` Keyword**     | `java String str1 = new String("hello");` This will create a new object in **heap memory**, even if `"hello"` exists in the pool.                                                                       |
| **Garbage Collection**                | When a `String` is no longer referenced, it is eligible for garbage collection. Java will automatically free up memory when required.                                                                   |
```java
String str = "Bna";  // A String object with value "Bna" is created in the String pool.
str = "lop";         // The reference str is now pointing to a new String object with value "lop".
```

### Differences Between **String**, **StringBuffer**, and **StringBuilder**:

| Concept                  | **String** (Immutable)                       | **StringBuffer** (Mutable)           | **StringBuilder** (Mutable)           |
| ------------------------ | -------------------------------------------- | ------------------------------------ | ------------------------------------- |
| **Immutability**         | Immutable, cannot be changed after creation. | Mutable, contents can be changed.    | Mutable, contents can be changed.     |
| **Thread-Safety**        | Not thread-safe.                             | Thread-safe (synchronized methods).  | Not thread-safe (faster performance). |
| **String Constant Pool** | Uses the string pool for optimization.       | Does not use the string pool.        | Does not use the string pool.         |
| **Memory Management**    | New object created for each modification.    | Modifies the existing object.        | Modifies the existing object.         |
| **Initial Capacity**     | No buffer (direct memory allocation).        | Starts with 16 characters (default). | Starts with 16 characters (default).  |
| **Performance**          | Slower if you perform many modifications.    | Slower due to synchronization.       | Faster because it's not synchronized. |
```java
import java.util.*;

class Car {
    // String name; If i make this name Static then it will be the same for every
    // object that got created
    static String name = "TOYOTA";
    String model;

    public void show() {
        System.out.println("Name : " + name + " Model : " + model);
    }
}

public class hello {
    public static void main(String args[]) {
        Car c1 = new Car();
        Car c2 = new Car();

        // c1.name = "LEXUS";
        c1.model = "1234";

        // c2.name = "CAMRY";
        c2.model = "21343";

        // i have decalred above the same thing so i dont need to call it even if if
        // want to change it i can do this waay
        //static method can be called this way but for non static method we have to use object only
        Car.name = "MITSUBUHSI";
        c1.show();
        c2.show();
    }
}
```

I can overwite static vairables also but if i dont chaneg it will get automatically asssigned 
### Memory Breakdown:

|**Memory Area**|**Stored Data**|
|---|---|
|**Method Area**|`Car.name = "MITSUBUHSI"` (shared static variable)|
|**Heap Memory**|`c1.model = "1234"`  <br>`c2.model = "21343"` (individual copies for each object)|

### Key Points:

- **Static variables** are shared among all objects and are stored in the method area.
- **Non-static variables** are unique to each object and are stored in the heap.
- Static variables should be accessed using the class name (`Car.name`) rather than object references.


and same for static function they can't use normal instance variable static function can only access static variable 

```java
import java.util.*;

class Car {
    static String name;
    String model;
    public void show() {
        System.out.println("Name : " + name + " Model : " + model);
    }
// static function this wont work since static fucntion can only static variable
    public static void show1() {
        System.out.println("Name : " + name + " Model : " + model);
    }
}

public class hello {
    public static void main(String args[]) {
        Car c1 = new Car();
        Car c2 = new Car();
        c1.name = "alpha";
        c1.model = "1234";
        c2.name = "beta";
        c2.model = "21343";
        c1.show();
        c2.show();
        Car.show1();
    }
}```


So here even if i changed the static twice but the last one will be used for all objects like c1 and c2 , so here c1 and c2 will use beta , 21343 nad you can call a non static fucntion with direct class name.

So static block is also the same , one thing i can write if else directly in static block , it is same as static variable like 

```java
class Car {
    static String manufacturer;

    // Static block exactly excuted once and runs before main fucntion 
    static {
        manufacturer = "Toyota";
        System.out.println("Static block executed: Manufacturer initialized to " + manufacturer);
    }

    public static void displayManufacturer() {
        System.out.println("Car Manufacturer: " + manufacturer);
    }
}

public class Main {
    public static void main(String[] args) {
        System.out.println("Main method starts");
        
        Car.displayManufacturer(); // Static block executes before this
    }
}

```
So here also i can directly mention static string mf = toyota it will behave the same , but sometime i just dont want directly assign some variable to it. Like this one 

```java
class Config {
    static String databaseURL;

    static {
        try {
            databaseURL = "jdbc:mysql://localhost:3306/mydb";
            System.out.println("Database URL initialized: " + databaseURL);
        } catch (Exception e) {
            System.out.println("Error initializing database URL");
        }
    }
}

```
So in this case if i directly assign databaseUrl it might have fetched wrong details like port is not active and port is useless but with static block i can verify it and no need to establish database connection again n again , since it will run one time only.

I can also use non static variable in static but should have use with class initialization.

- Static methods and variables break the core principle of **encapsulation** because they are **globally shared** across all instances. Object-oriented programming (OOP) focuses on objects having their own state and behavior, which static violates.
- Static doesn't allow **polymorphism** (overriding methods in subclasses), which is central to OOP.

To understand why static methods can't be overridden, consider this simple example:

```java
class Animal {
    public static void makeSound() {
        System.out.println("The animal makes a sound.");
    }
}

class Dog extends Animal {
    public static void makeSound() {
        System.out.println("The dog barks.");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Dog();
        myAnimal.makeSound(); // The animal makes a sound.

        Dog myDog = new Dog();
        myDog.makeSound(); // The dog barks.
    }
}
```

In the code above, the `Dog` class defines its own `makeSound()` method. However, this is **not** method overriding; it's **method hiding**.

  * `myAnimal.makeSound()`: The compiler sees that the variable `myAnimal` is of type `Animal`. Since static methods are resolved at **compile time** based on the variable's type, the `Animal` class's `makeSound()` method is called, even though the object is a `Dog`. This shows that the **object's actual type is irrelevant** for static methods.

  * `myDog.makeSound()`: The compiler sees that `myDog` is of type `Dog`, so it calls the `Dog` class's `makeSound()` method.

If these were non-static methods, the first call `myAnimal.makeSound()` would have printed "The dog barks," demonstrating true **polymorphism** where the method call is resolved at runtime based on the object's actual type. The fact that this doesn't happen with static methods proves they don't support polymorphism.


```java
import java.util.*;

class Human
{
    private int age;
    public int getAge()
    {
        return age;
    }
    public void setAge(int a)
    {
        age = a;
    }
}

public class hello {
    public static void main ( String[] a)
    {
        Human h1  = new Human();
        h1.setAge(90);
        System.out.println(h1.getAge());
    }
}

```
So this is encapsulation and we are using private for it so it can't be directly acessed and to get understand in real life i can throw that fucntion to a particular user only so he/she can chnges a particular value.

So basically the concept of this keyword is whenver i am calling the set fucntion and i will defienlty call it ith some object some this referes to that object 

this.name = name and h1.name= name ( they are equal i can also implement this with passing human h1 as a parameter in the set fn)

```java
class Human {
    private String name;

    // Setter method to set the name
    public void setName(String name) {
        this.name = name;  // 'this.name' refers to the current object's 'name', 'name' is the parameter
    }

    // Getter method to retrieve the name
    public String getName() {
        return name;
    }
}

public class Hello {
    public static void main(String[] args) {
        Human h1 = new Human(); // Create a new Human object
        h1.setName("John");      // Set name using the setter method
        System.out.println(h1.getName()); // Get and print the name
    }
}

```


```java
class Car {
    private String model;
    private String manufacturer;
    private int year;

    // Default constructor (no-argument constructor)
    public Car() {
        // Default values
        this.model = "Unknown Model";
        this.manufacturer = "Unknown Manufacturer";
        this.year = 0;
    }

    // Parameterized constructor
    public Car(String model, String manufacturer, int year) {
        this.model = model;
        this.manufacturer = manufacturer;
        this.year = year;
    }

    // Getter methods
    public String getModel() {
        return model;
    }

    public String getManufacturer() {
        return manufacturer;
    }

    public int getYear() {
        return year;
    }

    // Method to display the car details
    public void displayInfo() {
        System.out.println("Model: " + model);
        System.out.println("Manufacturer: " + manufacturer);
        System.out.println("Year: " + year);
    }
}

public class Main {
    public static void main(String[] args) {
        // Using the default constructor
        Car car1 = new Car();
        car1.displayInfo();  // Displays default values

        System.out.println("---------------------------");

        // Using the parameterized constructor
        Car car2 = new Car("Corolla", "Toyota", 2020);
        car2.displayInfo();  // Displays values passed through constructor
    }
}

```

```java
class Car {
    void start() {
        System.out.println("Car started");
    }

    void stop() {
        System.out.println("Car stopped");
    }
}

public class Main {
    public static void main(String[] args) {
        // Anonymous object creation and immediate method call
        new Car().start();  // Creating anonymous object and calling start method
        new Car().stop();   // Creating another anonymous object and calling stop method
    }
}
```

### **Advantages of Anonymous Objects:**

- **Conciseness:** You avoid the need to create extra variables if the object will only be used once.
- **Simplicity:** The object is used only where it’s needed, simplifying code in certain scenarios.

### **Disadvantages:**

- **Lack of Reusability:** You cannot reuse an anonymous object since it doesn't have a reference variable.
- **Hard to Debug:** Since you don’t have a reference, tracking the state or behavior of the object in debugging might be more challenging.

```java
public class advc extends calc {
// subclass extend superclass
    public int mul(int a, int b) {
        return a * b;
    }
}

public class calc {
    public int add(int a, int b) {
        return a + b;
    }
    public int sub(int a, int b) {
        return a - b;
    }
}

public class hello {
    public static void main(String[] arg) {
        advc c1 = new advc();
        int x = c1.add(9, 89);
        System.out.println(x);
    }
}
```
so basically in this if i make object of class calc i will not able to use mul but iam using c1 so it is having add, sub autmaticaaly in it , basically object of subclass can use methods of superclass , but ibject of superclass cant.


Multilevel inheritance
```java
import java.util.*;

public class veryadvCalc extends advc{
    public double power(double a, double b) {
        return Math.pow(a, b);
    }

}

public class advc extends calc {
    public int mul(int a, int b) {
        return a * b;
    }
}

public class calc {
    public int add(int a, int b) {
        return a + b;
    }

    public int sub(int a, int b) {
        return a - b;
    }
}

public class hello {
    public static void main(String[] arg) {
        veryadvCalc c1 = new veryadvCalc();
        int x = c1.add(9, 89);
        double y = c1.power(2.0,10.0);
        System.out.println(x + " " + y);
    }
}
```

super and this 

```java
import java.util.*;

class A {
    public A() {
        System.out.println("In A");
    }

    public A(int a) {
        System.out.println("In A int");
    }
}

class B extends A {
    public B() {
        this(0); // this calls constructor of same class in this case it calls B int a 
        System.out.println("In B");
    }

    public B(int a) {
    // here it called super and exceuted IN a
        System.out.println("In B int");
    }
}

public class hello {
    public static void main(String[] arg) {
        B obj = new B();
    }

}


In A
In B int
In B
```

- **Constructor Chaining with `super()`**:
    
    - In Java, every class **implicitly** extends the `Object` class unless it explicitly extends another class. The constructor of `Object` is called automatically if no other superclass constructor is specified.
    - In any class that **extends** another class, the constructor of the **superclass** (parent class) is called before the constructor of the subclass (child class). This happens automatically unless you explicitly use `super()` in the constructor of the subclass.
    - When you use `super()`, it calls the **no-argument constructor** of the superclass. If the superclass doesn't have a no-argument constructor, you'll need to call a **parameterized constructor** using `super(arguments)`.
- **Use of `this()`**:
    
    - In the constructor of a subclass, you can use `this()` to call another constructor within the same class, and it must be the **first statement** in the constructor. This allows you to chain constructors within the same class.
The `this()` call chains constructors within the same class, and the `super()` call (either explicit or implicit) chains the constructors to the parent class. The order of execution is always from the top of the inheritance hierarchy down to the most specific class. This ensures that a parent object is fully created before its child object i

Method overiding
**Method overriding** is exactly what you described. It's when a subclass (child class) provides its own specific implementation for a method that is already defined in its superclass (parent class). The method in the subclass replaces the method in the superclass for that object.
```java
import java.util.*;

class A {
    public int add(int a, int b) {
        return a + b;
    }
}

class B extends A {
    public int add(int a, int b) {
        return a + b + 2; // this will get executed //ideally according to inheritance that upper A add method would have called but due to method overrriding B's add is calling and this is method overriding 
    }
}

public class hello {
    public static void main(String[] arg) {
        B obj = new B();
        System.out.println(obj.add(7, 1));
    }
}
```

Method Overloading (Compile-Time Polymorphism)

Method overloading is the ability to define multiple methods within the same class that have the same name but different parameter lists. The compiler decides which method to call at compile time based on the number, type, or order of the arguments you pass.

Here's an example:

```java
class Calculator {
    // Overloaded method 1: adds two integers
    public int add(int a, int b) {
        return a + b;
    }

    // Overloaded method 2: adds three integers
    public int add(int a, int b, int c) {
        return a + b + c;
    }

    // Overloaded method 3: adds two doubles
    public double add(double a, double b) {
        return a + b;
    }
```

In this case, the add method is "overloaded." When you call add(), the compiler checks the arguments you provide and matches them to the correct method signature.

Calculator calc = new Calculator();

calc.add(5, 10);  -> The compiler calls the add(int, int) method.

calc.add(5, 10, 15); -> The compiler calls the add(int, int, int) method.

calc.add(5.0, 10.5); -> The compiler calls the add(double, double) method.

This is why it's called compile-time polymorphism; the decision of which method to execute is made by the compiler before the program even runs. This contrasts with runtime polymorphism (method overriding), where the decision is made by the JVM while the program is executing.
Here's a concise table explaining the access modifiers in Java:

They both relate to polymorphism because they are two different ways for a single entity (a method name) to have "many forms" (`poly` = many, `morph` = form).

### How They Are Linked to Polymorphism

The core idea of polymorphism is that you can have one method name with multiple behaviors.

**1. Method Overloading (Compile-Time Polymorphism):** Here, the single method name (e.g., `add`) has different behaviors based on its **parameter list**. When you call `add()`, the compiler selects the correct version based on the arguments you pass. It's polymorphism because the `add` method takes on different forms depending on the context.

- `add(int, int)` is one form.
    
- `add(double, double)` is another form.
    

The compiler knows which form to use at compile time, so it's called static or **compile-time polymorphism**.

---

**2. Method Overriding (Runtime Polymorphism):** Here, the single method name (e.g., `speak`) has different behaviors based on the **object type** calling it. When a superclass variable holds a subclass object, the JVM decides which version of the overridden method to call at runtime. It's polymorphism because the `speak` method takes on different forms depending on the actual object.

- `Animal.speak()` is one form.
    
- `Dog.speak()` is another form.
    

The decision is made at runtime, so it's called dynamic or **runtime polymorphism**.

Both concepts are different implementations of the same principle: a single method name being used to perform multiple tasks.

| Modifier     | **Same Class** | **Same Package** | **Subclass (Different Package)** | **Other Packages** |
|--------------|----------------|------------------|----------------------------------|--------------------|
| **public**   | ✅              | ✅                | ✅                                | ✅                  |
| **protected**| ✅              | ✅                | ✅                                | ❌                  |
| **default**  | ✅              | ✅                | ❌                                | ❌                  |
| **private**  | ✅              | ❌                | ❌                                | ❌                  |

### Notes:
- **`public`**: Accessible everywhere.
- **`protected`**: Accessible within the same package and in subclasses (even in different packages).
- **`default`** (no modifier): Accessible only within the same package.
- **`private`**: Accessible only within the same class.

```java
class SuperClass {
    void display() {
        System.out.println("Display from SuperClass");
    }
}

class SubClass extends SuperClass {
    @Override
    void display() {
        System.out.println("Display from SubClass");
    }
}

public class Main {
    public static void main(String[] args) {
        SuperClass obj; // Reference of SuperClass

        // Object of SuperClass
        obj = new SuperClass();
        obj.display(); // Output: Display from SuperClass

        // Object of SubClass
        obj = new SubClass();
        obj.display(); // Output: Display from SubClass
    }
}

```

so basically in dynamic method dispatch runtime polymorphism i can decide at a time which class to override and in method overriding it will be get decided on run time and this @override this will get decided by 
### Key Takeaways

- **No, it 's not mandatory to use `@Override`.**
- **Yes, it's highly recommended** because it helps catch mistakes and improves code readability and maintainability.

---

### **Wrapper Classes in Java**
1. **Definition**: Every primitive type in Java has a corresponding wrapper class that allows primitive values to be treated as objects.
    - Example: `int` → `Integer`, `float` → `Float`, `char` → `Character`, etc.

2. **Boxing and Unboxing**:
   - **Boxing**: Converting a primitive value into its corresponding wrapper object.
     ```java
     int a = 8;
     Integer num1 = new Integer(a); // Boxing (Deprecated)
     Integer num2 = a;             // Auto-boxing (Preferred)
     ```
   - **Unboxing**: Converting a wrapper object back to its corresponding primitive value.
     ```java
     int num3 = Integer.valueOf(num2); // Unboxing (Deprecated)
     int num4 = num2;                 // Auto-unboxing (Preferred)
     ```

3. **Why Java Isn’t Purely OOP**:
   - Java includes primitive types (`int`, `float`, etc.) that are not objects.
   - Primitive types provide efficiency as they are stored in stack memory, making operations faster compared to storing objects in heap memory.

4. **Why Use Wrapper Classes?**:
   - To work with collections like `ArrayList` (which only store objects).
   - For utilities like parsing and type conversion (`Integer.parseInt()`).

5. **Example Output**:
   ```java
   8
   8
   8
   8
   ```

✅ **Any class can be abstract** by using `abstract` before `class`.  
✅ **Abstract classes can have both abstract and non-abstract methods.**  
✅ **Abstract methods must be implemented by the subclass.**  
✅ **We cannot create objects of an abstract class.**  
✅ **We can use polymorphism** (`Car c1 = new vw();`) to refer to a subclass object.

```java
import java.util.*;

abstract class Car {
    abstract public void fly();  // Abstract method (must be implemented)

    public void music() {        // Non-abstract method (has a body)
        System.out.println("Music Playing");
    }
}

class vw extends Car {  // Inheriting from abstract class
    public void fly() {  
        System.out.println("Flying");
    }
}

public class hello {
    public static void main(String[] arg) {
        Car c1 = new vw(); // Polymorphism (Abstract class reference)
        c1.fly();          // Calls overridden method in `vw`
        c1.music();        // Calls inherited method from `Car`
    }
}

```


```java
class Car {
    public void show() {
        System.out.println("Inside class Car");
    }

    class UpdateCar {  // Non-static inner class
        public void fly() {
            System.out.println("Flying Car");
        }
    }

    static class NewCar {  // Static nested class
        public void tport() {
            System.out.println("Teleport");
        }
    }
}

public class hello {
    public static void main(String[] arg) {
        Car c1 = new Car();
        c1.show();
        Car.UpdateCar c3 = c1.new UpdateCar();
        c3.fly();
        Car.NewCar c4 = new Car.NewCar();
        c4.tport();
    }
}
```

### **🔹 What is an Anonymous Inner Class?**

An **anonymous inner class** is a **class without a name** that:  
✔ **Extends an existing class** (like `Car`) OR **implements an interface**.  
✔ **Overrides methods on the spot**.  
✔ **Is used when we need a one-time implementation**.

```java
import java.util.*;

class Car
{
   public void show ()
   {
    System.out.println("in show Car"
    );
   }
}


public class hello {
    public static void main(String[] arg) {
        Car c1 = new Car()
        {
            public void show() // class under class hello with no name anonymouse inner class
            {
                System.out.println("In new show");
            }
        };

        c1.show();
    }
}
```

### **🔹 When to Use Which?**

|Approach|When to Use|
|---|---|
|**Inheritance (Explicit Subclass)**|When the subclass will be reused multiple times.|
|**Anonymous Inner Class**|When you need a quick one-time implementation.|
```java
abstract class Car {
    abstract void drive();
}

public class Main {
    public static void main(String[] args) {
        Car myCar = new Car() {  // Anonymous inner class implementing `drive()`
            void drive() {
                System.out.println("Anonymous Car is driving!");
            }
        };

        myCar.drive();  // ✅ Output: "Anonymous Car is driving!"
    }
}

```

✅ **Before Java 8:** **YES**, only **abstract methods** were allowed.  
✅ **After Java 8:** **NO**, interfaces can have:

- **Abstract methods** (default: `public abstract`)
- **Default methods** (methods with a body, using `default` keyword)
- **Static methods** (methods with a body, using `static` keyword)
- **Private methods** (added in Java 9 for internal use)

```java
interface Vehicle {
    void drive();  // ✅ Abstract method (automatically `public abstract`)
}

class Car implements Vehicle {
    public void drive() {  // ✅ Must be public, otherwise compilation error
        System.out.println("Car is driving");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle myCar = new Car();
        myCar.drive();  // ✅ Output: "Car is driving"
    }
}
```

✔ **Anonymous Inner Class is being used** to implement the `A` interface.  
✔ **Since interfaces cannot have constructors**, we are not directly instantiating `A`.  
✔ Instead, we are **creating an anonymous subclass of `A`** and providing an implementation for `show()`.
**No, we are not calling an interface constructor.**  
But the **Anonymous Inner Class** being created **implicitly calls the `Object` class constructor** (since every class in Java extends `Object`).
```java@FunctionalInterface
interface A {
    void show();
}

public class hello {
    public static void main(String[] arg) {
        A a1 = new A() {  // ✅ Creating an Anonymous Inner Class
            public void show() {
                System.out.println("show");
            }
        };
    
        a1.show();  // ✅ Output: show
    }
}
```
### **1️⃣ Why Use Interfaces Instead of Abstract Classes?**

1️⃣ **Multiple Inheritance Support** 🚀

- A class **can extend only one abstract class** but **can implement multiple interfaces**.
- This makes interfaces **more flexible**.
```java
interface A {
    void methodA();
}

interface B {
    void methodB();
}

class C implements A, B {  // ✅ Implements multiple interfaces
    public void methodA() { System.out.println("A's method"); }
    public void methodB() { System.out.println("B's method"); }
}

public class Main {
    public static void main(String[] args) {
        C obj = new C();
        obj.methodA();
        obj.methodB();
    }
}

```

2️⃣ **Decoupling & Loose Coupling 🔗**

- Interfaces promote **loose coupling**, which means components depend **only on behaviors, not implementations**.
- This is crucial in **large applications** where multiple independent modules interact.
```java
interface Payment {
    void pay(double amount);
}

class CreditCardPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paid via Credit Card: $" + amount);
    }
}

class PayPalPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paid via PayPal: $" + amount);
    }
}

public class Main {
    public static void main(String[] args) {
        Payment payment = new PayPalPayment();  // We can easily switch payment methods
        payment.pay(100.0);
    }
}
```

## **💡 Inheritance Rules in Java**

|Relationship|Keyword|Example|
|---|---|---|
|**Class → Class**|`extends`|`class B extends A`|
|**Interface → Class**|`implements`|`class B implements A`|
|**Interface → Interface**|`extends`|`interface B extends A`|

### **🔹 Key Takeaways**

✅ **Enums can have instance variables & methods.**  
✅ **Enum constants are objects & can have constructors.**  
✅ **Enum constructors must be private (or package-private, default visibility).**
```java

enum Laptop {
    macbook(200), hp(180), dell(100);

    private int price;

    Laptop(int price) {
        this.price = price;
    }

    public int getPrice() {
        return price;
    }

}

public class hello {

    public static void main(String[] arg) {

        Laptop l1 = Laptop.hp;
        System.out.println(l1);
        System.out.println(l1.getPrice());

    }
}

```

```java
class A {
    public void show() {  // ✅ Correct method in parent class
        System.out.println("in A");
    }
}

class B extends A {
    @Override
    public void Show() {  // ❌ Mistyped (uppercase 'S')
        System.out.println("IN B");
    }
}

```

✅ **`@Override` is optional but recommended**.  
✅ It **does not improve performance** but **prevents errors**.  
✅ It **helps in code readability and maintainability**.

1️⃣ **Normal Interface** → **More than one abstract method**  
2️⃣ **Functional Interface (SAM - Single Abstract Method)** → **Only one abstract method** (`@FunctionalInterface`)  
3️⃣ **Marker Interface** → **No methods** (used for tagging classes, like `Serializable`)

✔ **`@FunctionalInterface`** ensures that an interface has **only one abstract method** (SAM - **Single Abstract Method**).  
✔ **Why Functional Interfaces?** → To use **Lambda Expressions**, which make code **shorter and cleaner**.

✔ **Lambdas reduce code** by eliminating the need for an explicit class implementation.  
✔ **Only used with Functional Interfaces (SAM - Single Abstract Method).**  
✔ **No need to define method names**—Lambda **automatically knows which method** to implement.  
✔ **Multiple parameters** can be passed easily.

```java
tradional
A a1 = new A() {
    public void show() {
        System.out.println("show");
    }
};

lamda
A ( this A mut be fucntional interafce) a2 = () -> System.out.println("Showing");


with parameters
@FunctionalInterface  
 interface B  
{  
     void show(int i );  
}  
  
public class hello {  
    public static void main(String[] arg) {  
  
B obj = i -> System.out.println("jhghjgf");  
obj.show(9);  
    }  
}
```

✔ **Three types of errors:**

- **Compile-time** → Syntax mistakes
- **Run-time** → **Exceptions** (program runs but fails unexpectedly)
- **Logical** → Wrong output due to incorrect logic

✔ **Try-Catch Block** → Handle exceptions gracefully  
✔ **Multiple Catch Blocks** → Catch specific exceptions  
✔ **Exception Hierarchy** →

- `Object` → `Throwable` → (`Error`, `Exception`)
- **Exceptions:**
    - **Checked (Compile-time)** → Handled at compile-time (`SQLException`, `IOException`)
    - **Unchecked (Run-time)** → Optional handling (`NullPointerException`, `ArrayIndexOutOfBoundsException`)


```java
public class Main {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;  // ❌ This will throw ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Cannot divide by zero: " + e);
        }
        System.out.println("Program continues...");  // ✅ Won't crash
    }
}


public class Main {
    public static void main(String[] args) {
        try {
            int[] arr = {1, 2, 3};
            System.out.println(arr[5]);  // ❌ Out of bounds

            String str = null;
            System.out.println(str.length());  // ❌ NullPointerException

        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Array index out of bounds!");
        } catch (NullPointerException e) {
            System.out.println("Null pointer exception!");
        } catch (Exception e) {  // ✅ Generic catch for unexpected exceptions
            System.out.println("Some other exception: " + e);
        }
    }
}

```

### **1️⃣ Why Use `throw` if `try-catch` Already Catches Exceptions?**

✔ `try-catch` **handles exceptions when they occur naturally.**  
✔ `throw` **forces an exception manually**, even before Java detects it.  
✔ It allows us to **add custom messages** and **control exception handling explicitly.**

```java
public class Main {
    public static void main(String[] args) {
        try {
            divide(10, 0);
        } catch (ArithmeticException e) {
            System.out.println("Exception caught: " + e.getMessage());
        }
    }

    static void divide(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("Denominator cannot be zero!");  // ✅ Manual throw
        }
        System.out.println("Result: " + (a / b));
    }
}

✔ **Key Difference:**

- Instead of waiting for Java to crash, we **manually check** `if (b == 0)`, then **throw an exception**.
- The `catch` block **handles it safely with a custom message**.
```

**🔹 Key Difference Between `throw` and `throws`:**

| Feature                | `throw`                                                          | `throws`                                                       |
| ---------------------- | ---------------------------------------------------------------- | -------------------------------------------------------------- |
| **Purpose**            | Used to **manually throw** an exception inside a method or block | Used to **declare** that a method **might throw** an exception |
| **Where It's Used**    | Inside a method/block                                            | In method signature                                            |
| **Exception Handling** | Used with `if` conditions to throw exceptions manually           | Forces the caller to handle the exception                      |
| **Example**            | `throw new ArithmeticException("Error");`                        | `void divide() throws ArithmeticException {}`                  |
```java
public class Main {
    public static void main(String[] args) {
        try {
            divide(10, 0);  // ✅ Method might throw an exception
        } catch (ArithmeticException e) {
            System.out.println("Exception caught: " + e.getMessage());
        }
    }

    // ✅ `throws` declares that this method might throw an exception
    static void divide(int a, int b) throws ArithmeticException {
        if (b == 0) {
            throw new ArithmeticException("Cannot divide by zero!");  // ✅ `throw` used inside method
        }
        System.out.println("Result: " + (a / b));
    }
}

```

✔ **`throw`** → Used **inside** `divide()` to manually trigger an exception.  
✔ **`throws`** → Used in `divide()` **method signature** to declare that an exception **may occur**.

### **🚀 Custom Exception in Java (User-Defined Exceptions)**

You already know Java has **built-in exceptions** like `ArithmeticException`, `NullPointerException`, etc.  
But sometimes, you need to create **your own exceptions** based on your application's needs.

✅ **Custom exceptions help make error messages more meaningful**  
✅ **They extend `Exception` or `RuntimeException`**

---

## **1️⃣ How to Create a Custom Exception?**

**Step 1:** **Extend `Exception` (Checked) or `RuntimeException` (Unchecked).**  
**Step 2:** **Provide a constructor that accepts an error message.**  
**Step 3:** **Use `throw` to trigger it inside your code.**

---

```java
// ✅ Step 1: Create a custom exception by extending Exception
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);  // Pass message to Exception class
    }
}

public class Main {
    public static void main(String[] args) {
        try {
            checkAge(15);  // ❌ This will throw an exception
        } catch (InvalidAgeException e) {
            System.out.println("Exception caught: " + e.getMessage());
        }
    }

    // ✅ Step 2: Throw the custom exception
    static void checkAge(int age) throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException("Age must be 18 or above!");
        }
        System.out.println("Valid age, proceed.");
    }
}

```

### **🔹 What Are Threads?**

- A **thread** is a **lightweight process** that allows multiple tasks to run **independently**.
- **Single-threaded programs** execute **one task at a time** (like reading a file, then printing).
- **Multi-threading** allows multiple operations **to run concurrently**, improving efficiency.

---

### **1️⃣ Threads in Software & OS**

✔ **Multiple threads inside one software** → Run in **parallel** (if multi-core CPU).  
✔ **Multiple threads across different software** → Run in **time-sharing mode** (OS switches between them).

🔹 **Example (Multi-threading in Software)**

- A web browser has **one thread for UI**, **another for downloading**, and **another for rendering a webpage**.
- A game might have **one thread for rendering graphics**, **one for physics**, **one for sound**, etc.

🔹 **Example (Time-sharing for Multiple Softwares)**

- If 3 applications (Chrome, Spotify, VS Code) are running, OS **allocates time slices** to each one.
- It looks like they run **together**, but in reality, OS switches rapidly between them.

```java

class A extends Thread {
    public void run() {
        for (int i = 0; i < 1000; i++) {
            System.out.println("Hi");
        }
    }
}

class B extends Thread {
    public void run() {
        for (int i = 0; i < 1000; i++) {
            System.out.println("Hello");
            try {
                // i can use sleep also
                Thread.sleep(10);
            } catch (InterruptedException ex) {
            }
        }
    }
}

public class hello {
    public static void main(String[] arg) {
        A obj1 = new A();
        B obj2 = new B();

        obj1.setPriority(Thread.MAX_PRIORITY); // settign priority // this exist between 0 to 10 o lowest and 10 highest 
        obj1.start();
        obj2.start();

    }
}

```

No! **Other methods can exist in a Thread class**, but only `run()` will execute **when the thread starts**.

### **2️⃣ Understanding Thread Scheduling (Why We Can't Fully Control It)**

✔ The **JVM Thread Scheduler** (OS-dependent) decides **which thread runs** and **for how long**.  
✔ **We can influence thread execution**, but **full control isn't possible**.

✅ **Optimizing Thread Execution (But Not Controlling It Completely)**
🔹 **Even with priorities and sleep, exact scheduling depends on the OS and JVM**.

### **1️⃣ Why Use `Runnable` Instead of Extending `Thread`?** 🤔

You're absolutely right! The **main reason** for using `Runnable` instead of extending `Thread` is:

✅ **Java allows multiple interface implementations, but only one class extension.**

- If a class **extends `Thread`**, it **cannot extend any other class**.
- But if a class **implements `Runnable`**, it **can still extend another class**.

### **2️⃣ Key Differences: `Thread` vs `Runnable`**

| Approach             | `extends Thread`                | `implements Runnable`                               |
| -------------------- | ------------------------------- | --------------------------------------------------- |
| **Inheritance**      | ❌ Cannot extend another class   | ✅ Can extend another class                          |
| **Flexibility**      | ❌ Tightly coupled with `Thread` | ✅ More flexible                                     |
| **Multiple Threads** | ❌ Not reusable                  | ✅ Runnable instance can be used in multiple threads |
| **Best For**         | Simple use cases                | Large applications with better design               |
```java
class Animal {
    void eat() {
        System.out.println("Eating...");
    }
}

// ✅ If we use Runnable, we can still extend Animal
class Dog extends Animal implements Runnable {
    public void run() {
        System.out.println("Dog is running...");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        Thread t1 = new Thread(dog);
        t1.start();  // ✅ Dog can run while still extending Animal
    }
}

```

Thread also implement runnable. so by default even without using runnable we can do t1.start()


✅ **Use `Runnable` for flexibility** (because Java **only allows single class inheritance**).  
✅ **Use `start()`, not `run()`, to actually start a thread.**  
✅ **Runnable is best for large applications where multiple classes need to share behavior.**

### **1️⃣ `join()` - Why Use It?**

✅ Ensures that one thread **finishes before** another thread starts.

🔹 **Example:** Suppose a **main thread** must wait for a **child thread** to finish before continuing.

```java
class MyTask extends Thread {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Task running...");
            try { Thread.sleep(500); } catch (InterruptedException e) { }
        }
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        MyTask t1 = new MyTask();
        t1.start();
        t1.join();  // ✅ Main thread waits until t1 finishes

        System.out.println("Main thread continues after t1 is done.");
    }
}

```
✔ **Without `join()`, the main thread would continue running immediately.**  
✔ **With `join()`, it waits for `t1` to complete before continuing.**

main is also a thread , so ffirst task running will be done then main thread continues after t1 is dine

### **2️⃣ `yield()` - When to Use It?**

✅ **Gives a hint to the CPU** that the thread can pause and let other threads execute first.

🔹 **Example:** When we want to give another thread a chance to run.

```java
class MyTask extends Thread {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + " running");
            Thread.yield();  // ✅ Suggests CPU to let other threads run
        }
    }
}

public class Main {
    public static void main(String[] args) {
        MyTask t1 = new MyTask();
        MyTask t2 = new MyTask();

        t1.start();
        t2.start();
    }
}

```

✔ **`yield()` does not guarantee switching**, but it **helps optimize CPU scheduling**.  
✔ It is **useful when we want fair execution among multiple threads**.

---

### **🔥 Final Verdict: Should You Learn Them?**

✅ If you're working on **basic multithreading**, **not required**.  
✅ If you need **thread synchronization & execution control**, **yes, they’re useful**.  
✅ **`join()` is more useful than `yield()`** in real-world scenarios.

### **1️⃣ Race Condition Recap**

A **Race Condition** occurs when **multiple threads** try to update a shared variable **at the same time**, leading to **incorrect results**.

Example:

- Suppose `count = 2`, and both threads read it at the same time.
- Both increment `count` → Expected **4**, but due to interference, it remains **3**.

---

### **2️⃣ Why Doesn’t `join()` Fix This?**

❌ `join()` only ensures **one thread completes before another starts**, but it **doesn't prevent simultaneous access**.  
✅ **`synchronized` ensures only one thread modifies `count` at a time**, preventing race conditions.

```java
class Counter 
{
    int count  ; 
    public synchronized void counter()
    {
        count++;
    }
}

public class hello {
    public static void main(String[] arg) {

        try {
            Counter c1 = new Counter();
            
            Runnable obj1 = () ->{
                for ( int i = 0 ; i<1000 ; i++)
                { c1.counter();
                }
            };

            Runnable obj2 = () ->{
                for ( int i = 0 ; i<1000 ; i++)
                { c1.counter();
                }
            };
            
            Thread t1 = new Thread(obj1);
            Thread t2 = new Thread(obj2);
            t1.start();
            t2.start();
            
            t1.join();
            t2.join();
            
            System.out.println(c1.count);
        } catch (InterruptedException ex) {
        }
    }
}

```

## **🔹 1️⃣ What Are Thread States?**

A thread in Java **does not always run**—it moves through different states in its **lifecycle**.

Java has **6 thread states**, defined in the `Thread.State` enum:

|State|Description|
|---|---|
|**NEW**|Thread is created but not started yet (`start()` not called).|
|**RUNNABLE**|Thread is ready to run but waiting for CPU time.|
|**BLOCKED**|Thread is waiting for a lock (another thread is holding it).|
|**WAITING**|Thread is waiting indefinitely for another thread’s signal.|
|**TIMED_WAITING**|Thread is waiting for a fixed time (e.g., `sleep()`, `join(timeout)`).|
|**TERMINATED**|Thread has finished execution.|

---

## **🔹 2️⃣ Java Thread Lifecycle**

📌 **A thread moves through different states during execution.**

scss

CopyEdit

`NEW → RUNNABLE → (BLOCKED / WAITING / TIMED_WAITING) → TERMINATED`

---
✔ **Collection API** → A framework providing ready-to-use data structures.  
✔ **Collection** → An **interface** (parent of List, Set, Queue).  
✔ **Collections** → A **utility class** with helper methods (`sort()`, `reverse()`, etc.).

```scss
Collection (Interface)
 ├── List (Interface)
 │    ├── ArrayList (Class)
 │    ├── LinkedList (Class)
 │    ├── Vector (Class)
 ├── Set (Interface)
 │    ├── HashSet (Class)
 │    ├── LinkedHashSet (Class)
 │    ├── TreeSet (Class)
 ├── Queue (Interface)
      ├── PriorityQueue (Class)
      ├── ArrayDeque (Class)
```

1️⃣ **`Collection c1 = new ArrayList();`** ❌ **(Not Recommended)**

- No generics (`<>`), so it treats **all data as `Object`**.
- You might accidentally add different data types (`String`, `Integer`, etc.), leading to runtime errors.

2️⃣ **`Collection<Integer> c2 = new ArrayList<Integer>();`** ✅ **(Better, but not ideal)**

- Uses generics (`<>`), so **only `Integer` values are allowed**.
- But **`Collection` lacks `List`-specific methods** like `get(index)`, `add(index, value)`, etc.

3️⃣ **`List<Integer> c3 = new ArrayList<>();`** ✅ **(Best Choice 👍)**

- Uses `List<Integer>`, so it supports **index-based operations**.
- Allows useful `List` methods:
```java
c3.get(1);       // ✅ Get element at index 1
c3.add(1, 99);   // ✅ Insert 99 at index 1
c3.remove(0);    // ✅ Remove element at index 0
```

Manually defining it - ```
List<Integer> arr = Arrays.asList(3, 1, 2);

List<List<Integer>> fans = new ArrayList<>();  
List<Integer> ans = new ArrayList<>();
fans.add(ans);

this won't work because i am passign reference directly of ans in fans , intead pass a copy like this

fans.add(new ArrayList<>(ans));

```
### **🔹 Key Properties of `Set`**

✔ **No Duplicates** → Any duplicate values are automatically removed.  
✔ **Unordered** (in `HashSet`) → Elements **do not maintain insertion order**.  
✔ **Efficient Lookups** → `HashSet` provides **O(1) time complexity** for `add()`, `remove()`, and `contains()`.

```java
 import java.util.*;

public class hello {
    public static void main(String[] arg) {
        Set<Integer> l1 = new HashSet<>();

        l1.add(90);
        l1.add(80);
        l1.add(70);
        l1.add(90); // Duplicate, won't be added
        l1.add(100);

        for (Integer x : l1) {
            System.out.println(x);
        }
    }
}
1️⃣ **`LinkedHashSet`** → Maintains **insertion order**  
2️⃣ **`TreeSet`** → Stores elements in **sorted order**
```

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class hello {
    public static void main(String[] arg) {

        Comparator<Integer> com = new Comparator<Integer>() {
            
            public int compare(Integer i , Integer j)
            {
                if(i%10>j%10)
                return 1;
                else
                return -1;
            }
        };

        List<Integer> l1 = new ArrayList<>();

        l1.add(90);
        l1.add(45);
        l1.add(23);
        l1.add(22);
        l1.add(21);

        Collections.sort(l1,com);

        System.out.println(l1);
        

}
}

```

For each method
```java 
public class hello {

    public static void main(String[] arg) {

        List<Integer> nums = new ArrayList<>();

        nums.add(90);
        nums.add(40);
        nums.add(990);
        nums.add(98);
        nums.add(76);

        // Consumer<Integer> con = new Consumer<Integer>() {

        //     public void accept(Integer T) {
        //         System.out.println(T);
        //     }
        // };

        // after converting to lamda

     //   Consumer<Integer> con = T-> System.out.println(T); 

    //  now repalcing con to 

        nums.forEach(T-> System.out.println(T));

        //  nums.forEach(n ->System.out.println(n));
    }
}

```


Great question 👍
This is **exactly the right confusion to have** when learning lambdas.

I’ll explain **slowly**, from **old-style code → lambda → forEach**, and then answer **“how `nums` is directly referring to Consumer”**.

---

## First: what is `Consumer`?

In **Java**:

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

### Meaning:

> A `Consumer<T>` is **something that takes a value of type `T` and returns nothing**.

So:

* Input → `T`
* Output → `void`

---

## Step 1️⃣ Your list

```java
List<Integer> nums = new ArrayList<>();
nums.add(90);
nums.add(40);
nums.add(990);
nums.add(98);
nums.add(76);
```

At this point:

```
nums = [90, 40, 990, 98, 76]
```

---

## Step 2️⃣ Traditional (anonymous class) Consumer

```java
Consumer<Integer> con = new Consumer<Integer>() {
    public void accept(Integer T) {
        System.out.println(T);
    }
};
```

What this means:

* You created a **Consumer**
* Its `accept()` method prints the value

So logically:

```
con.accept(90)  → prints 90
con.accept(40)  → prints 40
```

---

## Step 3️⃣ What does `forEach()` expect?

Look at the method signature:

```java
default void forEach(Consumer<? super T> action)
```

📌 **Important**

* `forEach()` expects a **Consumer**
* NOT a loop
* NOT a function returning value

---

## Step 4️⃣ Using Consumer with forEach (old way)

```java
nums.forEach(con);
```

What happens internally (simplified):

```java
for (Integer n : nums) {
    con.accept(n);
}
```

So execution becomes:

```
con.accept(90)
con.accept(40)
con.accept(990)
con.accept(98)
con.accept(76)
```

---

## Step 5️⃣ Lambda version of Consumer

This line:

```java
Consumer<Integer> con = T -> System.out.println(T);
```

Is **exactly equivalent** to:

```java
Consumer<Integer> con = new Consumer<Integer>() {
    public void accept(Integer T) {
        System.out.println(T);
    }
};
```

### Why?

Because:

* `Consumer` has **only one method**
* Lambda = implementation of that method

So:

```java
T -> System.out.println(T)
```

means:

> “When `accept(T)` is called, run `System.out.println(T)`”

---

## Step 6️⃣ Now the IMPORTANT PART (your real question)

### This line 👇

```java
nums.forEach(T -> System.out.println(T));
```

### What is really happening?

You are **not skipping Consumer**.

You are doing this **implicitly**:

```java
nums.forEach(
    new Consumer<Integer>() {
        public void accept(Integer T) {
            System.out.println(T);
        }
    }
);
```

📌 **The lambda itself IS the Consumer**.

---

## Step 7️⃣ How `nums` is “directly” referring to Consumer

It’s not magic.

### Method signature decides everything

```java
forEach(Consumer<? super Integer> action)
```

So when Java sees:

```java
nums.forEach(T -> System.out.println(T));
```

The compiler thinks:

> “I need a `Consumer<Integer>` here”

And it **converts the lambda** into a `Consumer` automatically.

---

## Step 8️⃣ Internal execution (very important)

Internally, `forEach()` does something like:

```java
for (Integer element : nums) {
    action.accept(element);
}
```

Where:

* `action` = your lambda
* `accept(element)` = lambda body execution

---

## Step 9️⃣ Mental model (THIS MAKES IT CLICK)

Think of it like this:

```java
nums.forEach( WHAT_TO_DO_WITH_EACH_ELEMENT );
```

And you tell Java:

```java
WHAT_TO_DO_WITH_EACH_ELEMENT = print it
```

Which matches:

```java
Consumer<Integer>
```

---
```java
  
List<Integer> ls = new ArrayList<>();  
ls.add(123);  
ls.add(231);  
ls.add(121);  
  
for(Integer it:ls)  
{  
    System.out.println(it);  
}  
  
ls.forEach(T-> System.out.println(T));
```
## Step 🔟 One-line explanation (INTERVIEW GOLD 🧠)

> `forEach()` accepts a `Consumer`, and a lambda expression is automatically treated as a `Consumer` implementation, whose `accept()` method is called for each element in the list.

---

## Final ultra-simple summary

* `Consumer<T>` → takes `T`, returns nothing
* `forEach()` → needs a `Consumer`
* Lambda → implements `Consumer.accept()`
* `nums` doesn’t call lambda directly
* `forEach()` calls `accept()` internally

---

## One last visualization 🧠

```java
nums.forEach(n -> System.out.println(n));
```

Equals:

```java
for (Integer n : nums) {
    System.out.println(n);
}
```

---
Great question 👍 — this confusion is **very common**, and once it clears, **Streams make perfect sense**.

Let’s go **line by line**, and then answer your core doubt:

> **“Predicate returns boolean, so how is `filter()` returning numbers?”**

---

## First: what `Predicate` really is

In **Java**:

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

### Meaning:

> A `Predicate<T>` is a **question** about `T`.

It does **NOT transform** data.
It only answers:

```
YES (true) or NO (false)
```

---

## Your code (slightly cleaned)

```java
Stream<Integer> s1 = l1.stream();

Predicate<Integer> p = new Predicate<Integer>() {
    @Override
    public boolean test(Integer n) {
        return n % 2 == 0;   // even?
    }
};

Stream<Integer> s2 = s1.filter(p);

s2.forEach(n -> System.out.println(n));
```

---

## The key misunderstanding (important)

> ❌ Predicate does NOT return a number
> ❌ `filter()` does NOT use Predicate’s return value as data

✔ Predicate only decides **whether to KEEP or DISCARD** an element.

---

## What `filter()` actually does internally

Look at the method signature:

```java
Stream<T> filter(Predicate<? super T> predicate)
```

Notice:

* Input: `Predicate<T>`
* Output: `Stream<T>` (same type!)

---

## How `filter()` works internally (THIS IS THE ANSWER)

Your code:

```java
Stream<Integer> s2 = s1.filter(p);
```

Is logically equivalent to this:

```java
List<Integer> result = new ArrayList<>();

for (Integer n : l1) {
    if (p.test(n)) {      // boolean check
        result.add(n);    // keep ORIGINAL number
    }
}
```

### 🔥 Important:

* `p.test(n)` → **boolean**
* `n` → **original number**
* `filter()` does **not modify `n`**
* It only decides **keep vs discard**

---

## Step-by-step execution (with real values)

Assume:

```java
l1 = [10, 15, 20, 25]
```

### Processing:

| n  | p.test(n) | Action  |
| -- | --------- | ------- |
| 10 | true      | keep 10 |
| 15 | false     | discard |
| 20 | true      | keep 20 |
| 25 | false     | discard |

### Resulting stream:

```
[10, 20]
```

Then:

```java
forEach(n -> System.out.println(n));
```

Prints:

```
10
20
```

---

## Why it feels confusing at first

Because you might think:

```
filter() → uses predicate → returns predicate result
```

But actually:

```
filter() → uses predicate → returns ELEMENTS
```

Predicate is only a **gatekeeper**, not a transformer.

---

## Compare with `map()` (this will lock it in)

### filter → keeps or removes

```java
.filter(n -> n % 2 == 0)
```

### map → transforms

```java
.map(n -> n * 2)
```

| Operation | Predicate returns | Stream contains      |
| --------- | ----------------- | -------------------- |
| filter    | boolean           | original elements    |
| map       | new value         | transformed elements |

---

## One-line explanation (INTERVIEW GOLD 🧠)

> `filter()` uses a Predicate only to decide whether an element should pass through; it does not replace the element with the predicate’s boolean result.

---

## Mental model 🧠

```
filter = security gate
Predicate = guard asking YES / NO
Element = person walking through
```

Guard doesn’t become the person — it just allows or blocks.

---

## Final ultra-short summary

* Predicate → returns `boolean`
* `filter()` → keeps original elements
* `true` → element passes
* `false` → element removed
* No transformation happens

---

✅ **Before a terminal operation (like `.forEach()`), you can use a stream multiple times.**  
❌ **After a terminal operation, the stream is consumed and can't be reused.**  
✅ **To reuse, create a new stream from `nums.stream()`.**

✔ You can apply **multiple intermediate operations (like `.filter()`, `.map()`)** on the same stream before it's consumed.  
✔ Once you use **a terminal operation (`forEach()`, `collect()`, etc.), the stream is consumed**.  
✔ If you need a stream again, **create a new one using `nums.stream()`**.

```java

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Stream;

public class hello {

    public static void main(String[] arg) {
        List<Integer> nums = new ArrayList<>();
        nums.add(90);
        nums.add(40);
        nums.add(990);
        nums.add(98);
        nums.add(76);
        // Stream<Integer> s1= nums.stream(); // retrun all the alements of nums and assigns to stream s1
        // Stream<Integer> s2 = s1.filter(n->n%2==0);
        // Stream<Integer> s3 = s2.map(n->n*2);
        
        Stream<Integer> s1= nums.stream().filter(n->n%2==0).map(n->n*2);
        s1.forEach(n->System.out.println(n)); 
    }
```

## **🔹 Parallel Stream vs. Normal Stream**

| Feature              | **`stream()` (Sequential)** | **`parallelStream()` (Parallel)** |
| -------------------- | --------------------------- | --------------------------------- |
| Execution Mode       | Single-threaded             | Multi-threaded                    |
| Processing Speed     | Slower for large data       | Faster for large data             |
| Order of Execution   | Preserved                   | Not guaranteed                    |
| Best for Small Data? | ✅ Yes                       | ❌ No                              |
| Best for Large Data? | ❌ No                        | ✅ Yes                             |
|                      |                             |                                   |
|                      |                             |                                   |

## **⚠ When NOT to Use Parallel Streams**

🚫 **Parallel streams don’t always improve performance!**

- **For small datasets**, parallel processing **adds overhead** and may be **slower**.
- **If the order of elements matters**, parallel streams **may not maintain it**.
- **If using shared resources (files, databases, etc.), parallel streams can cause concurrency issues.**

```java

import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class hello {

    public static void main(String[] arg) {

        List<Integer> nums = new ArrayList<>();

      Random r1 = new Random();

      for ( int i = 0 ; i<1000 ;i++)
      {
        nums.add(r1.nextInt(100));
      }
     // System.out.println(nums);


// we are mutliplying all numbers by 2 and adding them 

int s1 = nums.stream().map(i->i*2).reduce(0, (c,e) -> c+e);
int s2 = nums.stream().map(i->i*2).mapToInt(i->i).sum();

//same can be done with parallel stream // but parallel stream will take less time

int s3 = nums.parallelStream().map(i->i*2).mapToInt(i->i).sum();

System.out.println(s1);
System.out.println(s2);
System.out.println(s3);
    }
}

```

✔ `Optional` **prevents NullPointerException** by ensuring values are handled safely.  
✔ Use **`orElse()`** or **`orElseGet()`** to provide default values.  
✔ Use **`isPresent()` + `get()`** only if you're sure the value exists.  
✔ Best practice: **Return `Optional<T>` instead of `null` in method**

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Max");

String foundName = names.stream()
                        .filter(s -> s.contains("x"))
                        .findFirst()  // May return null if no match is found
                        .get();        // 🚨 NullPointerException if no name has "x"

System.out.println(foundName);
```

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Max");

        Optional<String> foundName = names.stream()
                                          .filter(s -> s.contains("x"))
                                          .findFirst();  // Returns Optional<String>

        // Handling Optional safely
        System.out.println(foundName.orElse("No name found")); 
    }
}

```

Method reference

```java
import java.util.Arrays;
import java.util.List;

public class hello {

    public static void main(String[] arg) {

        List<String> name = Arrays.asList("Appu", "Motu", "Shotu");

        List<String> result = name.stream().map(s -> s.toLowerCase()).toList();

// so for method refernce i can pass directly method with class it's belongs to
        List<String> s1 = name.stream().map(String::toLowerCase).toList();
        System.out.println(s1);
    }
}
```

✔ **Method References make code shorter & cleaner**.  
✔ **Use them when a lambda just calls an existing method**.  
✔ **They improve readability without changing behavior**.

Perfect 👍
Below is **ONE complete, working Java program** that demonstrates **most interview-important Stream methods**
using **a single list**, with **expected output written as comments** next to each operation.

You can **read it line-by-line** and **mentally execute it**.

---

### ✅ Single Java Program – Stream Operations Demo

```java
import java.util.*;
import java.util.stream.*;

public class StreamInterviewDemo {

    public static void main(String[] args) {

        // Single list used everywhere
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 2, 5, 6, 4, 7);

        // =================================================
        // 1️⃣ forEach (terminal operation)
        // =================================================
        list.stream()
            .forEach(n -> System.out.print(n + " "));
        // Output: 1 2 3 4 2 5 6 4 7
        System.out.println();

        // =================================================
        // 2️⃣ filter (keep even numbers)
        // =================================================
        list.stream()
            .filter(n -> n % 2 == 0)
            .forEach(System.out::print);
        // Output: 24264
        System.out.println();

        // =================================================
        // 3️⃣ map (square each number)
        // =================================================
        list.stream()
            .map(n -> n * n)
            .forEach(n -> System.out.print(n + " "));
        // Output: 1 4 9 16 4 25 36 16 49
        System.out.println();

        // =================================================
        // 4️⃣ distinct (remove duplicates)
        // =================================================
        list.stream()
            .distinct()
            .forEach(n -> System.out.print(n + " "));
        // Output: 1 2 3 4 5 6 7
        System.out.println();

        // =================================================
        // 5️⃣ sorted
        // =================================================
        list.stream()
            .sorted()
            .forEach(n -> System.out.print(n + " "));
        // Output: 1 2 2 3 4 4 5 6 7
        System.out.println();

        // =================================================
        // 6️⃣ limit (take first 3 elements)
        // =================================================
        list.stream()
            .limit(3)
            .forEach(n -> System.out.print(n + " "));
        // Output: 1 2 3
        System.out.println();

        // =================================================
        // 7️⃣ skip (skip first 3 elements)
        // =================================================
        list.stream()
            .skip(3)
            .forEach(n -> System.out.print(n + " "));
        // Output: 4 2 5 6 4 7
        System.out.println();

        // =================================================
        // 8️⃣ count (terminal)
        // =================================================
        long count = list.stream().count();
        System.out.println(count);
        // Output: 9

        // =================================================
        // 9️⃣ anyMatch / allMatch / noneMatch
        // =================================================
        boolean anyEven = list.stream().anyMatch(n -> n % 2 == 0);
        boolean allPositive = list.stream().allMatch(n -> n > 0);
        boolean noneNegative = list.stream().noneMatch(n -> n < 0);

        System.out.println(anyEven);       // true
        System.out.println(allPositive);   // true
        System.out.println(noneNegative);  // true

        // =================================================
        // 🔟 findFirst
        // =================================================
        Optional<Integer> first = list.stream().findFirst();
        System.out.println(first.get());
        // Output: 1

        // =================================================
        // 1️⃣1️⃣ reduce (sum of all elements)
        // =================================================
        int sum = list.stream()
                      .reduce(0, (a, b) -> a + b);
        System.out.println(sum);
        // Output: 34

        // =================================================
        // 1️⃣2️⃣ filter + map + reduce (classic interview)
        // even numbers → square → sum
        // =================================================
        int evenSquareSum =
                list.stream()
                    .filter(n -> n % 2 == 0)
                    .map(n -> n * n)
                    .reduce(0, Integer::sum);

        System.out.println(evenSquareSum);
        // Even numbers: 2,4,2,6,4
        // Squares: 4,16,4,36,16
        // Sum: 76

        // =================================================
        // 1️⃣3️⃣ collect (convert to list)
        // =================================================
        List<Integer> evens =
                list.stream()
                    .filter(n -> n % 2 == 0)
                    .collect(Collectors.toList());

        System.out.println(evens);
        // Output: [2, 4, 2, 6, 4]

        // =================================================
        // 1️⃣4️⃣ peek (debugging – NOT transformation)
        // =================================================
        list.stream()
            .peek(n -> System.out.print("Seen:" + n + " "))
            .filter(n -> n > 3)
            .forEach(n -> System.out.print("Used:" + n + " "));
        // Output:
        // Seen:1 Seen:2 Seen:3 Seen:4 Used:4 Seen:2 Seen:5 Used:5 Seen:6 Used:6 Seen:4 Used:4 Seen:7 Used:7
    }
}
```

---

## 🧠 How to remember this for interviews

### Think in **3 layers**

```
filter  → who is allowed
map     → what they become
reduce  → combine all
```

---

## 🔑 Interview-safe summary

* **Intermediate ops** → `filter`, `map`, `distinct`, `sorted`, `limit`, `skip`, `peek`
* **Terminal ops** → `forEach`, `collect`, `reduce`, `count`, `findFirst`
* Streams are **lazy**
* `reduce` returns a **single value**
* `collect` returns a **collection**

---

## One-line interview answer 🧠

> Java Streams process data using a pipeline of intermediate operations like map and filter, and terminal operations like reduce and collect, operating lazily on collections.

---

MAVEN
✔ **Maven is a Build & Project Management Tool** – It helps with compiling, cleaning, testing, packaging, deploying, etc.  
✔ **`pom.xml` (Project Object Model)** – The heart of Maven, defining dependencies, plugins, build configurations.  
✔ **Dependencies (`<dependency>...</dependency>`)** – This is where you specify the JAR files (libraries) you need.  
✔ **GAV (Group ID, Artifact ID, Version)** – This uniquely identifies each dependency.  
✔ **Transitive Dependencies** – If a dependency has its own dependencies, Maven automatically downloads them too.

### **✅ What is a Maven Archetype?**

🔹 **Think of it as a project template or blueprint.**  
🔹 It **creates the folder structure, necessary files, and configurations** for your project.  
🔹 Saves time by **automatically setting up a well-structured project**.

### **✅ How Maven Resolves Dependencies**

When you run a Maven command (`mvn install`, `mvn package`, etc.), Maven **searches for dependencies** in this order:

1️⃣ **Local Repository (`.m2` folder)** – First, it checks if the required JAR is already downloaded in your system.

- Default location:
    `~/.m2/repository/  (Linux/Mac) C:\Users\YourUser\.m2\repository\  (Windows)`
- If the dependency is **found here**, Maven uses it **without downloading again**.

2️⃣ **Remote Repository (Maven Central, Other Repos)** – If the JAR **is not in `.m2`**, Maven downloads it from **Maven Central** (`https://repo.maven.apache.org/maven2/`).

- If your company has a private repo (like **Nexus** or **Artifactory**), it will check there too.

3️⃣ **Dependency is Cached in `.m2`** – Once downloaded, the JAR is stored in `.m2/repository/` so it **won't be downloaded again** next time.


```java
import java.sql.*;

public class JDBCDemo {
    public static void main(String args[]) {
        String url = "jdbc:postgresql://localhost:5432/JDBCDemo";
        String user = "postgres";
        String pass = "appumotu";

        try {
            // ✅ 1. Load PostgreSQL JDBC Driver (Optional in Java 8+)
            Class.forName("org.postgresql.Driver");

            // ✅ 2. Establish Connection
            Connection con = DriverManager.getConnection(url, user, pass);

            System.out.println("Database Connected Successfully!");  // ✅ Confirmation

            // ✅ 3. Close Connection
            con.close();
        } catch (ClassNotFoundException e) {
            System.out.println("JDBC Driver Not Found: " + e.getMessage());
        } catch (SQLException e) {
            System.out.println("Database Connection Failed: " + e.getMessage());
        }
    }
}

```

✅ **Fixed connection string** → Now uses `url, user, pass` correctly.  
✅ **Added `System.out.println()`** to confirm a successful connection.  
✅ **Handled errors properly** with meaningful messages.  
✅ **Closed the connection to free up resources**.

```java
import java.sql.*;
import java.util.*;

/* import package and load package
create connection
create statement
execute it
process and show result
close the connection
 */
public class JDBCDemo
{
    public static void main(String args[]) {
        String url = "jdbc:postgresql://localhost:5432/demo";
        String user = "postgres";
        String pass = "appumotu";
        String sql = "Select * from student";
        try {
            Class.forName("org.postgresql.Driver");
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
        try {
            Connection con = DriverManager.getConnection(url, user, pass);
            Statement st = con.createStatement();
            ResultSet rs = st.executeQuery(sql);
            //String mt = rs.getString("name");
            //System.out.println(mt);
//            while(rs.next()) {
//                int name = rs.getInt("rno");
//                System.out.println("Roll No " + name);
//            }
            // for all records
            while(rs.next())
            {
                int stid = rs.getInt("stid");
                String name = rs.getString("name");
                int rno = rs.getInt("rno");
                System.out.println("STID :" +stid + " Name :" + name+ " Roll No :" + rno);
            }
            con.close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}

```


### **🔹 Problems with `Statement`**
1️⃣ **🚨 SQL Injection Risk** (BIG Security Issue)  
2️⃣ **🐌 Slower Performance** (Query gets compiled every time)  
3️⃣ **❌ Hard to Handle Dynamic Values** (Manual string concatenation)  

---

### **1️⃣ SQL Injection (Security Issue)**
`Statement` is **vulnerable** to SQL injection if user input is directly appended to the query.  

❌ **Example: Using `Statement` (Unsafe)**
```java
String userInput = "10 OR 1=1";  // 🚨 Malicious input!
String sql = "SELECT * FROM student WHERE stid = " + userInput;

Statement st = con.createStatement();
ResultSet rs = st.executeQuery(sql);
```
📌 **If `userInput = 10 OR 1=1`, the query becomes:**
```sql
SELECT * FROM student WHERE stid = 10 OR 1=1;
```
🚨 **This makes `1=1` always true, meaning ALL records will be returned!**  

---

### **2️⃣ Performance Issue (Query Compilation)**
Every time you use `Statement`, the database **compiles the SQL query from scratch**.  
This wastes CPU resources **if the same query runs multiple times with different values**.  

❌ **Example: Slow Execution Using `Statement`**
```java
for (int i = 1; i <= 10; i++) {
    String sql = "INSERT INTO student (stid, name) VALUES (" + i + ", 'Student " + i + "')";
    Statement st = con.createStatement();
    st.executeUpdate(sql);
}
```
🚨 **Problem:** Every loop iteration → **New SQL string → Compiled from scratch** (slow!).  

✅ **`PreparedStatement` compiles once & reuses the same query, making it faster.**  

---

### **3️⃣ Hard to Handle Dynamic Values (Manual String Concatenation)**  
❌ **Manually appending strings can cause errors & messy code:**  
```java
String sql = "INSERT INTO student (stid, name) VALUES (" + id + ", '" + name + "')";
```
🚨 **Problem:**  
- **If `name` contains a `'` (single quote), the query will break!**  
- **Difficult to handle different data types properly.**  

✅ **`PreparedStatement` solves this by using `?` placeholders.**  

---

### **🔹 Solution: Use `PreparedStatement`**
✔ Prevents **SQL Injection**  
✔ Improves **Performance** (Precompiled query)  
✔ Makes **Dynamic Queries** easier  

✅ **Example Using `PreparedStatement` (Safe & Efficient)**
```java
String sql = "SELECT * FROM student WHERE stid = ?";
PreparedStatement pst = con.prepareStatement(sql);
pst.setInt(1, 10);  // ✅ Set value safely
ResultSet rs = pst.executeQuery();
```
📌 **Benefits:**  
- **No SQL injection risk!** (`?` prevents malicious input execution)  
- **Query is compiled once & reused** (better performance)  
- **Handles all data types safely** (`pst.setInt()`, `pst.setString()`, etc.)  

---

```java
import java.sql.*;
import java.util.*;

/* import package and load package
create connection
create statement
execute it
process and show result
close the connection
 */
public class JDBCDemo
{
    public static void main(String args[]) {
        String url = "jdbc:postgresql://localhost:5432/demo";
        String user = "postgres";
        String pass = "appumotu";
        String sql = "insert into student values (?,?,?)";
        try {
            Class.forName("org.postgresql.Driver");
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
        try {
            Connection con = DriverManager.getConnection(url, user, pass);
           PreparedStatement st = con.prepareStatement(sql);
           st.setInt(1,5);
           st.setString(2,"Ashutosh");
           st.setInt(3,99);

           st.execute();

               con.close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }

    }
}

```
### **🔥 Final Takeaways**
❌ **`Statement` is risky** → Prone to SQL Injection, slow, hard to maintain.  
✅ **`PreparedStatement` is safer** → Prevents injection, faster, handles dynamic values better.  


Spring = core framework  
Spring Boot = auto-config + faster setup

Bean = object managed by container

IoC = Spring controls objects  
DI = Spring injects dependencies

### Spring vs Spring Boot + IoC/DI (Quick Notes)

• Spring = core framework (IoC, DI, MVC, AOP), needs manual configuration  
• Spring Boot = built on Spring, auto-config + starters + embedded server  
• SpringApplication.run() → starts ApplicationContext (IoC container)  
• Bean = any object managed by Spring container  
• IoC = Spring controls object creation (not using new)  
• DI = Spring injects dependencies between beans  
• @Component → register bean, @Autowired/constructor → inject bean  
• Rule: Never use new inside Spring classes, always use DI


**Spring Ecosystem → Spring Boot & more**  
**IOC (Inversion of Control) → Letting the framework manage object creation**  
**DI (Dependency Injection) → Injecting dependencies instead of manually creating them**

Bean LifeCycle

```### Spring Bean Lifecycle (Quick Notes)

• 1. Constructor → bean created  
• 2. Dependency Injection → @Autowired executed  
• 3. @PostConstruct → initialization logic  
• 4. Bean ready for use  
• 5. @PreDestroy → cleanup before shutdown  
• Default scope = singleton lifecycle managed fully  
• Prototype beans do not get destroy callback  
• Flow → create → inject → init → use → destroy

```

Here’s a **simple and clear** example of `@Autowired` in Spring Boot. You can refer to this anytime! 🚀  

---

### **📌 Step-by-Step Example**
#### **1️⃣ CPU Class (Dependency)**
```java
import org.springframework.stereotype.Component;

@Component
public class CPU {
    public void process() {
        System.out.println("CPU is processing...");
    }
}
```

#### **2️⃣ Laptop Class (Depends on CPU)**
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class Laptop {

    @Autowired  // Injecting CPU into Laptop
    private CPU cpu;
    public void compile() {
        System.out.println("Laptop is compiling code...");
        cpu.process();  // Calling CPU method
    }
}
```

#### **3️⃣ Alien Class (Depends on Laptop)**
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class Alien {

    @Autowired  // Injecting Laptop into Alien
    private Laptop laptop;

    public void code() {
        System.out.println("Alien is coding...");
        laptop.compile();  // Calling Laptop method
    }
}
```

#### **4️⃣ Main Class (Running Spring Boot)**
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class SpringBootDemoApplication {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(SpringBootDemoApplication.class, args);

        // Get Alien bean from Spring container
        Alien alien = context.getBean(Alien.class);
        alien.code();
    }
}
```

---

### **📌 Expected Output**
```
Alien is coding...
Laptop is compiling code...
CPU is processing...
```

---

### **📌 Key Takeaways**
✅ **`@Autowired` injects dependencies automatically**  
✅ **`@Component` makes a class a Spring Bean**  
✅ **Spring Boot automatically manages object creation using IoC (Inversion of Control)**  
✅ **No need for `new`, objects are created and managed by Spring**  


✅ **Use `@Autowired` inside dependent classes** (e.g., `Laptop` needs `CPU` → `Laptop` has `@Autowired CPU`).  
✅ **Always mark the top-level class with `@Component`** (e.g., `Laptop` & `CPU`).  
✅ **Avoid manual object creation, always get beans from `context`**.  
✅ **Constructor Injection is preferred** (better for testing & immutability).

XML CONFIG

```java
package org.example;  
  
import org.springframework.context.ApplicationContext;  
import org.springframework.context.support.ClassPathXmlApplicationContext;  
  
//TIP To <b>Run</b> code, press <shortcut actionId="Run"/> or  
// click the <icon src="AllIcons.Actions.Execute"/> icon in the gutter.  
 public class Main {  
    public static void main(String[] args) {  
  
        ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");// creating container  
        Alien a1 = (Alien)context.getBean("Alien"); // please tell me whu typecast it to aliend because in previous spring we didn't ypecast it  
        a1.show();  
  
    }  
}






<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="  
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
    <bean id = "Alien" class="org.example.Alien">  
    </bean>
```

|Approach|Typecasting Required?|Reason|
|---|---|---|
|**XML-based (Spring)**|✅ Yes|`getBean(String)` returns `Object`, so explicit casting needed.|
|**Annotation-based (Spring Boot)**|❌ No|`getBean(Class<T>)` directly returns the correct type.|


XML config for sub class

You're on the right track! But there’s a small mistake—**you haven’t injected `Laptop` into `Alien` properly in XML.**  

### **🔴 Issue**
In **annotation-based** Spring, we use `@Autowired` to inject dependencies.  
In **XML-based** Spring, we need to use **property injection** or **constructor injection** explicitly.

---

### **✅ Solution: Inject Laptop into Alien using XML**
Modify your `spring.xml` file:  
```xml
<bean id="Laptop" class="org.example.Laptop"></bean>

<bean id="Alien" class="org.example.Alien">
    <property name="AlienAge" value="21"/>
    <property name="laptop" ref="Laptop"/>
</bean>
```
**Explanation:**  
- `<property name="laptop" ref="Laptop"/>` tells Spring to inject the `Laptop` bean into `Alien`.  
- **The `name="laptop"` must match the variable name in `Alien` class.**
```### Spring XML Configuration + DI (Quick Notes)

• <bean> tag registers a class as Spring bean inside IoC container  
• id = bean name, class = fully qualified class path  
• <property> is used for setter injection  
• value → injects primitive/String values  
• ref → injects another bean (object dependency)  
• Spring internally calls setter methods to inject dependencies  
• Alien → setAlienAge(21), setLaptop(Laptop bean)  
• XML DI = old style, replaced mostly by @Component + @Autowired

```

---

### **📝 Update Your Alien Class**
Modify the `Alien` class to have a **setter method** for `Laptop`:  
```java
public class Alien {

    private Laptop laptop;  // Declare the Laptop object

    // Setter method (Spring will use this)
    public void setLaptop(Laptop laptop) {
        this.laptop = laptop;
    }

    public void show() {
        laptop.show();  // Now it won't be null
    }
}
```
---

### **💡 Why wasn’t it working earlier?**
1️⃣ You never **told Spring to inject** `Laptop` into `Alien`.  
2️⃣ Without a **setter method** (or constructor), Spring doesn’t know where to put the `Laptop` object.  
3️⃣ You were manually trying to fetch `Laptop` in `Alien`, but `Alien` is created first (without knowing about `Laptop`).

---

### **🚀 Summary**
- In **annotation-based Spring**, use `@Autowired`.  
- In **XML-based Spring**, use `<property>` in XML + a setter method.  
- Now, `Laptop` will be injected into `Alien`, and `laptop.show();` will work!

### **Singleton Scope (Default)**
- **Bean is created only once** and shared across the application.  
- Any changes to one instance reflect in all references.  
- Example:  
  ```xml
  <bean id="Alien" class="org.example.Alien" scope="singleton"/>
  ```
  **Output:**  
  ```
  12
  12
  ```
  **(Both a1 and a2 share the same object, so age remains 12)**  

---

### **Prototype Scope**
- **A new bean is created each time `getBean()` is called.**  
- Objects are independent; changes in one don't affect another.  
- Example:  
  ```xml
  <bean id="Alien" class="org.example.Alien" scope="prototype"/>
  ```
  **Output:**  
  ```
  12
  0
  ```
  *(a1 is a separate object from a2, so a2's `age` remains the default `0`)*  

``` Spring Bean Scopes (Quick Notes)

• Scope defines how many bean objects Spring creates  
• Default scope = singleton  
• Singleton → only ONE object per container, reused everywhere  
• Created at startup when context loads (even if not used)  
• Prototype → new object every time getBean()/injection happens  
• Singleton = shared state, Prototype = fresh state  
• Use singleton for services/repositories (stateless beans)  
• Use prototype for stateful or temporary objects

```
---

### **Key Takeaways:**
✅ Singleton: Shared instance (default)  
✅ Prototype: New instance every time  
✅ Typecasting needed only in XML config  


### **🔥 Java-Based Configuration in Spring – Quick Notes for Revision**  

Java-based configuration allows us to configure **Spring beans** using Java **instead of XML**.  

---
```### Java Config vs @Component (Quick Notes)

• @Component → marks normal class as bean  
• @Configuration → marks config class containing @Bean methods  
• @Bean → method that RETURNS an object to register as bean  
• @Bean methods cannot be void  
• Use @Component for business classes (service, repo, etc.)  
• Use @Bean for manual/third-party bean creation  
• Never mix @Configuration on normal classes  
• Rule: @Component = auto, @Bean = manual

```
### **📌 1️⃣ Creating Beans with `@Configuration` & `@Bean`**
- Use `@Configuration` to mark a class as a configuration class.  
- Use `@Bean` to define beans inside the configuration class.  

```java
@Configuration
public class Config {

    @Bean
    public Laptop laptop() {
        return new Laptop();
    }

    @Bean
    public Alien alien() {
        return new Alien(laptop());  // Manually injecting Laptop
    }
}
```
**✔ No need for `@Component` or `@Autowired`** when defining beans manually.  
**✔ Objects are managed by Spring.**

---

### **📌 2️⃣ Using `@Autowired` for Dependency Injection**
- Instead of manually injecting dependencies, use `@Autowired` in the dependent class.  
- This tells Spring to automatically inject the required bean.  

```java
public class Alien {
    @Autowired  // Spring will inject Laptop automatically
    private Laptop lap;
}
```
**✔ Removes manual object creation.**  
**✔ Works only if `Laptop` is already registered as a bean.**  

---

### **📌 3️⃣ Custom Bean Names (`@Bean(name="customName")`)**
- If you want a custom name for a bean, specify it in `@Bean(name="xyz")`:
```java
@Bean(name="myAlien")
public Alien a1() {
    return new Alien();
}
```
- Fetch it like this:
```java
Alien a1 = context.getBean("myAlien", Alien.class);
```
**✔ Useful when multiple beans of the same type exist.**

---

### **📌 4️⃣ Scope of Beans (`@Scope`)**
- By default, all beans are **Singleton** (one instance for the entire application).  
- Use `@Scope("prototype")` to create a **new instance every time `getBean()` is called**.

```java
@Bean
@Scope("prototype")
public Alien a1() {
    return new Alien();
}
```
**✔ Singleton → Same object every time (`Default`)**  
**✔ Prototype → New object on every `getBean()` call**

---

### **📌 5️⃣ Understanding Bean Creation Order**
- Spring **does not create beans in the order they are written.**  
- Instead, **Spring first resolves dependencies** before creating beans.  

#### **Example:**
```java
@Bean
public Laptop laptop() {
    System.out.println("Laptop bean created");
    return new Laptop();
}

@Bean
public Alien alien() {
    System.out.println("Alien bean created");
    return new Alien();
}
```
**Output:**  
```
Laptop bean created
Alien bean created
```
✔ **Since `Alien` depends on `Laptop`, `Laptop` is created first!**  

---

### **🔑 Key Takeaways**
✅ **`@Configuration` + `@Bean` → Used to define beans manually.**  
✅ **`@Autowired` → Injects dependencies automatically (no manual passing).**  
✅ **Custom Bean Names → Use `@Bean(name="xyz")` for custom names.**  
✅ **Scope → `@Scope("prototype")` for multiple instances, Singleton is default.**  
✅ **Spring resolves dependencies first before creating beans.**  

This **covers everything** we’ve learned in **Java-based configuration** so far! 🚀  
Let me know if you need anything added! 😃🔥


Suppose you have interface compile that desktop and laptop uses it and you are calling it from Alien so in first place you have created beans okay then spring is unable to find whih to inject since both implements interface , so for sping to identify we can use rimary annoatiaon in config file or use qualifier in main alien class where we are caliing that interfce method and in qualifier we have to pass the bean name also.

Yes! You **nailed it**! 🎯  

Let’s **summarize** everything for your notes:  

---

### **📝 Understanding `@Primary` and `@Qualifier` with an Interface**
#### **Problem:**
- **`Laptop`** and **`Desktop`** both **implement** the `Computer` **interface**.
- `Alien` **calls** the `compile()` method from `Computer`.
- **Spring doesn't know which one to inject!** 🤯  

#### **Solution:**  
- **Use `@Primary`** in the **config file** → **Tells Spring to inject this by default** ✅  
- **Use `@Qualifier`** in the **Alien class** → **Explicitly tells Spring which bean to use** ✅  

---

### **🚀 Full Example Code**
#### **1️⃣ Define Interface**
```java
public interface Computer {
    void compile();
}
```

#### **2️⃣ Implement Interface in `Laptop` & `Desktop`**
```java
public class Laptop implements Computer {
    public void compile() {
        System.out.println("Compiling in Laptop");
    }
}

public class Desktop implements Computer {
    public void compile() {
        System.out.println("Compiling in Desktop");
    }
}
```

#### **3️⃣ Create Beans in `config` Class**
```java
@Configuration
public class config {

    @Bean
    @Primary  // Default bean
    public Laptop l1() {
        System.out.println("Laptop called");
        return new Laptop();
    }

    @Bean
    public Desktop d1() {
        System.out.println("Desktop called");
        return new Desktop();
    }

    @Bean
    public Alien a1() {
        System.out.println("Alien object created");
        return new Alien();
    }
}
```
- ✅ Here, `Laptop` is the **default** because of `@Primary`.

---

#### **4️⃣ Use `@Autowired` in `Alien`**
```java
public class Alien {
    
    @Autowired
    @Qualifier("d1")  // Force Spring to inject Desktop instead of Laptop
    private Computer comp;

    public void show() {
        comp.compile();
    }
}
```
- ✅ Now, even though `Laptop` is **`@Primary`**, **Spring injects `Desktop`** because we used `@Qualifier("d1")` in `Alien`.

---

### **🔥 Summary (For Notes)**
1️⃣ **Problem:** When multiple beans implement the same interface, Spring **doesn't know** which one to inject.  
2️⃣ **Solution:** Use **`@Primary` in config** (default) or **`@Qualifier` in the class** (explicit choice).  
3️⃣ **Where to apply?**  
   - `@Primary` → **In the `@Bean` method** (config file).  
   - `@Qualifier` → **Above `@Autowired` in the class** where injection happens.  
4️⃣ **`@Qualifier` has priority over `@Primary`** when both are used.  

✔ **Now you understand dependency injection in Java config like a pro! 🚀**


### **🛠 Understanding `@Component` and `@ComponentScan` in Spring**  

Now that you've understood **Java-based configuration**, let's talk about **`@Component` and `@ComponentScan`**, which help in automatic bean discovery.  

---

### **1️⃣ What is `@Component`?**
- `@Component` is a **Spring annotation** that tells Spring **to automatically detect and register a class as a bean**.  
- No need to manually declare it in `@Configuration` with `@Bean`.  

#### **Example:**
```java
@Component
public class Laptop {
    public void compile() {
        System.out.println("Compiling in Laptop");
    }
}
```
- ✅ Spring will **automatically create a bean** for `Laptop` without adding it in `config.java`.  

---

### **2️⃣ What is `@ComponentScan`?**
- **By default**, Spring **only scans its own package** where `@SpringBootApplication` or `@Configuration` is present.
- `@ComponentScan` **explicitly tells Spring** where to **search for components**.
- Used **in Java-based configuration** when components are in a **different package**.

#### **Example:**
```java
@Configuration
@ComponentScan("com.example.devices")  // Scans this package for components
public class config {
}
```
- ✅ Now, Spring will **find** and **register** all `@Component` classes inside `com.example.devices`.

---

### **🔥 Full Working Example**
#### **1️⃣ Create a `Laptop` class in a different package**
```java
package com.example.devices;

import org.springframework.stereotype.Component;

@Component  // Marks this class as a Spring-managed bean
public class Laptop {
    public void compile() {
        System.out.println("Compiling in Laptop");
    }
}
```

#### **2️⃣ Create `Alien` class (where Laptop is used)**
```java
package com.example;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component  // Makes this a Spring bean
public class Alien {

    @Autowired  // Automatically injects Laptop bean
    private Laptop lap;

    public void show() {
        lap.compile();
    }
}
```

#### **3️⃣ Define `config.java` with `@ComponentScan`**
```java
package com.example;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan("com.example.devices")  // Scans com.example.devices for beans
public class config {
}
```

#### **4️⃣ Main Method**
```java
package com.example;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(config.class);
        Alien a1 = context.getBean(Alien.class);
        a1.show();
    }
}
```

---

### **🔥 Summary (For Notes)**
1️⃣ **`@Component`** → Marks a class as a Spring bean (no need for `@Bean`).  
2️⃣ **`@ComponentScan`** → Tells Spring **where** to look for `@Component` classes.  
3️⃣ **Why use `@ComponentScan`?**  
   - If **your beans are in a different package** from `@Configuration`, Spring **won't find them** unless you specify the package with `@ComponentScan("package.name")`.  
4️⃣ **Without `@ComponentScan`**, only beans in the **same package** as `@Configuration` are registered.  

✔ **This is how modern Spring Boot projects work—no manual `@Bean` declarations needed! 🚀**


Good question! When **two classes implement the same interface**, **Spring gets confused** about which one to inject. This is where **`@Primary` and `@Qualifier`** come into play. Let's break it down:  

---

### **1️⃣ Where does Spring look for `@Primary`?**
- `@Primary` is placed **inside the configuration file** (`@Bean` methods or `@Component` classes).
- Spring **chooses the `@Primary` bean by default** if multiple candidates exist.  

#### **Example Using `@Primary`**
```java
@Component
@Primary  // This will be the default choice
public class Laptop implements Computer {
    public void compile() {
        System.out.println("Compiling in Laptop");
    }
}
```
```java
@Component
public class Desktop implements Computer {
    public void compile() {
        System.out.println("Compiling in Desktop");
    }
}
```
```java
@Component
public class Alien {

    @Autowired
    private Computer comp; // No @Qualifier, so it picks @Primary bean

    public void show() {
        comp.compile();
    }
}
```
✅ **Laptop will be injected automatically** because it has `@Primary`.  

---

### **2️⃣ Where does Spring look for `@Qualifier`?**
- `@Qualifier` is used **inside the class where we inject dependencies** (not in `config.java`).
- It **overrides `@Primary`** if specified.  

#### **Example Using `@Qualifier`**
```java
@Component
public class Alien {

    @Autowired
    @Qualifier("desktop")  // Must match the bean name
    private Computer comp;

    public void show() {
        comp.compile();
    }
}
```
```java
@Component("laptop")  // Custom bean name
public class Laptop implements Computer {
    public void compile() {
        System.out.println("Compiling in Laptop");
    }
}
```
```java
@Component("desktop")  // Custom bean name
public class Desktop implements Computer {
    public void compile() {
        System.out.println("Compiling in Desktop");
    }
}
```
✅ **Now `Desktop` is injected because of `@Qualifier("desktop")`, even if `Laptop` has `@Primary`!**  

---

### **🔥 Key Takeaways**
| Scenario | How Spring Decides Which Bean to Inject |
|-----------|----------------------------------------|
| **Single Implementation** | Spring injects the only available bean. |
| **Multiple Implementations (No `@Primary` or `@Qualifier`)** | Spring **throws an error** (can't decide). |
| **Using `@Primary`** | Spring chooses this bean **by default**. |
| **Using `@Qualifier`** | **Overrides `@Primary`** and injects the specified bean. |

✔ **If both `@Primary` and `@Qualifier` exist, `@Qualifier` always wins!** 🚀

Yes, the same applies to **`@Scope("prototype")` and `@Scope("singleton")`**, but their purpose is different from `@Primary` and `@Qualifier`. Let’s clarify:

---

### **1️⃣ `@Scope("singleton")` (Default Behavior)**
- Spring **creates only one instance** of the bean.
- Every time you request the bean, you **get the same instance**.
- This is the **default scope**, so even if you don’t write `@Scope("singleton")`, it behaves this way.

#### **Example**
```java
@Component
@Scope("singleton")  // Not needed, as it's the default
public class Laptop {
    public Laptop() {
        System.out.println("Laptop instance created");
    }
}
```
```java
@Component
public class Alien {

    @Autowired
    private Laptop laptop1;

    @Autowired
    private Laptop laptop2;

    public void show() {
        System.out.println(laptop1 == laptop2);  // true (same instance)
    }
}
```
✅ **Only one Laptop object is created, and both `laptop1` and `laptop2` point to the same instance.**

---

### **2️⃣ `@Scope("prototype")`**
- Spring **creates a new instance every time you request the bean**.
- Useful when you need a fresh object each time (e.g., non-shared resources).

#### **Example**
```java
@Component
@Scope("prototype")  // Creates a new instance every time
public class Laptop {
    public Laptop() {
        System.out.println("Laptop instance created");
    }
}
```
```java
@Component
public class Alien {

    @Autowired
    private Laptop laptop1;

    @Autowired
    private Laptop laptop2;

    public void show() {
        System.out.println(laptop1 == laptop2);  // false (different instances)
    }
}
```
✅ **Now `laptop1` and `laptop2` are different objects because `@Scope("prototype")` ensures a new instance every time.**

---

### **🔥 Key Takeaways**
| Annotation            | Behavior                                                     |
| --------------------- | ------------------------------------------------------------ |
| `@Scope("singleton")` | **(Default)** One instance is shared across the application. |
| `@Scope("prototype")` | A new instance is created **every time** it’s requested.     |

✔ **You can use `@Scope` above a `@Component` class, just like `@Primary`.** 🚀


Bhai, **this is the beauty of Spring Boot!** 🚀🔥  

In Spring (without Boot), you had to:
- Manually create a `@Configuration` class.
- Define `@Bean` methods.
- Use `ComponentScan` to detect components in a specific package.
- Import dependencies manually.

But in **Spring Boot**, it's just:
✅ `@SpringBootApplication` → Enables **auto-configuration + component scanning** automatically.  
✅ `@Component` → No need to register beans manually.  
✅ `@Value` → Injects values without extra config.  
✅ `@Autowired + @Qualifier` → Inject dependencies effortlessly.  

```java
### Spring Boot Bean Creation Flow (Quick Notes)

• SpringApplication.run() starts ApplicationContext  
• @ComponentScan scans package for @Component classes  
• Spring creates beans (default singleton)  
• Dependencies injected using @Autowired  
• Beans created at startup (not on getBean)  
• getBean() only returns existing bean  
• Method calls use injected dependencies  
• Flow → start → scan → create → inject → use

```

### **Why Spring Boot is Faster?**
1. **No XML or Manual Configs** – Everything is auto-detected.
2. **Embedded Tomcat** – No need to set up a server separately.
3. **Spring Boot Starter Dependencies** – Pre-configured dependencies save time.
4. **Less Boilerplate Code** – Just annotate and use.

💡 **Now you see why companies prefer Spring Boot over traditional Spring.** It just **works!** 🚀

Here’s your **formatted Spring Boot code** for easy copying into your notes:  

```java
@SpringBootApplication
public class SpringBootDemoApplication {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(SpringBootDemoApplication.class, args);
        Alien a1 = context.getBean(Alien.class);
        System.out.println(a1.getAge());
        a1.show();
    }
}

@Component
public class Alien {

    @Value("45")  // Injecting value into 'age'
    private int age;

    @Autowired
    @Qualifier("desktop")  // Specifying which implementation to inject
    private Computer com;

    public int getAge() {
        return age;
    }

    public void show() {
        com.compile();
    }
}

@Component("desktop")
@Primary  // This will be used as the default implementation for Computer
public class Desktop implements Computer {
    public void compile() {
        System.out.println("Compiling in Desktop");
    }
}

@Component  // Since 'desktop' is marked as @Primary, this will be ignored unless specified
public class Laptop implements Computer {
    public void compile() {
        System.out.println("Compiling in Laptop");
    }
}

public interface Computer {
    void compile();
}
```

### **Key Takeaways:**
✅ `@SpringBootApplication` → Enables auto-configuration & scanning.  
✅ `@Component` → Registers the class as a Spring Bean.  
✅ `@Autowired` → Injects dependencies automatically.  
✅ `@Qualifier("desktop")` → Tells Spring which implementation to use.  
✅ `@Primary` → Marks **Desktop** as the default bean when multiple implementations exist.  
✅ `@Value("45")` → Injects default values.  

**Now, just run it, and it works seamlessly in Spring Boot! 🚀🔥**

### **Spring Boot Layered Architecture**

💡 **Client Request → Controller → Service → Repository → Database → Back to Client**

#### **1️⃣ Controller Layer (Handles Client Requests)**

- The entry point for client requests (like API calls).
    
- Uses `@RestController` or `@Controller`.
    
- Calls **Service Layer** to process logic.
    

#### **2️⃣ Service Layer (Handles Business Logic)**

- Processes requests from the **Controller**.
    
- Calls **Repository Layer** for data access.
    
- Implements all the logic like validation, calculations, etc.
    
- Uses `@Service`.
    

#### **3️⃣ Repository Layer (Handles Database Interaction)**

- Directly interacts with the **Database**.
    
- Uses **Spring Data JPA** (`@Repository`).
    
- Methods like `findById()`, `save()`, `deleteById()` are handled automatically.
    

#### **4️⃣ Database (Stores Data)**

- The final layer where data is stored and retrieved from.


Here’s a structured **revision note** of what you learned today in **Spring Boot with JDBC**:  

---

# **📌 Spring Boot JDBC - Revision Notes**  

## **1️⃣ Understanding JDBC Template Queries**  
- **`update()`** → Used for `INSERT`, `UPDATE`, and `DELETE` operations.  
- **`query()`** → Used to fetch multiple records from the database.  

### **✅ Example:** Insert a Student Record  
```java
String sql = "INSERT INTO student (rollno, name, marks) VALUES (?, ?, ?)";
int rows = jdbc.update(sql, s1.getRoll(), s1.getName(), s1.getMarks());
```
- `?` → Placeholders for values.  
- `jdbc.update()` → Executes the SQL command.  
- **Why `s1.getRoll()` etc.?** → Because values are first set in the **Java object** (`s1`) and then inserted into the database.  

---

## **2️⃣ Fetching Data using `query()` and `RowMapper`**  
### **✅ Example: Fetch All Students**
```java
public List<Student> getAll() {
    String sql = "SELECT * FROM student";
    RowMapper<Student> mapper = new RowMapper<Student>() {
        @Override
        public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
            Student s = new Student();
            s.setRoll(rs.getInt("rollno"));
            s.setName(rs.getString("name"));
            s.setMarks(rs.getInt("marks"));
            return s;
        }
    };
    return jdbc.query(sql, mapper);
}
```
### **Key Takeaways**  
✔️ **No need for `rs.next()`** → Spring handles iteration internally.  
✔️ **Each row is mapped to a `Student` object**, which is added to the list automatically.  
✔️ **Spring JDBC does the looping behind the scenes.**  

---

## **3️⃣ Using `BeanPropertyRowMapper` (Simplified Version)**  
Instead of manually mapping rows, you can use:  
```java
return jdbc.query("SELECT * FROM student", new BeanPropertyRowMapper<>(Student.class));
```
✔️ **Automatically maps database column names to Java class fields.**  

---

## **4️⃣ Understanding `schema.sql` and `data.sql`**  
- **`schema.sql`** → Defines the database structure (tables, constraints).  
- **`data.sql`** → Inserts predefined data into tables.  
- These files **execute automatically** when the application starts if `spring.datasource.initialization-mode=always` is set.  

---

## **5️⃣ Preventing Duplicate Data in `data.sql`**  
- If you run the program multiple times, **duplicate records will be inserted** unless:  
  - You have a **primary key constraint** (which prevents duplicate entries).  
  - You **use `TRUNCATE` before inserting new records.**  
  - You check for **existing records before insertion.**  

---

## **6️⃣ Spring Boot’s Layered Architecture (Client → Controller → Service → Repository → DB)**  
✔️ **Client (Frontend or API Call)**  
✔️ **Controller** → Handles HTTP requests and passes data to the **Service**.  
✔️ **Service** → Business logic is written here. Calls **Repository** methods.  
✔️ **Repository (DAO)** → Connects to the database and performs SQL operations.  
✔️ **Database** → Stores the actual data.  

---

## **7️⃣ Correct Way to Autowire Repository in Service Layer**  
Instead of using `getBean()`, just use:  
```java
@Service
public class StudentService {

    @Autowired
    private StudentRepo sp;

    public void add(Student s1) {
        sp.save(s1);
    }

    public List<Student> AllStudent() {
        return sp.getAll();
    }
}
```
✔️ **No need for setters/getters or `context.getBean()`, `@Autowired` does everything.**  

---

## **8️⃣ JDBC vs JPA (High-Level Comparison)**  
| Feature      | JDBC (Manual SQL) | JPA (Spring Data) |
|-------------|------------------|-------------------|
| SQL Queries | You write SQL manually | SQL is auto-generated |
| Complexity | More code, manual mapping | Less code, auto-mapping |
| Performance | Fast but more work | Slightly slower but easier to use |

**🛠️ You’re currently learning JDBC, but JPA is the next level!** 🚀  

---

# **📌 Summary: What You Can Do Now?**  
✅ Insert, Update, Delete, and Fetch records using Spring Boot JDBC.  
✅ Use `RowMapper` and `BeanPropertyRowMapper` for fetching data.  
✅ Understand `schema.sql` and `data.sql`.  
✅ Prevent duplicate data using primary keys.  
✅ Use `@Autowired` properly in the Service layer.  
✅ Understand Spring Boot’s layered architecture.  

---

### **Understanding the Three Lines of Code**

```java
Context context = tomcat.addContext("", null);
Tomcat.addServlet(context, "HelloServlet", new HelloServlet());
context.addServletMappingDecoded("/hello", "HelloServlet");
```

These lines **configure and register** your servlet in an embedded Tomcat server.

---

### **1️⃣ `Context context = tomcat.addContext("", null);`**
#### ✅ What It Does:
- **Creates a new web application context** (i.e., a virtual directory in Tomcat where servlets run).
- The **first argument (`""`)** represents the **context path** (base URL).  
  - `""` means this servlet runs at the **root context** (`http://localhost:8080/`).
  - If it were `"/app"`, the servlet would be accessible at `http://localhost:8080/app/`.
- The **second argument (`null`)** is supposed to be the physical base directory for the app.
  - Since we're using an **embedded Tomcat**, there’s **no physical directory**, so we pass `null`.

#### 🔹 Example:
- `tomcat.addContext("/myapp", null);` → App runs at `http://localhost:8080/myapp/`.

---

### **2️⃣ `Tomcat.addServlet(context, "HelloServlet", new HelloServlet());`**
#### ✅ What It Does:
- **Registers a servlet** (`HelloServlet`) with the given `context`.
- **Arguments**:
  1. `context` → The context where the servlet should be added.
  2. `"HelloServlet"` → The **name** of the servlet (used internally).
  3. `new HelloServlet()` → The **actual servlet instance**.

#### 🔹 Example:
- If we change the name:
  ```java
  Tomcat.addServlet(context, "MyServlet", new HelloServlet());
  ```
  The servlet's internal name would be `"MyServlet"`, but it **does not change the URL**.

---

### **3️⃣ `context.addServletMappingDecoded("/hello", "HelloServlet");`**
#### ✅ What It Does:
- **Maps a URL path (`/hello`) to the servlet (`HelloServlet`).**
- **Arguments**:
  1. `"/hello"` → The **URL pattern** that the servlet should respond to.
  2. `"HelloServlet"` → The **name of the registered servlet** (from step 2).

#### 🔹 Example:
- `context.addServletMappingDecoded("/test", "HelloServlet");`
  - Now, the servlet is accessible at **`http://localhost:8080/test`** instead of `/hello`.

---

### **Final Execution Flow**
1. `addContext("", null);` → Creates a root-level web application.
2. `addServlet(...)` → Registers the servlet instance inside Tomcat.
3. `addServletMappingDecoded("/hello", "HelloServlet");` → Maps `/hello` to `HelloServlet`.

Now, when you visit:  
👉 **`http://localhost:8080/hello`**,  
Tomcat calls your **`HelloServlet`**.

### 🏛️ **Spring Boot MVC - How It Works**
MVC (Model-View-Controller) is a design pattern used to separate concerns:
- **Controller**: Handles user requests and sends data to the **Model** for processing.
- **Model**: Processes data (business logic) and provides it to the **View**.
- **View**: Displays the processed data (usually JSP, Thymeleaf, or other UI).
- (JSP format for java used - )

### ~~📝 **Your Code Breakdown**~~
```java
@Controller
public class HomeServlet {

    @RequestMapping("/student")
    public String home() {
        return "index.jsp";
    }

    @RequestMapping("/brahmesh")
    public String bomb() {
        return "index.jsp";
    }
}
```
~~✔ **`@Controller`** → Marks this class as a Spring MVC controller.~~  
~~✔ **`@RequestMapping("/student")`** → Maps the `/student` URL to `home()` function.~~  
~~✔ **Multiple Methods** → A single controller can handle multiple request mappings, like `/student` and `/brahmesh`, both returning `index.jsp`.~~  

### ~~🔥 **Key Learnings**~~
~~✅ You can define multiple methods inside a `@Controller` class, and each method handles a different request path.~~  
~~✅ Spring Boot automatically looks for the `index.jsp` in the `webapp` folder.~~  
~~✅ The Controller does not process data—it only handles requests and returns the view.~~  


~~✅ **Your understanding is correct!**~~  

~~You're using **`HttpSession`** to store the result and pass it to `data.jsp` using JSTL. Also, you're fetching data using `req.getParameter()` from the URL/form. This is a valid approach!~~  

~~---~~

### ~~🔥 **How This Works**~~
~~1️⃣ The user enters `num1` and `num2` in `index.jsp`.~~  
~~2️⃣ The form submits to `/add`.~~  
~~3️⃣ The controller (`HomeServlet`) extracts `num1` and `num2`, computes the sum, and stores it in the **session**.~~  
~~4️⃣ `data.jsp` retrieves and displays the sum using JSTL.~~  

~~---~~

### ~~✅ **JSP Form (`index.jsp`)**~~
```jsp
<form action="add" method="get">
    <input type="number" name="num1" placeholder="Enter Number 1">
    <input type="number" name="num2" placeholder="Enter Number 2">
    <button type="submit">Add</button>
</form>
```

~~---~~

### ~~✅ **Controller (`HomeServlet.java`)**~~
```java
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpSession;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HomeServlet {

    @RequestMapping("/")
    public String home() {
        return "index.jsp";
    }

    @RequestMapping("/add")
    public String data(HttpServletRequest req, HttpSession session) {
        int a = Integer.parseInt(req.getParameter("num1"));
        int b = Integer.parseInt(req.getParameter("num2"));
        int result = a + b;

        session.setAttribute("result", result); // Storing in session
        System.out.println(result);

        return "data.jsp"; // Redirecting to JSP page
    }
}
```

~~---~~

### ~~✅ **Displaying Result in `data.jsp` using JSTL**~~
```jsp
<p>Result of Addition: ${sessionScope.result}</p>
```

~~---~~

### ~~🔥 **Why This Works**~~
~~✅ `req.getParameter()` gets input from the form.~~  
~~✅ `HttpSession` stores the computed sum.~~  
~~✅ `${sessionScope.result}` retrieves and displays the result in `data.jsp`.~~  

~~This is the **traditional** way (without using `Model`). 🚀~~

~~🔥 **Updated Spring Boot Notes – Super Clean!**~~  

~~✅ You can skip `HttpServletRequest` and use `@RequestParam` to directly get form values:~~  
```java
@RequestParam("num1") int num1
```

~~✅ Instead of `HttpSession`, use `Model` to pass data to JSP:~~  
```java
model.addAttribute("result", result);
```


~~Absolutely bhai! Let’s level it up and use **`ModelAndView`**, which is another clean way to handle both **data and view** together in Spring MVC.~~

~~---~~

### ~~✅ Here's how `ModelAndView` works:~~

~~You return a `ModelAndView` object instead of just a string (like with `Model` or `@RequestParam`):~~

```java
@RequestMapping("/add")
public ModelAndView data(@RequestParam("num1") int num1,
                         @RequestParam("num2") int num2) {

    int result = num1 + num2;

    ModelAndView mv = new ModelAndView();
    mv.setViewName("data.jsp");         // Set which JSP to render
    mv.addObject("result", result);     // Add data to be passed

    return mv;
}
```

~~---~~

### ~~✅ In `data.jsp`, access it just like before:~~

```jsp
<p>Result is: ${result}</p>
```

~~---~~

### ~~📌 TL;DR – When to use what?~~

| Option          | Use Case                                |
| --------------- | --------------------------------------- |
| `Model`         | Lightweight, just data + view name      |
| `ModelAndView`  | You want to explicitly bind data + view |
| `HttpSession`   | You need to keep data across pages      |
| `@RequestParam` | To extract form/query parameters easily |

### ~~✅ So in short:~~

- ~~`mv.addObject("alien", alien)` → `alien` naam se JSP ko ek Java object bhej diya.~~
    
- ~~JSP mein `${alien.property}` se uske values access kar liye.~~

~~**Bhai exactly!** Tumne jo bola na — _"we can completely skip `Model` or `ModelAndView` and just pass `Alien alien`"_ — **bilkul sahi pakde ho** 💯~~

~~Let me explain the magic behind it real quick 👇~~

~~---~~

### ~~✅ Spring's Automatic Data Binding (ModelAttribute Magic)~~

#### ~~✨ You can write:~~

```java
@RequestMapping("/addAlien")
public String data(Alien alien) {
    return "data.jsp";
}
```

~~And in your JSP:~~

```jsp
<p>ID: ${alien.aid}</p>
<p>Name: ${alien.aname}</p>
```

~~---~~

### ~~🔍 How does this work?~~

~~Spring:~~
1. ~~Dekhta hai tumhare form ke **input fields** — `aid`, `aname`~~
2. ~~Fir check karta hai tumhare method ke parameters mein `Alien alien`~~
3. ~~Agar matches mil jaye (i.e., same names), toh woh **automatically values bind** kar deta hai~~
4. ~~Then it puts that object in the **model** with the key `"alien"` (class name in lowercase)~~

~~---~~

### ~~☝️ Conditions for this to work:~~
- ~~Form `name` attributes must match your POJO fields.~~
- ~~Your class must have getters and setters (`Alien.java` ✅)~~
- ~~Spring automatically adds the object to the model (no need to use `addAttribute` or `addObject` manually)~~

~~---~~

### ~~💡 So what you learned is:~~

> ~~“If your method takes a custom object as a parameter and the form input names match the object's field names, Spring will auto-populate the object and auto-pass it to the JSP.”~~

~~This is called **Command Object Binding** — and it’s one of Spring MVC’s coolest features.~~

~~---~~
~~**Arre waah bhai!** You're absolutely on 🔥 now — you've got a solid grip on how `@ModelAttribute` works!~~

~~Let’s wrap this into a mini note-style recap, the **“Spring Boot ModelAttribute Magic”** style 😎~~

~~---~~

### ~~✅ `@ModelAttribute` – Two Ways To Use It~~

#### ~~🔹 1. On Method Parameters~~
```java
@RequestMapping("/addAlien")
public String data(@ModelAttribute("alien") Alien alien) {
    return "data.jsp";
}
```

- ~~Binds form data to the `Alien` object.~~
- ~~Adds it to the model with name `"alien"` (you can change the name via `"alien"`).~~
- ~~JSP accesses it like: `${alien.aid}`~~

~~---~~

#### ~~🔹 2. As a Method (Runs before every handler)~~

```java
@ModelAttribute("course")
public String courseName() {
    return "Spring Boot Beginner";
}
```

- ~~This method runs **before every controller method**.~~
~~- Adds `"course"` to the model automatically.~~
~~- Use it in any JSP: `${course}`~~

~~---~~

~~### 🤯 Why this is awesome?~~

~~- You can preload data like a course list, username, or config.~~
~~- Keeps your controller methods cleaner.~~
~~- Acts kinda like a mini filter for injecting common model data.~~

~~---~~

~~### 💡 Final wisdom drop:~~
~~> Use `@ModelAttribute` when you want to either bind a form object OR inject some shared model data for multiple views.~~

~~---~~

~~If you want, next we can go through how to show a list of objects in JSP using `jstl:forEach` and `@ModelAttribute` to inject them 🔥 Ready when you are!~~


~~Sahi socha bhai, **tune concept pakad liya hai**, bas thoda aur clearly samjha deta hoon so you can use this as a solid note:~~

~~---~~

~~### 💡 Cheat Sheet Style Samajh:~~

~~| Action Type | What You Pass in Controller | Use Case |~~
~~|-------------|------------------------------|----------|~~
~~| **POST (Form Submit)** | `public String methodName(ClassName obj)` | Spring automatically bind karega form data se object |~~
~~| **GET (Display / View Page)** | `public String methodName(Model model)` ya `ModelAndView` | Tu data ko view ke liye JSP tak bhejega |~~

~~---~~

~~### ✅ Example: POST (Form se Data lena)~~

~~```java~~
~~@RequestMapping("FormHandler")~~
~~public String handleForm(Job job) {~~
    ~~all.add(job);  // form se aayi values Job object me bind ho gayi~~
    ~~return "success";~~
~~}~~
~~```~~

~~Spring ne form ke input names ko `Job` ke fields se match karke object bana diya.~~

~~---~~

~~### ✅ Example: GET (View ke liye data bhejna)~~

~~```java~~
~~@RequestMapping("ViewJob")~~
~~public String viewJobs(Model model) {~~
    ~~model.addAttribute("all", all);  // data bhej diya JSP ke liye~~
    ~~return "ViewJob";~~
~~}~~
~~```~~

~~JSP mein tu use karega:~~

~~```jsp~~
~~<c:forEach var="job" items="${all}">~~
    ~~<p>${job.title}</p>~~
~~</c:forEach>~~
~~```~~

~~---~~

~~### 🔥 BONUS: ModelAndView Style (Alternative)~~

~~```java~~
~~@RequestMapping("ViewJob")~~
~~public ModelAndView viewJobs() {~~
    ~~ModelAndView mv = new ModelAndView("ViewJob");~~
    ~~mv.addObject("all", all);~~
    ~~return mv;~~
~~}~~
~~```~~

~~---~~

~~### 📌 Toh Summary:~~

~~> 🔄 **POST ke time**: Spring automatically form values se object bana deta hai. Tu class directly le sakta hai method mein.~~  
~~> 👀 **GET ke time**: Tu Model/ModelAndView se data JSP ko bhejega for displaying.~~

~~Tu bilkul sahi jaa raha hai bhai — seedha practical se seekh raha hai, best way 🔥~~  
~~Aage chalna ho toh bol, full support yahan hai 😎~~

~~Exactly bhai! You're right on track. Let me explain **stateless** in a clean and simple way that you can save as notes 👇~~

---

### 🧠 What is "Stateless" in REST?

In a **stateless** system (like REST), the **server does NOT store any information about the client’s previous requests**.

---

### 🔁 What does this mean?

Each request from the client to the server must contain **all the information** needed to process the request — whether it’s the first request or the hundredth.

---

### ✅ Example Flow:

#### 🔒 First Request:
User logs in and sends:
```json
{
  "username": "brahmesh",
  "password": "1234"
}
```
Server validates → responds with a **token** (usually JWT).

---

#### 🔐 Second Request:
Now, the client sends a new request with only the token:
```http
GET /getJobs
Authorization: Bearer <token>
```

Server reads the token → knows it’s Brahmesh → gives result.

> Server doesn’t remember anything about the user between requests. Everything is **passed with each request** (like the token).

---

### ❌ Opposite of Stateless → **Stateful**

In a stateful system (like old-style sessions), the server **remembers** you — like maintaining a session or login state. But it's **less scalable** and not ideal for REST APIs.

---

### 📌 Why Stateless is Good for REST?
- Easy to scale (each server can handle any request).
- More secure and predictable.
- Perfect for microservices and distributed systems.

---


So via rest i can to do transcatin b/w frontend and backedn 

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



JPA

repo.save(s1); to save it 

Bhai, sahi samjha tune! 🔥 You're picking up **Spring Data JPA magic** now. Let’s make a clean note for this:

---

### ✅ Custom Query in Spring Data JPA — Two Ways

#### 🔹 **1. Method Naming Convention (Magic Query)**
You don’t need to write queries manually. Spring generates them based on method names!

```java
List<Student> findByName(String name);                    // WHERE name = ?
List<Student> findByMarksGreaterThan(int marks);         // WHERE marks > ?
List<Student> findByNameAndMarks(String name, int marks); // WHERE name = ? AND marks = ?
```

> 🧠 Rule: Method name = `findBy` + field name(s) + operation

Spring sees this and auto-creates SQL behind the scenes.

---

#### 🔹 **2. Custom Query Using `@Query` Annotation**
If you want more control (joins, custom logic etc.), you can write:

```java
@Query("SELECT s FROM Student s WHERE s.name = :name")
List<Student> getStudentsByName(@Param("name") String name);
```

📝 **Notes:**
- Use **Java field names**, not column names in the query.
- `s.name` → refers to `private String name;` in your entity.

---

### ⚡ Bonus: You can also use native SQL
```java
@Query(value = "SELECT * FROM student WHERE marks > ?1", nativeQuery = true)
List<Student> getHighScoringStudents(int marks);
```

---

### 🧠 I learned this:
> In Spring Data JPA, I can create custom queries using either method naming conventions like `findByName()` or using `@Query` for more complex logic. Spring uses my entity’s field names, not table columns.


Bilkul sahi bhai! 💯

Here’s a crisp breakdown for **update** and **delete** in Spring Data JPA:

---

### ✅ **Delete in Spring Data JPA**

#### 🔹 Delete by object:
```java
repo.delete(student);  // Pass the entity object
```

#### 🔹 Delete by ID:
```java
repo.deleteById(1);    // Deletes student with roll = 1
```

#### 🔹 Delete all:
```java
repo.deleteAll();
```

---

### ✅ **Update in Spring Data JPA**

There’s **no separate method** for update — you use `save()` again:

```java
Student s = repo.findById(1).get();
s.setMarks(95);
repo.save(s);  // Acts as update if ID exists
```

> ✨ If the ID already exists → it updates  
> If the ID is new → it inserts

---

### 🧠 I learned this:
> In Spring Data JPA, `repo.delete()` or `deleteById()` removes data. For updating, `save()` works again — if the primary key exists, it updates; else, it inserts.

Bhai yeh raha tera **short summary of key points for JPA** — quick and clean:

---

### ✅ **Spring Data JPA – Key Points to Remember**

1. **Entity class banani hoti hai**  
   - Annotate with `@Entity`
   - Should have a default (no-arg) constructor  
   - At least one field marked as `@Id`

2. **Repository banani hoti hai**  
   - Interface extends `JpaRepository<Entity, IdType>`
   - Spring automatically implements basic CRUD

3. **Application.properties config:**
   properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/dbname
   spring.datasource.username=postgres
   spring.datasource.password=yourpass
   spring.jpa.hibernate.ddl-auto=update
   spring.jpa.show-sql=true
   
4. **Beans injection:**
   - Use `@Autowired` to inject `Repo` or `Service`
   - Use `@Component` and `@Scope("prototype")` if manually creating beans

5. **Saving data:**
   - Use `repo.save(entityObj)`  
   - Works for both insert and update

6. **Fetching data:**
   - `repo.findAll()`
   - `repo.findById(id)` returns `Optional<Entity>`

7. **Custom Queries:**
   - Spring magic: `findByName`, `findByMarksGreaterThan`
   - Or use `@Query("SELECT s FROM Student s WHERE s.name = :name")`

8. **Deleting data:**
   - `repo.delete(entityObj)` or `repo.deleteById(id)`

### 📝 **Learning Note**
You explored **Spring Data** and realized how powerful and convenient it is. By just:
- Removing the Service and Controller layers,
- Adding a **Repository interface**,
- Including **Spring Data JPA** in your `pom.xml`,

You were able to:
- Access all auto-generated REST endpoints,
- Perform basic CRUD operations effortlessly by simply running the app on port **8080**,
- Use Spring Data’s magic to skip boilerplate code and get things done faster.

Spring Data REST exposing repositories automatically like that is incredibly useful for prototyping and admin tools!

Bilkul bhai, AOP ke 7 basic concepts ko apni hi bhasha mein samjhte hain 

---

### 1. **Aspect**  
**Soch le ye ek kaam karne wala banda hai.**  
Jaise logging karne wala, ya security check karne wala. Ye banda kaam ko har jagah repeat hone se rokta hai — ek hi jagah likh, har jagah chalega.

📌 **Example**: Tujhe har method ke pehle log likhna hai, toh tu ek `LoggerAspect` banayega.

---

### 2. **Advice**  
**Ye hota hai ki woh banda (Aspect) kaam kab kare.**  
Jaise:
- `@Before` → method se pehle
- `@After` → method ke baad
- `@Around` → method ke pehle-baad dono

📌 **Example**: `@Before` advice bolega – bhai method chalu hone se pehle log daal de.

---

### 3. **Pointcut**  
**Ye decide karta hai ki kaha pe kaam hona chahiye.**  
Method ka naam ho, package ho, ya annotation — tu yahan define karega ki kaunse method pe jaake kaam kare.

📌 **Example**: `execution(* com.brahmesh.service.*.*(..))` → sab service ke methods pe chalega.

---

### 4. **Join Point**  
**Ye wahi exact place hai jaha kaam kiya jaa sakta hai.**  
Mostly method call hota hai. Basically method chalne se pehle, baad mein, ya uske around.

📌 **Example**: `getAllJobs()` method call hona ek join point hai.

---

### 5. **Advice Types** (isko 3 pe divide kar le):

- **@Before** – method chalne se pehle
- **@AfterReturning** – method chalne ke baad agar success hua
- **@AfterThrowing** – agar method exception phenke
- **@After** – method khatam hone ke baad chahe success ya fail
- **@Around** – pura method ko wrap kar leta hai (jaise try-catch-finally)

---

### 6. **Target Object**  
**Ye woh asli service ya class hai jisme tera business logic hai.**  
Aspect iss object ke methods ko intercept karta hai.

📌 **Example**: Tera `JobService` jisme tu `addJob()` likhta hai, woh target object hai.

---

### 7. **Weaving**  
**Ye hota hai pura AOP ka jadoo lagana.**  
Matlab tere aspect ko tere service method ke sath jodna — runtime pe ya compile time pe.

📌 **Spring me weaving mostly runtime pe hoti hai** using proxies.

---

🧠 **Ek line me yaad rakh**:  
Tere kaam (logging, security) ko har method me repeat mat kar — ek **Aspect** me likh, woh **Advice** ke through, **Pointcut** se target methods (Join Points) pe lagega. Aur yeh sab jodta hai Spring ka weaving system.

### 📦 In Short:

| Annotation        | When it Runs                | Use For                            |
| ----------------- | --------------------------- | ---------------------------------- |
| `@Before`         | Before method executes      | Validation, pre-logging            |
| `@After`          | After method (success/fail) | Cleanup, logs                      |
| `@AfterReturning` | After method returns OK     | Logging result                     |
| `@AfterThrowing`  | On exception thrown         | Error handling                     |
| `@Around`         | Wraps entire method         | Modify behavior, measure time etc. |
```java
@Around("execution (* com.telusko.springbootrest.service.JobService.getJob(..)) && args(postId)")
	public Object validateAndUpdate(ProceedingJoinPoint jp,int postId) throws Throwable {
	if (postId<0) {
		LOGGER.info("PostId is negative, updating it");
		
		postId=-postId;
		LOGGER.info("new Value "+postId);
	}
	
	Object obj=jp.proceed(new Object[] {postId});
		
		
		
		return obj;
	}
```
```java

    //return type, class name.method name(args)

    @Before("execution(* com.brahmesh.JobProject.service.JobService.*(..))")
    public void logBeforeMethod(JoinPoint jp)
    {
        log.info("Method Called" + jp.getSignature().getName());
    }

    @After("execution(* com.brahmesh.JobProject.service.JobService.*(..))")
    public void logAfterMethod(JoinPoint jp)
    {
        log.info("Method Exceuted" + jp.getSignature().getName());
    }


```

**Spring security**

When you run your Spring Boot app, **before** the request reaches the **DispatcherServlet** and then your controller, it first passes through **Spring Security's filter chain**, which consists of **multiple filters** that handle authentication, authorization, etc and everytime it will create it will create session ID ( login and logout fucntionality)

manaully given the credentials

spring.application.name=spring-sec-demo  
spring.security.user.name = brahmesh  
spring.security.user.password= 1234

to test it via postman

in authorizarion basic auth give your user and password



You learned how **CSRF (Cross-Site Request Forgery)** works.  
When you’re logged in (e.g., Gmail), a **session ID** is stored in the browser, allowing requests to stay authenticated.  
But if a malicious site tricks you into sending a **GET request**, it might work using your session.  
**POST, PUT, DELETE** requests are protected by **CSRF tokens**, which must be included in the request headers.  
You can retrieve the token using:

```java
@GetMapping("csrf-token")
public CsrfToken getToken(HttpServletRequest req) {
    return (CsrfToken) req.getAttribute("_csrf");
}
```
> Then send it in Postman under the `X-CSRF-TOKEN` header for secure requests.
```java
@Bean  
public SecurityFilterChain sf(HttpSecurity hp) throws Exception {  
   return hp.build();  
}
```
That means:
Spring Security will apply default security rules.
These include things like:
Requiring authentication for all endpoints
Auto-generating login page
Enabling CSRF
Using form-based login
Creating a default user (user) and showing a password in terminal
So yes — in this case, HttpSecurity is handling all the security measures by itself, because you didn’t customize anything. It's just a pass-through saying "use defaults".


---

### 🔐 Full Line-by-Line Explanation

```java
http.csrf(customizer -> customizer.disable())
```

🛡 **Disable CSRF** — Useful for APIs or tools like Postman where you don't want to deal with CSRF tokens.

> Bhai GET/POST/PUT sab karle bina token ke.

---

```java
.authorizeHttpRequests(request -> request.anyRequest().authenticated())
```

🔒 **All URLs need login** — No endpoint is public unless you add exceptions.

> Bhai, har request ke liye login zaroori hai.

---

```java
.httpBasic(Customizer.withDefaults())
```

👤 **Enable basic authentication** — Like browser or Postman will ask for username/password in a popup or header.

> Bhai, username-password de ke hi ghus paayega.

---

```java
.sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS));
```

🪪 **No session will be created** — Great for REST APIs where each request should be independent, stateless.

> Bhai, koi session mat bana, har request fresh treat karo.

---

### 🔁 Final Result?

You've now set up:

* ✅ Stateless REST API
* ✅ HTTP Basic Auth (username/password)
* ❌ No CSRF
* 🔒 All routes protected

---


## **Java Method Chaining & Spring Security Configuration — Quick Reference**

### **1. Method Chaining in Java**
- **Syntax:**  
  `object.method1().method2().method3();`
- **How it works:**  
  - Each method returns an object (often `this`), allowing the next method to be called on it.
  - Methods can be from the same class or different classes, depending on return types.
- **Order of execution:**  
  - Methods are called **left to right**.
  - Each method is called on the result of the previous one.
- **Error handling:**  
  - If any method throws an exception, the chain stops and the exception is thrown.
  - If a method returns `null`, the next call will throw a `NullPointerException`.
- **Final result:**  
  - The return value of the last method in the chain.

---

### **2. Spring Security Filter Chain Example**

```java
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http.csrf(customizer -> customizer.disable())
        .authorizeHttpRequests(request -> request.anyRequest().authenticated())
        .httpBasic(Customizer.withDefaults())
        .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS));
    return http.build();
}
```

- **What each part does:**
  - `.csrf(...disable())` — Disables CSRF protection.
  - `.authorizeHttpRequests(...anyRequest().authenticated())` — All requests require authentication.
  - `.httpBasic(...)` — Enables HTTP Basic authentication.
  - `.sessionManagement(...STATELESS)` — No HTTP session is created (stateless API).
  - `return http.build();` — Builds and returns the configured security filter chain.

---

### **3. Key Points**
- Each method in the chain must return an object for the next method.
- If any method fails (throws), the rest are not executed.
- Common in builder patterns and configuration code (like Spring Security).

---

**Keep this as a handy reference for method chaining and Spring Security configuration!**

### 🧠 Bhai Summary:

> Har request ke liye login zaroori hai, session nahi banega, aur CSRF ko disable kar diya — bilkul API-style setup.

Let me know if you want to start using JWT tokens next or switch some routes to be public.

| Value    | Meaning                                                                            |
| -------- | ---------------------------------------------------------------------------------- |
| `Strict` | Only send cookies **if the site is same-origin**. Cross-site = ❌ block.            |
| `Lax`    | Allow cookies on **GET** requests (like clicking a link), but not for `POST`, etc. |
| `None`   | Allow **cross-site cookies**, but cookie **must be secure (HTTPS)**                |
It’s a setting that tells the **browser** when to **send cookies** along with a request — especially when the request is coming **from another site (cross-site request)**.

---

## 🛡️ Spring Security Notes – Part 1

### ✅ **Authentication with In-Memory Users**

* Used `UserDetailsService` to define custom users.
* Created users using `User.withDefaultPasswordEncoder()` (for testing only).
* Stored them in `InMemoryUserDetailsManager`.

```java
@Bean
public UserDetailsService userDetailsService() {
    UserDetails user = User.withDefaultPasswordEncoder()
        .username("brahmesh")
        .password("lol")
        .roles("USER")
        .build();

    UserDetails admin = User.withDefaultPasswordEncoder()
        .username("appu")
        .password("lolu")
        .roles("admin")
        .build();

    return new InMemoryUserDetailsManager(user, admin);
}
```

### 🔐 **Spring Security Basics**

* Default login form appears when Spring Security is on the classpath.
* Auto-generates a password if not defined.
* All routes require authentication unless configured otherwise.

### 🛡️ **SecurityFilterChain Configuration**

```java
http.csrf(csrf -> csrf.disable())
    .authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
    .httpBasic(Customizer.withDefaults());
```

* **CSRF disabled**: useful for APIs.
* **httpBasic()**: enables basic auth via browser or Postman.

### 🔁 **Session vs Stateless**

* Session-based auth: server stores login state.
* Stateless (like JWT): server doesn’t store anything; token is passed with each request.

### 💡 Other Concepts Touched

* `server.servlet.session.cookie.same-site=strict` prevents cookies from being sent across sites (mitigates CSRF).

You're on the right path—this is where Spring Security starts getting real. Let's simplify everything you're doing, *bhai language style*, to make it easy.

---

### 🧠 **The Goal**

You want Spring Security to **authenticate users from your own database**, not from hardcoded in-memory users.

---

### ✅ **So what’s needed?**

1. **Spring needs to know:**

   * How to get user details (username, password, roles) from the database.
   * How to validate those details.

2. **To do this, you implement:**

   * `UserDetailsService` → tells Spring how to find a user by username.
   * `UserDetails` → wraps your User object in a format Spring understands.

---

### 🔄 **Your Flow**

Let’s break your code step-by-step:

---

#### 1. **Custom `UserDetailsService`**

```java
@Override
public UserDetails loadUserByUsername(String username) {
    User user = repo.findByUsername(username);
    if(user == null) throw new UsernameNotFoundException("User 404");
    return new UserPrincipal(user); // 👈 Spring expects a UserDetails object
}
```

* This finds your user from DB.
* You return `UserPrincipal` because it's your class that implements `UserDetails`.

---

#### 2. **Custom `UserDetails` (your `UserPrincipal` class)**

```java
public class UserPrincipal implements UserDetails {

    private User user;

    public UserPrincipal(User user) {
        this.user = user;
    }

    @Override
    public String getUsername() {
        return user.getUsername(); // 👈 return actual username from DB
    }

    @Override
    public String getPassword() {
        return user.getPassword(); // 👈 return password from DB
    }

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return Collections.singleton(new SimpleGrantedAuthority(user.getRole()));
    }

    // Other methods like isAccountNonExpired(), etc. can return true
}
```

---

### 🟡 So Why `UserPrincipal`?

Spring needs an object that implements `UserDetails`. Your `User` entity likely doesn't. So you wrap your DB `User` inside a `UserPrincipal` which acts as a **bridge** between your entity and Spring Security.

---

### 🧩 Final Piece – `AuthenticationProvider`

```java
@Bean
public AuthenticationProvider authenticationProvider() {
    DaoAuthenticationProvider provider = new DaoAuthenticationProvider();
    provider.setUserDetailsService(userDetailsService); // your service
    provider.setPasswordEncoder(NoOpPasswordEncoder.getInstance()); // plain text password
    return provider;
}
```

This is the engine that actually *checks* your username and password using:

* your `UserDetailsService` (to get user)
* your encoder (to verify password)

---

### 🔥 TL;DR

| Component                | What it Does                      |
| ------------------------ | --------------------------------- |
| `UserDetailsService`     | Finds user from DB                |
| `UserDetails`            | Wraps user for Spring Security    |
| `AuthenticationProvider` | Authenticates using the two above |

---
Exactly bhai, **you’ve got the core idea of JWT spot on!** Let me break it down in your style 👇

---

### ✅ JWT Flow (Bhai Style):

1. **User Logs In (POST /login):**

   * You send **username + password** to the server.
   * Server **verifies credentials** and if valid…
   * **Server creates a JWT** (signed using a secret key).
   * Server gives this **JWT to the client** (usually in the response body or headers).

2. **Client Stores JWT:**

   * **Yes**, JWT is **stored on the client-side** — usually in:

     * Local Storage (more common)
     * OR Cookies (if HttpOnly secure flow needed)

3. **Further Requests (GET /profile, POST /order, etc.):**

   * For **every API call after login**, client sends the JWT **in the header**:

     ```
     Authorization: Bearer <JWT_TOKEN>
     ```
   * Server **reads the token**, verifies the signature, checks expiry, and then allows the request if valid.

4. **Does Server Send New JWT after Every API Call?**

   * ❌ **NO** — once token is issued, server doesn’t send a new one every time.
   * ✅ Server just **verifies the same token** on every request until it **expires**.

---

### ⚠️ Token Expiry & Refresh

* **JWTs usually have expiry** (e.g., 15 min or 1 hour).
* After expiry:

  * Either user logs in again, or
  * You implement **refresh tokens** (another topic) to issue new access tokens without re-login.

---

### ✍️ Summary Note:

**I learned**:

* JWT is created once after login and sent to the client.
* Client stores the JWT and sends it with every API request.
* Server verifies the token, but doesn't resend a new one every time.
* Token lives on client-side; server doesn't store it (JWT is stateless).

Great! Here's a quick summary of what you've **learned so far** about JWT:

---

### ✅ **What You’ve Implemented:**

#### 🔐 Authentication Setup

* **AuthenticationManager** bean:

  ```java
  @Bean
  public AuthenticationManager authenticationManager(AuthenticationConfiguration config) throws Exception {
      return config.getAuthenticationManager();
  }
  ```

#### 🧑‍💼 Login Endpoint

* Controller that authenticates a user and returns a JWT:

  ```java
  @PostMapping("login")
  public String login(@RequestBody User user) {
      Authentication authentication = authenticationManager.authenticate(
          new UsernamePasswordAuthenticationToken(user.getUsername(), user.getPassword())
      );

      if (authentication.isAuthenticated())
          return jwtService.generateToken(user.getUsername());
      else
          return "Login Failed";
  }
  ```

#### 🔑 JWT Service

* Dynamically generating a secret key (HMAC SHA-256).
* Creating a JWT with:

  * Empty custom claims
  * `sub` (subject) as the username
  * `iat` (issued at) and `exp` (30 minutes expiry)
  * Signing with HS256.

  ```java
  public String generateToken(String username) {
      Map<String, Object> claims = new HashMap<>();

      return Jwts.builder()
          .setClaims(claims)
          .setSubject(username)
          .setIssuedAt(new Date(System.currentTimeMillis()))
          .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 30))
          .signWith(getKey(), SignatureAlgorithm.HS256)
          .compact();
  }
  ```

---

### 📘 Notes / Suggestions:

1. **Dynamic Key Issue**:

   * You’re generating a new secret key each time your application starts.
   * 🔥 This is **not ideal for production** — once restarted, all existing tokens will become invalid.
   * ✅ Store the secret key in `application.properties` or an environment variable instead.

   Example:

   properties
   jwt.secret=your-256-bit-secret-key-base64
   ```

   Then inject it like:
java
   @Value("${jwt.secret}")
   private String secretKey;
 

2. **Token Structure**:

   * Claims and expiry are correct.
   * You can later add roles or user ID in `claims` if needed.

Excellent! You’ve now **completed the core JWT authentication flow** in a Spring Boot app. Here’s your **progress summary** and a few optional next steps.

---

### ✅ What You’ve Learned and Implemented:

#### 🔒 `SecurityConfig.java`:

* You disabled CSRF (since it’s a stateless API).
* Allowed anonymous access to `/registration` and `/login`.
* Marked other endpoints as protected.
* Plugged in your `JwtFilter` **before** `UsernamePasswordAuthenticationFilter`.
* Configured stateless session:

```java
http.csrf(csrf -> csrf.disable())
    .authorizeHttpRequests(req -> req
        .requestMatchers("registration", "login").permitAll()
        .anyRequest().authenticated())
    .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
    .addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class);
```

---

#### 📤 `JwtFilter.java` (extends `OncePerRequestFilter`):

* Extracts token from `Authorization` header.
* Retrieves username.
* Validates token & loads `UserDetails`.
* Sets `SecurityContextHolder` if everything checks out.

✅ Perfect placement in filter chain and solid implementation.

---

#### 🔐 `JwtService.java` (extended):

* Token parsing and validation using `io.jsonwebtoken`.
* Functions:

  * `extractUserName()` — from `sub` (subject) claim.
  * `validateToken()` — ensures user matches and token not expired.
  * `extractAllClaims()` — parses JWT using signing key.
  * Helpers like `extractExpiration()`.

---

### 💡 Small Suggestions for Production Readiness

1. **Exception Handling in Filter**:
   Add a block to handle bad tokens:

   ```java
   try {
       // token extraction + validation logic
   } catch (JwtException | IllegalArgumentException e) {
       response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
       response.getWriter().write("Invalid or expired token");
       return;
   }
   ```

2. **Secret Key**:
   As mentioned earlier, use a **static secret from config**, not regenerated on each run:

   ```java
   @Value("${jwt.secret}")
   private String secretKey;
   ```

3. **Token Expiry Configurable**:
   Extract token validity into a config property:

   ```java
   @Value("${jwt.expiration}")
   private long jwtExpirationInMs;
   ```

4. **Logging for Debugging** (optional):
   Helpful for understanding flow during development:

   ```java
   logger.info("Token for user {} is valid", userName);
   ```

---

### ✅ You have completed:

* ✅ JWT token generation
* ✅ Securing endpoints
* ✅ Stateless authentication via JWT filter
* ✅ Validation of tokens in a custom filter

---

WHOLE SUMMARY 

Absolutely! Here's a **simple and visualized flow** to help you **remember the full JWT authentication flow** — from login to securing requests — using **steps, keywords, and visuals** that stick.

---

## 🔐 **JWT Authentication Flow in Spring Boot**

### 🌟 1. **Login → Generate Token**

```pgsql
[POST /login]
     |
     |---> Controller receives username & password
            ↓
     |---> Authenticates using AuthenticationManager
            ↓
     |---> If authenticated:
            → Call JwtService to generate token
            → Return token in response
```

### ✅ In Code:

```java
Authentication authentication = authenticationManager.authenticate(
   new UsernamePasswordAuthenticationToken(user.getUsername(), user.getPassword())
);

if (authentication.isAuthenticated())
   return jwtService.generateToken(user.getUsername());
```

---

### 🔑 2. **JwtService – Generate Token**

**Inputs**: username
**Outputs**: JWT (signed token string)

```
JwtService
   |
   |---> Set Claims (optional)
   |---> Set Subject = username
   |---> Set Expiry time
   |---> Sign with secret key (HS256)
   ↓
[ return token ]
```

---

### 🛡️ 3. **Client Makes Authenticated Request**

```
[Client] → Sends API request with:
Authorization: Bearer <JWT>
```

---

### 🔍 4. **JWT Filter (OncePerRequestFilter)**

```
JwtFilter
   |
   |---> Extract Authorization header
   |     ↓
   |  If header starts with "Bearer ":
   |     → Extract token
   |     → Get username from token
   |     → Load UserDetails from DB
   |     → Validate token (match user, check expiry)
   |
   |---> If valid → set authentication in SecurityContext
```

✅ Now the user is authenticated **for the rest of the request lifecycle**.

---

### 🔒 5. **Spring Security Config**

```
SecurityConfig
   |
   |---> Allow: /login, /register (permitAll)
   |---> Block: all other paths (authenticated)
   |---> Stateless session (no HTTP session)
   |---> Add JwtFilter before UsernamePasswordAuthenticationFilter
```

---

### 🔁 6. **Flow Recap – In Simple Steps**

#### 🚪 Login:

* Send credentials → Get token

#### 🔑 Get Token:

* JWT built with username + expiry + signature

#### 🚗 Make Requests:

* Attach token in header

#### 🔍 Intercept Request:

* Filter extracts, validates, authenticates

#### 🔒 Protect Endpoints:

* Only valid tokens can access

---

### 🧠 Mnemonic to Remember: **L-J-R-V-P**

| Step  | Meaning                                             |
| ----- | --------------------------------------------------- |
| **L** | **Login** → Authenticate & get token                |
| **J** | **JWT** → Generate with claims, expiry              |
| **R** | **Request** → Send with `Authorization` header      |
| **V** | **Validate** → In filter using secret & userDetails |
| **P** | **Protect** → Endpoints with Security Config        |

---

### 📌 Optional: Diagram

```
User → [Login Controller] → [AuthenticationManager] → [JwtService]
     ←──────────── Token ────────────

User → [Protected Endpoint]
     → JWT in Header →
        [JwtFilter]
           |
           → validateToken()
           → loadUserDetails()
           → set SecurityContext

Spring Security → grants access if authenticated
```
🟢 **Awesome! You've now implemented Google OAuth2 login using Spring Security** — and yes, you're absolutely right: it **really is that simple** compared to manual JWT auth!

---

## ✅ **What You’ve Just Learned**

### 🌍 Google OAuth2 with Spring Security

You did all of this with **just two key components**:

---

### 1. **Security Configuration**

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http.authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
        .oauth2Login(Customizer.withDefaults()); // Enables Google Login
    return http.build();
}
```

> 🔐 This tells Spring:
>
> * Require login for all routes
> * Use default OAuth2 login flow (with Google)

---

### 2. **Application Properties**

```properties
spring.security.oauth2.client.registration.google.client-id=YOUR_CLIENT_ID
spring.security.oauth2.client.registration.google.client-secret=YOUR_CLIENT_SECRET
```

> ✅ This connects your app to Google’s OAuth2 provider.

---

## 🧠 **Why It's Simpler than JWT + Manual Auth**

| Feature                | Manual JWT Auth       | Google OAuth2               |
| ---------------------- | --------------------- | --------------------------- |
| Login Form             | You implement it      | Handled by Google           |
| Token Generation       | You write logic       | Done by Google              |
| User Authentication    | You manage user store | Google does it              |
| Token Validation       | You write a filter    | Handled via Spring Security |
| Simplicity             | Medium complexity     | **Super simple**            |
| Security + Maintenance | Your responsibility   | Google handles most         |

---

## 🔁 Quick Recap: OAuth2 Flow

```
[User Clicks "Login with Google"]
        ↓
[Redirect to Google Login Page]
        ↓
[User Logs In at Google]
        ↓
[Google redirects back with auth code]
        ↓
[Spring Security exchanges code for token]
        ↓
[User authenticated → security context populated]
```
