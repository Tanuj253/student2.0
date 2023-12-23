---
toc: True
comments: True
layout: post
title: P4 Simulation
description: Lesson for simulations in Python
type: collab
author: Beijan, Karam, Justin, Rayhan
courses: {'csp': {'week': 10}}
unit: 3
---

# Simulations:
## 3.16, 3.17: Simulations and Algorithmic Efficiency

<h3> What is a Simulation?</h3>

- A simulation, in context of computer science, is a digital representation of a situation in the real world.
- Examples:
    - Experiments: When an experiment is to dangerous to perform in the real world or too expensive, a simulation can be made of it and be performed digitally.
    - Training and Education: Simulations such as flight simulators and medical simulation can be very practical in aiding dangerous proffesions to receive training.
    - Video Games: Some video games try to aim to be as releastic as possible with physics and graphics to try to simulate the real world.


<h3> Simple Dice Roll


```python
import random

def roll_die():
    # Gives random integer between 1 and 6
    return random.randint(1, 6)

# Defines the variable "result" as the result of function roll_die()
result = roll_die()
print("Roll:", result)

```

<h3> Dice Roll


```python
import random

def roll_die():
    # Gives random integer between 1 and 6
    return random.randint(1, 6)

def main():
    # Allows user to control how many rolls are executed
    num_rolls = int(input("How many times would you like to roll the die? "))
    
    # Makes sure that the entered value is a valid integer
    if num_rolls <= 0:
        print("Please enter a valid number of rolls.")
        return

    print("Rolling the die", num_rolls, "times:")
    
    for i in range(num_rolls):
        roll_result = roll_die()
        print("Roll", i + 1, ":", roll_result)

if __name__ == "__main__":
   main()
```

## Popcorn Hack #1
-Create a similation that generates a random number using the random library and randint like shown above. Be creative.


```python
#Enter code here
```

## 3.17 Algorithmic Efficiency
- A problem is a general description of a task that can (or cannot) be solved algorithmically
    - A decision problem is a problem with a yes/no answer
    - An optimization problem is a problem with the goal of finding the "best" solution among many
- Efficiency is an estimate of the amount of computational resources used by an algorithm
- An algorithm's efficiency is determined through formal or mathematically reasoning

One way do demonstrate how algorithmic efficiency can be used to improve a program is through a simple sorting program. 

## Bubble Sort
Bubble Sort is a simple sorting algorithm that repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. The pass through the list is repeated until no swaps are needed, which means the list is sorted.


```python
def bubble_sort(list):
    n = len(list)
    for i in range(n):
        for j in range(0, n - i - 1):
            if list[j] > list[j + 1]:
                list[j], list[j + 1] = list[j + 1], list[j]

import random
list = [random.randint(1, 10) for _ in range(10)]
bubble_sort(list)
print("Bubble Sorted listay is:", list)
 
```

## Popcorn Hack 2
- Using bubble sort, make multiple strings that sort alphabetically.


```python
#Enter code here
```

## Quick Sort
Quick Sort is a popular divide-and-conquer sorting algorithm. It divides the array into smaller sub-arrays and recursively sorts them. It's generally more efficient than Bubble Sort for larger lists.


```python
def quick_sort(list):
    if len(list) <= 1:
        return list
    else:
        pivot = list[0]
        less_than_pivot = [x for x in list[1:] if x <= pivot]
        greater_than_pivot = [x for x in list[1:] if x > pivot]
        return quick_sort(less_than_pivot) + [pivot] + quick_sort(greater_than_pivot)

import random
list = [random.randint(1, 10) for _ in range(10)]
list = quick_sort(list)
print("Quick Sorted listay is:", list)

```

## Homework Hacks

# Hack 1
- Create a random decision maker that choses between 2 string options.


```python
#Enter code here
```

# Hack 2
Modify the following function that calculates the sum of a range of numbers from 1-99999999 more efficient.
Hints:
- The formula for finding the sum of an arithmetic series is n/2 * (first_number + last_number) where n is the number of terms.
- To find the number of terms in a list, use len(list)
- To index the first term of a list, use list[0] and to index the last term, use list[-1].


```python
#edit this code
def calculate_sum(numbers):
    total = 0
    for num in numbers:
        total += num
    return total
#numbers = [i for i in range(1, 9999999)]
print(calculate_sum(numbers))
```
