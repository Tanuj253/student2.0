---
toc: True
comments: True
layout: post
title: P5 Lists and search
description: Lesson for developing algorithms in Python
type: collab
author: Brandon, Ninaad, Aaron, Eshaan
courses: {'csp': {'week': 9}}
unit: 3
---

## LISTS:

A Python list is an ordered and changeable collection of data objects. Unlike an array, which can contain objects of a single type, a list can contain a mixture of objects.
They start from 0. (The 1st element would actually be the 0th element)

**List Functions**

**Creating a list:**

In order to create a list named "aList", type aList = [].
This creates an empty list.
A list with elements would look like this aList = [element1,element2,element3]

**Append:**

In Python, the append() method is used to add an element to the end of a list.
The element in the parenthesis is what is added to the list.


```python
# College Board Pseudo Code
aList ← []

USER_INPUT ← ("Enter an item you want (or 'q' to quit): ")

REPEAT UNTIL USER_INPUT ← q{
    APPEND (aList, USER_INPUT)
}

DISPLAY(aList)
```


```python
aList = []

while True:
    user_input = input("Enter an item you want (or 'q' to quit): ")
    
    if user_input.lower() == 'q':
        break
    
    aList.append(user_input)

print("Things You Want:", aList)
```

    Things You Want: ['1', 'item', '2']


**Accessing an element:**

In order to access a specific element from a list, you would put the element in []. For example, If I had to access the third element in the list aList, I would say "aList[2]". 


```python
# College Board Pseudo Code
aList ← []

USER_INPUT ← ("Enter an item you want (or 'q' to quit): ")

REPEAT UNTIL USER_INPUT ← q{
    APPEND (aList, USER_INPUT)
}

DISPLAY(aList[2])
```


```python
aList = []

while True:
    user_input = input("Enter an item you want (or 'q' to quit): ")
    
    if user_input.lower() == 'q':
        break
    
    aList.append(user_input)

print("The Second thing on your list is", aList[1])
print(aList)
```

    The Second thing on your list is 3
    ['2', '3', '6', 'hunt']


**Deleting an element:**

In order to delete an element, choose the element the same way you would access it but put "del" before it. For example, If I had to delete the third element in a list called aList, I would say "del aList[2]".


```python
# College Board Pseudo Code
aList ← []

USER_INPUT ← ("Enter an item you want (or 'q' to quit): ")

REPEAT UNTIL USER_INPUT ← q{
    APPEND (aList, USER_INPUT)
}

REMOVE (aList, 2)

DISPLAY(aList)
```


```python
aList = []

while True:
    user_input = input("Enter an item you want (or 'q' to quit): ")
    
    if user_input.lower() == 'q':
        break
    
    aList.append(user_input)

print("This is your list: ", aList)

del aList[1]

print("This is your new list: ", aList)
```

    This is your list:  ['5', '4', '4']
    This is your new list:  ['5', '4']


# Popcorn Hack #1

Create a list (with user input) using objects with different types. Remove the first element from the list and print the list. 


```python
# Code goes here:


```

**Assigning a value:**

To assign the values a list named bList to aList of one list to another you simply have to do aList = bList.

**Length of a list:**

To get the length of a list named aList, you just need to do len(aList). This gives you the number of elements in a list.


```python
aList = ["Yeezys","Yeezys"]
bList = ["No Yeezys"]

bList = aList
list_length = len(bList)

print("Things I want:", bList)
print("How many yeezys:",list_length)
```

    Things I want: ['Yeezys', 'Yeezys']
    How many yeezys: 2


For each item in a list:

If you want to do something to each item in a list, you will need to use this. an example of it is shown below.


```python
my_list = [1, 2, 3, 4, 5]
print("Original List:", my_list)

for i in range(len(my_list)):
    my_list[i] += 1

print("Modified List:", my_list)
print("Length of the list:", len(my_list))
```

    Original List: [1, 2, 3, 4, 5]
    Modified List: [2, 3, 4, 5, 6]
    Length of the list: 5


# Popcorn Hack #2

