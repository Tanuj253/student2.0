---
toc: True
comments: True
layout: post
title: P5 Data Abstraction
description: Period 5 Student led teaching on Data Abstraction
type: collab
courses: {'csp': {'week': 7}}
author: Nandan, Arnav, Torin, Remy
---

## 3.1 Variables And Assignments

Variables are considered abstraction as they assign a value to another value. In other words, they basically describe values to each other. Variables are important as it can be used in many different uses such as they can store useful information, especially if you have a long lines of code. Variables are mostly used to store data such as numbers (we are going to go over integers today), booleans, lists, strings and dictionaries.  

> Note:
 A variable needs to have a proper name, cannot have spaces inside of them and needs to be reasonable.

 For example:

 * highScore is a better and shorter way of saying highestScoreInTheGame

 * firstName is more specific than n

 * instead of is raining (because it has spaced) israining or is_it_raining is a better way of looking at the variable

 * phonenumber is better than saying 555-number (it just looks neater to look at)


```python
firstName = "Nandan" # this is a string
print(firstName)

highScore = 33 # this is an integer
print(highScore)
```

    Nandan
    33


<p style="color:DodgerBlue;">Popcorn Hack#1 - Make your own variable either with a number or a string and print it out</p>

<h3>Data Types:</h3>
    
<p>Integer: A whole number</p>
<p>Boolean: True or False based on operators</p>
<p>String: Anything that contains "{text is stored in here}"</p>



```python
### BOOLEAN EXAMPLES
age = 16  # Here is the number example
print(bool(age))  

print(age<8)

PASS_POINTS = 75
FAIL_POINTS = 50

if PASS_POINTS < FAIL_POINTS:
    passed = True
else:
    passed = False

if passed:
    print("Student has passed")
else:
    print("Student failed")


## STRING EXAMPLE

age = "16"
print(age)


```

    True
    False
    Student failed
    16


<p style="color:DodgerBlue;">Popcorn Hack #2 - Use the variable types and print all the different types of them, boolean, string, and integer.</p>

## 3.1 Variables And Assignments


Assignments and variables are fundamental ideas in computer programming. They give programmers the ability to manage, store, and alter data. In the realm of programming and computer science, knowing how to declare and assign values to variables is an essential ability.



```python
currentName = "Arnav"
print(currentName)

currentName = "Nandan" # Because this variable is declared second, even though it was already declared, it will print the most recent declaration
print(currentName)

bigNumber = 10000
isWednesday = True
firstName = "Arnav Nadar"

print(str(bigNumber) + "  "  + str(isWednesday) + "  " + str(firstName))
```

    Arnav
    Nandan



```python
num1 = 25
num2 = 15
num3 = 30
num2 = num3 
num3 = num1
num1 = num2
print(num1)
print(num2)
print(num3)
```

    30
    30
    25


<p style="color:DodgerBlue;">Popcorn Hack #3 - Use the variable types and the variable changing and create a small program that incorporates the changing of variables</p>

## 3.2 Data Abstraction - Lists & Strings
<p>
Lists and strings are a simple method of data abstraction in which a number of values (list elements or characters) are stored in a single variable. Each value in a list or string can be referred to with its natural-number index.
</p>
<p>
A string is simply a sequence of characters; letters, numbers, special characters, etc.
</p>
<p>
A list is an ordered sequence of variables (elements), each of which contains some data value. The elements need not be all the same data type. 

</p>


```python
#Each element in a list/string is referred to with a natural-number index.
#FOR THE PURPOSES OF THE AP EXAM ONLY, the indexes start at 1.
#IN PYTHON (and most other languages), the indexes start at 0.

#Example string
name = "llanfairpwllgwyngyllgogerychwyrndrobwllllantysiliogogogoch"

#Returning the 18th character in the string ('y') - Remember that Python indices start at 0!
print(name[17])

#Example list
rn_list = ["Ramskoeldia consimilis", 187.52, "e", True, 0, None, "987654", [1,5,8]]

#Returning the 1st, 4th, and 7th elements in the list
print(rn_list[0])
print(rn_list[3])
print(rn_list[6])
```

    y
    Ramskoeldia consimilis
    True
    987654


