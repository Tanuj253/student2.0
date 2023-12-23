---
toc: True
comments: True
layout: post
title: P4 Developing Algorithms
description: Lesson for developing algorithms in Python
type: collab
author: Patrick, Kaiyu, Aidan, Miguel
courses: {'csp': {'week': 9}}
unit: 3
---

# Developing Algorithms

### What is an Algorithm   
An algorithm is a set of instructions for solving a specific problem or performing a particular task.

### Purpose of developing Algorithms
- Automate and simplify repetitive code
- More efficient functions


```python
## Example of algorithm
import random
# Define min and max values
min_num = 1
max_num = 10
# Generate Random Number
random_number = random.randint(min_num, max_num)
# Output
print(f"Random number: {random_number}")
```

    Random number: 9


![image.png](image.png)

### Same goal, different Process
The examples show different processes to see if you got an A, passed, or failed on a test. However, these two algorithms yield different results. The first algorithm uses three different "if" statements which will check each condition independently so two messages can be displayed. The second algorithm uses an elif statement to ensure that only one of the three conditions will be displayed.


```python
display("What did you get on your test?")
testScore = int(input())
if testScore > 90:
    display("You got an A!")
if testScore > 70:
    display("Congragulations. You didn't fail.")
if testScore < 70:
    display("You failed.")
```


    'What did you get on your test?'



    'You failed.'



```python
display("What did you get on your test?")
testScore = int(input())
if testScore > 90:
    display("You got an A!")
elif testScore > 70:
    display("Congratulations. You didn't fail.")
else:
    display("You failed.")
```


    'What did you get on your test?'



    'You failed.'


### Algorithms in statistic/mathematical calculation
Algorithms in statistical and mathematical calculations refer to systematic step-by-step procedures or methods for solving specific problems, performing computations, or making decisions. These algorithms are designed to ensure in various mathematical and statistical tasks.


```python
# Mean
numberlist = [3, 6, 9, 12, 15]
mean = sum(numberlist) / len(numberlist)
print("Mean:", mean)
```

    Mean: 9.0



```python
# Median
numberlist = [3, 6, 9, 12, 15]
n = len(numberlist)
if n % 2 == 0:
    median = (numberlist[n // 2 - 1] + numberlist[n // 2]) / 2
else:
    median = numberlist[n // 2]
print("Median:", median)
```

    Median: 9



```python
# Mode
from collections import Counter

def find_mode(numbers):
    count = Counter(numbers)
    max_count = max(count.values())
    modes = [num for num, freq in count.items() if freq == max_count]
    
    return modes 
numbers = [3, 9, 9, 12, 15]
modes = find_mode(numbers)

if len(modes) == 1:
    print(f"The mode is {modes[0]}")
else:
    print("There are multiple modes:", modes)
```

    The mode is 9


### Popcorn Hack #1
Create your own algorithm using numbers to calculate a value.


```python
## Insert your popcorn hack here:
```

### Using algorithms to organize lists
There are two different ways in which data can be sorted.
- SELECTION SORT: Splits list into sorted & unsorted, takes the greatest or smallest value and puts it into sorted section, repeat
- INSERTION SORT: Splits list into sorted & unsorted, takes any value from list, inserts into correct position on sorted section, repeat


```python
### Selection sort example
list = [5, 2, 9, 3, 6]

for i in range(len(list)):
    min_index = i
    for j in range(i + 1, len(list)):
        if list[j] < list[min_index]:
            min_index = j
    list[i], list[min_index] = list[min_index], list[i]

print("Smallest to Biggest: ", list)
```

    Smallest to Biggest:  [2, 3, 5, 6, 9]


- i represents the position of the value in the list that is being evaluated and is the starting value that it chosen as the smallest
- j represents the position of a value outside the sorted position of the list
- If j is smaller than the original minimum index, the new minimum index will be the value of j
- Process of sortining most minimum number into index is continued until all numbers are in order from least to greatest


```python
### Insertion sort example
def insertion_sort(list):
    for i in range(1, len(list)):
        key = list[i]
        j = i - 1
        while j >= 0 and key < list[j]:
            list[j + 1] = list[j]
            j -= 1
        list[j + 1] = key

list = [64, 25, 12, 22, 11]
insertion_sort(list)
print("Smallest to Biggest:", list)
```

    Smallest to Biggest: [11, 12, 22, 25, 64]


- i is starting point of unsorted section
- key=list[i] is the current variable that is inserted into the sorted section, list[i] is the element at i index position
- j+1 portion places the key in the proper location of the sorted section

### Popcorn Hack #2
Use one of the two sorting methods to organize the following list from BIGGEST to SMALLEST:

[1,5,34,2,6,7,65,90,42]


```python
### Insert your popcorn hack here:
```

## Homework Hacks

### Homework Hack 1
Create your own algorithm using a loop until you reach a desired value. Be creative!


```python
# Example algorithm of loop

# Initialize a counter to keep track of the count of odd numbers
count = 0
# Loop through numbers from 1 to 10
for number in range(1, 11):  
 # Check if the number is odd
 if number % 2 != 0: 
        # Increment the count if the number is odd
        count += 1  

print("Count of odd numbers from 1 to 10:", count)
```

    Count of odd numbers from 1 to 10: 5



```python
## Insert your code here:
```

### Homework Hack 2
Write two different algorithms of your own choosing that reach the same end output. Refer to the examples in the lesson above for support.


```python
## Insert your code here:
```
