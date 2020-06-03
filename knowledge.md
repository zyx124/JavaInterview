### Core Java 

#### String
1. String constant pool
2. String is immutable
3. StringBuilder (Synchronized) vs StringBuffer(Non-synchronized)

**difference of final, finally, finalize**: 
A final class cannot be instantiated, a final method cannot be overriden, a final variable cannot be reassigned.
The finally keyword is used to create a block of code that follows a try block. A finally block of code always excecutes, whether or not an exception has occured.
finalize() is a method that garbage collector always calls just before the deletion of the object eligible for garbage colletion.
