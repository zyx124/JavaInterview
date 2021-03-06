Binary operations:
x << y is equivalent to x * Math.pow(2, y)



Change an array to a set (remove duplicates)

```java
int[] array = new int[] {1, 1, 4, 5, 6, 7, 4};
Integer[] array2 =  {1, 34,4,5 ,56};
//change array to a list then change to a set(remove duplicates)
Set<Integer> set1 = new HashSet<Integer>(Arrays.stream(array)
                                         .boxed() //box the primitive type to wapper class
                                         .sorted()
                                         .collect(Collectors.toList()));

Set<Integer> set2 = new HashSet<Integer>(Arrays.toList(array2));
```

Change a set to an array

```java
T[] array = mySet.toArray(new T[mySet.size()]);
```

change int array to Integer array

``` java
int[] array = new int[] {1, 1, 4, 5, 6, 7, 4};
Integer[] newArray = Arrays.stream(array)
    						.boxed()
    						.collect(Collectors.toList());
```



How to remove elements from map when iterating? (also applies for other types in Collections)

```java
// When using Map.remove(), ConcurrentModificationException will occur.

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





## sort 

sort an array

```java
Arrays.sort(myArray);
Arrays.sort(myArray, startIdx, endIdx);
Array.sort(myArray, Collections.reverseOrder());
```

sort a list

```java 
// Integer list
List<Integer> nums = Arrays.asList(1,3,4,54,4);
nums.sort(Comparator.naturalOrder());
nums.sort(Comparator.reverseOrder());

// String list
List<String> stringList = Array.asList("ny", "nj", "fl", "ca");
stringList.sort(String.CASE_INSENSITIVE_ORDER); //ignore cases
stringList.sort(Comparator.naturalOrder());

// Object list
List<Movie> movies = Array.asList(new Movie("Rings"), new Movie("Back to the future"), new Movie("Matrix"));
movies.sort(Comparator.comparing(Movie::getTitle));
```



Fill an array with same numbers

```java
Arrays.fill(myArray, 10);
Arrays.fill(myArray, 1, 5, 10); // (array, startIdx, endIdx, element)
```

create an array with index

```java 
// method 1
Arrays.setAll(myArray, i -> i);

// method 2
int[] array = IntStream.range(1, 100).toArray();
```

**How to sort objects**

1. Implement a `Comparator` and override `compare`.
2. Implement the `Comparable` interface and override `compareTo` 

```java
// 1. Using Comparator
import java.util.*;
 
class Student
{
    private String name;
    private int age;
 
    public Student(String name, int age)
    {
        this.name = name;
        this.age = age;
    }
 
    @Override
    public String toString()
    {
        return "{" + "name='" + name + '\'' +
                    ", age=" + age + '}';
    }
 
    public String getName() {
        return name;
    }
 
    public int getAge() {
        return age;
    }
}
 
class Main
{
    public static void main(String[] args)
    {
        Student[] students = { new Student("John", 15), new Student("Sam", 20),
                                new Student("Dan", 20), new Student("Joe", 10) };
 
        Arrays.sort(students, new Comparator<Student>() {
            @Override
            public int compare(Student first, Student second)
            {
                if (first.getAge() != second.getAge()) {
                    return first.getAge() - second.getAge();
                }
                return first.getName().compareTo(second.getName());
            }
        });
 
        System.out.println(Arrays.toString(students));
    }
}

```

```java
// 2. Using Comparable 
import java.util.*;
 
class Student implements Comparable<Student>
{
    private String name;
    private int age;
 
    public Student(String name, int age)
    {
        this.name = name;
        this.age = age;
    }
 
    @Override
    public String toString()
    {
        return "{" + "name='" + name + '\'' +
                    ", age=" + age + '}';
    }
 
    public String getName() {
        return name;
    }
 
    public int getAge() {
        return age;
    }
 
    @Override
    public int compareTo(Student o)
    {
        if (this.age != o.getAge()) {
            return this.age - o.getAge();
        }
        return this.name.compareTo(o.getName());
    }
}
 
class Main
{
    public static void main(String[] args)
    {
        Student[] students = { new Student("John", 15), new Student("Sam", 20),
                                new Student("Dan", 20), new Student("Joe", 10) };
 
        Arrays.sort(students);
        System.out.println(Arrays.toString(students));
    }
}
```



## sum

sum of array elements (average() method can also apply)

```java
Array.stream(myArray).sum(); // int[] myArray
Array.stream(myArray).maptoInt(Integer::intValue).sum(); // Integer[] myArray
```

sum of list elements

```java
List<Integer> integers = Arrays.asList(1, 3, 4, 5, 5);
// method 1
Integer sum = integers.stream()
    				  .reduce(0, (a, b) -> a + b);
Integer sum = integers.stream()
    			      .reduce(0, Integer::sum);
// method 2
Integer sum = integers.stream()
    				  .collect(Collectors.summingInt(Integer::intValue));

// method 3
Integer sum = integers.stream()
    				  .mapToInt(Integer::intValue)
    				  .sum();
```

 

## DataType Conversion

char array to String

```java
char[] chars = {'a', 'b', 'c'};
String str = new String(chars);
```

String to char array

```java
String str = "abc";
char[] chars = str.toCharArray();
```

String to List of Characters

```java
String s = "abcd";
// mapToObj box the char to Character or compilse error will happen
List<Character> charList = s.chars().mapToObj(i -> (char)i).collect(Collectors.toList());
```



ArrayList to Array

```java
List<Character> cList = new ArrayList<Character>(Arrays.asList('a', 'b', 'c'));

Character[] cArray = cList.toArray(new Character[cList.size()]);
// or
Character[] CArray = cList.toArray(Character[]::new);

/*
if we only use toArray() method, the array object type is Object.
*/
```

String array to ArrayList

```java
String[] array = {"a", "b", "c"};
// method 1
List<String> list1 = new ArrayList<String>(Arrays.asList(array));

// method 2
List<String> list2 = new ArrayList<String>();
Collections.addAll(list2, array);


```

ArrayList to int array

```java
ArrayList<Integer> list = new ArrayList<Integer>();
int[] array = list.stream().mapToInt(Integer::intValue).toArray();
```



array to List

```java
List<T> list = List.of(array);
```



char to String

```java
char c = 'a';
String s1 = String.valueOf(c);
String s2 = Character.toString(c);
```

string to char

```java
char c = s.charAt(0);
```



## ASCII conversion

check if a char is lower

ASCII value of common characters:

0 - 48

a- 97

A - 65

```java
String S = "World";
System.out.println(Character.isLetter(S.charAt(0)));

// if there is only alphanumeric
// in ascii, 'a' represent 97, 'A' represent 65
// if a char is alphanumeric and larger or equal than 65, it is a lowercase letter.
System.out.println(S.charAt(0) - 'a' >= 0)
```

## Split a String

```java
// 1.
String str = "peter, james, ricky";
String[] splitted = str.split(", ");
```

## Traverse a Map

```java
class MapIteration {
    public static void main(String[] arg) {
        Map<String, String> map = new HashMap<>();
        map.put("abc", "ABC");
        map.put("Hi", "There");
        
        // method 1
        for (Map.Entry<String, String> entry: map.entrySet()) {
            System.out.println("Key: " + entry.getKey() +
                              "Value: " + entry.getValue());
        }
        
        // method 2
        map.forEach((k, v) -> {
            System.out.println("Key: " + k + "Value: " + v);
        })
    }
}
```

