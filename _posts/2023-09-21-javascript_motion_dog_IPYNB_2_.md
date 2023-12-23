---
toc: True
layout: post
title: Sprite Sheet Animation
description: A guide on simple sprite animations using Javascript, and how to implement them into games
courses: {'csse': {'week': 5}, 'csp': {'week': 14, 'categories': ['1.A', '3.B']}, 'csa': {'week': 3}}
type: ccc
permalink: /cpt/animate
author: Ekamjot Kaire
---

### Setting up Spritesheet
This blog will go through the steps by using [this spritesheet](https://github.com/Ekamjot-Kaire/student/blob/main/_notebooks/images/dogSprites.png) as a demonstration: 


<strong> Download the image into your files. Then, create an image folder in the _notebooks directory and save this image in that folder. </strong>

<br>
<br>
<br>

### Base HTML

Create the webpage layout of your animation, including the div in which each image is generated. Below is HTML for a very basic structure which can be used to test your sprites:

```html
<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="dogSprite" src="/teacher/images/dogSprites.png">
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="idle" checked>
            <label for="idle">Idle</label><br>
            <input type="radio" name="animation" id="barking">
            <label for="barking">Barking</label><br>
            <input type="radio" name="animation" id="walking">
            <label for="walking">Walking</label><br>
        </div>
    </div>
</body>
```

### Setting up Javascript 

Set up the main function, which will contain the object class for the animation, including the parameters of the object, and the different methods of what the object can do. 

For most cases, it is simplest to have the function called when the page is loaded, which would ensure the code works every time someone opens the game. We will also set a few variables, which are further explained in the code comments. 

Since we are switching over to Javascript, be sure to put all code within the `<script>` tag, which will allow the computer to know the code is no longer HTML

```javascript
<script>
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');  // sets the canvas as a variable by calling the canvas element from the HTML code, using the id we set
        const ctx = canvas.getContext('2d'); // the getContext function is a given function within the canvas object. It allows us more functionality with the sprite image.

        // constant variables used for sprite
        const SPRITE_WIDTH = 160;
        const SPRITE_HEIGHT = 144;
        const FRAME_LIMIT = 48;
       

        // sets canvas properties
        const SCALE_FACTOR = 2;
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        //more code will be placed here later
    });

</script>
```

> NOTE: the particular sprite sheet given for this animation has sprites that are 160 by 144 pixels. Setting the width and height as a variable allows us to change the scale easily. When using other sprite sheets, be sure to find out what the dimensions of the sprite are

### Using Object Oriented Programming 

Object Oriented Programming is a model in programming which creates 'objects'. Specific data or parameters can then be assigned to the object in classes. The classes are essentially 'blueprints', which can be used to create more instances of the same object. This makes it easier to reuse and keep track of data.

We are going to create a class for our sprite. Each class can have different methods, which are functions assigned to each class. Our sprite object will have 3 methods; constructor method, draw method, and update method

The constructor method sets the values which will be assigned to this class. The values assigned are explained in the code's comments

We are also creating a draw method, in order to 'draw' the image on the canvas created earlier. The parameters given to the method are the same we set in the constructor method

> place this class structure within the onLoad function we created earlier 

```javascript
        class Dog {
            constructor(){  // constructor method for outlining data points about the sprite
                this.image = document.getElementById("dogSprite");
                this.spriteWidth = SPRITE_WIDTH;
                this.spriteHeight = SPRITE_HEIGHT;
                this.width = this.spriteWidth;  //takes sprite dimensions and accounts for it in image generation
                this.height = this.spriteHeight;
                this.x = 0;  //starts image generation in coordinate (0,0) px on sprite sheet
                this.y = 0;
                this.scale = 1;  //scale of image
                this.minFrame = 0;
                this.maxFrame = FRAME_LIMIT;  // how many sprites there are in a row
                this.frameX = 0;  // sets position of sprite that is being generated. There are the two parameters we will be changing in order to crete the animation. 
                this.frameY = 0;
            }

            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * this.spriteWidth,
                    this.frameY * this.spriteHeight,
                    this.spriteWidth,
                    this.spriteHeight,
                    this.x,
                    this.y,
                    this.width * this.scale,
                    this.height * this.scale
                );
            }
```

### How the animation works

As mentioned above, the sprite sheet contains sprites placed side by side at even intervals. The code we write will generate an image with the dimensions 160 by 144 pixels, since that is the size of an individual sprite. The first image generated starts at the origin point (0,0) px. The update method will then shift the origin point over to the right by 160 pixels, so that the next image generated is the 2nd frame. 

### Writing the Update Method

```javascript
class Dog {
    constructor() {
            this.image = document.getElementById("dogSprite");
            this.x = 0;
            this.y = 0;
            this.minFrame = 0;
            this.maxFrame = FRAME_LIMIT;
            this.frameX = 0;
            this.frameY = 0;
        }

        // draw dog object
        draw(context) {
            context.drawImage(
                this.image,
                this.frameX * SPRITE_WIDTH,
                this.frameY * SPRITE_HEIGHT,
                SPRITE_WIDTH,
                SPRITE_HEIGHT,
                this.x,
                this.y,
                canvas.width,
                canvas.height
            );
        }

        // update frameX of object
        update() {
            if (this.frameX < this.maxFrame) {
                this.frameX++;
            } else {
                this.frameX = 0;
            }
        }
    }
}
```

> NOTE: This code only plays one of the three animations. In other words only one row of the sprite sheet is being played. In order to have the animation change, we will need to code user inputs into the update method. 

### Making Your Animation User Responsive 

In order to make our animations adaptive to user inputs, we can use eventListeners to detect when a user clicks on a different button. While we are using radio buttons in this example, eventListeners can be set to detect other user inputs, such as a mouse movement or keystroke, or even an in-game event like losing the game. 

These eventListeners will then trigger a certain function which changes the animation on the screen. 

The parameter this.frameY is what controls which animation is being played, but we can change its value through eventListeners. 'Idle' would set this.frameY equal to 0, which would play the first row of animation on the spreadsheet (remember: python starts it's index from 0). The "bark" button would play the second row by setting this.frameY equal to 1, and so on. 

> Place the following code after the Dog class

```javascript
const dog = new Dog();  // This makes the game object, Dog

// Add event listener to the parent container for event delegation
const controls = document.getElementById('controls');
controls.addEventListener('click', function (event) {
    if (event.target.tagName === 'INPUT') {
        const selectedAnimation = event.target.id;
        switch (selectedAnimation) {
            case 'idle':
                dog.frameY = 0;
                break;
            case 'barking':
                dog.frameY = 1;
                break;
            case 'walking':
                dog.frameY = 2;
                break;
            default:
                break;
        }
    }
});

```

### Calling Class Methods 

All the functions have been written; all thats left is to put them together by calling them when the page loads.  

Place the following code outside after previous code, but still within the onLoad function:

```javascript
// Animation recursive control function
function animate() {
    // Clears the canvas to remove the previous frame.
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Draws the current frame of the sprite.
    dog.draw(ctx);

    // Updates the `frameX` property to prepare for the next frame in the sprite sheet.
    dog.update();

    // Uses `requestAnimationFrame` to synchronize the animation loop with the display's refresh rate,
    // ensuring smooth visuals.
    requestAnimationFrame(animate);
}

animate();
```

## Put It all Together

Create a new HTML file and place all the code together! The animation should run similar to the example show below


> Hacks

* Try and see if you can get the animation to respond to different user inputs (other then the radio buttons)
<br>

*  Or; get the dog sprite to actually move across the screen during the walk animation using translations 

<strong> your code should end up looking like the example below </strong>


```python
%%html

<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="dogSprite" src="/teacher/images/dogSprites.png">  // change sprite here
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="idle" checked>
            <label for="idle">Idle</label><br>
            <input type="radio" name="animation" id="barking">
            <label for="barking">Barking</label><br>
            <input type="radio" name="animation" id="walking">
            <label for="walking">Walking</label><br>
        </div>
    </div>
</body>

<script>
    // start on page load
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = 160;  // matches sprite pixel width
        const SPRITE_HEIGHT = 144; // matches sprite pixel height
        const FRAME_LIMIT = 48;  // matches number of frames per sprite row, this code assume each row is same

        const SCALE_FACTOR = 2;  // control size of sprite on canvas
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        class Dog {
            constructor() {
                this.image = document.getElementById("dogSprite");
                this.x = 0;
                this.y = 0;
                this.minFrame = 0;
                this.maxFrame = FRAME_LIMIT;
                this.frameX = 0;
                this.frameY = 0;
            }

            // draw dog object
            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * SPRITE_WIDTH,
                    this.frameY * SPRITE_HEIGHT,
                    SPRITE_WIDTH,
                    SPRITE_HEIGHT,
                    this.x,
                    this.y,
                    canvas.width,
                    canvas.height
                );
            }

            // update frameX of object
            update() {
                if (this.frameX < this.maxFrame) {
                    this.frameX++;
                } else {
                    this.frameX = 0;
                }
            }
        }

        // dog object
        const dog = new Dog();

        // update frameY of dog object, action from idle, bark, walk radio control
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'idle':
                        dog.frameY = 0;
                        break;
                    case 'barking':
                        dog.frameY = 1;
                        break;
                    case 'walking':
                        dog.frameY = 2;
                        break;
                    default:
                        break;
                }
            }
        });

        // Animation recursive control function
        function animate() {
            // Clears the canvas to remove the previous frame.
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draws the current frame of the sprite.
            dog.draw(ctx);

            // Updates the `frameX` property to prepare for the next frame in the sprite sheet.
            dog.update();

            // Uses `requestAnimationFrame` to synchronize the animation loop with the display's refresh rate,
            // ensuring smooth visuals.
            requestAnimationFrame(animate);
        }

        // run 1st animate
        animate();
    });
</script>
```



<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="dogSprite" src="/teacher/images/dogSprites.png">  // change sprite here
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="idle" checked>
            <label for="idle">Idle</label><br>
            <input type="radio" name="animation" id="barking">
            <label for="barking">Barking</label><br>
            <input type="radio" name="animation" id="walking">
            <label for="walking">Walking</label><br>
        </div>
    </div>
</body>

<script>
    // start on page load
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = 160;  // matches sprite pixel width
        const SPRITE_HEIGHT = 144; // matches sprite pixel height
        const FRAME_LIMIT = 48;  // matches number of frames per sprite row, this code assume each row is same

        const SCALE_FACTOR = 2;  // control size of sprite on canvas
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        class Dog {
            constructor() {
                this.image = document.getElementById("dogSprite");
                this.x = 0;
                this.y = 0;
                this.minFrame = 0;
                this.maxFrame = FRAME_LIMIT;
                this.frameX = 0;
                this.frameY = 0;
            }

            // draw dog object
            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * SPRITE_WIDTH,
                    this.frameY * SPRITE_HEIGHT,
                    SPRITE_WIDTH,
                    SPRITE_HEIGHT,
                    this.x,
                    this.y,
                    canvas.width,
                    canvas.height
                );
            }

            // update frameX of object
            update() {
                if (this.frameX < this.maxFrame) {
                    this.frameX++;
                } else {
                    this.frameX = 0;
                }
            }
        }

        // dog object
        const dog = new Dog();

        // update frameY of dog object, action from idle, bark, walk radio control
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'idle':
                        dog.frameY = 0;
                        break;
                    case 'barking':
                        dog.frameY = 1;
                        break;
                    case 'walking':
                        dog.frameY = 2;
                        break;
                    default:
                        break;
                }
            }
        });

        // Animation recursive control function
        function animate() {
            // Clears the canvas to remove the previous frame.
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draws the current frame of the sprite.
            dog.draw(ctx);

            // Updates the `frameX` property to prepare for the next frame in the sprite sheet.
            dog.update();

            // Uses `requestAnimationFrame` to synchronize the animation loop with the display's refresh rate,
            // ensuring smooth visuals.
            requestAnimationFrame(animate);
        }

        // run 1st animate
        animate();
    });
</script>


