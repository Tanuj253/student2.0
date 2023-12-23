---
toc: True
comments: True
layout: post
title: P4 Lists and search
description: Lesson for developing algorithms in Python
type: collab
author: Tanvi, Anagha, Drishya, Miheer
courses: {'csp': {'week': 9}}
unit: 3
---

## 3.10-3.11 Lists and Searches

## Basic Operations


```python
#Defining the List
aList= [1,4,2,6,7,3,]
# Accessing Elements in a list
print(aList[1])


#Storing Element at an Index to a variable
Element = aList[3]
print(Element)


#Setting an Element at an Index to a variable
element = 4
aList[5]= element
print(aList[5])

#Insert a value at a certain index
aList.insert(3, 10)
print(aList[3])

#Adding another value to the list (append)
aList.append(5)
print(aList)

#Removing a value from the List at a specific Index
aList.remove(2)
print(aList)

#Accessing the Length of a list
print(len(aList))


```

    4
    6
    4
    10
    [1, 4, 2, 10, 6, 7, 4, 5]
    [1, 4, 10, 6, 7, 4, 5]
    7


## PopCorn hack #1

- Create and define your own list and insert a value at a certain index


```python
#Enter code here
```

## Finding the Minimum Value in a List



```python
nums = [30, 45, 95, 56, 73, 98, 25]
min_value = nums[4]  # Start with the first element as the minimum

for score in nums:
    if score < min_value:
            min_value = score


print(min_value)  # Display the minimum value

```

    25


## PopCorn hack #2

- Using the code above try to find the maximum value in the list


```python
#Enter Code here
```

## Sum of Even Numbers in a list solution



```python
#Defining numbers in list
nums = [7, 5, 10, 6, 9, 4, 3, 12]
sum_even = sum([score for score in nums if score % 2 == 0])  #Using for loop to work through each number in the list and using mod to interpret if any numbers have a remainder of 0
print(sum_even)
```

    32


## Determining Outputs for code Segments with Length function



```python
#Defining the list
words = ['old', 'computer', 'science', 'far', 'potato']
new_words = []
for word in words:
    if len(word) != 3: #if the length of the word is not equal to three
        new_words.append(word)  #adds it to a new list if the length is not equal to three

print(new_words)



    
    
    
     


```

    ['computer', 'science', 'potato']


## If Else Statements with Lists


## Performing a Binary Search
- index: organizing the data by assigning a reference value to each element
- Put the number is order either ascending or descending
- Search starts with middle number first which is found by adding the highest and lowest index number and dividing it by 2
- This divides the range by 2
- Repeat this process by shrinking the range each time till the desired target is found
- Every time the process is repeated and leads to a target it is considered a comparison



## Performing a Sequential Search
- Each element in a list is examined in the order of the first element till the desired target
- Order doesn’t matter

## Homework Hack 1
- What will be the middle index number in a Binary Search given the set of number: 3, 6, 12, 14, 50, 53
- Which of the following lists can a Binary Search be used to find the desired target
- This is a multiple choice question


```python
fruits= ['Apple', 'Carrot', 'Mango', 'Peach']
A= [1, 3, 5, 8, 9, ]
B= [6, -1, 5, 12, 8]

#Type your choices below


```

## Homework Hack 2
- Given a provided list perform two of the basic operations


```python
yourlist = [8,10,35,39,49,52]

#type out your answers below and run the code and include output in screenshot
```
