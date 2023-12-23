---
toc: True
comments: True
layout: post
title: P2 Iteration
description: Team SAIGA lecture on iterations.
type: collab
courses: {'csp': {'week': 8}}
author: Ishraq, Anthony, Grayson, Aditya, Sumedha
---

## Flow of Information

The information in a code flows in a direction based on the loops and conditional statements written.

## Control Statements 

These statements determine the order in which the code is executed. Two major ways of writing control statements are using iteration and transfer statements.

# Iteration
#### By: (Sumedha, Aditya, Ishraq, Grayson, Anthony)

## What is iteration?

We use iteration when we need to repeat the same code for a multiple number of times. Iteration is the looping of data in a code sequence. In Python, iteration is done using While loops and For loops.

## While loops

While loops are used when the looping depends on a condition. As long as the condition is true, the code will be executed. When the condition becomes false, the loop stops.


In a while loop, we need to specify the index value (where the loop starts) and the increment value (what the loop increases by).


```python
i = 0
while i < 5:
    print(i)
    i += 1
```

## Hack 1:

#### Write a code for printing even numbers from 1 to 10 using a while loop.


```python
i=0
# your code here
print (i)

```

## Transfer Functions

Statements which transfer control from one part of a program to another. Examples are: break, continue, and pass statements.

## Break Funtion

To stop a code from printing after a certain value, use the break function. We use an if statement to decide where the output will stop. The break function is placed before the increment and after the if statement.


```python
i = 1
while i < 5:
  print(i)
  if i == 2:
    break
  i += 1
```

## Continue Function

To skip an output value and continue on to the end value, use the continue function. We use an if statement to decide which value should be skipped. The continue function is placed after both the increment and the if statement.


```python
i = 0
while i < 5:
  i += 1 
  if i == 2:
    continue
  print(i)
```

## Hack 2:

#### Write a code to list the numbers from 1 to 20 while skipping the multiples of 2 using a while loop.


```python
i = 0
# your code here    
    print(i)
```

## Pass Function

The pass function is like a placeholder after empty code has been written. When a pass statement is added, there will be no effect, but there will be no error message. If there is no pass statement, then an error message will appear.


```python
for x in []:
    pass
```

## Else Statements

If a condition isn't true anymore, then we can write a code using an else statement which stops the iteration from continuing.


```python
i = 1
while i < 5:
  print(i)
  i += 1
else:
  print("i is no longer less than 5")
```

## For Loops

For Loops are used to iterate over a group of values such as lists, strings, dictionaries, tuples, etc. 


```python
gemstones = ["aquamarine", "beryl", "citrine"]
for x in gemstones:
  print(x)
```

## Looping through Strings

We can iterate through strings by using a for loop that shows each character in the string. Example below: 


```python
for x in "beryl":
    print(x)
```

## Break Function

Using a break function, we can stop the loop from iterating when the output is a certain value. This is the same as in while loops.


```python
gemstones = ["aquamarine", "beryl", "citrine"]
for x in gemstones:
  print(x)
  if x == "beryl":
    break
```

## Continue Function

Using a continue function, we can skip an output value and continue the iteration till the end of the sequence. This is similar to while loops.


```python
gemstones = ["aquamarine", "beryl", "citrine"]
for x in gemstones:
    if x == "beryl":
        continue
    print(x)
```

## Range Function

Using a range function, we can iterate through a specific range of values. This function starts with zero and increments by 1, which it does automatically, and we specify exactly which values to iterate through.


```python
for x in range(8):
    print(x)
```

We can define the starting value if it is other than zero by adding a second parameter.


```python
for x in range(2,8):
    print(x)
```

We can also define the increment value by adding a third parameter.


```python
for x in range(1,8,2):
    print(x)
```

## Else Statements

We use else statements to signal when a code is done iterating through a set of values. It is used to code for a message which is printed to show when the iteration is complete.  


```python
for x in range(8):
    print(x)
else:
    print("The code is finished!")
```

## Nested Loops

Nested Loops are loops within loops. This is used to return an output which is a combination of the outer and inner loops.


```python
for x in [1, 2]:
    print(x)
    for y in [2, 3, 4, 5]:
        print(y)
```


```python
for i in range(4,6):
    print (i)
    for x in range(1,11):
        print (i, "X", x, "=", i * x)
```

## Hack 3

Write a code using a for loop that outputs numbers from 1 to 100 and skips multiples of 4.

Upper 90%: Add multiple parameters to specify your values. Use continue to skip the proper values. Double check outputs.


```python
# Your code here
```

## Homework



# Create a function that takes a list of numbers and arranges the in descending order
For example this function should be able to take the list [35,4,6,90,2,55,13,8] and arrange it into [2,4,6,8,13.35,55,90]

## Extra Credit
Make it so this function can also sort strings by their length
For example [Car,4,11,twenty,27,trees] should sort into [Car,4,trees,twenty,11,27]


```python
#start here
def shortestToLongestSorter(nums):
    pass
```
