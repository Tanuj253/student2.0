---
toc: True
comments: True
layout: post
title: P1 Algorithims
description: Algorithims
type: collab
courses: {'csp': {'week': 7}}
author: Tarun, Deva, Shuban, Hayden
---

### Algorithims

Definition: A finite set of instructions to accomplish a task

### Algorithim Basics

Sequence: Follows a sequence of steps

Selection: choose different outcomes at steps based on a condition that is made

Iteration: Repetition of code segment if condition is met


















![Screenshot%202023-10-04%20at%2010.48.37%20PM.png](Screenshot%202023-10-04%20at%2010.48.37%20PM.png)

### Psuedo Code
Definition: A planning language/text that conveys the general implementation of an algorithim to all programers

Sample College Board Psuedo Code(Strings):

![Screenshot%202023-10-04%20at%2010.49.55%20PM.png](Screenshot%202023-10-04%20at%2010.49.55%20PM.png)


### Robot MCQ(Algorithims)


![Screenshot%202023-10-04%20at%2010.52.58%20PM.png](Screenshot%202023-10-04%20at%2010.52.58%20PM.png)

Develop an general algorithm that allows the robot in EVERY box to go around the black rectangular box once, back to its original starting point.

![Screenshot%202023-10-04%20at%2010.58.27%20PM.png](Screenshot%202023-10-04%20at%2010.58.27%20PM.png)

![Screenshot%202023-10-04%20at%2011.01.14%20PM.png](Screenshot%202023-10-04%20at%2011.01.14%20PM.png)


![thing.png](thing.png)

### String Operations

Length function gives the length of a string

    Python Example/Pseudo Code example: len(“codecodecode”) returns 12
    
String Concatenation is similar to adding strings together 

    Pseudo Code: Concat(“String1 ”,”String2”) returns “String1 String2”
    
    Python Code: “String1 “+”String2” concatenates the strings together
    
String substringing can collect a range of character from a string

    Pseudo Code: substring(“MortensonBest”,1,4) returns Mort
    
    PythonCode: string=”MortensonBest”
    
                           string[0:4] returns “Mort”

# String Algorithims

### Palindrome
Takes user input and converts to a palindrome by duplicating the string backwards.
Users can choose to duplicate last character or not.

Constraints: defaults to no duplication unless user types exactly "yes"


A palindrome string or number is the same read forwards or backwards. Simple as that. 

Ex. 

“bob”

“hannah”

1337331

9


### Example of Palindrome Algorithim Used:

Takes user input and converts to a palindrome by duplicating the string backwards.
Users can choose to duplicate last character or not.

Constraints: defaults to no creation of palindrome unless user types exactly "yes"


```python
mystr = str(input("Enter your string: "))
mybool = input("Would you like to make a palindrome that also duplicates the middle character?").lower()

if mybool == "yes":
    secondstr = mystr[::-1]
else:
    secondstr = mystr[:-1]
    secondstr = secondstr[::-1]
endresult = mystr+secondstr
print(endresult)
```

    Enter your string: codecodecode
    Would you like to make a palindrome that also duplicates the middle character?no
    codecodecodedocedocedoc


#### Popcorn Hack: Write steps for an algorithim that can find the index of any character in a string

Example:
Input: "C"
String <- "ABCDEFG"

Output: 2

#### Answer
1. Collect Input Character
2. Define the string
3. Loop through each character
4. While looping compare input character and the character in the string and if same print the index

## Homework Hack For String Algorithims
Write Python Code or Psuedo Code that is able to take a string as input and detect if its a palindrome

# Math Algorithims


![Screenshot%202023-10-04%20at%2011.12.24%20PM.png](Screenshot%202023-10-04%20at%2011.12.24%20PM.png)



### Fibonacci Sequence

![Screenshot%202023-10-04%20at%2011.15.35%20PM.png](Screenshot%202023-10-04%20at%2011.15.35%20PM.png)




Prints the first n fibonacci numbers in a list
Constraints: n > 1


```python
f0 = 0
f1 = 1
fiblist = [f0,f1]
r = int(input("How many items do you want in your fibonacci sequence? Enter a number 2 or greater"))
for i in range(r-2):
    x = fiblist[-1]
    y = fiblist[-2]
    fiblist.append(x+y) # Add the sum of the previous 2 numbers to the list
print(fiblist)
```

    How many items do you want in your fibonacci sequence? Enter a number 2 or greater5
    [0, 1, 1, 2, 3]


Popcorn Hack: Can anybody tell how we can fix this code so it correctly makes a fibonacci sequence? 




![Screenshot%202023-10-05%20at%208.35.06%20AM.png](Screenshot%202023-10-05%20at%208.35.06%20AM.png)



### Sorting Algorithims

![Screenshot%202023-10-04%20at%2011.33.06%20PM.png](Screenshot%202023-10-04%20at%2011.33.06%20PM.png)

Sorts an array of integers in increasing order. Relatively inefficient, however.


```python
array = [3,5,6,1,4,2]
n = len(array)
for i in range(n):
    for j in range(0,n-i-1):
        if array[j] > array[j+1]:
            array[j], array[j+1] = array[j+1], array[j]
print(array)
```

    [1, 2, 3, 4, 5, 6]


## Selection Sort
![Screenshot%202023-10-04%20at%2011.34.25%20PM.png](Screenshot%202023-10-04%20at%2011.34.25%20PM.png)

### Homework Hack 2

Create a program that uses at least 1 of the algorithmic concepts covered in this notebook in a real-world application








```python

```