Create a new list that modifies the original list by multiplying each element by 2 and prints the length of the list.



```python
my_list = [1, 2, 3, 4, 5]
print("Original List:", my_list)

# Code goes here:
```

    Original List: [1, 2, 3, 4, 5]


# Search:

A search algorithm is an algorithm that retrieves information stored in a data structure or calculated in the search space of a problem domain, with either discrete or continuous values.

**Linear Search**

The most basic way of searching in which each element is searched in the order it appears in the list. The list doesn't need to be organized.

For the code given below, the algorithm compares each value to the previous value, and if the value is smaller than the previous value, that value becomes the new min. The algorithm continues until it reaches the end of the list. 


```python
nums[] ← [86, 75, 98, 100, 71, 65, 84]

min ← nums[0] 

FOR EACH score IN nums[] DO
    IF score < min THEN
        min ← score
    END IF
END FOR

OUTPUT "The minimum value in the list is:", min

```


```python
nums = [86, 75, 98, 100, 71, 65, 84]
print(nums)

min = nums[0]

for score in nums:
    if (score < min):
        min = score

print("The minimum value in the list is:", min)
```

    [86, 75, 98, 100, 71, 65, 84]
    The minimum value in the list is: 65


# Popcorn Hack #3

Create a linear search algorithm that determines the maximum value in a list.


```python
nums = [86, 75, 98, 100, 71, 65, 84]
print(nums)

# Code goes here:
```

**Binary Search**

A binary search, also known as half-interval search, logarithmic search, or binary chop is a search algorithm that finds the position of a target value within a sorted array.

How does Binary Search work:

     - Binary search works on sorted arrays. 

     - Binary search begins by comparing an element in the middle of the array with the target value. 

     - If the target value matches the element:

         - Its position in the array is returned and exit the program.

     - If the target value is less than the element:

         - The search continues in the lower half of the array. Go to Step-2

     - If the target value is greater than the element, the search continues in the upper half of the 
     array. Go to Step-2
     
     - Go back to Step-2



```python
# Returns index of x in arr if present, else -1
def binary_search(arr, low, high, x):
 
    # Check base case
    if high >= low:
 
        mid = (high + low) // 2
 
        # If element is present at the middle itself
        if arr[mid] == x:
            return mid
 
        # If element is smaller than mid, then it can only
        # be present in left subarray
        elif arr[mid] > x:
            return binary_search(arr, low, mid - 1, x)
 
        # Else the element can only be present in right subarray
        else:
            return binary_search(arr, mid + 1, high, x)
 
    else:
        # Element is not present in the array
        return -1
 
# Test array
arr = [ 2, 3, 4, 10, 40 ]
x = 10
 
# Function call
result = binary_search(arr, 0, len(arr)-1, x)
 
if result != -1:
    print("The Element is in index", str(result))
else:
    print("Element is not present in array")
```

    The Element is in index 3


#

# Big O Notation

Usually used to denote how much run time an algorithm takes or how much memory is used by an algorithm.

In our case we are using it to show how many tries it will take for a search algorithm to find something. 

Linear Search vs Binary Search big O notation:
    
     - Linear search complexity is denoted by O(n) as every element in the array is compared only once. 
    
         - In linear search, best-case complexity is O(1) where the element is found at the first index. 
    
         - Worst-case complexity is O(n) where the element is found at the last index or the element is not present in the array.
    
     - Binary search, best-case complexity is O(1) where the element is found at the middle index. The worst-case complexity is O(log base 2 n).

# Homework Hack 

Create a binary search algorithm that sorts for the value 25.


```python
nums = [12, 23, 25, 45, 47, 89, 91]

# Code goes here: 
```

<style> 
@import url('https://fonts.googleapis.com/css2?family=Roboto');
h1{ text-align: center; font-size: 50px; color: #0352fc; font-family: 'Roboto', serif;}
h2{ text-align: left; font-size: 18px; color: #0352fc;}
body{ text-align: left; font-size: 15px; font-family: 'Roboto', serif; background: black; }
</style>
