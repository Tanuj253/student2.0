---
toc: True
comments: True
layout: post
title: U4|Iteration|Student-P1
description: while Loops, for Loops, Developing Algorithms Using Strings, Nested Iteration, Informal code analysis >>> AP Exam Weighting 17.5-22.5%
type: ccc
courses: {'csa': {'week': 8}}
authors: Aliya, Ekam, Emma, Raunak, Luna
---

# U4 | Iteration

---

# 4.1 while Loops

---

- A while loop is a fundamental control structure in programming used for repeated execution of a block of code as long as a condition is true.
- The loop starts by evaluating the condition. If the condition is true, the code inside the loop is executed.
- After each iteration, the condition is re-evaluated, and if it's still true, the loop continues.
If the condition is false initially, the loop code is never executed.
- While loops are used when you don't know in advance how many times the loop needs to execute.
- There's a risk of infinite loops if the condition never becomes false, so be cautious.
You can use variables and complex expressions as loop conditions.
- It's essential to update the loop control variable within the loop to prevent infinite loops.
- While loops are typically used for tasks such as iterating over collections or waiting for a specific condition to be met.
- You can always break out of a while loop prematurely using the break statement.

## Example of While Loops


```java
public class PyramidPattern {
    public static void main(String[] args) {
        int height = 5;
        int row = 1;

        while (row <= height) {
            int spaces = height - row;
            int stars = 2 * row - 1;

            // Print spaces
            int spaceCount = spaces;
            while (spaceCount > 0) {
                System.out.print(" ");
                spaceCount--;
            }

            // Print stars
            int starCount = stars;
            while (starCount > 0) {
                System.out.print("*");
                starCount--;
            }

            System.out.println(); // Move to the next line for the next row
            row++;
        }
    }
}
PyramidPattern.main(null);
```

# 4.2 for Loops

---

- Iterative statement that checks for condition
- Repeatedly execute a a block of code as long as the condition is met
- Condition specifies amount of times

# for Loops vs. while Loops
- while Loops: use when number of iterations is unknown
- for Loops: use when number of iterations is known


```java
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}
```


```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}
```

- Three parts in for loop header: variable initialization, Boolean (conditional) expression, and increment/decrement statement

Question: Which part is which?

- variable initialization (int i=0): sets variable before loop starts
- Boolean (conditional) expression (i < 5): defines condition for loop to run, in this case, the loop continues as long as i is less than 5, so loops 5 times i 05
- increment/decrement statement (i++): increases variable each time code block in the loop is executed, in this case it increases by 1
- variable can be used in the code block for other various reasons besides specifying how many times the loop will repeat
- Boolean (conditional) expression and increment/decrement statement together determine how many times the loop will repeat



```java

int sum = 0;
int sum_all = 0;

for (int i = 0; i < 10; i++) {
    if (i % 2 == 0)
        sum += i;
    sum_all += i;
}

System.out.println(sum);
System.out.println(sum_all);
```

# 4.3 Developing Algorithms Using Strings

---

LEARNING OBJECTIVES:
For algorithms in the context of a particular specification that involves ```String``` objects:
- identify standard algorithms
- modify standard algorithms
- develop an algorithm


**Java has many methods that are helpful when working with strings:**

* ```String .substring``` --> retrieves portion of a string
* ```String .equals``` --> compares two strings
* ```String .length``` --> returns length of a string
* ```for Loop``` --> iterating through characters of a string

<br>
<br>

<h3> Finding a substring within a string </h3>

We can use the "window" method:

A "window" is created the same length as the substring. We then iterate through the main string in sections and compare to the substring

For example:

<h3> I T E R A T E </h3>with substring "ERA"

<br>
<br>
<br>