<p style="color:DodgerBlue;">Popcorn Hack #4 - Make your own list based on your intersts and access the items in the list according to your interest</p>

## 3.2 Data Abstraction
> Data abstraction is the process of hiding certain details and showing only essential information to the user. Abstraction can be achieved with either abstract classes or interfaces (which you will learn more about in the next chapter). You can also use the abstract keyword to create abstract methods that must be overridden in the subclass.You do this by putting information in lists, dictionaries, and objects. You can also create your own data types using classes. This is called data abstraction. Here is an example of data abstraction: 

```python

 def __init__(self, name, age):
    self.name = name
    self.age = age

 def myfunc(self):
    print("Hello my name is " + self.name)
```
This is a class called Person. It takes in two parameters, name and age. It also has a method called myfunc that prints out a message with the name of the person and their age. Lets see this as a list. Lets make a function that takes in a list of people and prints out their names and ages. 

```python
def print_people(people):
    for person in people:
        print(person.name + " is " + str(person.age) + " years old") // this will print out the key and the value
```
This function takes in a list of people and prints out their names and ages. Lets see this as a dictionary. Lets make a function that takes in a dictionary of people and prints out their names and ages. First lets make the dictionary. 

```python
people = {
    "John": 32,
    "Rob": 23,
    "Amanda": 19
}
```
Then we can make the function. 

```python
people = {
    "John": 32,
    "Rob": 23,
    "Amanda": 19
} // this is the dictionary
def print_people(people): // this is the function
    for person in people: // this is the for loop
        print(person + " is " + str(people[person]) + " years old") // this will print out the key and the value
```
 
# Converting string to and from JSON
First we need to import the json module. Then we can use the json.dumps() function to convert a string to JSON. We can also use the json.loads() function to convert JSON to a string. Here is an example of converting a string to JSON:

```python
import json
dict = '{"name": "John", "age": 30, "city": "New York"}'
json_obj = json.dumps(dict)
print(json_obj)
```

# Hacks
> - You can use the type() function to find out what type of data type a variable is as shown below:
```python
x = 5
print(type(x))
```
> - You can use the isinstance() function to check if a variable is a certain data type as shown below:
```python
x = 5
print(isinstance(x, int))
```
> - You can use the dir() function to find out what methods an object has as shown below:
```python
x = 5
print(dir(x))
```

# Homework
> - Create a class called Person with the attributes name and age. Then create a function called print_people that takes in a list of people and prints out their names and ages.
> - Create a function that takes in a dictionary of people and their ages and prints out the name of the oldest person.

<h4>JSON</h4>

JSON is used to represent structured data, especially lists in JavaScript. However you can also use this in python as well. 

# Converting string to and from JSON
First we need to import the json module. Then we can use the json.dumps() function to convert a string to JSON. We can also use the json.loads() function to convert JSON to a string. Here is an example of converting a string to JSON:


```python

import json
list = ["Torin", "Remy", "Nandan", "Arnav"]
json_obj = json.dumps(list)
print(json_obj)
```

    ["Torin", "Remy", "Nandan", "Arnav"]


<p style="color:DodgerBlue;">Popcorn Hack #5 - Try using some of the lists in order to assign it to variables many times. Do JSON with a different list as well. </p>

## Hack 1 
Identify the correct variable types for each of the practice problems
* String
* Boolean
* Integer
* List




```python
secretNumber = 15
print(secretNumber)

food = "Pizza"
print(food)

names = ["Nandan", "Arnav", "Torin", "Remy"]
print(names)

IamCool = True

print(IamCool)

##Bonus Problem:

names_2 = {
    "Nandan": "TeamMate1",
    "Arnav": "TeamMate2",
    "Torin": "TeamMate3",
    "Remy": "TeamMate4",
}

print(names_2)


```

    15
    Pizza
    ['Nandan', 'Arnav', 'Torin', 'Remy']
    True
    {'Nandan': 'TeamMate1', 'Arnav': 'TeamMate2', 'Torin': 'TeamMate3', 'Remy': 'TeamMate4'}


## Hack 2

Create a program of interest using the variable types, lists, and assignment operators that have been discussed in the lesson. This could be anything, use what you learned and are interested in to create a small program showcasing what you've learnt so far. 
