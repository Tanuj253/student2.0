---
toc: True
layout: post
title: Abstract Fibonacci
description: The Fibonacci algorithm runs two ways with abstraction.  This will be used as model to build GitHub analytics.
courses: {'csa': {'week': '13'}}
type: ccc
image: /images/fibonacci.png
categories: ['1.B', '4.C', '5.A', 'C4.8', 'C4.9']
---

```Java
/*
 * Creator: Nighthawk Coding Society
 * Mini Lab Name: Fibonacci sequence, featuring a Stream Algorithm
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
        this(8); // telescope to avoid code duplication, using default as 20
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
        //initialize fibonacci and time mvc
        this.init();
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


```Java

public class FiboFor extends Fibo {

    public FiboFor() {
        super();
    }

    public FiboFor(int nth) {
        super(nth);
    }

    @Override
    protected void init() {
        super.name = "FiboFor extends Fibo";
        long limit = this.size;
        // for loops are likely the most common iteration structure, all the looping facts are in one line
        for (long[] f = new long[]{0, 1}; limit-- > 0; f = new long[]{f[1], f[0] + f[1]})
            this.setData(f[0]);
    }

    /*
    Tester class method.
     */
    static public void main(int... numbers) {
        for (int nth : numbers) {
            Fibo fib = new FiboFor(nth);
            fib.print();
            System.out.println();
        }
    }
}

FiboFor.main(2, 3, 4, 5);

```

    Init method = FiboFor extends Fibo
    fibonacci Number 2 = 1
    fibonacci List = [0, 1]
    fibonacci Hashmap = {0=[0], 1=[0, 1]}
    fibonacci Sequence 1 = [0]
    fibonacci Sequence 2 = [0, 1]
    
    Init method = FiboFor extends Fibo
    fibonacci Number 3 = 1
    fibonacci List = [0, 1, 1]
    fibonacci Hashmap = {0=[0], 1=[0, 1], 2=[0, 1, 1]}
    fibonacci Sequence 1 = [0]
    fibonacci Sequence 2 = [0, 1]
    fibonacci Sequence 3 = [0, 1, 1]
    
    Init method = FiboFor extends Fibo
    fibonacci Number 4 = 2
    fibonacci List = [0, 1, 1, 2]
    fibonacci Hashmap = {0=[0], 1=[0, 1], 2=[0, 1, 1], 3=[0, 1, 1, 2]}
    fibonacci Sequence 1 = [0]
    fibonacci Sequence 2 = [0, 1]
    fibonacci Sequence 3 = [0, 1, 1]
    fibonacci Sequence 4 = [0, 1, 1, 2]
    
    Init method = FiboFor extends Fibo
    fibonacci Number 5 = 3
    fibonacci List = [0, 1, 1, 2, 3]
    fibonacci Hashmap = {0=[0], 1=[0, 1], 2=[0, 1, 1], 3=[0, 1, 1, 2], 4=[0, 1, 1, 2, 3]}
    fibonacci Sequence 1 = [0]
    fibonacci Sequence 2 = [0, 1]
    fibonacci Sequence 3 = [0, 1, 1]
    fibonacci Sequence 4 = [0, 1, 1, 2]
    fibonacci Sequence 5 = [0, 1, 1, 2, 3]
    



```Java
public class FiboStream extends Fibo {

    public FiboStream() {
        super();
    }

    public FiboStream(int nth) {
        super(nth);
    }

    @Override
    protected void init() {
        super.name = "FiboStream extends Extends";

        // Initial element of stream: new long[]{0, 1}
        // Lambda expression calculate the next fibo based on the current: f -> new long[]{f[1], f[0] + f[1]}
        Stream.iterate(new long[]{0, 1}, f -> new long[]{f[1], f[0] + f[1]})
            .limit(super.size) // stream limit
            .forEach(f -> super.setData(f[0]) );  // set data in super class
    }

    /*
    Tester class method.
     */
    static public void main(int... numbers) {
        for (int nth : numbers) {
            Fibo fib = new FiboFor(nth);
            fib.print();
            System.out.println();
        }
    }
}

FiboStream.main(7);
```

    Init method = FiboFor extends Fibo
    fibonacci Number 7 = 8
    fibonacci List = [0, 1, 1, 2, 3, 5, 8]
    fibonacci Hashmap = {0=[0], 1=[0, 1], 2=[0, 1, 1], 3=[0, 1, 1, 2], 4=[0, 1, 1, 2, 3], 5=[0, 1, 1, 2, 3, 5], 6=[0, 1, 1, 2, 3, 5, 8]}
    fibonacci Sequence 1 = [0]
    fibonacci Sequence 2 = [0, 1]
    fibonacci Sequence 3 = [0, 1, 1]
    fibonacci Sequence 4 = [0, 1, 1, 2]
    fibonacci Sequence 5 = [0, 1, 1, 2, 3]
    fibonacci Sequence 6 = [0, 1, 1, 2, 3, 5]
    fibonacci Sequence 7 = [0, 1, 1, 2, 3, 5, 8]
    


# Hacks
Solve Fibonacci using two additional algorithms.

