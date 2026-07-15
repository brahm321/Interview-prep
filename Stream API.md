

```java
// 1. Convert to IntStream
input.chars() 
    // 2. Cast ASCII ints to Character objects
    .mapToObj(c -> (char) c) 
    // 3. Group and Count (Preserving Order)
    .collect(Collectors.groupingBy(Function.identity(), LinkedHashMap::new, Collectors.counting())) 
    // 4. Stream the Key-Value pairs
    .entrySet().stream() 
    // 5. Filter based on the count condition
    .filter(entry -> entry.getValue() == 1L) 
    // 6. Extract just the character
    .map(Map.Entry::getKey) 
    // 7. Get the final result
    .findFirst();
```

You have the master blueprint locked in. Now, let's put it through its paces.

When you sit down for those technical rounds, the interviewer will usually take that exact blueprint and ask you to tweak just **one or two lines** at the end of the chain to get a different result.

Here are the most common String-based Stream API questions and exactly how to adjust your blueprint for each one.

### 1. Find all duplicate characters in a String

Instead of looking for a count of 1, you want anything with a count greater than 1. And instead of `findFirst()`, you gather them all into a List.

Java

```java
String input = "programming";

List<Character> duplicates = input.chars()
    .mapToObj(c -> (char) c)
    // Order doesn't strictly matter here, so HashMap is fine
    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting())) 
    .entrySet().stream()
    // TWEAK: Filter for values greater than 1
    .filter(entry -> entry.getValue() > 1L) 
    .map(Map.Entry::getKey)
    // TWEAK: Collect to a List instead of findFirst
    .collect(Collectors.toList()); 

System.out.println(duplicates); // Output: [p, r, g, m]
```

### 2. Find the First Repeated Character

This is almost identical to the non-repeating question, but you swap the filter condition. Because it asks for the _first_, you **must** use `LinkedHashMap` to preserve the order.

Java

```java
String input = "java backend";

Character firstRepeated = input.chars()
    .mapToObj(c -> (char) c)
    // CRITICAL: LinkedHashMap to keep insertion order
    .collect(Collectors.groupingBy(Function.identity(), LinkedHashMap::new, Collectors.counting()))
    .entrySet().stream()
    // TWEAK: Look for the first one that appears more than once
    .filter(entry -> entry.getValue() > 1L)
    .map(Map.Entry::getKey)
    .findFirst()
    .orElseThrow(() -> new RuntimeException("No repeating characters"));

System.out.println(firstRepeated); // Output: a
```

### 3. Find the Highest Frequency Character

This one skips the `.filter()` entirely. Instead of filtering, you use `.max()` on the `entrySet` stream to find the entry with the highest value.

Java

```java
String input = "microservices";

Character mostFrequent = input.chars()
    .mapToObj(c -> (char) c)
    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
    .entrySet().stream()
    // TWEAK: Find the maximum by comparing the values (the counts)
    .max(Map.Entry.comparingByValue())
    .map(Map.Entry::getKey)
    .orElseThrow(() -> new RuntimeException("String is empty"));

System.out.println(mostFrequent); // Output: c (or e, r, s, i depending on the map traversal if tied)


String s4 = "lolo";  
 Boolean ans = IntStream.range(0,s4.length()/2)  
         .allMatch(i->s4.charAt(i) == s4.charAt(s4.length() -1 -i));  
 System.out.println(ans);
```

### 4. Remove all duplicate characters (Preserving Order)

This one actually lets you bypass the `groupingBy` entirely. It tests if you know about the `.distinct()` intermediate operation and the `Collectors.joining()` method.

Java

```java
String input = "Programmmminggggggggg";

String result = Arrays.stream(input.split("")) // Creates a Stream<String> immediately
        .distinct()
        .collect(Collectors.joining());

System.out.println(result); // Output: Progamin
```

> **The Interviewer Trick:** Sometimes they will give you a string with mixed casing (e.g., `"Java"`). If they ask you to treat 'J' and 'j' as the same character, just add `.toLowerCase()` right before `.chars()`like this: `input.toLowerCase().chars()...`


```java
String sentence = "Java is great and java is fast";

// 1. Split into words
Arrays.stream(sentence.split("\\s+")) 
    // 2. Normalize to lowercase so "Java" == "java"
    .map(String::toLowerCase) 
    // 3. Group and count
    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting())) 
    // 4. Stream the Key-Value pairs
    .entrySet().stream() 
    // 5. Filter for duplicates (count > 1)
    .filter(entry -> entry.getValue() > 1L) 
    // 6. Extract just the word
    .map(Map.Entry::getKey) 
    // 7. Collect the results
    .collect(Collectors.toList());
```


```java
String sentence = "Java Developer Interview";

String reversedSentence = Arrays.stream(sentence.split("\\s+"))
    // TWEAK: Map each word to its reversed version using StringBuilder
    .map(word -> new StringBuilder(word).reverse().toString())
    // TWEAK: Join them back together with a space in between
    .collect(Collectors.joining(" "));

System.out.println(reversedSentence); 
// Output: avaJ repoleveD weivretnI
```


```java

String sentence = "Java is great, but Java is also hard.";

Map<String, Long> wordCount = Arrays.stream(
        // TWEAK: Strip punctuation before splitting
        sentence.replaceAll("[^a-zA-Z ]", "").split("\\s+") 
    )
    .map(String::toLowerCase)
    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

System.out.println(wordCount); 
// Output: {also=1, but=1, java=2, hard=1, is=2, great=1}
```

### 2. Find the First Repeated Word

This tests your memory of the `LinkedHashMap` rule. Order matters here.

