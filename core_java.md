#### Core Java

## **String**

1. String constant pool

2. String is **immutable**

   This means that every time we "change" the string, a new String object is created. Same for `substring` method.

   ![string-substring-jdk7](http://www.programcreek.com/wp-content/uploads/2013/09/string-substring-jdk71-650x389.jpeg)

3. StringBuilder (Synchronized) vs StringBuffer(Non-synchronized)

The largest length of a String: 2^16 - 1 = 65535 when String is in the constant pool. 65534 in execution by javac.

## Keywords

**difference of final, finally, finalize**: 
A `final` class cannot be instantiated, a final method cannot be overridden, a final variable cannot be reassigned.
The `finally` keyword is used to create a block of code that follows a try block. A `finally` block of code always executes, whether or not an exception has occurred.
`finalize()` is a method that garbage collector always calls just before the deletion of the object eligible for garbage collection.

**static**:

- Static variables are initialized only once before other instanced variables.

- static methods can only call other static methods. static methods can be accessed directly by class. Static methods cannot be **overridden**.

**default**: Java 8 introduces `default` methods for interface so that the child classes do not necessarily overwrite the added methods.

## **OOP**

**Inheritance**: Java doesn't support multiple inheritance as it may cause diamond problems. Java supports multiple interface implementations.

**Polymorphism**: 

| Static             | Dynamic            |
| ------------------ | ------------------ |
| Overload (compile) | Override (runtime) |

**difference between interfaces and abstract classes**

| Interface                                                    | Abstract class                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| only have `public abstract` methods                          | can have `abstract` and non abstract methods. Since Java 8, `default` and `static` methods are allowed. |
| fields are default`public static final`, can have `default ` methods since java 8 | can have `final, non-final, static, non-static` fields       |
| a class can `implements`multiple interface                   | can only `extends`one class                                  |

**Marker Interface**: no methods or constants inside. Provides run time information about objects. E.g. **Cloneable, Serializable**.



## Design Pattern

**Singleton**: restrict only one instance of a certain class. 

```java
public class Singleton implements Serializable, Cloneable {
    private static Singleton instance;
    
    // private constructor
    private Singleton() {}
    
    // static synchronized singleton getInstance
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton()
        }
        return instance;
    }
    
    // prevent clone 
    @Override
    protected Object clone() throws CloneNotSupportException {
        throw CloneNotSupportException();
    }
    
    // prevent serializable
    protect Object readResolve() {
        return instance;
    }
}
```

**factory**: 

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

HashMap also needs overwrite ```equals``` method for keys. Besides, `hashCode()`also needs to be created, which can utilize Objects.hash(Objects...) to realize.

**EnumMap**

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



**TreeMap**

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

```ascii
                     ┌───────────┐
                     │  Object   │
                     └───────────┘
                           ▲
                           │
                     ┌───────────┐
                     │ Throwable │
                     └───────────┘
                           ▲
                 ┌─────────┴─────────┐
                 │                   │
           ┌───────────┐       ┌───────────┐
           │   Error   │       │ Exception │
           └───────────┘       └───────────┘
                 ▲                   ▲
         ┌───────┘              ┌────┴──────────┐
         │                      │               │
┌─────────────────┐    ┌─────────────────┐┌───────────┐
│OutOfMemoryError │... │RuntimeException ││IOException│...
└─────────────────┘    └─────────────────┘└───────────┘
                                ▲
                    ┌───────────┴─────────────┐
                    │                         │
         ┌─────────────────────┐ ┌─────────────────────────┐
         │NullPointerException │ │IllegalArgumentException │...
         └─────────────────────┘ └─────────────────────────┘
```



**user defined exception**:

```java
// first define a base exception
public class BaseException extends RuntimeException{
    // ...
}

// then other exceptions extends the base 
public class UserNotFoundException extends BaseException {
    //...
}
```



## Multithreading 

**Two ways to create threads**

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

**States of a thread**

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

**how to stop a thread**

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
    public volatile boolean running = true;
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

**volatile vs ThreadLocal**

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



**Daemon Thread**: JVM will exit when all the non daemon thread has stopped, whether or to the daemon thread has stopped. 

set a thread as deamon

```java
Thread t = new MyThread();
t.setDaemon(true);
t.start()
```

**synchronized**

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

**wait, notify**

`wait()`will release the lock temporarily so that other threads can get the lock. Then `notify()`will make the waiting thread get back its lock. `notifyAll()`will wake all the waiting thread up. 

**ReentrantLock**

`java.util.concurrent`has `ReentrantLock`to replace `synchronized` to add locks.

**optimistic, pessimistic** locks

- optimistic: read and write won't cause concurrent problems. will check modification when update. e.g. `StampedLock`
- pessimistic: add locks every time retrieving data in case that other thread will change the data. e.g. `ReadWriteLock`

**ExecutorService and thread pool**

```java
ExecutorService executor = Executors.newFixedThreadPool(3);
executor.submit(task1);
executor.submit(task2);
executor.submit(task3);
executor.shutdown(); // shutdown the service
```

`newFixedThreadPool(), newSingleThreadPool(), newCachedThreadPool()` are all using `ThreadPoolExecutor()`, which has `corePoolSize, maximumPoolSize, keepAliveTime, unit, wordQueue, RejectExecutionHandler` parameters.

**Rejection Strategy:**

- AbortPolicy: default, will throw an Exception and stop the execution
- CallerRunsPolicy: give the task to the current thread
- DiscardPolicy: ignore the task
- DiscardOldestPolicy: ignore the oldest task.



**Future**

Java provides a `Callable<T>` interface to return a value, since`Runnable`has no return. 

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

