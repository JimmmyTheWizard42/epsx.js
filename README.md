Hello everyone!

This is a *super super super* small game engine I programmed for lightweight JavaScript games.
Here's a simple tutorial on how to use some of the functions and custom code:
- createPsxNode ( ```width```, ```height```, ```color```, ```posX```, ```posY``` )
  - This is used to create a 2d physics object. The parameters are self explanatory.
  - Example: ```var myObject = createPsxNode(50, 50, "red", 0, 0);```

- addPsxNode ( ```node``` )
  - This is used to add a 2d physics object to the canvas. ```node``` should be entered as a variable with the value of the ```createPsxNode()``` function.
  - Example: ```addPsxNode(myObject);```

- psx.*variable*
    - This is used to define the gravity and groundheight of the canvas.
    - Example: ```psx.gravity = 1; console.log(psx.gravity); psx.ground = 450; console.log(psx.ground);```

Here is a simple two-player game I made using epsx.js:
```javascript
//create canvas element
const canvas = document.createElement( "canvas" );
canvas.width = 700;
canvas.height = 500;
canvas.style.borderStyle = "solid";
document.body.appendChild( canvas );

//get context
const ctx = canvas.getContext( "2d" );

//scene function
function fillScene( color ) {
	ctx.fillStyle = color;
  ctx.clearRect(0, 0, canvas.width, canvas.height);
}

//physics engine
var psx = {
	gravity : 0.5,
  ground : 450
}
var psxObjects = [  ];

//entity creation function
function createPsxNode ( width, height, color, posX, posY ) {
	var node = {
    	w : width,
        h : height,
        c : color,
        x : posX,
        y : posY,
        yVelocity : 0,
    }
    return node;
}

//entity addition function
function addPsxNode ( node ) {
	psxObjects.push( node );
}

//player properties
var speed = 10;
var jumpHeight = 15;

//player nodes
var player1 = createPsxNode(50, 80, "red", 100, 100);
addPsxNode(player1);

var player2 = createPsxNode(50, 80, "blue", 550, 100);
addPsxNode(player2);

//keyboard events
const keys = {};
document.addEventListener("keydown", (event) => {
  keys[event.keyCode] = true;
});
document.addEventListener("keyup", (event) => {
  keys[event.keyCode] = false;
});

function updatePlayerPosition() {
	//player1 AKA "WASD"
    if (keys[87]) {
      // w
      if ( player1.y + player1.h >= psx.ground ) {
      	player1.yVelocity = -jumpHeight;
      }
    }
    if (keys[65]) {
     // a
     player1.x -= speed;
    }
    if (keys[68]) {
      // d
      player1.x += speed;
    }
    
    //player2 AKA "UDLR"
    if (keys[38]) {
      // u
      if ( player2.y + player2.h >= psx.ground ) {
      	player2.yVelocity = -jumpHeight;
      }
    }
    if (keys[37]) {
     // l
     player2.x -= speed;
    }
    if (keys[39]) {
      // r
      player2.x += speed;
    }
    
    //rAF callback
    requestAnimationFrame(updatePlayerPosition);
}

updatePlayerPosition();

//game update
function update() {
    //draw bg
    fillScene("white");

    //physics update - loop through added entities and apply gravity
    for (let i = 0; i < psxObjects.length; i++) {
        psxObjects[i].y += psxObjects[i].yVelocity;
        if (psxObjects[i].y + psxObjects[i].h >= psx.ground) {
            psxObjects[i].y = psx.ground - psxObjects[i].h; // Adjust position to stay on the ground
            psxObjects[i].yVelocity = 0;
        } else {
            psxObjects[i].yVelocity += psx.gravity;
        }
    }

    //draw psxObjects
    for (let i = 0; i < psxObjects.length; i++) {
        ctx.fillStyle = psxObjects[i].c;
        ctx.fillRect(psxObjects[i].x, psxObjects[i].y, psxObjects[i].w, psxObjects[i].h);
    }

    //rAF callback
    requestAnimationFrame(update);
}
update();
```