Java

```java
String sentence = "Spring boot is lightweight and spring boot is fast";

String firstRepeated = Arrays.stream(sentence.split("\\s+"))
    .map(String::toLowerCase)
    // TWEAK: LinkedHashMap to preserve the order of words
    .collect(Collectors.groupingBy(Function.identity(), LinkedHashMap::new, Collectors.counting()))
    .entrySet().stream()
    // TWEAK: Find the first one with a count > 1
    .filter(entry -> entry.getValue() > 1L)
    .map(Map.Entry::getKey)
    .findFirst()
    .orElseThrow(() -> new RuntimeException("No repeated words"));

System.out.println(firstRepeated); // Output: spring
```

### 3. Find the Longest Word in a Sentence

This question actually lets you **skip the grouping phase entirely**. It tests if you know how to use the `.max()`operation directly on a stream of strings.

Java

```java
String sentence = "Microservices architecture can be complex";

String longestWord = Arrays.stream(sentence.split("\\s+"))
    // TWEAK: Use max() with a custom Comparator based on string length
    .max(Comparator.comparingInt(String::length))
    .orElse("");

System.out.println(longestWord); // Output: architecture
```

### 4. Reverse Each Word in a Sentence (Keeping Word Order)

_A TCS favorite._ They want `"Hello World"` to become `"olleH dlroW"`. You don't need grouping here, just a `.map()`transformation and `.joining(" ")`.

Java

```java
String sentence = "Java Developer Interview";

String reversedSentence = Arrays.stream(sentence.split("\\s+"))
    // TWEAK: Map each word to its reversed version using StringBuilder
    .map(word -> new StringBuilder(word).reverse().toString())
    // TWEAK: Join them back together with a space in between
    .collect(Collectors.joining(" "));

System.out.println(reversedSentence); 
// Output: avaJ repoleveD weivretnI
```

### 5. Find the Word with the Highest Frequency

Back to the grouping blueprint, but using `.max()` on the `entrySet`.

Java

```java
String sentence = "the quick brown fox jumps over the lazy dog and the dog barks";

String mostFrequent = Arrays.stream(sentence.split("\\s+"))
    .map(String::toLowerCase)
    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
    .entrySet().stream()
    // TWEAK: Compare the map entries by their count (the Value)
    .max(Map.Entry.comparingByValue())
    .map(Map.Entry::getKey)
    .orElse("");

System.out.println(mostFrequent); // Output: the
```


### 1. Count the number of employees in each department

_The classic warm-up question._

Java

```java
Map<String, Long> countByDept = employeeList.stream()
    .collect(Collectors.groupingBy(
        Employee::getDepartment, // 1. Classifier
        Collectors.counting()    // 2. Downstream Collector
    ));

System.out.println(countByDept);
// Output: {Product Development=2, Security And Transport=1, Sales And Marketing=2, Infrastructure=1, HR=2, Account And Finance=1}
```

### 2. Find the average age of Male and Female employees

_Highly frequently asked at Capgemini._

Java

```java
Map<String, Double> averageAgeByGender = employeeList.stream()
    .collect(Collectors.groupingBy(
        Employee::getGender,                     // 1. Classifier
        Collectors.averagingInt(Employee::getAge) // 2. Downstream Collector
    ));

System.out.println(averageAgeByGender);
// Output: {Male=30.166666666666668, Female=29.333333333333332}
```

### 3. Find the highest paid employee in each department

_An Infosys favorite._ This is where candidates stumble because `maxBy` wraps the result in an `Optional`.

Java

```java
Map<String, Optional<Employee>> highestPaidByDept = employeeList.stream()
    .collect(Collectors.groupingBy(
        Employee::getDepartment, // 1. Classifier
        // 2. Downstream Collector (maxBy requires a Comparator)
        Collectors.maxBy(Comparator.comparingDouble(Employee::getSalary)) 
    ));

// To print them out nicely without the "Optional[]" wrapper:
highestPaidByDept.forEach((dept, emp) -> 
    System.out.println(dept + " : " + emp.get().getName())
);
```

### 4. Separate the employees who are younger or equal to 25 years from those older than 25

_The "Partitioning" trick question._ Interviewers will ask this to see if you know the difference between `groupingBy` and `partitioningBy`.

**The Rule:** If you are grouping by a condition that results in True/False (a Predicate), use `partitioningBy`. It is significantly faster than `groupingBy` for boolean checks.

Java

```java
Map<Boolean, List<Employee>> partitionedByAge = employeeList.stream()
    // TWEAK: Using partitioningBy instead of groupingBy
    .collect(Collectors.partitioningBy(e -> e.getAge() > 25));

System.out.println("Older than 25: " + partitionedByAge.get(true));
System.out.println("25 or younger: " + partitionedByAge.get(false));
```

### The "Pro-Tip" Optimization (CollectingAndThen)

For question #3 (highest paid per department), an interviewer might push you: _"Returning `Map<String, Optional<Employee>>` is messy. Can you return `Map<String, Employee>` directly without the Optional?"_

You can use `Collectors.collectingAndThen()` to unwrap the Optional exactly at the moment it gets collected:

Java

```java
Map<String, Employee> cleanHighestPaidByDept = employeeList.stream()
    .collect(Collectors.groupingBy(
        Employee::getDepartment,
        // Wrap the maxBy in collectingAndThen to extract the value immediately
        Collectors.collectingAndThen(
            Collectors.maxBy(Comparator.comparingDouble(Employee::getSalary)),
            Optional::get // Extracts the Employee from the Optional
        )
    ));
```

