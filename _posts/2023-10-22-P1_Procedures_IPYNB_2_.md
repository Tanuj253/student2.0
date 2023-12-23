---
toc: True
comments: True
layout: post
title: P1 Procedures
description: Lesson for developing procedures in Python
type: collab
author: Abbey, Nupur, Sreeja, Tanvi
courses: {'csp': {'week': 10}}
unit: 3
---

## 3.12 Parts of a Procedure:





## Vocab
Procedure- A named group of programming instructions that may have parameters and return values
They are referred to by different names, such as method or function, depending on the programming language

Parameter- input value of a procedure. Arguments specify the values of the parameters when a procedure is called

A Procedure call- interrupts the sequential execution of statements, causing the program to execute the statements wtithin the procedure before continuing . Once the last statement in the proceed has been executed, flow of control is returned to the point immediately following where procedure was called.



A procedure is a group of programming instructions. They're also known as methods or functions, depending on the programming language. You can use a procedure to use the same set of instructions, again and again, without having to rewrite it into your code.In AP Pseudocode, here's how they'll represent procedures: <img src="https://runestone.academy/ns/books/published/mobilecsp/_static/assets/img/AP_Procedures.png"
width="400"
height="200"
/>



```python
first_number = 5
second_number = 7
sum_value = first_number + second_number
print (sum_value)

```

It goes without saying that, if you wanted to sum more than one set of numbers at a time, you'd have to rewrite these lines over and over again...

Let's see how we'd go about writing this as a function, or a procedure in Python.

A function in Python looks like this:



```python
def name_of_function (parameter1, parameter2):
  #code statements here

```

So our summing machine would look like this!



```python
def summing_machine(first_number, second_number):
  print (first_number + second_number)

```

When you call a procedure, your program acts like those lines of code are written out. Once it's done executing those code lines, your program goes back to reading code sequentially.

When you call that procedure with defined values or variables, those values are known as arguments.



```python
def summing_machine(first_number, second_number):
  print (first_number + second_number)

summing_machine(5, 7)
#In this example, 5 and 7 are examples of arguments.


```

For example:


```python
def summing_machine(first_number, second_number):
  value = first_number + second_number
  return (value)

answer = summing_machine(5, 7)
print (answer)

#you can use this to manipulate the value the function returns as well.

print (answer + 1)
```

LETS LOOK AT AN EXAMPLE:  

In this example this is a simple procedure to create a code for calculating the area



```python


# Define the calculate_area procedure
def calculate_area(length, width):
    # Calculate the area here
    area = length * width
    return area


# Call the procedure with different values
length1 = 5
width1 = 3
area1 = calculate_area(length1, width1)




# Print the results
print(f"The area of the first rectangle is {area1}")








    



```

    The area of the first rectangle is 15


## Popcorn Hack 1
a) Define Return Values and Output Parameters in your own words:

b )Code a procedure that finds the square root of any given

## Answers
a return value basically returns whatever the program is running, which means that it keeps the program running. Output parameters are, I think, the values that the program exits, or outputs. So whatever is printed


```python
x = 64


def sqrt(x):
    value = x**0.5
    return value


answer = sqrt(x)
```

## Calling Procedures Quiz

Practice AP mq questions from collageboard: 
## 1 In Python, what can be done by a value returned by a procedure?

a. Define input variables for another procedure

b. Print it to console

c. Call another procedure with it

d. Assign it to a variable or use it in expressions



## 2. When a Procedure is called, what happens to the programs execution?

a. it prints the result of the procedure

b. it exits the program and terminates execution

c. it moves to the next line of code in the program

d. it acts as if the lines of code on the procedure are written out





## 3 Which of the following is an example of a procedural call in Python?

a. `def summing_machine(first_number, second_number)`

b. `summing_machine(5, 7)`

c. `print(first_number + second_number)`

d. `return(value)`







## 3.13 Developing Procedures and Procedural Abstraction



Procedures are an example of abstraction. 

