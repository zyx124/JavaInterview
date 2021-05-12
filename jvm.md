## JVM



![JVM architecture](https://static.javatpoint.com/images/jvm-architecture.png)

**ClassLoader:**

- Bootstrap ClassLoader: load the *rt.jar* file which contains all class files of Java Standard Edition like *java.lang, java.net, jave.util*.
- Extension ClassLoader: Child of Bootstrap ClassLoader
- System/Application ClassLoader: child of Extension ClassLoader.

---

**Class Area**: stores per-class structures such as the runtime constant pool, fields and methods data. 

**Heap**: runtime area where **objects** are allocated. Heap is shared by all threads. It's also the main region for **Garbage Collection**.

Java 1.7 and previous version:

Young generation

Old generation

Permanent generation

![](https://snailclimb.gitee.io/javaguide/docs/java/jvm/pictures/java%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F/JVM%E5%A0%86%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84-JDK7.png)

Java 1.8 and later: 

![](https://snailclimb.gitee.io/javaguide/docs/java/jvm/pictures/java%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F/JVM%E5%A0%86%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84-jdk8.png)

**Stack:** stores **frames**. It holds local variables and partial results, and plays a part in method invocation and return. 

Each **thread** has a private JVM stack. 

A frame is created every time a method is invoked. A frame is destroyed when its method invocation completes. 

May have `StackOverFlowError` and `OutOfMemoryError`

**Program Counter Register: **PC register contains the address of the JVM instruction currently being executed. It's the only area that has no `OutOfMemoryError`.

**Native Method Stack**: It contains all the native methods used in the application.

---

**Execution Engine:** 

- a virtual processor 
- Interpreter: read bytecode and executes
- Just-In-Time( JIT) compiler: improves the performance.

**Java Native Interface:** JNI is a framework which provides an interface to communicate with another application written in another language.

## Garbage Collection

HotSpot VM GC types:

- Partial GC
  - Young GC
  - Old GC
  - Mixed GC
- Full GC

Objects are allocated into Eden area, after the first collection of the *Young Generation*, they will go into S0 or S1 if they still survive, and the age is added by 1. After the age is larger than a value (default 15), the objects will go to the Old Generation.

An object is at first allocated into Eden area, if space is not enough, JVM will start a Minor GC to put the objects into the Survivor area. 

Large objects like Strings and arrays go directly into the Old Generation.

**Garbage collector**:

- Serial: pause all the other threads until the GC finishes
- Parnew: Parallel GC threads, still stop other threads
- Parallel Scavenge: focus on how to efficiently use cpu.
- CMS (Concurrent Mark Sweep):  multiple GC while the app is still running. 
- G1 (Garbage First): designed for multi-processor machines with large memory spaces. 

## Class Loader

types:

- Bootstrap CL: loads JDK internal classes. It serves as a parent of all the other `ClassLoader` instances.
- Extension CL: child of Bootstrap CL and takes care of loading the extensions of the standard core Java Classes.
- System CL: child of Extension CL and takes cares of loading all the application level classes into the JVM. 

**How CL works?** 

![](https://media.geeksforgeeks.org/wp-content/uploads/jvmclassloader.jpg)

principles of a Java CL:

**Delegation Model:** Delegation Hierarchy Algorithm to load the classes into the Java File. 

**Visibility Principle:** a class loaded by a parent CL is visible to the child CL, but classes loaded by child are not visible to the parent.

**Uniqueness Property:** no repetition of classes. 

