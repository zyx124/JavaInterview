####  	Core Java

## Wrapper Class

Wrapper classes encapsulate values inside to create objects. Integer is a wrapper class of int. Wrapper class is needed because:

- java only supports call by value
- serialization 
- synchronization
- collections framework works with only objects.

## **String**

1. String constant pool

2. String is **immutable**

   This means that every time we "change" the string, a new String object is created. Same for `substring` method.

   ![string-substring-jdk7](http://www.programcreek.com/wp-content/uploads/2013/09/string-substring-jdk71-650x389.jpeg)

3. StringBuilder (Synchronized) vs StringBuffer(Non-synchronized)

The largest length of a String: 2^16 - 1 = 65535 when String is in the constant pool. 65534 in execution by javac.



**Why string is immutable?**

Security: Network connections, database connections, user name and password are often represented in strings. Mutability will let these parameters easily be changed.

Synchronization: Immutable String is thread safe.

Caching: if the two string have the same value, the compiler will optimized them into one object.



## Keywords

**difference of final, finally, finalize**: 
A `final` class cannot be instantiated, a final method cannot be overridden, a final variable cannot be reassigned.
The `finally` keyword is used to create a block of code that follows a try block. A `finally` block of code always executes, whether or not an exception has occurred.
`finalize()` is a method that garbage collector always calls just before the deletion of the object eligible for garbage collection.

**static**:

- Static variables are initialized only once in **compile** time before other instanced variables.

- static methods can only call other static methods. static methods can be accessed directly by class. Static methods cannot be **overridden**.

**default**: Java 8 introduces `default` methods for interface so that the child classes do not necessarily overwrite the added methods. For multiple implementation of interfaces, the default methods can be called as 

```java
interface A {
    default void call() {
        System.print("a");
    }
}

interface B {
    default void call() {
        System.print("b");
    }
}

class C implements A, B {
    @Override
    void call() {
        A.super.call();
        B.super.call();
    }
}
```

**Field Modifier**:

| Access modifier    | scope                         |
| ------------------ | ----------------------------- |
| public             | same class ands other classes |
| private            | same class                    |
| protected          | same class and sub-class      |
| No access modifier | same package                  |



## **OOP**

**Objects**: with fields and methods to store states and behavior. 

**Inheritance**: Inheritance is a hierarchy relation between subclasses and the super class. Subclass can obtain properties of its super class. 

Java allows **single, multilevel, hierarchy** inheritance. Java doesn't support multiple inheritance as it may cause diamond problems. Java supports multiple interface implementations.

**How to solve diamond problem?** 

**Polymorphism**: 

| Static             | Dynamic            |
| ------------------ | ------------------ |
| Overload (compile) | Override (runtime) |

**Covariant:** From java 5.0 onwards, overriding can have different return types from parent class. Child's return type should be the **sub-type** of the parent class.

**interface vs abstract class**

| Interface                                                    | Abstract class                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| only have `public abstract` methods                          | can have `abstract` and non abstract methods. Since Java 8, `default` and `static` methods are allowed. |
| fields are default`public static final`, can have `default ` methods since java 8 | can have `final, non-final, static, non-static` fields       |
| a class can `implements`multiple interface                   | can only `extends`one class                                  |

**Marker Interface**: no methods or constants inside. Provides run time information about objects. E.g. **Cloneable, Serializable**.

**Serializable**: The serialization process is to convert an object into byte stream and deserialization is to reverse the stream into the object. Serializable will tell JVM this class can be serialized. 

`static` and `transient` fields won't be serialized. Once a class implements `Serializable`, all its subclasses are serializable. 

When an object has an reference to another object, these objects must also implement `Serializable`, or JVM will throw `NotSerializableException`. 

We should create a `serialVersionUID ` so that if we change our class structure, JVM won't through `InvalidClassException`.

**Encapsulation**: provide access control through hide the fields of a class as private. 

Why: Flexibility, Reusability, Maintainability

**toString()**: convert the objects into string representation. Without it, will print the address of the object.

## Java 8

**Lambdas**: anonymous function 

**Functional Interfaces**: Interface with a **single Abstract methods**, like **Runnable**, **Comparable**

- 



## I/O Stream

**Stream type**: 

- **Byte stream** (`InputStream`, `OutputStream`): handling byte, for processing raw data files like binary files.

  ![IO_InputOutputStreams](/home/zyx/repo/JavaInterview/IO_InputOutputStreams.png)

  

- **Character Stream** (`Reader`, `Writer`): handling characters, use Unicode so can be internationalized. Often used to process text files.

  <img src="/home/zyx/repo/JavaInterview/IO_InputOutputReadersWriters.png" alt="IO_InputOutputReadersWriters" style="zoom:150%;" />

  - **Buffered Stream**: read buffer memory, native input API is called only when the buffer is empty, native output API is called when the buffer is full. `flush ` method can be called to save the buffer to the disk.

    

    

## Design Pattern

**Singleton**: restrict only one instance of a certain thread. 

```java
public class Singleton implements Serializable, Cloneable {
    // lazy instantiation
    private static Singleton instance; 
    
    // Eager instantiation
    // private static Singleton instance = new Singleton();
    
    
    // private constructor
    private Singleton() {}
    
    // static synchronized singleton getInstance
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
    
    // prevent clone 
    @Override
    protected Object clone() throws CloneNotSupportException {
        throw new CloneNotSupportException();
    }
    
    // prevent serializable
    protected Object readResolve() {
        return instance;
    }
}
```

Enum singleton has serialization and thread-safe guaranteed, prevent **reflections**

```java
public enum EnumSingleton {
    
    INSTANCE("Initial class info"); 
 
    private String info;
 
    private EnumSingleton(String info) {
        this.info = info;
    }
 
    public EnumSingleton getInstance() {
        return INSTANCE;
    }
    
    // getters and setters
}
```



**factory**: without exposing the creation logic and refer to newly created object using a common interface.

```java
   public interface Shape {
       public void draw() {
           System.out.println("draw shape");
       }
   }
   
   public class Circle implements Shape {
       @Override
       public void draw() {
           System.out.println("draw circle");
       }
   }
   
   public class Square implements Shape {
       @Override
       public void draw() {
           System.out.println("draw square");
       }
   }
   
   public class ShapeFactory {
       public Shape getShape(String type) {
           if (type == null) return null;
           if (type.equalsIgnoreCase("circle")) {
               return new Circle();
           }
           if (type.equalsIgnoreCase("square")) {
               return new Square();
           }
           return null;
       }
   }
```



## Collections

#### Hierarchy

![](https://facingissuesonitcom.files.wordpress.com/2019/07/java-collection-framework-hierarchy.jpg)


#### Iterator

Why? Unsafe to add or delete collection while iterating. 

How to remove elements from map when iterating? (also applies for other types in Collections)

```java
// When using remove() in a loop, ConcurrentModificationException will occur.
List<Integer> list = new ArrayList<>();
for (Integer i ： list) {
    list.remove(i); // Exception!
}

// method 1: Using Iterator.remove()
Map<String, String> map = new HashMap<>();
Iterator<Map.Entry<String, String>> it = map.entrySet().iterator();
while (it.hasNext()) {
    Map.Entry<String, String> entry = it.next();
    if (entry.getKey().equals("test")) {
        it.remove();
    }
}
// or 
for (Iterator<Map.Entry<String, String>> it = map.entrySet().iterator(); it.hasNext()) {
    Map.Entry<String, String> entry = it.next();
    if (entry.getKey().equals("test")) {
        it.remove();
    }
}
// method 2: Using removeIf() in Collections
map.entrySet().removeIf(e -> map.getKey().equals("test"));
```

When java compiler deals with the for each loop, it will converts the loop into the type using iterators.

Iterator can iterator set, which cannot be iterate directly using a for loop.

#### hashcode() and equals()

Hashcode() will get same result every time it is called. If two objects are same through equals(), then Hashcode() will be the same. If not, Hashcode() may not required to be different.

#### Comparable and comparator (sort objects)

1. Implement a `Comparator` and override `compare`. It is often used **externally**.

   ```java
   Collections.sort(arraylist, new Comparator<T>() {
       @Override
       public int compare(T o1, T o2) {
           return o1 - o2;
       }
   })
   ```

   

2. Implement the `Comparable` interface and override `compareTo` 

   

```java
class Book implements Comparable<Book> {
    public String name, id, author, publisher;
    public Book(String name, String id, String author, String publisher) {
        this.name = name;
        this.id = id;
        this.author = author;
        this.publisher = publisher;
    }
    public String toString() {
        return ("(" + name + ", " + id + ", " + author + ", " + publisher + ")");
    }
    @Override
    public int compareTo(Book o) {
        // usually toString should not be used,
        // instead one of the attributes or more in a comparator chain
        return toString().compareTo(o.toString());
    }
}

@Test
public void sortBooks() {
    Book[] books = {
            new Book("foo", "1", "author1", "pub1"),
            new Book("bar", "2", "author2", "pub2")
    };

    // 1. sort using Comparable
    Arrays.sort(books);
    System.out.println(Arrays.asList(books));

    // 2. sort using comparator: sort by id
    Arrays.sort(books, new Comparator<Book>() {
        @Override
        public int compare(Book o1, Book o2) {
            return o1.id.compareTo(o2.id);
        }
    });
    System.out.println(Arrays.asList(books));
}
```



When list methods ```contains``` and ```indexOf``` are applied to Objects, we should rewrite the ```equals``` method. 

```java
public class Person {
    public String name;
    public int Age;
    
    public Person(String name) {
        this.name = name;
    }
    // rewrite equals method
    public boolean equals(Object o) {
        if (o instanceOf Person) {
            Person p = (Person) o;
        	return Objects.equals(this.name, p.name) && this.age == p.age;
        }
        return false;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        List<Person> list = List.of(
       		new Person("Ricky"),
            new Person("Bob");
            new Person("Jack");
        );
        System.out.println(list.contains(new Person("Bob"))); // true because equals method has been created.
    }
}
```

**HashMap** also needs overwrite ```equals``` method for keys. Besides, `hashCode()`also needs to be created, which can utilize `Objects.hash(Objects...)` to realize.

#### HashMap implementation

put():

- initialize buckets (array), load factor and capacity.
- hash() the key and calculate index
- if no collision, put into the bucket, if there's collision, use LinkedList to go behind 
- if the linked list is too large, change to red-black tree
- if node exist, change the value
- if load factor is reached, expand 

get(): 

- if first node is the result, get it
- if collision, use equals() to get the entry (O(n) or O(logn))

#### 



#### **LinkeList vs ArrayList**:

| ArrayList                                  | LInkedList                              |
| ------------------------------------------ | --------------------------------------- |
| dynamic array inside                       | doubly  linked list interal             |
| manipulating data slow for bit shifting    | manipulating data fast, no bit shifting |
| only implements List                       | implements list and deque               |
| better for storing and accessing data( O1) | Manipulating data better                |

#### **EnumMap**

If the keys are `Enum` types, `EnumMap` can be used to improve the speed.

```java
import java.util.*;
import java.time.DayOfWeek;

public class MyClass {
    public static void main(String args[]) {
      Map<DayOfWeek, String> map = new EnumMap<>(DayOfWeek.class);
        map.put(DayOfWeek.MONDAY, "1");
        map.put(DayOfWeek.TUESDAY, "2");
        map.put(DayOfWeek.WEDNESDAY, "3");
        map.put(DayOfWeek.THURSDAY, "4");
        map.put(DayOfWeek.FRIDAY, "5");
        map.put(DayOfWeek.SATURDAY, "6");
        map.put(DayOfWeek.SUNDAY, "7");
        System.out.println(map); //{MONDAY=1, TUESDAY=2, WEDNESDAY=3, THURSDAY=4, FRIDAY=5, SATURDAY=6, SUNDAY=7}
        System.out.println(map.get(DayOfWeek.MONDAY)); // 1
      
    }
}
```



#### **TreeMap**

TreeMap inherits from SortedMap, the keys of which is sorted when being put into the map. Therefore, `Comparable`interface should be implemented, which requires that if equal, return 0, if larger, return 1, if smaller, return -1.

```java
class Student {
    public String name;
    public int score;
    Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
    
    public String toString() {
        return String.format("%s: score=%d", name, score);
    }
}

publc class Main {
    public static void main(String[] args) {
        Map<Student, Integer> map = new TreeMap<>(new Comparator<Student> {
            public int compare(Student p1, Student p2) {
                if (p1.score == p2.score) {
                    return 0;
                }
                return p1.score > p2.score ? -1:1;
            }
        });
        map.put(new Student("Tom", 88), 1);
        map.put(new Student("Bob", 66), 2);
        map.put(new Student("Lily", 99), 3);
        for (Student key: map.keySet()) {
            System.out.println(key);
        }
        System.out.println(map.get(new Student("Bob", 66))); 
            
           
    }
}
```



## Exception handling

![](https://www.protechtraining.com/static/bookshelf/java_fundamentals_tutorial/images/ExceptionClassHierarchy.png)

**Checked vs Unchecked** Exceptions:

Checked exception is checked by compiler at compile time: `IOException`, `SQLException`

Unchecked exception is checked at run time by JVM, `ArrayIndexOutOfBoundsException`, `NullPointerException`.



**Exception vs Error**: Error is a serious problem and **can** be caught (but usually not). Exception can be caught to make the program keep running.



A static block can **only** throw RunTimeException, or try catch a checked exception.

**Exception Handling:**

```java
public static void main(String[] args) {
    Scanner scanner = null;
    try {
        scanner = new Scanner(new File("test.txt"));
        while (scanner.hasNext()) {
            System.out.println(scanner.nextLine());
        }
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } finally {
        if (scanner != null) {
            scanner.close();
        }
    }
}

// Transfer to the try-with-resources. It can deal with multiple sources and do not need finally block to close the sources. It still can have catch and finally blockes though.
public static void main(String[] args) {
    try (Scanner scanner = new Scanner(new File("test.txt"))) {
        while (scanner.hasNext()) {
            System.out.println(scanner.nextLine());
        }
    } catch (FileNotFoundException fnfe) {
        fnfe.printStackTrace();
    }
}
```

`throws` vs `throw`

```java
void process (String file) throws IOException {
    try {...}
    finally {
        throw new NumberFormatException("null");
    }
}


```

how to get full stack of exception

```java
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
            throw new IllegalArgumentException(e); // must pass e to the next exception or it will be retyped and cannot be located.
        }
    }

    static void process2() {
        throw new NullPointerException();
    }
}
```





#### **user defined exception**:

```java
// first define a base exception, create a checked exception
public class BaseException extends Exception{
    // ...
}

// first define a base exception, create an unchecked exception
public class BaseException extends RuntimeException{
    // ...
}

// then other exceptions extends the base 
public class UserNotFoundException extends BaseException {
    //...
}
```



## Multithreading 

#### Thread vs Process

Process is a program and usually programs don't share the memory and consists of multiple thread. Thread just deal with simple task and may share the memory with other threads. 

#### **Two ways to create threads**

```java
// 1
class MyThread1 extends Thread {
    @Override
    public void run() {
        System.out.println("start my thread 1")
    }
}

// 2
class Mythread2 implements Runnable {
    @Override 
    public void run() {
        System.out.println("start my thread 2")
    }
}
```

`Runnable` is preferred because you can treat the objects separately instead of only get the thread behavior. It allows the objects loosely coupled with concurrency ways. 

#### Methods

To create a thread, `start()` is used. If only use `run()`, it only calls the simple method. `start()` can only be used once, but `run()` can be called multiple times.

`join()` makes the thread into a waiting state until the reference thread terminates.

`wait()`: Instance method of an object used for synchronization. Can only be called from a **synchronized block** to release the lock on the object.

`Thread.sleep()`: static method, just pause the current thread and does **not** release locks.

`Thread.yield()`: inform the scheduler that this thread is willing to relinquish its current use of processor and to be scheduled back as soon as possible. The behavior is varying.

#### life cycle of a thread

![](https://static.javatpoint.com/images/thread-life-cycle.png)





#### **States of a thread**

```ascii
         ┌─────────────┐
         │     New     │
         └─────────────┘
                │
                ▼
┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐
 ┌─────────────┐ ┌─────────────┐
││  Runnable   │ │   Blocked   ││
 └─────────────┘ └─────────────┘
│┌─────────────┐ ┌─────────────┐│
 │   Waiting   │ │Timed Waiting│
│└─────────────┘ └─────────────┘│
 ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─
                │
                ▼
         ┌─────────────┐
         │ Terminated  │
         └─────────────┘
```



#### **how to stop a thread**

1. `interrupt()`method to send a request to stop the thread, the target thread check `isInterrupted()`to know if itself has stopped. If the target thread is waiting, it will catch `InterruptedException`.

2. Set a state variable for the thread 

```java
public class Main{
    public static void main(String[] args) throws InterruptedException{
        HelloThread t = new HellowThread();
        t.start();
        Thread.sleep(1);
        t.running = false;
    }
}

class HelloThread extends Thread {
    public volatile boolean running = true; // use volatile to make sure the synchronization with the main memory.
    public void run() {
        int n = 0;
        while (running) {
            n++;
            System.out.print(n + "hello");
        }
        System.out.println("end!");
    }
}
```

#### **volatile vs ThreadLocal**

The `volatile` keyword is to tell the JVM always update and fetch the latest value of a variable. After a thread changes the value of a value, other threads can immediately see the latest value.

```ascii
┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐
           Main Memory
│                               │
   ┌───────┐┌───────┐┌───────┐
│  │ var A ││ var B ││ var C │  │
   └───────┘└───────┘└───────┘
│     │ ▲               │ ▲     │
 ─ ─ ─│─│─ ─ ─ ─ ─ ─ ─ ─│─│─ ─ ─
      │ │               │ │
┌ ─ ─ ┼ ┼ ─ ─ ┐   ┌ ─ ─ ┼ ┼ ─ ─ ┐
      ▼ │               ▼ │
│  ┌───────┐  │   │  ┌───────┐  │
   │ var A │         │ var C │
│  └───────┘  │   │  └───────┘  │
   Thread 1          Thread 2
└ ─ ─ ─ ─ ─ ─ ┘   └ ─ ─ ─ ─ ─ ─ ┘
```

`ThreadLocal()`is the "local variable" of a thread which makes sure that ThreadLocal variable is independent. It must be applied in `try... finally` and be cleared in `finally`.



**Daemon Thread**: JVM will exit when all the non daemon thread has stopped, whether or not the daemon thread has stopped. 

set a thread as daemon

```java
Thread t = new MyThread();
t.setDaemon(true);
t.start()
```

#### **synchronized**

use `synchronized`keyword to add lock to blocks of code. `synchronized` methods add lock to `this`.

```java 
public class Counter {
    private int count = 0; 
    
    public void add(int n) {
        synchronized(this) {
            count += n;
        }
    }
    
    // this is equivalent to the following 
    // public synchronized void add(int n) {...}
    
    public void dec(int n) {
        synchronized(this) {
            count -= n;
        }
    }
    
    public int get() {
        return count;
    }
}
```

#### **wait, notify**

`wait()`will release the lock temporarily so that other threads can get the lock. Then `notify()`will make the waiting thread get back its lock. `notifyAll()`will wake all the waiting thread up. 

#### **ReentrantLock**

`java.util.concurrent`has `ReentrantLock`to replace `synchronized` to add locks. It provides synchronization method while accessing shared resources. 

#### **optimistic, pessimistic** locks

- optimistic: read and write won't cause concurrent problems. will check modification when update. e.g. `StampedLock`
- pessimistic: add locks every time retrieving data in case that other thread will change the data. e.g. `ReadWriteLock`

#### **ExecutorService and thread pool**

```java
ExecutorService executor = Executors.newFixedThreadPool(3);
executor.submit(task1);
executor.submit(task2);
executor.submit(task3);
executor.shutdown(); // shutdown the service
```

`newFixedThreadPool(), newSingleThreadPool(), newCachedThreadPool()` are all using `ThreadPoolExecutor()`, which has `corePoolSize, maximumPoolSize, keepAliveTime, unit, wordQueue, RejectExecutionHandler` parameters.

#### **Rejection Strategy:**

- AbortPolicy: default, will throw an Exception and stop the execution
- CallerRunsPolicy: give the task to the current thread
- DiscardPolicy: ignore the task
- DiscardOldestPolicy: ignore the oldest task.



#### **Future**

Java provides a `Callable<T>` interface to return a value, since `Runnable`has no return. 

```java
class Task implements Callable<String> {
    public String call() throws Exception {
        return longTimeCalculation();
    }
}

```

```java
ExecutorService executor = Executors.newFixedThreadPool(3);
Callable<String> task = new Task();
Future<String> future = executor.submit(task);
String result = future.get();
```

`ExecutorService.submit()` and `invokeAll()` return `Future` type, which allows to get results in the future. If the main thread use `get()` methods of `Future`objects, we can get the asynchronous result. 

