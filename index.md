---
marp: true
theme: default
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.svg')
---

<!-- ![bg left:40% 80%](https://marp.app/assets/marp.svg) -->

# **Lambda Expression and Stream API**

---

## **Agenda**

- Understanding Lambda
- Using Lambda
- Functional Interfaces
- Method References
- Streams
- Stream Methods and Operations
- Parallel Stream

---

## **Why Lambdas?**

* Enable functional Programming
* Readable and concise code
* Easy to use API and Libraries

---

## **Code in OOP**

* Everything is an object
* All code blocks are "associated" with classes and objects

---

## **Greeter**

```java
public class Greeter {

    public void greet() {
        System.out.println("Hello World!");
    }

    public static void main(String[] args) {
        Greeter greeter = new Greeter();
        greeter.greet();
    }

}
```

---

## Greeter

- Accept an argument that let greet method know what to do

```java

    public void greet(____) {
        _______;
    }

```

---

## Greeter

* An argument and switch statement for different behaviours

* Pass the behaviour itself as an argument

```java

    public void greet(____) {
        _______;
    }

```

---

## Greeter

- Passing behaviour as an argument

```java

    public void greet(Greeting greeting) {
        greeting.perform();
    }

```

---

## Greeter

- Passing Behaviours

```java
    interface Greeting {
        void perform();
    }


    public void main() {
        Greeting greeting = new HelloWorldGreeter();

        Greeter greeter = new Greeter();
        greeter.greet(greeting);
    }

```

---

## Lambda Expression

- Introduce more elegant and easier syntax for passing behaviors

- Functions as values and implementation

```java

    String names = "foo";

    double pi = 3.14;

```

```java
    public void main() {
        Greeting greeting = () -> System.out.println("Hello World!");

        Greeter greeter = new Greeter();
        greeter.greet(greeting);
    }
```

---

## Lambda Expression Examples

```java
    greet(() -> System.out.println("Hello World!"));
```

```java
    () -> { return 42; }
```

```java
    () -> 42
```

```java
    (int x) -> x + 1
```

```java
    (int x, int y) -> x + y
```

```java
    stringLengthCountFunction = (String s) -> s.length()
```

---

## Lambda as Interface type

- Interface acts as type for lambda expressions

```java
    interface Greeting {
        void perform();
    }
```

```java
    Greeting greeting = () -> System.out.println("Hello World!");
```

---

## Functional Interface

- Interface with only one abstract method

```java
    @FunctionalInterface
    interface Adder {
        int add(int a, int b);
    }
```

---

## Runnable Interface

```java
    Runnable runnable = () -> System.out.println("Hello World!");
```

```java
    Thread thread = new Thread(runnable);
    thread.start()
```

---

## Java 8 Functional Interfaces

- java.util.function

```java
    @FunctionalInterface
    public interface Function<T, R> {
        R apply(T t);
    }
```

```java
    @FunctionalInterface
    public interface Predicate<T> {
        boolean test(T t);
    }
```

---

```java
    @FunctionalInterface
    public interface Consumer<T> {
        void accept(T t);
    }
```

```java
    @FunctionalInterface
    public interface Supplier<T> {
        T get();
    }
```

---

## **Streams**

- A way to process collections of objects
- Enable declarative programming
- Stream are lazily processed
- Stream are immutable
- Streams can be parallelized
- Stream does not store its elements. It is a view of a data source such as a collection or an array

---

## **Creating Streams**

- Can be created in different ways

### From a collection

```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);


  Stream<Integer> stream = list.stream();
```

---

## From an Array

```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);


  Stream<Integer> stream = list.stream();

```

---

## From a generator Function

```java
  Stream<Double> randomNumbers = Stream.generate(Math::random);
```

---

## From Values

```java
  Stream<String> stream = Stream.of("a", "b", "c");
```

---

## **From a File**

```java
  Stream<String> lines = Files.lines(Paths.get("file.txt"));
```

---

## **Stream Methods**

### Filter

```java
  filter(Predicate<T> predicate)
```

```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
  Stream<Integer> evenNumbers = list.stream().filter(x -> x % 2 == 0);
```
---

## **Stream Methods**

### Map

```java
  map(Function<T, R> mapper)
```

```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
  Stream<Integer> evenNumbers = list.stream().map(x -> x * x);
```

---

## **Stream Methods**

### FlatMap

* This method returns a new stream that is obtained by flattening the elements of the original stream, which are streams themselves, into a single stream.


```java
  flatMap(Function<T, Stream<R>> mapper)
```

```java
    List<List<Integer>> list = Arrays.asList(
    Arrays.asList(1, 2, 3),
    Arrays.asList(4, 5, 6),
    Arrays.asList(7, 8, 9)
    );

    Stream<Integer> flatStream = list.stream().flatMap(x -> x.stream());
```

---

## **Stream Methods**

### Distinct

```java
  distinct()
```

```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 1, 2, 3);
  Stream<Integer> distinctNumbers = list.stream().distinct();
```

---

## **Stream Methods**

### Peek

```java
  peek(Consumer<T> action)
```

```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
  
  Stream<Integer> debugStream = list.stream()
        .filter(number -> number % 2 == 0)
        .peek(evenNumber -> System.out.println("Filtered value: " + evenNumber));

  debugStream.forEach(System.out::println);

/*
Output
Filtered value: 2
Filtered value: 4

2
4
*/
```
---

## **Stream Operations**

### Intermediate Operations

* Returns a new stream, but not final result
* New stream can be used to perform additional operations
* `filter()`, `map()`, `flatMap()`, `peek()`, `distinct()`, `sorted()`, `limit()`, `skip()` and so on.


---

## **Stream Operations**

### Terminal Operations

* Returns a final result or produce a side effect (printing to console)
* Responsible for triggering the evaluation of the stream
* `forEach()`, `count()`, `collect()`, `reduce()`, `min()`, `max()`, `findFirst()` and so on.

---

```java
  List<String> words = Arrays.asList("apple", "banana", "orange", "avocado");
  
  words.stream()
    .filter(x -> x.startsWith("a"))
    .findFirst()
    .ifPresent(System.out::println);
```
---

## **Collectors**

- Collectors provides a way to perform reduction operation on streams
- Accumulating elements into collections
- Summarizing elements according to various criteria
- Grouping elements or joining into strings


---

```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
  
  List<Integer> evenNumbers = list.stream().filter(x -> x % 2 == 0).collect(Collectors.toList());

```

---

```java
  String sentence = Stream.of("This", "is", "a", "sentence.")
        .collect(Collectors.joining(" "));
```

---

```java
  Map<Integer, Long> group = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    .collect(Collectors.groupingBy(n -> n % 2, Collectors.counting()));
```

---

## **Parallel Streams**

- Parallel streams are capable of processing collections concurrently

```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
  
  List<Integer> evenNumbers = list.parallelStream().filter(x -> x % 2 == 0).collect(Collectors.toList());

```

---
## **A usecase for Parallel Streams**

- Making multiple network calls
  
  ```java
    List<String> urls = Arrays.asList("https://www.google.com", "https://www.facebook.com", "https://www.twitter.com");
    
    List<String> responses = urls.parallelStream().map(url -> makeNetworkCall(url)).collect(Collectors.toList());
  ```


