---
toc: True
layout: post
title: U9 Fibonacci (Teacher)
description: Defines a parent class that provides constructor, accessor and settter methods for Fibonacci.  The child class needs to define the algorithm to init the Fibonacci Sequence.  Java fundamentals for creating objects, extending class, creating abstract class, utilizing  methods, super and this. in.   Programming fundamentals include given for loop and hacks for while, recursion implementation.  Additionally, the parent class shows use of ArrayList and HashMap data structures, which are topic for later unit.
courses: {'csa': {'week': 11, 'categories': ['1.B', '4.C', '5.A']}}
categories: ['C4.8', 'C.49']
type: ccc
---

```java
/*
 * Creator: Nighthawk Coding Society
 * Mini Lab Name: Fibonacci sequence
 * 
*/

import java.util.ArrayList;  
import java.util.HashMap;
import java.util.stream.Stream;

/* Objective will require changing to abstract class with one or more abstract methods below */
abstract class Fibo {
    String name;  // name or title of method
    int size;  // nth sequence
    int hashID;  // counter for hashIDs in hash map
    ArrayList<Long> list;   // captures current Fibonacci sequence
    HashMap<Integer, Object> hash;  // captures each sequence leading to final result

    /*
     Zero parameter constructor uses Telescoping technique to allow setting of the required value nth
     @param: none
     */
    public Fibo() {
        this(8); // telescope to this(n) to avoid code duplication, using a default value
    }

    /*
     Construct the nth fibonacci number
     @param: nth number, the value is constrained to 92 because of overflow in a long
     */
    public Fibo(int nth) {
        this.size = nth;
        this.list = new ArrayList<>();
        this.hashID = 0;
        this.hash = new HashMap<>();
        this.init();  //call abstract method
    }

    /*
     This Method should be "abstract"
     Leave method as protected, as it is only authorized to extender of the class
     Make new class that extends and defines init()
     Inside references within this class would change from this to super
     Repeat process using for, while, recursion
     */
    protected abstract void init();

    /*
     Number is added to fibonacci sequence, current state of "list" is added to hash for hashID "num"
     */
    public void setData(long num) {
        list.add(num);
        hash.put(this.hashID++, list.clone());
    }

    /*
     Custom Getter to return last element in fibonacci sequence
     */
    public long getNth() {
        return list.get(this.size - 1);
    }

    /*
     Custom Getter to return last fibonacci sequence in HashMap
     */
    public Object getNthSeq(int i) {
        return hash.get(i);
    }

    /*
     Console/Terminal supported print method
     */
    public void print() {
        System.out.println("Init method = " + this.name);
        System.out.println("fibonacci Number " + this.size + " = " + this.getNth());
        System.out.println("fibonacci List = " + this.list);
        System.out.println("fibonacci Hashmap = " + this.hash);
        for (int i=0 ; i<this.size; i++ ) {
            System.out.println("fibonacci Sequence " + (i+1) + " = " + this.getNthSeq(i));
        }
    }
}
```


```java
public class FiboFor extends Fibo {

    public FiboFor() {
        super();
    }

    public FiboFor(int nth) {
        super(nth);
    }

    @Override
    protected void init() {
        super.name = "For Extends";
        long limit = this.size;
        // for loops are likely the most common iteration structure, all the looping facts are in one line
        for (long[] f = new long[]{0, 1}; limit-- > 0; f = new long[]{f[1], f[0] + f[1]})
            this.setData(f[0]);
    }

    /*
    Tester class method.  If this becomes abstract you will not be able to test it directly ...
    Change this method to  call "main" class of each of the extended classes
     */
    static public void main(String[] args) {
        FiboFor fib = new FiboFor();
        fib.print();
    }
}

FiboFor.main(null);
```

    Init method = For Extends
    fibonacci Number 8 = 13
    fibonacci List = [0, 1, 1, 2, 3, 5, 8, 13]
    fibonacci Hashmap = {0=[0], 1=[0, 1], 2=[0, 1, 1], 3=[0, 1, 1, 2], 4=[0, 1, 1, 2, 3], 5=[0, 1, 1, 2, 3, 5], 6=[0, 1, 1, 2, 3, 5, 8], 7=[0, 1, 1, 2, 3, 5, 8, 13]}
    fibonacci Sequence 1 = [0]
    fibonacci Sequence 2 = [0, 1]
    fibonacci Sequence 3 = [0, 1, 1]
    fibonacci Sequence 4 = [0, 1, 1, 2]
    fibonacci Sequence 5 = [0, 1, 1, 2, 3]
    fibonacci Sequence 6 = [0, 1, 1, 2, 3, 5]
    fibonacci Sequence 7 = [0, 1, 1, 2, 3, 5, 8]
    fibonacci Sequence 8 = [0, 1, 1, 2, 3, 5, 8, 13]



```java
public class FiboStream extends Fibo {

    public FiboStream() {
        super();
    }

    public FiboStream(int nth) {
        super(nth);
    }

    @Override
    protected void init() {
        super.name = "Stream Extends";
        Stream.iterate(new long[]{0, 1}, f -> new long[]{f[1], f[0] + f[1]})
            .limit(super.size)
            .forEach(f -> super.setData(f[0]) );
    }

    /*
    Tester class method.  If this becomes abstract you will not be able to test it directly ...
    Change this method to  call "main" class of each of the extended classes
     */
    static public void main(String[] args) {
        FiboStream fib = new FiboStream();
        fib.print();
    }
}


FiboStream.main(null);
```

    Init method = Stream Extends
    fibonacci Number 5 = 3
    fibonacci List = [0, 1, 1, 2, 3]
    fibonacci Hashmap = {0=[0], 1=[0, 1], 2=[0, 1, 1], 3=[0, 1, 1, 2], 4=[0, 1, 1, 2, 3]}
    fibonacci Sequence 1 = [0]
    fibonacci Sequence 2 = [0, 1]
    fibonacci Sequence 3 = [0, 1, 1]
    fibonacci Sequence 4 = [0, 1, 1, 2]
    fibonacci Sequence 5 = [0, 1, 1, 2, 3]


## Hacks
Working with common algorithms lets developer explore the language in a functional way.  Two additional ways to solve fibonacci are using a while loop and recursion.  This will require definition of two new child classes with a static tester method "static public void main(String[] args)" to create objects and print results.

1. Create while or do-while class extending parent fibonacci
2. Create recursive class and methods extending parent fibonacci
3. Create definitions and comments within code, be sure that you can discuss work.  
4. Extra for guaranteed "A" and get you on the path to 5 on exam.  Try to create a different algorithm with extend.  See if you can make a parent class for Fibo and new Class extend from shared parent.
    - Finding nth term of linear sequence
    - Sum the nth value of a series of numbers
    - Calculate the Factorial a number
    - Create a palindrome checker

[Design Idea](https://jinja.nighthawkcodingsociety.com/algorithm/fibonacci/)

[Alternate Algorithm, Palindrome](https://jinja.nighthawkcodingsociety.com/algorithm/palindrome/)