```java
public class StringFinder {
    public static void main(String[] args) {
        String word = "iterate";
        String sub = "xxx";
        boolean found = false; // will be set to true once substring is found

        for (int i = 0; i < word.length(); i++) { //iterating forwards: starting at first index (0) and going to the length of the word.. let's try word.length
            String portion = word.substring(i, i + sub.length());
            if (sub.length() > portion.length()) break;
            if (portion.equals(sub)) { // make sure you use .equals!!
                found = true;
                break;
            }
        }

        if (found)
            System.out.println("substring is found within string!");
        else
            System.out.println("substring is NOT within string");
    }

}


StringFinder.main(null);
```

<h4> POPCORN HACK: Run the code.. what happened? How can we fix it?</h4>

Tell us below!

<br>
<br>

<h4> Another issue:</h4>

<h3> I T E R A T E </h3>
What if our substring was the word "RATE"? Note that RATE is at the end of the whole string

<br>
<br>

<h3> HACKS </h3>

**Create a algorithm similar to the one above. Except this time, use iteration to count the number of vowels within the main string.**

HINT: Use the boolean expressions we have learned in previous lessons. Which would you use when comparing your "window" with multiple options of substrings?

# 4.4 Nested Iteration

**nested iteration**
<details>occurs when we have a loop inside of another loop, similar to nested conditional statements in unit 3

When you have one loop inside another, the inner loop has to finish all its rounds before the outer loop moves to the next round. If the inner loop has a "stop" command, it only stops for that round of the outer loop. The next time the outer loop starts a new round, the inner loop starts over.

If you have two nested loops without stops, and the outer one runs n times while the inner one runs m times each time the outer one goes around, the inner loop will run m times n times, which is m * n times in total. This rule also applies if you have more than two nested loops. To find the total number of times the innermost loop runs, just multiply how many times each loop runs per round.


```java
public class NestedLoopsDemo {
    public static void main(String[] args) {
        int n = 3; //numb of times the outside loop runs
        int m = 2; //numb of times the inside loop runs

        //the nested loops
        for (int i = 1; i <= n; i++) {
            System.out.println("Outer loop iteration: " + i);
            for (int j = 1; j <= m; j++) {
                System.out.println("Inner loop iteration: " + j);
            }
        }
    }
}
NestedLoopsDemo.main(null)
```

### Break Statement

**break statement**
<details>is used to exit a loop prematurely, typically when a certain condition is met. In the case of nested loops, it can be used to break out of the innermost loop.


```java
public class BreakExample {
    public static void main(String[] args) {
        for (int i = 1; i <= 3; i++) {
            System.out.println("Outer loop iteration " + i);

            for (int j = 1; j <= 3; j++) {
                System.out.println("Inner loop iteration " + j);

                if (i == 2 && j == 2) {
                    System.out.println("Breaking inner loop");
                    break; //break out of the inside loop when i is 2 and j is 2.
                }
            }
        }
    }
}
BreakExample.main(null)
```

### Popcorn HACK

When the targetNumber is found, you can print a message and use the break statement to exit the loop. When it's not found, you can print a message indicating that the number was not found.


```java
public class BreakHack {
    public static void main(String[] args) {
        int targetNumber = 42; //numb we want
        int[] numbers = {10, 20, 30, 40, 50, 60, 70}; //numb array

        for (int number : numbers) {
            if (number == targetNumber) {
                //if numb is found
                //print message to break out loop
            }
        }
        //if numb isnt found
        //print message showing numb wasnt found if you want
    }
}
BreakHack.main
```

### Continue Statement

**continue statement**
<details>is used to skip the current iteration of a loop and move to the next iteration. In the case of nested loops, it applies to the innermost loop.


```java
public class ContinueExample {
    public static void main(String[] args) {
        for (int i = 1; i <= 3; i++) {
            System.out.println("Outer loop iteration " + i);

            for (int j = 1; j <= 3; j++) {
                if (i == 2 && j == 2) {
                    System.out.println("Skipping inner loop iteration " + j);
                    continue; //skip the iteration when i is 2 and j is 2.
                }
                System.out.println("Inner loop iteration " + j);
            }
        }
    }
}
ContinueExample.main(null)
```

