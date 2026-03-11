# Electronic Battleship Game on FPGA

A digital implementation of the classic Battleship game using Verilog HDL on a Tang Nano 9K FPGA board. Players compete in a two-player strategy game with interactive button controls and real-time feedback via LEDs and seven-segment displays.

**Course:** CS303 Digital Systems Design  
**Term:** Fall 2024-2025

## 📋 Project Overview

This project implements a complete two-player Battleship game entirely in hardware using Verilog. The game runs on a Tang Nano 9K FPGA, featuring real-time game state management, debounced button inputs, and multi-display output for intuitive gameplay.

### Key Features

- **Two-Player Gameplay:** Full turn-based Battleship game with Player A and Player B
- **Hardware-Based Logic:** Entire game logic implemented in Verilog with state machine control
- **Interactive UI:** 
  - 4 Push buttons for X/Y coordinate input and actions
  - 4 Switches for ship placement
  - 8 LEDs for hit/miss/win indicators
  - 4 Seven-segment displays for game state and score display
- **Debounced Input:** Noise filtering for reliable button input processing
- **Clock Division:** Frequency division for timing critical operations (27 MHz → 50 Hz)
- **State-Driven Architecture:** 16-state finite state machine managing game flow

## 🎮 Game States

The game progresses through the following states:

| State | Description |
|-------|-------------|
| **IDLE** | Waiting for game start |
| **SHOW_A** | Displaying Player A's board |
| **A_IN** | Player A placing ships |
| **ERROR_A** | Invalid ship placement for Player A |
| **SHOW_B** | Displaying Player B's board |
| **B_IN** | Player B placing ships |
| **ERROR_B** | Invalid ship placement for Player B |
| **SHOW_SCORE** | Displaying current scores |
| **A_SHOOT** | Player A's turn to attack |
| **A_SINK** | Player A sunk an opponent ship |
| **A_WIN** | Player A wins the game |
| **B_SHOOT** | Player B's turn to attack |
| **B_SINK** | Player B sunk an opponent ship |
| **B_WIN** | Player B wins the game |

## 📁 Project Structure

```
Electronic-Battleship-Game/
├── top.v               # Top-level module connecting all components
├── battleship.v        # Main game logic and state machine
├── clk_divider.v       # Clock frequency division module
├── debouncer.v         # Button input debouncing module
├── ssd.v              # Seven-segment display decoder
├── tangnano9k.cst     # Constraint file for board pin mapping
├── Project.json       # Project configuration
├── Project_pnr.json   # Place and Route configuration
└── README.md          # This file
```

## 🔧 Verilog Modules

### `top.v` - Top-Level Module
The main interface connecting all components. Instantiates the game logic, debouncer, clock divider, and display modules.

**Ports:**
- `clk`: Board input clock (27 MHz)
- `sw[3:0]`: Input switches (ship placement)
- `btn[3:0]`: Push buttons (coordinate input and actions)
- `led[7:0]`: Output LEDs (game status)
- `seven[7:0]`: Seven-segment display outputs
- `segment[3:0]`: Display segment selectors

### `battleship.v` - Game Logic Core
Implements the complete game engine with state machine, ship placement validation, and hit/miss detection.

**Key Features:**
- 16-state finite state machine
- Two 16-bit game boards (A and B) representing 4×4 grids
- Score tracking and win detection
- Input validation for ship placement
- Hit/miss detection and display updates

### `clk_divider.v` - Clock Divider
Reduces the 27 MHz board clock to a lower frequency for game timing.
- Input: 27 MHz clock
- Output: Divided clock (configurable, default ~50 Hz)
- Parameter: `toggle_value` controls output frequency

### `debouncer.v` - Input Debouncer
Eliminates button noise through synchronization and metastability filtering.
- Uses shift register technique for noise suppression
- Detects clean rising edges on button inputs
- Provides stable signal to game logic

### `ssd.v` - Seven-Segment Display Decoder
Converts 4-bit binary values to seven-segment display codes for score and status display.

## 🎯 Gameplay Instructions

### Setup Phase
1. **Player A Setup:** Places 4 ships on their 4×4 grid using X/Y coordinates (0-3)
2. **Player B Setup:** Places 4 ships on their 4×4 grid using X/Y coordinates (0-3)

### Playing Phase
1. Players alternate turns attacking opponent's grid
2. Enter target coordinates using buttons (X and Y)
3. **Hit:** LED indicates hit, score increases
4. **Miss:** LED indicates miss
5. **Win:** First player to hit all 4 opponent ships wins

### Input Controls
- `btn[3:0]`: Navigate and confirm selections
- `sw[3:0]`: Binary input for X and Y coordinates
- `led[7:0]`: Game status feedback
- Seven-segment displays: Current state, scores, and coordinates

## 🛠️ Building and Deployment

### Prerequisites
- Tang Nano 9K FPGA Board
- EDA IDE (Gowin EDA or compatible)
- USB cable for board programming

### Steps
1. Open the project in EDA IDE: `Project.json`
2. Review the board pin constraints: `tangnano9k.cst`
3. Synthesize and Place & Route
4. Download bitstream to Tang Nano 9K board

## 📊 Game Board Representation

Each player's board is represented as a 16-bit register (for a 4×4 grid):
- Bit set to 1: Ship present at that location
- Bit set to 0: Empty water

Example 4×4 board (16 bits):
```
Y=3: [1][0][1][0]  → bits 15-12
Y=2: [0][1][0][0]  → bits 11-8
Y=1: [1][0][0][1]  → bits 7-4
Y=0: [0][1][0][0]  → bits 3-0
     X=0,1,2,3
```

## 📝 Technical Specifications

- **FPGA Board:** Tang Nano 9K (Gowin)
- **Clock Frequency:** 27 MHz (internal)
- **Game Clock:** ~50 Hz (divided)
- **Game Grid:** 4×4 per player
- **Ships per Player:** 4
- **HDL Language:** Verilog 2001
- **State Machine:** 16 states (4-bit encoded)

## 📚 Learning Outcomes

This project demonstrates:
- Hardware state machine design and implementation
- Synchronous digital logic design
- Input/output interface management
- Clock domain crossing and synchronization
- Combinatorial and sequential logic
- FPGA-specific design patterns

**Author:** CS303 Course Project  
**Last Updated:** Fall 2024-2025
