# 🧟 Zombie Fighter Game

A 2D side-scrolling survival shooter game built with HTML5 Canvas and vanilla JavaScript. Fight endless waves of zombies while managing your health and trying to achieve the highest score possible!

---

## 📋 Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Game Features](#game-features)
- [Technology Stack](#technology-stack)
- [Game Mechanics](#game-mechanics)
- [Key Classes & Functions](#key-classes--functions)
- [Data Flow & Architecture](#data-flow--architecture)
- [Setup & Installation](#setup--installation)
- [How to Play](#how-to-play)
- [Customization Guide](#customization-guide)
- [Known Issues & Future Improvements](#known-issues--future-improvements)
- [Browser Compatibility](#browser-compatibility)

---

## 🎯 Overview

**Zombie Fighter** is a fun, arcade-style survival game where players defend themselves against waves of increasingly fast zombies. With a scoring system that scales difficulty, responsive controls, and engaging gameplay, it's a perfect portfolio project showcasing game development fundamentals.

### Game Concept
- **Genre**: 2D Side-Scrolling Shooter
- **Objective**: Survive as long as possible by shooting zombies
- **Difficulty**: Increases with score
- **Platform**: Browser-based (HTML5 Canvas)

### Language Composition
- JavaScript: 91.5% (game logic)
- HTML: 4.9% (structure)
- CSS: 3.6% (styling)

---

## 📁 Project Structure

```
Zombie_fighter/
├── 📄 index.html           # Main HTML entry point
├── 🎨 style.css            # Game styling and layout
├── 🎮 main.js              # Core game logic (11.16 KB)
├── 📁 images/              # Sprite images directory
│   ├── survivor1.png       # Player character sprite
│   └── jombie1.png         # Zombie enemy sprite
└── 📄 README.md            # Documentation
```

### File Descriptions

#### `index.html`
The HTML structure defining the game canvas and control buttons.
```html
- <canvas id="myCanvas">: Canvas element (99.999% width, 90% height)
- <div id="controls">: Container for game control buttons
  - #left: Move left button
  - #jump: Jump button
  - #right: Move right button
  - #pause: Pause/resume button
- <script src="main.js">: Loads main game logic
```

#### `style.css`
Minimal CSS for layout and positioning.
- Button styling with 5px margins
- Canvas positioning (relative) with red border
- Control buttons flex container
- Absolute positioning for game over and start buttons

#### `main.js`
Complete game engine (11,160 bytes / ~380 lines).
Contains:
- **Survivor Class**: Player character with physics
- **Jombie Class**: Zombie enemies with difficulty scaling
- **Game Loop**: RequestAnimationFrame animation
- **Collision Detection**: AABB collision checking
- **Input Handling**: Keyboard and button controls
- **Game States**: Main menu, playing, paused, game over

#### `images/`
Sprite image directory (not in repo, must be added):
- `survivor1.png` - Player sprite (100x100px recommended)
- `jombie1.png` - Zombie sprite (100x100px recommended)

---

## ✨ Game Features

### 1. **Player Character (Survivor)**
- Moves left/right across the game world
- Can jump with gravity-based physics
- Starts with 10 health points
- Dies when health reaches 0

### 2. **Enemy Zombies (Jombies)**
- Spawn from screen edges randomly
- Move toward player with progressive speed
- Eliminated when shot
- Destroy blocks on collision

### 3. **Shooting System**
- Click anywhere to aim and shoot
- Bullets have gravity and physics
- Automatically removed off-screen
- Collision detection with enemies

### 4. **Block Obstacles**
- Provide cover for player
- Block zombie movement
- Destroyed when zombies collide
- Fixed starting positions (left and right of center)

### 5. **Scoring System**
- +10 points per zombie eliminated
- Score displayed in-game
- Final score shown on game over
- Affects zombie difficulty

### 6. **Difficulty Scaling**
```
Score 0-99:     Zombie speed = 2 pixels/frame
Score 100-199:  Zombie speed = 4 pixels/frame
Score 200+:     Zombie speed = 6 pixels/frame
```

### 7. **Game States**
- **Main Menu**: Instructions displayed, "Start Game" button
- **Playing**: Active gameplay with enemies spawning
- **Paused**: Game frozen, "Resume" button available
- **Game Over**: Final score displayed, "Start New Game" button

### 8. **Controls**
**Desktop/Keyboard:**
- Arrow Up or Spacebar: Jump
- Arrow Left: Move left
- Arrow Right: Move right
- Mouse Click: Shoot
- Pause Button: Toggle pause

**Mobile/Touch:**
- Left Button: Move left
- Jump Button: Jump
- Right Button: Move right
- Pause Button: Pause/resume
- Screen Tap: Shoot

---

## 🛠️ Technology Stack

| Technology | Purpose | Usage |
|-----------|---------|-------|
| **HTML5** | Markup & structure | Canvas element, buttons |
| **Canvas API** | Graphics rendering | Draw sprites, bullets, blocks |
| **JavaScript ES6** | Game logic | Classes, animation, collision |
| **CSS3** | Styling | Layout, positioning, flexbox |
| **Browser APIs** | Input & animation | Keyboard events, requestAnimationFrame |

### Key Browser Features Used
- **Canvas 2D Context**: Rendering graphics
- **requestAnimationFrame**: Smooth 60 FPS animation
- **Event Listeners**: Keyboard and mouse input
- **Image Objects**: Sprite loading and drawing
- **Math Functions**: Angle calculation, physics

---

## 🎮 Game Mechanics

### Physics System

#### Gravity
```javascript
const gravity = 0.2;  // Applied to bullets
const sgravity = 0.1; // Applied to player
```
- Gravity affects both player and bullets
- Player velocity increases downward until hitting ground
- Bullets arc naturally due to gravity

#### Movement
```javascript
// Player movement (10 pixels per action)
ArrowLeft:  fighter.x -= 10
ArrowRight: fighter.x += 10
Jump:       fighter.velocity.y = -8
```

#### Jumping Mechanics
- Player can only jump when touching ground (`fighter.y + fighter.height >= canvas.height`)
- Jump velocity: -8 pixels/frame
- Gravity gradually decreases upward velocity
- Player returns to ground naturally

### Collision Detection

#### AABB (Axis-Aligned Bounding Box) Collision
```javascript
function collision(a, b) {
    return (
        a.x < b.x + b.width &&
        a.x + a.width > b.x &&
        a.y < b.y + b.height &&
        a.y + a.height > b.height
    )
}
```

#### Bullet-Enemy Collision
```javascript
function collisionbulletenemy(bullet, jombie) {
    return (
        bullet.x < jombie.x + jombie.width &&
        bullet.x + bullet.radius > jombie.x &&
        bullet.y < jombie.y + jombie.height &&
        bullet.y + bullet.radius > jombie.y
    )
}
```

### Shooting Mechanics

#### Angle Calculation
```javascript
// Click listener calculates angle to mouse position
angle = Math.atan2(mouseY - startY, mouseX - startX);

// Create bullet with velocity components
bullets.push({
    x: startX,
    y: startY,
    vx: speed * Math.cos(angle),  // Horizontal velocity
    vy: speed * Math.sin(angle),  // Vertical velocity
    radius: 7
});
```

#### Bullet Physics
```javascript
// Each frame:
b.vy += gravity;  // Apply gravity
b.x += b.vx;      // Update position
b.y += b.vy;
```

### Enemy Spawning

#### Spawn Logic
```javascript
// Spawn every 2-4 seconds (random)
setInterval(() => {
    setTimeout(() => {
        const val = Math.random();
        const x = val > 0.5 ? -100 : canvas.width + 100;
        const direction = val > 0.5 ? "right" : "left";
        
        enemies.push({
            x: x,
            jombie: new Jombie(x, direction)
        });
    }, (Math.random() * 3 + 1) * 1000);
}, 2000);
```

---

## 🏗️ Key Classes & Functions

### Class: `Survivor`

Represents the player character.

#### Constructor
```javascript
function Survivor() {
    this.x = xPos;                    // X position
    this.y = yPos;                    // Y position
    this.width = 100;                 // Sprite width
    this.height = 100;                // Sprite height
    this.velocity = { x: 0, y: 0 };   // Velocity vector
}
```

#### Properties
- **x, y**: Position on canvas
- **width, height**: Dimensions (100x100)
- **velocity**: Object with x and y velocity components
- **image**: Sprite image from `images/survivor1.png`

#### Methods

**draw()**
```javascript
this.draw = function () {
    c.drawImage(image, self.x, self.y, self.width, self.height);
}
```
- Renders the survivor sprite at current position
- Called after image loads and on each frame update

**update()**
```javascript
this.update = function () {
    // Apply gravity
    if (this.y + this.height + this.velocity.y <= canvas.height) {
        this.velocity.y += sgravity;
        this.y += this.velocity.y;
    } else {
        // Hit ground - stop falling
        this.y = canvas.height - this.height;
        this.velocity.y = 0;
    }
    this.draw();
}
```
- Applies gravity each frame
- Updates position based on velocity
- Prevents falling through ground
- Calls draw() to render

### Class: `Jombie`

Represents zombie enemies.

#### Constructor
```javascript
function Jombie(x, d) {
    this.x = x;              // Spawn X position
    this.y = canvas.height - 100;  // Ground level Y
    this.width = 100;        // Sprite width
    this.height = 100;       // Sprite height
    // d = direction ('left' or 'right')
}
```

#### Properties
- **x, y**: Position on canvas
- **width, height**: Dimensions (100x100)
- **image**: Sprite from `images/jombie1.png`
- **direction**: Movement direction (passed from spawning)

#### Methods

**draw()**
```javascript
this.draw = function () {
    c.drawImage(image, self.x, canvas.height - 100, this.width, this.height);
}
```
- Renders zombie sprite at current position
- Always at ground level (Y = canvas.height - 100)

**update()**
```javascript
this.update = function () {
    this.draw();
    
    // Speed increases with score
    let speed;
    if (score < 100) speed = 2;
    else if (score < 200) speed = 4;
    else speed = 6;
    
    // Move toward player
    if (d == 'right') this.x += speed;
    else this.x -= speed;
}
```
- Draws zombie
- Calculates speed based on score
- Moves horizontally toward player

### Function: `animate()`

Main game loop running at ~60 FPS.

```javascript
function animate() {
    // Clear and redraw background
    c.clearRect(0, 0, canvas.width, canvas.height);
    c.fillStyle = 'rgba(0,0,0,0.66)';
    c.fillRect(0, 0, canvas.width, canvas.height);
    
    // Draw UI (score and health)
    c.fillStyle = 'white';
    c.font = '20px Arial';
    c.fillText(`your score is : ${score}`, ...);
    c.fillText(`health : ${health} /10`, ...);
    
    // Update blocks
    blocks.forEach((b) => block(b.x, b.y));
    
    // Update player
    fighter.update();
    
    // Update bullets (apply gravity, check collisions)
    for (let i = 0; i < bullets.length; i++) {
        const b = bullets[i];
        b.vy += gravity;
        b.x += b.vx;
        b.y += b.vy;
        // Draw and remove off-screen bullets
    }
    
    // Update enemies
    for (let j = 0; j < enemies.length; j++) {
        const e = enemies[j];
        e.jombie.update();
        // Remove off-screen enemies
    }
    
    // Collision detection
    enemies.forEach((enemy, enemyindex) => {
        // Bullet-enemy collisions
        bullets.forEach((bullet, bulletindex) => {
            if (collisionbulletenemy(bullet, enemy.jombie)) {
                score += 10;
                enemies.splice(enemyindex, 1);
                bullets.splice(bulletindex, 1);
            }
        });
        
        // Zombie-block collisions
        blocks.forEach((b, blockindex) => {
            if (collision(enemy.jombie, b)) {
                enemies.splice(enemyindex, 1);
                blocks.splice(blockindex, 1);
            }
        });
        
        // Zombie-player collisions
        if (collision(enemy.jombie, fighter)) {
            enemies.splice(enemyindex, 1);
            health -= 1;
        }
    });
    
    // Check game over
    if (health == 0) {
        cancelAnimationFrame(animationid);
        // Display game over screen
        buttonstartnewgame();
    } else {
        animationid = requestAnimationFrame(animate);
    }
}
```

**Key Loop Operations:**
1. Clear and redraw background
2. Display score and health
3. Draw blocks and update player
4. Update all bullets (apply gravity)
5. Update all enemies
6. Check all collisions (bullet-enemy, zombie-block, zombie-player)
7. Handle game over condition
8. Schedule next frame

### Function: `handleKeyPress(event)`

Keyboard input handler.

```javascript
function handleKeyPress(event) {
    switch (event.key) {
        case 'ArrowUp':
        case ' ':  // Spacebar
            if (fighter.y + fighter.height < canvas.height) return;
            fighter.velocity.y = -8;
            break;
            
        case 'ArrowLeft':
            fighter.x -= 10;
            startX -= 10;
            // Check block collision and revert if blocked
            blocks.forEach((b) => {
                if (b.x + b.width == fighter.x || fighter.x + fighter.width == b.x) {
                    fighter.x += 10;
                    startX += 10;
                }
            });
            break;
            
        case 'ArrowRight':
            fighter.x += 10;
            startX += 10;
            // Check block collision and revert if blocked
            blocks.forEach((b) => {
                if (b.x + b.width == fighter.x || fighter.x + fighter.width == b.x) {
                    fighter.x -= 10;
                    startX -= 10;
                }
            });
            break;
    }
}
```

**Features:**
- Arrow keys and spacebar for movement/jumping
- Block collision detection to prevent walking through blocks
- Updates both `fighter.x` and `startX` (shooting position)

### Function: `main()`

Initializes the game and displays main menu.

```javascript
function main() {
    // Display instructions
    c.clearRect(0, 0, canvas.width, canvas.height);
    c.fillStyle = 'black';
    c.fillRect(0, 0, canvas.width, canvas.height);
    c.fillStyle = 'white';
    c.font = '40px Arial';
    c.fillText('GENERAL INSTRUCTIONS', canvas.width / 2 - 300, 100);
    
    // Display game rules and controls
    c.font = '20px Arial';
    c.fillText('1. Click on screen to fire a bullet...', ...);
    c.fillText('2. Use spacebar to jump...', ...);
    // ... more instructions
    
    // Hide controls and show start button
    document.querySelector('#controls').style.display = 'none';
    const button = document.createElement('button');
    button.innerText = 'start game';
    button.id = 'start';
    button.addEventListener('click', () => {
        fighter = new Survivor();
        button.remove();
        document.querySelector('#controls').style.display = 'flex';
        
        // Start enemy spawning
        setInterval(() => {
            setTimeout(() => {
                const val = Math.random();
                const x = val > 0.5 ? -100 : canvas.width + 100;
                const direction = val > 0.5 ? "right" : "left";
                enemies.push({ x: x, jombie: new Jombie(x, direction) });
            }, (Math.random() * 3 + 1) * 1000);
        }, 2000);
        
        animate();  // Start game loop
    });
    document.body.appendChild(button);
}
```

### Function: `collision(a, b)`

AABB collision detection.

```javascript
function collision(a, b) {
    return (
        a.x < b.x + b.width &&
        a.x + a.width > b.x &&
        a.y < b.y + b.height &&
        a.y + a.height > b.height
    );
}
```

**Checks if two rectangular objects overlap:**
- Left edge of A is left of right edge of B
- Right edge of A is right of left edge of B
- Top edge of A is above bottom edge of B
- Bottom edge of A is below top edge of B

### Function: `collisionbulletenemy(bullet, jombie)`

Specialized collision for bullets vs zombies.

```javascript
function collisionbulletenemy(bullet, jombie) {
    return (
        bullet.x < jombie.x + jombie.width &&
        bullet.x + bullet.radius > jombie.x &&
        bullet.y < jombie.y + jombie.height &&
        bullet.y + bullet.radius > jombie.y
    );
}
```

**Treats bullet as circular (using radius) and zombie as rectangular.**

---

## 📊 Data Flow & Architecture

### Game Loop Flow
```
initialize game (main menu)
    ↓
wait for start button click
    ↓
create survivor player
    ↓
start enemy spawning interval
    ↓
    ┌─────────────────────────────────┐
    │    animate() loop (60 FPS)       │
    │                                  │
    │ 1. Clear canvas               │
    │ 2. Update player position     │
    │ 3. Update all bullets         │
    │    - Apply gravity            │
    │    - Move projectiles         │
    │    - Check bounds             │
    │ 4. Update all enemies         │
    │    - Move based on direction  │
    │    - Check bounds             │
    │ 5. Collision detection        │
    │    - Bullet vs Enemy          │
    │    - Enemy vs Block           │
    │    - Enemy vs Player          │
    │ 6. Update score/health        │
    │ 7. Check game over            │
    │                                  │
    │    requestAnimationFrame() →  │
    └─────────────────────────────────┘
    ↓
if health == 0
    ↓
display game over screen
    ↓
wait for restart button
    ↓
location.reload() (refresh page)
```

### Data Structures

#### Player (Survivor Object)
```javascript
{
    x: number,           // Current X position
    y: number,           // Current Y position
    width: 100,          // Sprite width
    height: 100,         // Sprite height
    velocity: {
        x: number,       // Horizontal velocity
        y: number        // Vertical velocity (gravity affected)
    },
    image: Image,        // Loaded image sprite
    draw: function,      // Render method
    update: function     // Update physics and draw
}
```

#### Enemy (Jombie Object)
```javascript
{
    x: number,           // Current X position
    y: number,           // Always ground level
    width: 100,          // Sprite width
    height: 100,         // Sprite height
    image: Image,        // Loaded zombie sprite
    draw: function,      // Render method
    update: function     // Move and draw
}
```

#### Bullet Object
```javascript
{
    x: number,           // Current X position
    y: number,           // Current Y position
    vx: number,          // Horizontal velocity (constant)
    vy: number,          // Vertical velocity (affected by gravity)
    radius: 7            // Bullet radius for collision
}
```

#### Block Object
```javascript
{
    x: number,           // X position (fixed)
    y: number,           // Y position (fixed)
    width: 100,          // Block width
    height: 100          // Block height
}
```

#### Game State Variables
```javascript
const canvas = ...;      // Canvas element
const c = ...;           // Canvas 2D context
const gravity = 0.2;     // Bullet gravity
let bullets = [];        // Active bullets array
let enemies = [];        // Active enemies array
let survivors = [];      // Player array
let blocks = [];         // Block obstacles array
let score = 0;           // Current score
let health = 10;         // Current health (0-10)
let ispaused = false;    // Pause state
let animationid;         // Current animation frame ID
```

### Game States

#### 1. Main Menu State
```
Display: Instructions on black background
Buttons: "Start Game"
Controls: Hidden
Enemies: Not spawning
Update Loop: Not running
```

#### 2. Playing State
```
Display: Canvas with player, enemies, bullets, blocks
UI: Score and health displayed
Controls: Visible and active
Enemies: Spawning every 2-4 seconds
Update Loop: animate() running continuously
Events: Keyboard input, mouse clicks, button clicks
```

#### 3. Paused State
```
Display: Game frozen on screen
Controls: "Resume" button visible
Update Loop: Cancelled (stopped)
Re-entry: animate() resumed
```

#### 4. Game Over State
```
Display: "GAME OVER" text, final score
Buttons: "Start New Game"
Controls: Removed/hidden
Update Loop: Cancelled
Re-entry: location.reload() (page refresh)
```

### Input Handling Flow

#### Keyboard Input
```
keydown event → handleKeyPress(event)
    ↓
switch (event.key)
    ↓
ArrowUp/Spacebar: fighter.velocity.y = -8
ArrowLeft: fighter.x -= 10, startX -= 10
ArrowRight: fighter.x += 10, startX += 10
    ↓
next animate() frame applies changes
```

#### Mouse Input
```
click event on canvas
    ↓
Calculate angle: Math.atan2(mouseY - startY, mouseX - startX)
    ↓
Calculate velocity components:
    vx = speed * Math.cos(angle)
    vy = speed * Math.sin(angle)
    ↓
Create bullet object and add to bullets array
    ↓
next animate() frame renders bullet
```

#### Button Input
```
Left Button: Triggers ArrowLeft key handler
Jump Button: Sets fighter.velocity.y = -8
Right Button: Triggers ArrowRight key handler
Pause Button: Toggles ispaused state
    ↓
if paused: cancelAnimationFrame()
if resumed: animate() called again
```

---

## ⚙️ Setup & Installation

### Prerequisites
- Modern web browser (Chrome 60+, Firefox 55+, Safari 11+, Edge 79+)
- Local web server (Python, Node.js, etc.) - required for image loading
- Sprite images (PNG format, 100x100px recommended)

### Step-by-Step Installation

#### 1. Create Project Structure
```bash
mkdir zombie-fighter
cd zombie-fighter
mkdir images
```

#### 2. Create HTML File (`index.html`)
```html
<!DOCTYPE html>
<html>
<head>
    <title>Zombie Fighter</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="gamecontainer">
        <canvas id="myCanvas"></canvas>
        <div id="controls">
            <button id="left">Left</button>
            <button id="jump">Jump</button>
            <button id="right">Right</button>
            <button id="pause">Pause</button>
        </div>
    </div>
    <script src="main.js"></script>
</body>
</html>
```

#### 3. Create CSS File (`style.css`)
```css
button {
    margin: 5px;
    padding: 10px;
    border-radius: 5px;
    cursor: pointer;
}

* {
    margin: 0px;
}

body {
    background-color: grey;
    border: 1px solid red;
}

#controls {
    display: flex;
    justify-content: center;
}

#myCanvas {
    position: relative;
    border: 1px solid red;
    background-color: black;
}

#newgame, #start {
    position: absolute;
    padding: 10px;
    margin: 5px;
    border-radius: 15px;
    cursor: pointer;
}
```

#### 4. Copy Game Logic
Copy the complete `main.js` code from the repository into your project.

#### 5. Add Sprite Images
- Download or create `survivor1.png` (100x100px, player sprite)
- Download or create `jombie1.png` (100x100px, zombie sprite)
- Place both in the `images/` folder

**Note:** If you don't have images, the game will still work but sprites won't render. Use solid colored rectangles instead:
```javascript
// In Survivor.draw() and Jombie.draw()
// Replace c.drawImage() with:
c.fillStyle = 'blue';  // or 'green' for zombies
c.fillRect(this.x, this.y, this.width, this.height);
```

#### 6. Start Local Web Server

**Using Python 3:**
```bash
cd zombie-fighter
python -m http.server 8000
# Open browser to http://localhost:8000
```

**Using Node.js (http-server):**
```bash
cd zombie-fighter
npx http-server
# Open browser to http://localhost:8080 (or shown in terminal)
```

**Using Python 2:**
```bash
cd zombie-fighter
python -m SimpleHTTPServer 8000
# Open browser to http://localhost:8000
```

**Using Live Server (VS Code Extension):**
- Install "Live Server" extension
- Right-click `index.html` → "Open with Live Server"

#### 7. Test the Game
1. Open `http://localhost:8000` in your browser
2. Click "Start Game" button
3. Use controls to move and jump
4. Click on screen to shoot zombies
5. Survive as long as possible!

---

## 🎮 How to Play

### Main Menu
```
┌─────────────────────────────────────┐
│      GENERAL INSTRUCTIONS           │
│                                     │
│ 1. Click to fire in that direction │
│ 2. Use Spacebar to jump            │
│ 3. Use Arrow keys to move          │
│ 4. Zombies speed up with score     │
│ 5. Game ends when health = 0       │
│ 6. Click Start to begin            │
│                                     │
│         [START GAME]                │
└─────────────────────────────────────┘
```

### Gameplay

#### Objective
Eliminate as many zombies as possible before your health reaches 0.

#### Scoring
- **+10 points** for each zombie eliminated
- Score increases zombie difficulty (see Difficulty Scaling)

#### Health System
- Start with **10 health**
- Lose **1 health** per zombie contact
- Game ends at **0 health**
- No way to regain health

#### Controls (Desktop)
```
↑ or SPACE  →  Jump
←          →  Move Left
→          →  Move Right
Mouse Click →  Shoot bullet in mouse direction
P Button    →  Pause/Resume
```

#### Controls (Mobile/Touch)
```
LEFT Button   →  Move left
JUMP Button   →  Jump
RIGHT Button  →  Move right
PAUSE Button  →  Pause/resume
Screen Tap    →  Shoot in tap direction
```

#### Difficulty Progression
```
Score 0-99
    └─ Zombie Speed: 2 px/frame (slow)
    └─ Easy to eliminate zombies
    └─ Good time to build up score

Score 100-199
    └─ Zombie Speed: 4 px/frame (medium)
    └─ Zombies move twice as fast
    └─ Better accuracy needed

Score 200+
    └─ Zombie Speed: 6 px/frame (fast)
    └─ Very challenging
    └─ Requires quick reactions
```

### Strategy Tips

1. **Use Blocks for Cover**: The green blocks on screen provide protection. Hide behind them!
2. **Lead Your Shots**: Zombies are moving, aim ahead of them for better hits
3. **Jump and Move**: Don't stay in one place, keep moving to avoid zombies
4. **Multiple Targets**: With spawn rate, zombies come quickly at higher scores
5. **Watch Your Health**: Each contact costs 1 health, manage your positioning
6. **Block Destruction**: Blocks get destroyed by zombies, so your cover won't last forever

### Example Gameplay

```
Time: 0:00 - Game Starts
- Score: 0, Health: 10/10
- First zombies spawn from edges
- You practice shooting

Time: 0:30 - Building Score
- Score: 70, Health: 9/10
- Zombies still manageable at speed 2
- One zombie got through

Time: 1:30 - Difficulty Spike
- Score: 100, Health: 7/10
- Zombie speed doubled to 4 px/frame
- Difficulty noticeably harder
- More zombies on screen

Time: 3:00 - High Score
- Score: 250, Health: 2/10
- Zombie speed at maximum (6 px/frame)
- Very intense gameplay
- Barely surviving

Time: 3:15 - Game Over
- Score: 270, Health: 0/10
- Last zombie touched player
- Game Over screen shows final score
- Option to play again
```

---

## 🎨 Customization Guide

### Modifying Game Difficulty

#### Change Starting Health
```javascript
// In main.js, find:
let health = 10;

// Change to:
let health = 15;  // More forgiving
// or
let health = 5;   // Very hard
```

#### Adjust Zombie Spawn Rate
```javascript
// Find the spawn code:
setInterval(() => {
    setTimeout(() => {
        // spawn logic
    }, (Math.random() * 3 + 1) * 1000);
}, 2000);

// Modify intervals:
}, 1000);  // Spawn every 1 second (faster)
}, 3000);  // Spawn every 3 seconds (slower)

// Modify delay range:
(Math.random() * 2 + 1) * 1000   // 1-3 second delay
(Math.random() * 5 + 1) * 1000   // 1-6 second delay
```

#### Change Zombie Speed Tiers
```javascript
// In Jombie.update(), find:
if (score < 100) { speed = 2; }
else if (score < 200) { speed = 4; }
else { speed = 6; }

// Modify to:
if (score < 50) { speed = 1; }
else if (score < 150) { speed = 3; }
else if (score < 300) { speed = 5; }
else { speed = 8; }
```

#### Adjust Bullet Speed
```javascript
// In canvas click event, find:
const speed = 12;

// Change to:
const speed = 8;   // Slower bullets
const speed = 15;  // Faster bullets
```

#### Adjust Gravity
```javascript
// At top of main.js:
const gravity = 0.2;

// Change to:
const gravity = 0.1;   // Less gravity (bullets go further)
const gravity = 0.5;   // More gravity (bullets drop faster)
```

### Adding New Features

#### 1. Sound Effects
```javascript
// Add to index.html before </body>:
<audio id="shootSound" src="shoot.mp3"></audio>
<audio id="hitSound" src="hit.mp3"></audio>

// In main.js, when creating bullet:
document.getElementById('shootSound').play();

// When zombie dies:
document.getElementById('hitSound').play();
```

#### 2. Power-ups
```javascript
// Add to game state:
let powerups = [];

// Create power-up object when zombie dies:
if (score % 50 === 0) {  // Every 5 zombies
    powerups.push({
        x: enemy.jombie.x,
        y: enemy.jombie.y,
        width: 30,
        height: 30,
        type: 'health'  // or 'rapid_fire'
    });
}

// Check collision with player:
powerups.forEach((p, i) => {
    if (collision(fighter, p)) {
        if (p.type === 'health' && health < 10) health++;
        powerups.splice(i, 1);
    }
});
```

#### 3. High Score System
```javascript
// In main.js:
// Save score to localStorage
if (health === 0) {
    const highScore = localStorage.getItem('highScore') || 0;
    if (score > highScore) {
        localStorage.setItem('highScore', score);
        alert(`New High Score: ${score}!`);
    }
}

// Display high score in menu:
const highScore = localStorage.getItem('highScore') || 0;
c.fillText(`High Score: ${highScore}`, canvas.width / 2 - 100, canvas.height / 2 - 50);
```

#### 4. Multiple Difficulty Modes
```javascript
// Add difficulty selection before game start:
let difficulty = 'normal';  // 'easy', 'normal', 'hard'

// Modify health based on difficulty:
if (difficulty === 'easy') health = 15;
else if (difficulty === 'hard') health = 5;

// Modify enemy speeds:
const speedMultiplier = difficulty === 'easy' ? 0.5 : 
                        difficulty === 'hard' ? 1.5 : 1;
const baseSpeed = 2 * speedMultiplier;
```

#### 5. Level System
```javascript
// Add backgrounds for different levels:
let level = 1;
let backgrounds = {
    1: 'forest.png',
    2: 'city.png',
    3: 'space.png'
};

// Change background every 300 points:
if (Math.floor(score / 300) !== level - 1) {
    level = Math.floor(score / 300) + 1;
    // Load new background
}
```

### Styling Customization

#### Change Canvas Color
```css
#myCanvas {
    background-color: darkblue;  /* Was black */
    border: 3px solid yellow;    /* Was 1px red */
}
```

#### Customize Buttons
```css
button {
    background-color: #4CAF50;
    color: white;
    border: none;
    padding: 15px 32px;
    font-size: 16px;
    border-radius: 10px;
    cursor: pointer;
    transition: 0.3s;
}

button:hover {
    background-color: #45a049;
}

button:active {
    transform: scale(0.98);
}
```

#### Change Block Color
```javascript
// In block() function:
function block(x, y) {
    c.beginPath();
    c.fillStyle = 'orange';  // Was 'green'
    c.fillRect(x, y, 100, 100);
}
```

#### Customize Text Styling
```javascript
// In animate() function, change font and color:
c.font = '30px Courier New';      // Was '20px Arial'
c.fillStyle = 'yellow';            // Was 'white'
c.fillText(`SCORE: ${score}`, ...);
```

---

## 🐛 Known Issues & Future Improvements

### Known Issues

#### 1. Image Loading Without Server
**Problem**: CORS (Cross-Origin Resource Sharing) restrictions prevent images from loading in local file:// URLs.
**Solution**: Always use a local web server (Python http.server, Node.js, etc.)

#### 2. Mobile Responsiveness
**Problem**: Game canvas may not scale properly on different screen sizes.
**Current**: Uses 99.999% width and 90% height.
**Fix**: Add responsive scaling:
```javascript
function resizeCanvas() {
    canvas.width = window.innerWidth * 0.99999;
    canvas.height = window.innerHeight * 0.9;
}
window.addEventListener('resize', resizeCanvas);
```

#### 3. Block Collision Detection Edge Cases
**Problem**: Player can sometimes partially overlap blocks.
**Current**: Checks exact edge alignment (a.x + a.width == b.x).
**Fix**: Use more generous collision buffer:
```javascript
const buffer = 5;
if (b.x + b.width + buffer > fighter.x && b.x - buffer < fighter.x + fighter.width) {
    // Collision detected
}
```

#### 4. Shooting Angle at Screen Edges
**Problem**: Angle calculation can be unpredictable if mouse is exactly at player.
**Current**: Uses Math.atan2() which handles 0 velocity.
**Note**: This is typically not an issue.

#### 5. Frame Rate Inconsistency
**Problem**: On slower devices, 60 FPS may not be maintained.
**Impact**: Gameplay feels sluggish, zombies move irregularly.
**Solution**: Implement delta time (frame-independent movement):
```javascript
let lastTime = Date.now();
function animate() {
    const currentTime = Date.now();
    const deltaTime = (currentTime - lastTime) / 16.67;  // Normalize to 60 FPS
    lastTime = currentTime;
    
    // Move enemies: this.x += speed * deltaTime;
}
```

### Future Improvements

#### Short Term
- [ ] Add sound effects (shooting, hits, game over)
- [ ] Implement high score system (localStorage)
- [ ] Add pause functionality improvements
- [ ] Create on-screen health bar visualization
- [ ] Add blood effect particles on zombie hit

#### Medium Term
- [ ] Multiple zombie types with different behaviors
- [ ] Power-up system (rapid fire, health, shield)
- [ ] Weapon variety (shotgun, laser, etc.)
- [ ] Level/stage system with different backgrounds
- [ ] Enemy boss battles every 500 points
- [ ] Combo system (consecutive kills = multiplier)

#### Long Term
- [ ] Leaderboard with online submission
- [ ] Mobile app version
- [ ] Multiplayer mode (local co-op)
- [ ] Customizable character skins
- [ ] Campaign mode with story
- [ ] Achievements and badges system
- [ ] Settings menu (difficulty, graphics, controls)
- [ ] Tutorial mode with guided gameplay

#### Technical Improvements
- [ ] Implement proper physics engine
- [ ] Add particle effects (explosions, blood)
- [ ] Create sprite animation system
- [ ] Implement proper game state management
- [ ] Add comprehensive logging and debugging tools
- [ ] Optimize collision detection (quadtree/spatial hashing)
- [ ] Separate game logic from rendering
- [ ] Add unit tests for game mechanics

---

## 🌐 Browser Compatibility

| Browser | Version | Support | Status |
|---------|---------|---------|--------|
| **Chrome** | 60+ | ✅ Full | Excellent |
| **Firefox** | 55+ | ✅ Full | Excellent |
| **Safari** | 11+ | ✅ Full | Good |
| **Edge** | 79+ | ✅ Full | Excellent |
| **Opera** | 47+ | ✅ Full | Good |
| **IE** | 11 | ❌ None | Not supported (no Canvas support) |

### Feature Support Matrix

| Feature | Required | Browser Support |
|---------|----------|-----------------|
| HTML5 Canvas | Core | All modern browsers |
| Canvas 2D Context | Core | All modern browsers |
| requestAnimationFrame | Core | 99%+ of modern browsers |
| Image Object | Sprites | All modern browsers |
| Event Listeners | Input | All modern browsers |
| localStorage | Optional (high score) | All modern browsers |

### Performance Notes

- **Desktop**: 60 FPS consistently
- **Laptop**: 50-60 FPS typical
- **Mobile Browsers**: 30-60 FPS depending on device
- **Older Devices**: May drop to 20-30 FPS

---

## 📁 Repository Information

**Repository Name**: Zombie_fighter
**Repository ID**: 817559974
**Owner**: Saiprakashreddy9
**Created**: June 20, 2024
**Last Updated**: June 15, 2025
**Visibility**: Public
**License**: MIT (Open source)

### Repository Size
- Total Size: 328 KB
- Main Game File (main.js): 11.16 KB
- Other Files: ~5 KB

### Files in Repository
1. **index.html** (593 bytes) - HTML structure
2. **main.js** (11,160 bytes) - Complete game logic
3. **style.css** (436 bytes) - Styling
4. **images/** - Sprite directory (not in repo, add yourself)
5. **README.md** - Documentation

---

## 🤝 Contributing

Feel free to fork this project and submit pull requests for:
- Bug fixes
- Performance improvements
- New features
- Documentation enhancements
- Sprite asset improvements

### How to Contribute

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📝 Quick Reference

### Game Constants
```javascript
canvas.width = innerWidth * 0.99999;  // Canvas width
canvas.height = innerHeight * 0.9;    // Canvas height
gravity = 0.2;                        // Bullet gravity
sgravity = 0.1;                       // Player gravity
health = 10;                          // Starting health
score_per_kill = 10;                  // Points per zombie
bullet_speed = 12;                    // Initial bullet speed
bullet_radius = 7;                    // Bullet collision radius
sprite_size = 100;                    // Sprite dimensions (100x100)
spawn_interval = 2000;                // Spawn every 2 seconds
spawn_delay_min = 1000;               // Min delay before spawn
spawn_delay_max = 4000;               // Max delay before spawn
```

### Quick Commands

**To modify game difficulty:**
1. Edit `health` variable (starting health)
2. Edit `speed` values in `Jombie.update()` (zombie speeds)
3. Edit spawn intervals (frequency of zombies)
4. Edit `gravity` (affects bullet trajectory)

**To add sprite images:**
1. Create `images/` folder
2. Add `survivor1.png` (player sprite)
3. Add `jombie1.png` (zombie sprite)
4. Restart local server

**To change styling:**
1. Modify `style.css` for button/canvas styling
2. Modify canvas drawing calls in `main.js` for game appearance

### Troubleshooting

| Issue | Solution |
|-------|----------|
| Sprites not loading | Use local web server, not file:// URL |
| Game runs slow | Check browser console for errors, reduce spawn rate |
| Controls not working | Ensure canvas is focused (click on it) |
| Buttons not appearing | Check CSS display properties |
| Zombies don't move | Check score variable isn't set too high |
| Game won't start | Click "Start Game" button, check console |

---

## 📚 Learning Resources

### Canvas API
- [MDN Canvas Documentation](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)
- [Canvas Tutorial](https://www.w3schools.com/graphics/canvas_intro.asp)

### Game Development
- [HTML5 Games Fundamentals](https://www.gamedev.net/learn/)
- [Simple 2D Collision Detection](https://developer.mozilla.org/en-US/docs/Games/Techniques/2D_collision_detection)

### JavaScript
- [JavaScript Game Development](https://www.javascript.info/)
- [requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)

---

## 💡 Project Highlights

✨ **What This Project Teaches:**
- HTML5 Canvas rendering and animation
- Game loop architecture
- Collision detection algorithms (AABB)
- Physics simulation (gravity, velocity)
- Event handling (keyboard, mouse, touch)
- Object-oriented programming with classes
- Game state management
- Browser APIs and performance optimization

🎮 **Perfect For:**
- Learning game development fundamentals
- Portfolio project demonstration
- Understanding game architecture
- Practice with vanilla JavaScript
- Browser-based game concepts

---

**Status**: ✅ Complete and playable
**Last Updated**: June 2025
**Created for**: Game development learning and portfolio showcase

