---
toc: True
comments: True
layout: post
title: P4 Procedures
description: Lesson for developing procedures in Python
type: collab
author: Howie, Will, Nihar, Jayden
courses: {'csp': {'week': 10}}
unit: 3
---

## 3.12-3.13 Developing Procedures

## What is a procedure?

A procedure is a named group of programming instructions

2 Main Types of Procedures:
- Procedure that returns a value of data
- Procedure that executes a block of statements

## Learning Objective:
Be able to look at code and determine the purpose, and be able to write a procedure to manage complexity of program. understand the following terms and what they look like as well as how to use them in code

- Procedure Parameters: variables that are defined within the procedure's parentheses and serve as placeholders for values that can be passed to the procedure when it is called, allowing the procedure to operate on different data or values each time it is executed.
- Procedure Algorithms: a step-by-step set of instructions that defines a specific task or operation to be performed within a computer program
- Procedure Return Values: data or results that a procedure can send back to the part of the program that called it, allowing the procedure to provide information or perform a specific task and then share the outcome with the rest of the program.

## Name, Parameters, Algorithm, Return Values, and Calling a Procedure


```python
## Example of Procedure that Updates A Quiz Grade
def updatequiz(currentgrade, quizgrade):
    if quizgrade > currentgrade:
        currentgrade = quizgrade
    return currentgrade

currentgrade = 75
quizgrade = 88
currentgrade = updatequiz(currentgrade, quizgrade)
print(currentgrade)
```

    88


## Popcorn Hack #1:
Identify the name of the procedure below, tell us the purpose and parameters of the procedure and identify the algorithm return value:

- Name:
- Parameters:
- Algorithm (paste lines): 
- Return Value: 
- Calling Procedure Line


```python
def updateweather(currentweather, weather):
    if currentweather> weather:
        weather = currentweather
        print("today is warmer than yesterday")
    else:
        print("today is colder than yesterday")
    return currentgrade

currentweather = 71
weather = 66
currentgrade = updateweather(currentweather, weather)
print("the temperature right now is", currentweather, "degrees")
```

    today is warmer than yesterday
    the temperature right now is 71 degrees


## Costructing Classes for Names & Data with Procedures


```python
# Class Construction
class Short:
    name = ""
    height = 0

class Tall:
    name = ""
    height = 0

# Procedure to Classify as Short or Tall
def classify_person(name, height):
    if height < 70:
        short_person = Short()
        short_person.name = name
        return short_person
    else:
        tall_person = Tall()
        tall_person.name = name
        return tall_person
```

Class Definitions: The code defines two classes, "Short" and "Tall," each having two attributes: name and height. These attributes can be used to store the name and height of individuals.

Classification Procedure: The classify_person function takes two parameters: name and height. Depending on the provided height, it creates an instance of either the "Short" or "Tall" class. It sets the name attribute for the person and returns the corresponding instance.

## Popcorn Hack #2:
Use the example above to use a procedure to create two classes of your choosing. Create at least 2 objects and class them with your procedure


```python
# Popcorn Hack 2
```

## Calling Methods of an Object w/ Procedures


```python
# Creating Classes
class Short:
    name = ""
    height = 0

class Tall:
    name = ""
    height = 0

#Procedure to classify as short or tall
def classify_height(name, height):
    if height < 70:
        short_person = Short()
        short_person.name = name
        return short_person
    else:
        tall_person = Tall()
        tall_person.name = name
        return tall_person

# Create objects and classify them as short or tall
person1 = classify_height("Nihar", 70)
person2 = classify_height("Will", 76)
person3 = classify_height("Jayden", 75)
person4 = classify_height("Howie", 70)

# Display results for all four objects
for person in [person1, person2, person3, person4]:
    print(f"{person.name} is {'Short' if person is Short else 'Tall'}.")


```

    Nihar is Tall.
    Will is Tall.
    Jayden is Tall.
    Howie is Tall.


## HW Hacks!!
1. Create a procedure that replaces a value with a new one (ex. update height)
2. Create a procedure that constructs classes of your choosing and create at least 4 objects to be sorted into your classes. Call your procedure to display your objects.


```python
# HW Hack 1
```


```python
# HW Hack 2
```
