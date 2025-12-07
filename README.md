# Soccer-Minigame
# README – MIPS Bitmap Soccer Game

## Overview
This project implements an interactive **soccer mini-game** using **MIPS assembly**.  
The program runs on **MARS or QtSPIM** and uses memory-mapped I/O for:

- Bitmap display (32×64 grid, 2048 pixels)
- Keyboard input (MMIO registers)

The player controls a **yellow ball** using `W`, `A`, `S`, `D` keys and must score **3 goals** to win.  
The game displays:

- A start menu image
- A soccer field background
- Movable player ball
- Random goal (posts + net)
- Score icons (mini soccer balls)
- Text prompts and warnings

---

## Features

### ✓ Bitmap Rendering
- Renders full field background  
- Draws player ball  
- Draws randomly placed goal  
- Draws score icons  
- Shows a detailed start picture  

### ✓ Player Movement (Keyboard)
| Key | Action |
|-----|--------|
| `w` | Move up |
| `s` | Move down |
| `a` | Move left |
| `d` | Move right |

### ✓ Gameplay Mechanics
- Random goal placement  
- Goal detection  
- Boundary collision detection  
- Score tracking  
- Win condition after 3 goals  
- Reset behavior  

### ✓ Start Menu
Press:
- `1` → Start game  
- `2` → Exit program  

---

## Project Structure

### **Data Section**
Contains:
- Color constants  
- `display_buffer` and `squares` (8192 bytes each)  
- `start_picture` bitmap  
- Game variables (`ball_pos`, `goal_pos`, `goals_count`, etc.)  
- Prompts and message strings  

### **Text Section**
Contains all game logic:

#### 1. Start Screen
- Copies `start_picture` into display buffer  
- Waits for key `'1'` or `'2'`  

#### 2. Game Initialization
- Clears field  
- Draws background  
- Sets ball start position  
- Generates new random goal  

#### 3. Rendering Engine
- DrawBall  
- DrawBackground  
- DrawGoal  
- DrawScore  
- DisplayText  

#### 4. Game Loop
- Waits for keyboard input  
- Moves ball accordingly  
- Checks:
  - Goal scoring  
  - Boundaries  
  - Game over  

#### 5. Utility Functions
- RandomGoalPosition  
- Pixel write helpers  
- Collision checks  
- Display copy routines  

---

## Memory-Mapped I/O Requirements

### **Keyboard**
 -0xFFFF0000 – Ready flag
 -0xFFFF0004 – ASCII value of key
 
### **Bitmap Display Settings (MARS Tool)** 
-Display Width: 64
-Display Height: 32
-Unit Width: 4 bytes
-Unit Height: 4 bytes
-Base Address: squares or display_buffer 

---

## How to Run the Game

### **In MARS:**
1. Open MARS  
2. Load the `.asm` file  
3. Open **Tools → Bitmap Display**  
4. Configure settings:  
- Width = 64
- Height = 32
- Unit Width = 4
- Unit Height = 4
- Base Address = squares
5. Open **Tools → Keyboard and Display MMIO Simulator**  
6. Assemble (`F3`) and Run (`F5`)  
7. On the start screen, press:
- `1` to start  
- `2` to exit  

### **Controls**
- Move ball: **W A S D**  
- Score 3 goals to win  

---

## Gameplay Rules

### **Goal**
When the ball enters the net area:
- Score increments  
- Score icon disappears  
- New goal spawns  

### **Boundary Collision**
If ball exits play area:
- Warning message appears  
- Ball resets to center  
- Score does NOT change  

### **Winning**
After 3 goals:
- "Game Over – You Won!" message appears  
- Game stops  

---

## Customization

### Change Ball Color

yellow: .word 0xe8e337

### Change Ball Color
- Modify loops under:
DrawLeftPost
DrawTopBar
Grey Fill Rows
Grey Fill Cols

### Known Limitations
- Display fixed at 32×64 pixels
- No diagonal movement
- Random goal must stay in bounds
- Bitmap arrays must match 2048 pixels

### Future Improvements
- Animated ball movement
- Multiple levels
- Additional obstacles
- Sound effects (syscall 31)
- Double buffering for smooth rendering
