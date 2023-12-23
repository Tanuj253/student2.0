---
toc: True
comments: True
layout: post
title: P5 Simulation
description: Lesson for simulations in Python
type: collab
author: Ronit, Vance, Ashwin, Jared
courses: {'csp': {'week': 10}}
unit: 3
---

## Simulations 


> A simulation is the use of a computer software to represent the dynamic responses of one system by the behaviour of another system modeled after it. A simulation uses a mathematical descriptions, or models, of a real system in the form of a computer program.

![simulation](https://www.simscale.com/wp-content/uploads/2022/11/dron-quadcopter-simulation.png)

## College Board Essential Knowledge

> Simulation are absractions of more complex objects or phenomena for a specific purpose 

- Mimic Real World Events
- Allows investigation of phenomenons without contraints of the Real World
- Helps you draw accurate inferences

> Simulations utilize varying sets of values to reflect the changings states of a phenomenon

- simulations can simplfly things for functionality
- Simulations can contain bias from real world elements, that were chosen to be included or excluded

> Simulations work best when the real world experemnts are too impractical or time consuming. For example, simulating how different cars behave when they crash, would be much better than crashng actual cars in the real world, which would be expensive and dangerous.

<a href="https://ibb.co/f4jKcBY"><img src="https://i.ibb.co/NZck4Q6/simulations-vs-experiments.png" alt="simulations-vs-experiments" border="0"></a>


## Rolling the Dice

<a href="https://ibb.co/PGBhfPD"><img src="https://i.ibb.co/XxmsvKY/craps-rolling-seven-7.jpg" alt="craps-rolling-seven-7" border="0"></a>

> Simulating something like a dice roll in real life would require accounting for things like: weight, flaws in design, thrust, and gravity.
- KEEP IT SIMPLE! just use a random-number generator! Ignore minor causes of variablility

## Random

- "Random" is a built-in python function that allow the user to draw a random value from a set range.
- A Random Number Generator (RNG) is a common simulation that selects a random value from an array.
- The following code cell utilizes "random" to select a number from 1 to 100.


```python
#imports random module so we can use it in our code
import random

#sets variable random_number as a random number between 1 and 100
random_number = random.randint(1, 100)

#Printing out your random Number
print(random_number)
```

## More complex usage of "random"; Coin Toss Simulation


```python
import random
def flip_coin():
    return random.choice(["Heads", "Tails"])
def coin_flip_simulation(num_flips):
    heads_count = 0
    tails_count = 0
    for _ in range(num_flips):
        result = flip_coin()
        if result == "Heads":
            heads_count += 1
        else:
            tails_count += 1
    return heads_count, tails_count
if __name__ == "__main__":
    num_flips = 1000  #This is the number of coin flips you want to simulate
    heads, tails = coin_flip_simulation(num_flips)
    print("Number of Heads: "+ str(heads))
    print("Number of Tails: " + str(tails))
    print("Heads Probability: "+ str({heads / num_flips}))
    print("Tails Probability: "+ str({tails / num_flips}))
```

    Number of Heads: 501
    Number of Tails: 499
    Heads Probability: {0.501}
    Tails Probability: {0.499}


## Popcorn Hack #1

Utilize "random" to create a basic simulation of a rolling TWO dice. Print the sum of both dice rolls. Remember to practice good syntax when naming your variables. 


```python
import random

#Code, Code, Code
```

## Algorithms
>Simulations often utilize algorithms and equations to perform tasks because simulations don't always have the same output
- the output of a simulation depends on the input

>An algorithm is a finite sequence of instructions used to solve problems or perform computations. 
- commonly used alongside functions


## Example Algorithm in a function


```python
#Defining Function
def algorithm(input):
    
    #Manipulating input and preparing it for the output.  
    output = input+2
    
    #Return the output
    return output

#Call the Function to start the algorithm
algorithm(5)
    
```




    7



## Mathematics
- Math can also prove to be very useful in certain types of situations.
- Commonly used along with Algorithms when simulating various things

![math](https://pythontutorialhome.files.wordpress.com/2019/05/image-2.png)


## Popcorn Hack #2

Simulate how long an object will fall for using an algorithm, with user-inputed variables for height dropped. Use the following formula as a reference.

![gravity ](https://hepweb.ucsd.edu/ph110b/110b_notes/img272.png)

- t = time (output)
- h = height dropped from (input)
- g = constant (given)


```python
# Constant, Acceleration due to gravity (m/s^2)
G = 9.81 

def simulation(height_dropped):
    # Code Code Code
```

# Using Loops in Simulations

> For loops can also be used in simulations
- They can simulate events that repeat but don't always have the same output



```python
# Example For Loop

#Creating For Loop to repeat 4 times
for i in range(4):
    
    #Action that happens inside for loop
    print("This is run number: " + str(i))
    
```

    This is run number: 0
    This is run number: 1
    This is run number: 2
    This is run number: 3


## Popcorn Hack #3

You are gambling addict (sigma). 

Each session you roll 2 dice.

If your dice roll is greater than or equal to 9 you win the session.

If you win over 5 sessions, you win the jackpot.

Simulate your odds to predict if you will hit the jackpot (how many rounds did you win?) using a for loop and random.



```python
# Code Code Code
```

## BONUS POPCORN HACK
> Welcome to Flight Simulator! Your goal is to complete a Python program that simulates a flight We've set up some initial values for altitude, speed, and fuel. Your task is to update these values to make the flight more realistic.

- Your mission:

1. Use random changes to simulate altitude, speed, and fuel changes.
2. Keep the flight going until it reaches 10,000 feet or runs out of fuel.
3. Make sure altitude, speed, and fuel remain realistic.


```python
import random

# Initial parameters
altitude = 0
speed = 0
fuel = 100

print("Welcome to Flight Simulator!")

# Code Code Code
```

## QUIZ TIME

- Quick true or false quiz, whoever answers this correctly(raise your hand) gets a piece of gum or a dinero. 
<hr>

> T or F    
- A simulation will always have the same result.
> T or F    
- A simulation investigates a phenomenom without real-world constraints of time, money, or safety.
> T or F    
- A simulation has results which are more accurate than an experiment,
> T or F    
- A simulation can model real-worl events that are not practical for experiments

## HOMEWORK HACK #1 

First finish Popcorn Hack #3. Expand the simulation to involve your own money.

starting money: $100

(Dice Roll <= 3) &rarr; lose $70

( 6> Dice Roll >3) &rarr; lose $40

( 9> Dice Roll >=6) &rarr; win $20

( Dice Roll>= 9 + Session Win) &rarr; win $50

Jackpot &rarr; win $100


```python
# Code Code Code
```

## HOMEWORK HACK #2

> Given initial parameters for a car simulation, including its initial speed, acceleration rate, deceleration rate, maximum speed, and initial distance, write a  program to simulate the car's journey and determine the final speed, distance covered, and time taken before it either covers 1000 meters or slows down to below 5 m/s?


```python
# Initial parameters
speed = 0  # Initial speed
acceleration = 2  # Acceleration rate in m/s^2
deceleration = 1  # Deceleration rate in m/s^2
max_speed = 60  # Maximum speed in m/s
distance = 0  # Initial distance
time = 0  # Initial time

#Code Code Code
```
