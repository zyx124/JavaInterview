## Core Java 

#### String
1. String constant pool
2. String is immutable
3. StringBuilder (Synchronized) vs StringBuffer(Non-synchronized)

**difference of final, finally, finalize**: 
A final class cannot be instantiated, a final method cannot be overriden, a final variable cannot be reassigned.
The finally keyword is used to create a block of code that follows a try block. A finally block of code always excecutes, whether or not an exception has occured.
finalize() is a method that garbage collector always calls just before the deletion of the object eligible for garbage collection.

#### collections

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







