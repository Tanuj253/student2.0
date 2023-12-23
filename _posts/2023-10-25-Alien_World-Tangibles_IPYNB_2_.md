---
hide: True
toc: True
layout: notebook
title: Individual Review JM
description: Recap of key events and learnings
type: tangibles
courses: {'compsci': {'week': 2}}
---

## Issue Recap
During the project, I have tried to keep issues to track work and progress.  

[Online Issues](https://github.com/jm1021/alienWorld/issues/created_by/jm1021) and suumary below...

- [Restructure of Index Files](https://github.com/jm1021/alienWorld/issues/8).   This highlights problems addressed and before and after changes.  Additionally, a big portion of this review document discussess index.html and some of its key challenges.
- [Resize Images](https://github.com/jm1021/alienWorld/issues/7).   This is a quick discussion of how I resized my images.   They were large and it was causing characters to slow down.  After resize, things are faster and my eye can not tell the difference.
- [Dynamic Screen Resizing](https://github.com/jm1021/alienWorld/issues/6).  Tho figure out how to resize background and characters took a lot of time and a few lines of code that are show.
- [ClassIO Diagram](https://github.com/jm1021/alienWorld/issues/3).  This document has worked well to determine objects.  Though, there has been some additions as project continues.  The laregest is managing Game Objects in GameObjectArray.  This is shown in use in index.html in GameLoop and Resizing.
- [Migrating to OOP](https://github.com/jm1021/alienWorld/issues/9).  This is a sample and test of scaling index.html to only do background.  This could be a great starting point to using code from this project to migrate your own system.

## Overview of game control - index.html
Main game page (aka Game Controller).  This has been an area of learning and rework.

### HTML Elements
HTML canvas container ```html (<div id="canvasContainer"></div>```.  This is location where canvas elements are added to the DOM (background, dog, monkey, ...).  

Minima Layout. It is very important to know how a minima site works and layout files provided by a Theme.  Everyting we do in the game is considered content in the minima layout. Thus, the game is content between GitHub Pages "header" and "footer".
   - [Minima Readme](https://github.com/jekyll/minima/blob/master/README.md)
   - [Minima Layout](https://github.com/jekyll/minima/blob/master/_layouts/base.html)

```html
  <!-- include head.html -->

  <body>

    <!-- include header.html -->

    <main class="page-content" aria-label="Content">
      <div class="wrapper">
        <!-- content -->
      </div>
    </main>

    <!-- include footer.html -->

  </body>
```

This project inherits style from _sass/minima directory.  Developers should start in this directory before adding CSS already defined in GitHub Pages.

- custom-styles.scss is file used to customize style.  Developers start here to differentiate style.

```scss
@import "minima/Nighthawkpages-dracula-highlight"; 
@import "minima/dark-mode"; // Dracula Highlight recommended for dark mode
```

- dark-mode.scss is file that provides inverted background feature for anyting inside a "canvas" tag, this is intended feature to go with idea of being on an alien planet

```scss
:root {
    --default-canvas-filter: invert(100%);
}
/* more code not shown */
canvas {
    filter: var(--default-canvas-filter);
}
```



### JavaScript Module and Imports

In any system of size you will import code from other files. 
- Observe script tag becomes a "module" in order to support imports
- The import commands reference code from files that contain the Class definitions that will be used to define "Game Objects". 
- The imports are the key Classes in game that are used to create objects.

```html
<script type="module">
    import GameEnv from '{{site.baseurl}}/assets/js/alienWorld/GameEnv.js';
    import Background from '{{site.baseurl}}/assets/js/alienWorld/Background.js';
    import Character from '{{site.baseurl}}/assets/js/alienWorld/Character.js';
    import { initDog } from '{{site.baseurl}}/assets/js/alienWorld/CharacterDog.js';
    import {initMonkey} from '{{site.baseurl}}/assets/js/alienWorld/CharacterMonkey.js';

    // more code not shown
</script>

```

### Game Constants

A game can have constants and static objects that are used to control game behavior.

```js
// Setup Globals
    GameEnv.gameSpeed = 2;
    GameEnv.gravity = 3;
```

### Game Assets

A game can have several assets.  In this game we are using images for backgrounds and sprite images for characters.   These are loaded into DOM prior to moving forward to game logic.

```js
// Prepare HTML with Background Canvas
const backgroundCanvas = document.createElement("canvas");
backgroundCanvas.id = "background";
document.querySelector("#canvasContainer").appendChild(backgroundCanvas);
// Background object
const backgroundSpeedRatio = 0.2
new Background(backgroundCanvas, backgroundImg, backgroundSpeedRatio);

// Prepare HTML with Platform Canvas
const platformCanvas = document.createElement("canvas");
platformCanvas.id = "platform";
document.querySelector("#canvasContainer").appendChild(platformCanvas);
// Platform object
const platformSpeedRatio = 0.2;
initPlatform(platformCanvas, platformImg, platformSpeedRatio);
//...
```



### Game Loop

This code is the heartbeat of the game.  All the Game Objects are refreshed.
- gameLoop is a function that is activated when page loads
- requestAnimationFrame recursively calls gameLoop according to refresh rate of computer screen

```js
// Game loop
function gameLoop() {
    for (var gameObj of GameObject.gameObjectArray){
        gameObj.update();
        gameObj.draw();
    }
    requestAnimationFrame(gameLoop);  // cycle game, aka recursion
}
```

### Event Listeners

The start file, in this case the index.html, often will often act on events produced by user.  

- Resize.  A common destop action is resize of window.   This will impact many Game Object as they are in proporition to size of window.

```js
// Window resize
window.addEventListener('resize', function () {
    GameEnv.setGameEnv();  // Update GameEnv dimensions

    // Call the sizing method on all game objects
    for (var gameObj of GameObject.gameObjectArray){
        gameObj.size();
    }
});

// Toggle "canvas filter property" between alien and normal
var isFilterEnabled = true;
const defaultFilter = getComputedStyle(document.documentElement).getPropertyValue('--default-canvas-filter');
toggleCanvasEffect.addEventListener("click", function () {
    for (var gameObj of GameObject.gameObjectArray){
        if (isFilterEnabled) {  // toggle off
            gameObj.canvas.style.filter = "none";  // remove filter
        } else { // toggle on
            gameObj.canvas.style.filter = defaultFilter;  // remove filter
        }
    }
    isFilterEnabled = !isFilterEnabled;  // switch boolean value
});
```

## Game environment - assets/js/alienWorld/GameEnv.js

A technique used in Object Oriented Programming is to handle system wide constant values in a "static" variables.  These variables are maintained in the "GameEnv" class and can be included and accessed with class name prefix.

In code in index.html you will see reference to GameEnv.speed and calls to set GamEnv.setGameEnv.  Properties are set in this code that enable objects to reference things like Top of screen, or ratio of Object speed versus GameSpeed.


Class definition of Global attributes

```js
export class GameEnv {
    // Prototype static variables
    static innerWidth;
    static prevInnerWidth;
    static innerHeight;
    static top;
    static bottom;
    static prevBottom;
    static gameSpeed;
    static gravity;

    // Make the constructor private to prevent instantiation
    constructor() {
        throw new Error('GameEnv is a static class and cannot be instantiated.');
    }

     // Setter for Top position
     static setTop() {
        // set top of game as header height
        const header = document.querySelector('header');
        if (header) {
            this.top = header.offsetHeight;
        }
    }

    // Setter for Bottom position
    static setBottom() {
        // set bottom of game as background height
        const background = document.querySelector('#background');
        if (background) {
            this.bottom = background.offsetHeight;
        }
    }
    
    // Setter for Game Environment 
    static setGameEnv() {
        // store previous for ratio calculatins on resize
        this.prevInnerWidth = this.innerWidth;
        this.prevBottom = this.bottom;
    
        // game uses available width and heith
        this.innerWidth = window.innerWidth;
        this.innerHeight = window.innerHeight;

        this.setTop();
        // this.setBottom() is ignored for now as resize of background object determinse bottom
    }
}

export default GameEnv;
```

## Game objects - assets/js/alienWorld/GameObject.js

This is a more collaboritive area.  
- I had a big focus on starting static gameObjectArray [ ] as this was critical to events and main game loop.  A
- The collision code in game object and binary capture of status and relative position were key element in the code design below.


```js
import GameEnv from './GameEnv.js';

class GameObject {
    // container for all game objects in game
    static gameObjectArray = [];
    constructor(canvas, image, speedRatio) {
        this.x = 0;
        this.y = 0;
        this.frame = 0;
        this.canvas = canvas;
        this.ctx = canvas.getContext('2d');
        this.image = image;
        this.width = image.width;  // Image() width (meta data)
        this.height = image.height; // Image() height
        this.collisionWidth = 0;
        this.collisionHeight = 0;
        this.aspect_ratio = this.width / this.height;
        this.speedRatio = speedRatio;
        this.speed = GameEnv.gameSpeed * this.speedRatio;
        this.collisionData = {};
        // Add this object to the game object array so collision can be detected
        // among other things
        GameObject.gameObjectArray.push(this); 
    }

    // X position getter and setter
    getX() {
        return this.x;
    }

    setX(x) {
        this.x = x;
    }

    // Y position getter and setter
    getY() {
        return this.y;
    }

    setY(y) {
        this.y = y;
    }

    /* Default action is no action
     * override when you extend for custom action
    */
    collisionAction(){
        // no action
    }

    /* Collision checks
     * uses GameObject isCollision to detect hit
     * calls collisionAction on hit
    */
    collisionChecks() {
        for (var gameObj of GameObject.gameObjectArray){
            if (this != gameObj ) {
                this.isCollision(gameObj);
                if (this.collisionData.hit){
                    this.collisionAction();
                }
            }
        }
    }

    /* Collision detection method
     * usage: if (player.isCollision(platform)) { // action }
    */
    isCollision(otherGameObject) {

        this.collisionData = {
            hit: (this.x + this.collisionWidth > otherGameObject.x &&
            this.x < otherGameObject.x + otherGameObject.collisionWidth &&
            this.y + this.collisionHeight > otherGameObject.y &&
            this.y < otherGameObject.y + otherGameObject.collisionHeight),
            touchPoints: {
                this: {
                    object: this,
                    top: (this.y > otherGameObject.y), 
                    bottom: (this.y < otherGameObject.setY), 
                    left: (this.x > otherGameObject.x), 
                    right: (this.x < otherGameObject.x) 
                },
                other: {
                    object: otherGameObject,
                    top: (this.y < otherGameObject.y), 
                    bottom: (this.y > otherGameObject.y), 
                    left: (this.x < otherGameObject.x), 
                    right: (this.x > otherGameObject.x) 
                }
            } 
            
        };
    }
}

export default GameObject;

```


## Migrating to OOP - Background

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