(In fact, it's part of a special form of abstraction known as procedural abstraction.) 

This is because you can call a procedure without knowing how it works. A common built-in procedure in programming languages is the print() procedure.

Procedures allow you to solve a large problem based on the solution to smaller sub-problems. 


 







     

## Popcorn Hack #1
 Simplify code to make the output be one value that gets printed 



```python
firstnum = 1
secondnum = 2
thirdnum = 3
fourthnum = 4
print(firstnum, secondnum, thirdnum, fourthnum)
```

    1 2 3 4


## Answer


```python
firstnum = 1
secondnum = 2
thirdnum = 3
fourthnum = 4

print(firstnum)

```

    1


When you divide a computer program into separate sub-programs, it's known as modularity.
Procedures can also help you simplify your code and improve its readability.
Instead of this:



```python
first_number = 7
second_number = 5
sum_value = first_number + second_number
print (sum_value)

first_number = 8
second_number = 2
sum_value = first_number + second_number
print (sum_value)

first_number = 9
second_number = 3
sum_value = first_number + second_number
print (sum_value)

```

    12
    10
    12


you get this:



```python
def summing_machine(first_number, second_number):
  sum_value = first_number + second_number
  print (sum_value)

summing_machine(5, 7)
summing_machine(8, 2)
summing_machine(9, 3)

```

    12
    10
    12




This is especially important in larger programs, where you might already be looking at hundreds of lines of code.


Procedures can be reused. Parameters usually represent general categories of data, such as numbers or strings. 

This means you could use one procedure for a wide range of individual scenarios.

Remember that placing a return statement inside a procedure will cause an immediate return from the procedure back to where the procedure is called. 

The return statement may appear at any point inside the procedure.

Procedural abstraction also allows programmers the flexibility to modify or fix procedures without affecting the whole program, as long as the procedure continues to function in the same way. 

Furthermore, they can make a change or edit once, in the procedure, and it will be implemented everywhere the procedure is called.


## Popcorn Hack #2: create a Python program that generates multiplication tables for a specific number up to a specified limit. 




```python
def generate_multiplication_table(number, limit):
    """
    Generates a multiplication table for the given number up to the specified limit.
    
    :param number: The number for which to generate the table.
    :param limit: The upper limit of the table (e.g., how far the multiplication will go).
    """
    print(f"Multiplication Table for {number} (Up to {limit}):\n")
    
    for i in range(1, limit + 1):
        result = number * i
        print(f"{number} x {i} = {result}")

# Usage
generate_multiplication_table(5, 10)

```

    Multiplication Table for 5 (Up to 10):
    
    5 x 1 = 5
    5 x 2 = 10
    5 x 3 = 15
    5 x 4 = 20
    5 x 5 = 25
    5 x 6 = 30
    5 x 7 = 35
    5 x 8 = 40
    5 x 9 = 45
    5 x 10 = 50


## Anatomy of Python Classes

Class Definition: Classes are defined using the class keyword, followed by the class name, and a colon. 

The class body is indented beneath the class definition.



```python
class MyClass:
    # class body
    Pass

```

Attributes: These are variables that belong to a class. 

They can be accessed using the dot notation on an instance of the class.




```python
class MyClass:
    def __init__(self, attribute1, attribute2):
        self.attribute1 = attribute1
        self.attribute2 = attribute2

```

Methods: These are functions that are defined within a class. 
    
    They can perform specific operations on the object or modify its attributes.




```python
class MyClass:
    def my_method(self):
        # method body
        pass

```

Constructor: The __init__ method is a constructor in Python classes. 

It is called when an object is created and allows the class to initialize the object's attributes.



```python

class MyClass:
    def __init__(self, attribute1, attribute2):
        self.attribute1 = attribute1
        self.attribute2 = attribute2

```

Self: It represents the instance of the class and is used to access the class's attributes and methods.



```python

class MyClass:
    def my_method(self):
        print(self.attribute1)

```

## Homework
1) Write a Python function called procedural_abstraction that demonstrates procedural abstraction by performing a specific task. Your function should take at least one parameter and return a result. 
2) Write a Python function called summing_machine that takes two parameters, first_number and second_number, and returns the sum of these numbers. Use this function to calculate the sum of 7 and 5. Print the result.



