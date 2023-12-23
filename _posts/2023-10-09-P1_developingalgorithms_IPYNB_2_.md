---
toc: True
comments: True
layout: post
title: P1 Developing Algorithms
description: Lesson for developing algorithms in Python
type: collab
author: Tarun, Tejas, Tanuj, Chris
courses: {'csp': {'week': 9}}
unit: 3
---

# Developing Algorithms


## How to do simple operators in JS

Use the "+" operator to add two numbers in JavaScript

Use the "-" operator to subtract two numbers in JavaScript

Use the "*" operator to multiply two numbers in JavaScript

Use the "/" operator to divide two numbers in JavaScript

Use the "%" operator to get the remainder of a division operation in JavaScript

## Iterating

Iteration is going through a set of laws or objectives for the loops to complete. This allows for the desired objective to be completed.

## Randomization 

Raandomization is used to generate a random number. For example, RANDOM(a,b) would generate any number from a to b. Each result from randomization is equally likely to occur. Each execution might produce a different result.


## Popcorn hack #1: How to find the mean in JS


```python
%%js


// define an array
var arry = [4,62,3,38];
// define a function to calculate the mean using an inputted array
function calculateAverageOfArray(array) {
    var total = 0;
    var count = 0;
// add all the values of the array
    jQuery.each(arry, function(index, value) {
        total += value;
        count++;
    });
// take the sum of the array and divide it by the length of the array
    return total / count;
}

console.log("The average of your list is " + calculateAverageOfArray(arry));
```


    <IPython.core.display.Javascript object>


## How to calculate the median


```python
%%js

// define the function median that finds the median of an array numbers
function median(numbers) {
    // sort the list numbers
    const sorted = Array.from(numbers).sort((a, b) => a - b);
    
    // find the index of the middle element in the array
    const middle = Math.floor(sorted.length / 2);

    // see if the array has an even length
    if (sorted.length % 2 === 0) {
        // if it's even then calculate the average of the middle two values
        return (sorted[middle - 1] + sorted[middle]) / 2;
    }

    // if odd return the middle value
    return sorted[middle];
}

// run median with the inputted array
console.log(median([1, 3, 8, 2, 6]));

```


    <IPython.core.display.Javascript object>


## How to calculate the mode in JS


```python
%%js


// Create a function that finds the mode of an array
var mode = function mode(arr) {
    // use the function "reduce" to go through every item in the list while collecting the data
    return arr.reduce(function(current, item) {
        // update the variable "val" with the frequency of "item" and increase it by 1.
        var val = current.numMapping[item] = (current.numMapping[item] || 0) + 1;
        
        // check if the mode of "item" is greater than the greatest mode so far
        if (val > current.greatestFreq) {
            // if it is then update the mode
            current.mode = item;
            current.greatestFreq = val;
        }
        
        // return the updated value of "current" then repeat the process
        return current;
    }, {mode: null, greatestFreq: -Infinity, numMapping: {}}).mode;
};

// run the mode function with an inputted array and print it to the console
console.log(mode([6, 6, 7, 7, 7, 8, 8, 8, 8, 8, 9, 10,7,7,7,7,7,7,7,7]));

```


    <IPython.core.display.Javascript object>


## One way to sort a list


```python


numList = [43, 2, 6, 2, 96, 32]
numList.sort()
print(numList)
```

    [2, 2, 6, 32, 43, 96]


## Another way to sort a list


```python
def bubbleSort(arr):
    n = len(arr)
    swapped = False  # a boolean to keep track of whether a swap has occurred.
    for i in range(n-1):  # a loop to go iterate through the entire array
        for j in range(0, n-i-1):  # loop to compare adjacent elements
            if arr[j] > arr[j + 1]:
                # if current element is greater than the next element then swap them
                swapped = True
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
        if not swapped:
            # if no swaps happen then the array is sorted
            return
arr = [64, 34, 25, 12, 22, 11, 90]
bubbleSort(arr)  # call the function to sort the array
print("Sorted array is:")
for i in range(len(arr)):
    print("%d" % arr[i], end=" ")
```

    Sorted array is:
    11 12 22 25 34 64 90 

## What does this code do?


```python
import random


randomNumber = random.randint(1, 100)

attempts = 0


def startGame():
    print("Welcome to the Number Guessing Game!")
    print("I'm thinking of a number between 1 and 100.")
    print("Try to guess the number.")

    askForGuess()


def askForGuess():
    input_str = input("Enter your guess (between 1 and 100): ")
    try:
        guessedNumber = int(input_str)
        checkGuess(guessedNumber)
    except ValueError:
        print("Please enter a valid number.")
        askForGuess()


def checkGuess(guess):
    global attempts
    if guess < randomNumber:
        
        print("Try a higher number.")
        attempts += 1
        askForGuess()
    elif guess > randomNumber:
        
        print("Try a lower number.")
        attempts += 1
        askForGuess()
    else:
        
        attempts += 1
        print(f"Congratulations! You guessed the number {randomNumber} in {attempts} attempts.")
startGame()

```

## Homework Hacks: 

* Find the median test grade of a class
* Create a simple game using random numbers