### Patterns and Shapes


```java
import java.util.Scanner;

public class InteractivePyramid {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter the symbol you want to use: ");
        char symbol = scanner.next().charAt(0);

        System.out.print("Enter the number of rows for the pyramid: ");
        int numRows = scanner.nextInt();

        for (int i = 1; i <= numRows; i++) {
            //print space before the symbol
            for (int j = 1; j <= numRows - i; j++) {
                System.out.print(" ");
            }

            //print
            for (int k = 1; k <= 2 * i - 1; k++) {
                System.out.print(symbol);
            }

            System.out.println(); //next line
        }
        scanner.close();
    }
}
InteractivePyramid.main(null)
```

## Hacks

1. **Modify pyramid code:**

- Create different patterns (other then pyramid) by modifying nested loop structure

2. **Questions**

- What is a nested iteration, continue statement, and break statement (in your own words)?
- Create a simple example of a continue statement **or** break statement

---

# 4.5 Informal Code Analysis

<b>Learning objective</b>: Compute statement execution counts & informal run-time comparison of iterative statements

<b>Essential Knowledge</b>: A statement execution count indicates the number of times a statement is executed by the program

<h3> What IS informal code analysis? </h3>

Answer:


```java
// CODE EXAMPLE #1 (for loop)
public class InformalCodeAnalysis {
    public static void main(String[] args) {
        int count = 0;
        for (int k = 0; k < 30; k++)
        {
            if (k % 3 == 0) // statement 1
            {
                count++; // statement 2
            }
        }
    }
}
```

<b>How many times will statement 1 execute? </b>

Answer:

<b>How many times will statement 2 execute?</b>

Answer:


```java
// CODE EXAMPLE #2 (for loop)
public class InformalCodeAnalysis {
    public static void main(String[] args) {
        int count = 0;
        for (int k = 4; k < 30; k+=3)
        {
            count++; // statement 3
        }
    }
}
```

<b>How many times will statement 3 execute?</b>

Answer:


```java
// Rewrite the code segment below to have a faster run-time based on statement execution counts
for (int k = 0; k < 135; k++)
{
    if (k % 5 == 0)
    {
        System.out.println(k);
    }
}
```


```java
// CODE EXAMPLE #3 (while loop)

int num = (int)(Math.random() * 10);
while (num % 2 != 0)
{
    num = (int)(Math.random() * 10) // statement 4
}
```

<b>What is the min/max number of times statement 4 will execute?</b>

Answer:


```java
// CODE EXAMPLE #4 (nested loop)

for (int outer = 0; outer < 3; outer++)
{
    for (int inner = 0; inner < 4; inner++)
    {
        // statement #5
    }
}
```

<b>How many times will statement #5 execute?</b>

Answer:


```java
// CODE EXAMPLE #5 (nested loop)

int k = 0;
while (k < 5)
{
    int x = (int)(Math.random() * 6) + 1;
    while (x != 6)
    {
        // statement #6
        x = (int)(Math.random() * 6) + 1;
    }
    k++;
}
```

<b>How many times will statement #6 execute?</b>

Answer:

# 4.5 Hacks


<b>#1 How many times will statement #1 and statement #2 execute in the code segments below? </b>


```java
for (int k = 0; k < 1000; k++)
{
    // statement #1
}
```


```java
for (int k = 6; k < 50; k++)
{
    // statement #2
}
```

<b>#2 How many times will statement #3 execute for the code segment below?</b>


```java
int k = 1;
while (k <=7)
{
    for (int z = 0; z < 4; z++)
    {
        // statement #3
    }
    k++;
}
```

<b>#3 Create 3 seperate code segments that execute a statement 10 times using: </b>

(a) a for loop

(b) a while loop

(c) a nested loop


```java
// 3a code
```


```java
// 3b code
```


```java
// 3c code
```
