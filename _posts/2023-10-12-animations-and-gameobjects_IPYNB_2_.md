---
layout: post
toc: True
title: Animations & GameObjects
courses: {'csse': {'week': 8}}
categories: []
type: ccc
author: Safin Singh, Rohan Juneja
---

# A (Very) Brief Intro to Gravity

![]({{site.baseurl}}/images/falling_ball.png)

Stuff naturally tends to fall. Why?

Gravity pulls everything down (to the center of our Earth)

We all know what speed is (think of mph). Notice that when you drop an object, it speeds up (from 0 to some higher number). This is caused by the force of gravity!

The rate at which the object speeds up is known as **Acceleration due to Gravity**

`g = 9.8 m/s^2` basically means:
- Every 1 second,
- the object’s speed increases by 9.8 m/s (~22 mph)
- downwards, towards the center of the Earth

## Why does this matter?
- Objects in our game need to fall down because that’s how it works in real life!
- We need to implement gravity to emulate this behavior

[Example Platformer](https://safinsingh.github.io/platformer/)

# Canvas Grid

![]({{site.baseurl}}/images/canvas_grid.png)

# Gravity Activity

Currently when the character jumps, it goes straight up into the air and doesn't come back down. Can you make it so it acts accurate to the real world (hint: implement gravity, see the comment for an idea of where to place your code)

BONUS: Prevent the player from double jumping


```python
 %%html
<style>
    canvas {
        margin: 0;
        border: 1px solid white;
    }
</style>
<canvas id='canvas1'></canvas>
<script src="{{ site.baseurl }}/assets/js/player.js"></script>
<script>
    // Create empty canvas
    let canvas = document.getElementById('canvas1');
    let c = canvas.getContext('2d');
    // Set the canvas dimensions
    canvas.width = 650;
    canvas.height = 400;

    // Create a player object
    player = new Player(c);
    
    // Define keyboard keys and their states
    let keys = {
        right: {
            pressed: false
        },
        left: {
            pressed: false
        }
    };
    // Animation function to continuously update and render the canvas
    function animate() {
        requestAnimationFrame(animate);
        c.clearRect(0, 0, canvas.width, canvas.height);
        player.update();
        if (keys.right.pressed && player.position.x + player.width <= canvas.width - 50) {
            player.velocity.x = 15;
        } else if (keys.left.pressed && player.position.x >= 50) {
            player.velocity.x = -15;
        } else {
            player.velocity.x = 0;
        }
    }
    animate();
    // Event listener for keydown events
    addEventListener('keydown', ({ keyCode }) => {
        switch (keyCode) {
            case 65:
                console.log('left');
                keys.left.pressed = true;
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                keys.right.pressed = true;
                break;
            case 87:
                console.log('up');
                player.velocity.y -= 20;
                break;
        }
    });
    // Event listener for keyup events
    addEventListener('keyup', ({ keyCode }) => {
        switch (keyCode) {
            case 65:
                console.log('left');
                keys.left.pressed = false;
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                keys.right.pressed = false;
                break;
            case 87:
                console.log('up');
                /* FILL IN HERE
             think about adding a conditional here (bonus) */
                player.velocity.y = -20;
                break;
        }
    });
</script>

```


<style>
   #canvas {
       margin: 0;
       border: 1px solid white;
   }
</style>
<canvas id='canvas'></canvas>
<script src="{{ site.baseurl }}/assets/js/player.js"></script>
<script>
   // Create empty canvas
   let canvas = document.getElementById('canvas');
   let c = canvas.getContext('2d');
   // Set the canvas dimensions
   canvas.width = 650;
   canvas.height = 400;

   // Create a player object
   player = new Player(c);

   // Define keyboard keys and their states
   let keys = {
       right: {
           pressed: false
       },
       left: {
           pressed: false
       }
   };
   // Animation function to continuously update and render the canvas
   function animate() {
       requestAnimationFrame(animate);
       c.clearRect(0, 0, canvas.width, canvas.height);
       player.update();
       if (keys.right.pressed && player.position.x + player.width <= canvas.width - 50) {
           player.velocity.x = 15;
       } else if (keys.left.pressed && player.position.x >= 50) {
           player.velocity.x = -15;
       } else {
           player.velocity.x = 0;
       }
   }
   animate();
   // Event listener for keydown events
   addEventListener('keydown', ({ keyCode }) => {
       switch (keyCode) {
           case 65:
               console.log('left');
               keys.left.pressed = true;
               break;
           case 83:
               console.log('down');
               break;
           case 68:
               console.log('right');
               keys.right.pressed = true;
               break;
           case 87:
               console.log('up');
               player.velocity.y -= 20;
               break;
       }
   });
   // Event listener for keyup events
   addEventListener('keyup', ({ keyCode }) => {
       switch (keyCode) {
           case 65:
               console.log('left');
               keys.left.pressed = false;
               break;
           case 83:
               console.log('down');
               break;
           case 68:
               console.log('right');
               keys.right.pressed = false;
               break;
           case 87:
               console.log('up');
               /* FILL IN HERE
            think about adding a conditional here (bonus) */
               player.velocity.y = -20;
               break;
       }
   });
