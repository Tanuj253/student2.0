---
toc: True
comments: True
layout: notebook
title: Alien World Project Plan
description: The concept of making an Alien World simply was created by looking at assets that were publicly available.  The idea is to follow student provided Mario Game, but try to customize background and actions to be focused on an Alien World.  The Theme of the game is CompSci education, loops, objects, and Monkeys/Dogs as space aliens.
type: hacks
courses: {'compsci': {'week': 0}}
---

## Backgound
Over my Computer Science years I have found that it is a lot more interesting to code if you start with something fun.   Loops are kind of boring, but if you add animation, alien planet, and monkeys what is not to like.

### Initial concepts: 

- Mr M (Product Owner, Scrum Master)
    - Introduce basic concepts to non coder, even younger children
    - Make game easy, instead of start over continue but require more events for success
    - Use features like collision, role play, gravity, squish to help in countdown
    - Try to integrate coding concepts like Object Oriented Programming,into game

### Advanced Concepts

- Quinn. Ideas from CSA friend Quinn
    In a distant galaxy, far, far away, there was a world known as "Codeonia." In this realm, software engineers and programmers lived harmoniously, creating and coding various applications for the universe. Among the inhabitants of Codeonia was Java the Hut, a wise and experienced programmer known for his mastery of programming languages.
    One day, a group of mischievous bugs and glitches threatened to plunge Codeonia into chaos. These digital nuisances were led by Darth Compiler, a formidable dark coder who sought to corrupt the code of the entire galaxy.

    - Monkeys (Good and Bad)
        - C++3PO
        - Darth Compiler
        - Ewak exception handle
        - R2-D-Arrays
        - Boba Fetch API

    - More worlds
    - Jetpack
    - New characters
        - Space Jellyfish
        - Robots
        - Aliens

    - Enemies have different abilities
    - Go between planets for levels
    - Zero gravity level
    - Astroid level
    - Star power up
    - Black hole

- Mr Lopez
    - Surfaces with different properties
        - Bouncy
        - Sticky
        - Damaging

    - Create obstacles or challenges that require players to use conditional statements (e.g., if-else) to make decisions, such as determining if a door is locked or open based on collected keys.
    - Use abilities by calling functions.
    - Different resources require different variables which we show the player.
    - Introduce the concept of objects and classes by having players interact with different spaceship types, each with its own properties and behaviors

- Rohan
  - Maybe add some competitive aspects to the game --> who completed the course in fastest time
  - Could have Monkey vs Dog competition
     - Each competitor chooses one and then they can race against each other
     - Could also emulate the "space race" that was present during cold war
  - Could use HTML canvas in order to store characters
  - Natural enemies such as winds, requisite power to reach space, etc. can be combined with physical enemies
  - Different power ups you could buy based on your completion of previous levels/things that have been collected
     - Would need to store these per player across sessions / levels
  - Graphic for swithing between planets
  - End goal could be to "liberate" the universe from some big enemy
  - Local storage or cloud database can be used to store user data
  - Can have different questions you would have to answer while in a level


## Outline of Scenes

Scene Design will be an ongoing process, as we enhance game play it is expected to have new ideas.  By splitting game up into parts it provides unique development opportunities to each individual.

1. Game Intro (Map design of Game)
2. Identity and Force/Power Design
3. Game Play, start with different options  Dog Sprite, Monkey Sprite, Mario Sprite
4. Note, each level could start with Character or Background and build up as code progresses

```
Game Intro   Identity       Force/Power     Journey
|-------|    |---------|    |---------|    |----------|
| In a  |    | Setup 1 |    | Setup 2 |    | Game Play|
| galaxy| -> |  Pick   | -> |  Pick   | -> | Selection| -> Game Entry 
| ....  |    |Character|    |  Power  |    | Portal   |
|-------|    |---------|    |---------|    |----------|
```

### Intro Screen

Option at Intro Screen to begin game, play codeOnia intro, go to Identity, Force/Power screens.

- A codeOnia Intro, long version with character introductions
    - Quick exit or short version
- Repeating Gif of Game Play


### Identity

Option at Intro Screen to establish a character Dog, Monkey (aka Sprite)

- C++3PO would be guide character in scene.  
- This step would introduce 'Objects' and 'Variables'


### Force/Power

Option at Intro Screen to add powers to character to help them win the game

- R-2Darray-2 would be guide character in scene
- Force/Power would introduce 
  - enhanced Data Structure to hold powers
  - methods in object associated with powers


### Game Entry
The entry to the game would introduce 'Conditions`

- Darth-If-Vader would be nemisis or character that inflicts obstacles in game
- Based off of obstacles, character, and powers game play begins

### Game One Idea
Escape the loop.
- Use non-animated character in this game, a space monkey.
- Create a looping and consistent experience in game.  Keep game timer and loop timer.
- Objective of game is to reduce "code counter" or "loop" from 10 to zero
  - Jump on top of NPC platform characters for point reduction, refactor Goomba code for more appropriate character
  - Add points to loop counter for collision with NPC character
  - Have sticky or water hazards that delay progress, have Power/Force to free from trap 
  - Have a condition open up when counter gets to zero, use Power/Force to travel to and go through a portal to exit game level
- Follow Mario code guildelines for ...
  - platform
  - rivers or sticky traps
  - replace gombas with more appropriate character
  
### Game Two Idea (Thoughts to be determined)
Escape the nested loops.
- Use dog as character and animation. 
- Start force/power features turning Dog into ball to allow horizontal collision for reward
- Start using jetpack experience to interact with objects above platform for super inner loop reduction.

## Game Three Idea  (Thoughts to be determined)
Defeat code infections from Dart Compiler.  Have ways to bypass level one and level two may inserting coding.
- Use character from selections in Identity / Force Power obtained from local storage.
- 

## GitHub Pages Project
[alienWorld repository](https://github.com/jm1021/alienWorld)

### Make a GitHub Pages Repository
> Goto GitHub [alienWorld repository](https://github.com/jm1021/alienWorld)

- Clone to your machine and get started with VSCode

```bash
$ cd vscode # work in vscode directory for consistency
$ git clone https://github.com/jm1021/alienWorld.git 
$ cd alienWorld 
$ code . # open VSCode in project directory
```

## Anatomy of Alien World
The GitHub Pages repository is for Game and History of project.

- index.html is the code to start the game
- assets/js/*.js are Objects of the Game
- _notebooks/Plan.ipynb contains this plan
_ _posts/* contains daily meetings, hacks, and early code examples

### Game Code and OOP
Most game code you find on the internet is built in a single file.  However, once you reach a certain amount of complexity this becomes difficult to manage.  Thus, this project is broken into objects. 

In the early days of this project, it was easy to see repetition in code (draw() and update()) as well as grouping of attributes and classes using "isa" and "hasa" relationships.  This picture is a talking point.  A more active verion of this picture is the "classDiagram.drawio" in this repository.

![drawio classes]({{site.baseurl}}/images/draw_io_example.png)

### Migrating to OOP - Background

To migrate to this project it can begin by changingthe background to your preference.  This code starts in index.htmol and simply change the image in the frontmatter.

Swap beteen alien_planet.jpg and alien_plant2.jpg

- index.html simply change yml to point to your preference

```yml
image: /images/alien_planet.jpg
```

Rework ADJUST in Background.js to crop out imperfections in image

- assets/js/alienWorld/Background.js change adjust if you want to reduce height of image

```js
size() {
        // Update canvas size
        const ADJUST = 1 // visual layer adjust; alien_planet.jpg: 1.42, try 1 for others
        // ... code not shown xxx
}
```