</script>



# GameObject Activity (Part 1)

We want a platform on the ground. You can use whatever image you want or just a rectangle on the canvas. The platform should be represented by the GameObject class.


```python
%%html
<style>
    canvas {
        margin: 0;
        border: 1px solid white;
    }
</style>
<!-- PART 2: Change this to "canvas3" for the Collision activity! -->
<canvas id='canvas2'></canvas>
<script src="{{ site.baseurl }}/assets/js/player.js"></script>
<script src="{{ site.baseurl }}/assets/js/game_object.js"></script>
<script>
    // Create empty canvas
    // PART 2: Change this to "canvas3" for the Collision activity!
    let canvas = document.getElementById('canvas2');
    let c = canvas.getContext('2d');
    // Set the canvas dimensions
    canvas.width = 650;
    canvas.height = 400;

    // Create a player object
    player = new Player(c);
   
   /* initialize your GameObject (first parameter should be c) */

    // Define keyboard keys and their states
    let keys = {
        right: {
            pressed: false
        },
        left: {
            pressed: false
        }
    };
    // Animation function to continuously update and render the canvas
    function animate() {
        requestAnimationFrame(animate);
        c.clearRect(0, 0, canvas.width, canvas.height);
        player.update();
        /* ADD STUFF HERE
      make sure to draw your game object here */
       /* ADD STUFF HERE (PART 2)
      (part 2) - add some handling to ensure the player doesn't go through the game object (use your collision method, bonus for restitution could be done here */
        if (keys.right.pressed && player.position.x + player.width <= canvas.width - 50) {
            player.velocity.x = 15;
        } else if (keys.left.pressed && player.position.x >= 50) {
            player.velocity.x = -15;
        } else {
            player.velocity.x = 0;
        }
    }
    animate();
    // Event listener for keydown events
    addEventListener('keydown', ({ keyCode }) => {
        switch (keyCode) {
            case 65:
                console.log('left');
                keys.left.pressed = true;
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                keys.right.pressed = true;
                break;
            case 87:
                console.log('up');
                player.velocity.y -= 20;
                break;
        }
    });
    // Event listener for keyup events
    addEventListener('keyup', ({ keyCode }) => {
        switch (keyCode) {
            case 65:
                console.log('left');
                keys.left.pressed = false;
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                keys.right.pressed = false;
                break;
            case 87:
                console.log('up');
                player.velocity.y = -20;
                break;
        }
    });
</script>

```


<style>
    #canvas {
        margin: 0;
        border: 1px solid white;
    }
</style>
<canvas id='canvas'></canvas>
<script src="{{ site.baseurl }}/assets/js/player.js"></script>
<script src="{{ site.baseurl }}/assets/js/layer.js"></script>
<script>
    // Create empty canvas
    let canvas = document.getElementById('canvas');
    let c = canvas.getContext('2d');
    // Set the canvas dimensions
    canvas.width = 650;
    canvas.height = 400;

    // Create a player object
    player = new Player(c);

   /* initialize your layer (first parameter should be c) */

    // Define keyboard keys and their states
    let keys = {
        right: {
            pressed: false
        },
        left: {
            pressed: false
        }
    };
    // Animation function to continuously update and render the canvas
    function animate() {
        requestAnimationFrame(animate);
        c.clearRect(0, 0, canvas.width, canvas.height);
        player.update();
        /* ADD STUFF HERE
      make sure to draw your layer here */
       /* ADD STUFF HERE (PART 2)
      (part 2) - add some handling to ensure the player doesn't go through the layer (use your collision method, bonus for restitution could be done here */
        if (keys.right.pressed && player.position.x + player.width <= canvas.width - 50) {
            player.velocity.x = 15;
        } else if (keys.left.pressed && player.position.x >= 50) {
            player.velocity.x = -15;
        } else {
            player.velocity.x = 0;
        }
    }
    animate();
    // Event listener for keydown events
    addEventListener('keydown', ({ keyCode }) => {
        switch (keyCode) {
            case 65:
                console.log('left');
                keys.left.pressed = true;
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                keys.right.pressed = true;
                break;
            case 87:
                console.log('up');
                player.velocity.y -= 20;
                break;
        }
    });
    // Event listener for keyup events
    addEventListener('keyup', ({ keyCode }) => {
        switch (keyCode) {
            case 65:
                console.log('left');
                keys.left.pressed = false;
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                keys.right.pressed = false;
                break;
            case 87:
                console.log('up');
                player.velocity.y = -20;
                break;
        }
    });
</script>



# Collision Activity (Part 2)

The player should not be able to go through the platform.  The GameObject class should have a method which can tell if a player is colliding with it.

![]({{site.baseurl}}/images/collisions.png)

BONUS: if the player hits the ground at a high velocity, it should bounce up a little bit (restitution)


```python
%%html
<!-- copy paste your code from part 1 and continue here! -->
<!-- ensure that you change the canvas ID for this part; there are instructions in the above comments -->
```
