# Simple Game Project: Collector

Let's build a complete game from scratch. We'll create "Collector" - a game where you move around collecting items while avoiding hazards.

## What We're Building

**Collector**: A simple 3D game where:
- Player controls a character that can move and jump
- Collectible items spawn in the level
- Hazards damage the player
- Score increases when items are collected
- Game ends when health reaches zero

## What You'll Learn

- Creating a project from scratch
- Building a playable character
- Implementing game mechanics
- Creating UI
- Putting it all together

## Project Structure

```
Collector/
├── 01-project-setup.md      # Create the project
├── 02-player-character.md   # Build the player
├── 03-collectibles.md       # Create pickups
├── 04-hazards.md           # Add dangers
├── 05-game-mode.md         # Game rules
├── 06-user-interface.md    # HUD and menus
├── 07-polish.md            # Sound and effects
└── 08-next-steps.md        # Where to go from here
```

## Prerequisites

Before starting, make sure you've completed:
- [x] Windows Prerequisites setup
- [x] Unreal Engine installed
- [x] Read Programming Fundamentals
- [x] Read Unreal Engine basics

## Time Estimate

This project takes approximately 4-8 hours to complete, depending on your pace.

## Let's Begin!

**[Start with Project Setup →](01-project-setup.md)**

---

# Part 1: Project Setup

## Step 1: Create the Project

1. Open Epic Games Launcher
2. Launch Unreal Engine
3. In the Project Browser:
   - Select **Games**
   - Choose **Blank** template
   - Select **Blueprint** (we'll learn with visual scripting first)
   - Set quality to **Maximum**
   - Uncheck **Starter Content** (we'll make our own)
4. Choose location: `C:\UnrealProjects\`
5. Name: `Collector`
6. Click **Create**

## Step 2: Understand What We Have

The blank template gives us almost nothing - perfect for learning!

**Content Browser** should show:
```
Content/
└── (empty)
```

**World Outliner** shows:
- Atmospheric Fog
- Floor
- Light Source
- Player Start
- Sky Sphere
- SkyLight

## Step 3: Create Folder Structure

Organization matters! Create these folders in Content Browser:

Right-click → New Folder:
```
Content/
├── Blueprints/
│   ├── Characters/
│   ├── Pickups/
│   └── Hazards/
├── Maps/
├── Materials/
├── UI/
└── Audio/
```

## Step 4: Save the Level

1. File → Save Current Level As...
2. Navigate to `Content/Maps/`
3. Name it `MainLevel`
4. Click Save

## Step 5: Set Default Map

1. Edit → Project Settings
2. Go to **Maps & Modes**
3. Set **Editor Startup Map** to `MainLevel`
4. Set **Game Default Map** to `MainLevel`

Now the game will always start with our level.

## Step 6: Create a Simple Ground

Let's improve the default floor:

1. Delete the existing floor (select in World Outliner, press Delete)
2. Add a new floor:
   - Place Actors panel (left side) → Basic → Cube
   - Drag into viewport
3. Scale it: In Details panel
   - Scale: X=50, Y=50, Z=1 (makes a large flat surface)
   - Location: X=0, Y=0, Z=0

## Step 7: Add Some Platforms

Add a few raised platforms for interest:

**Platform 1:**
- Add another Cube
- Location: X=500, Y=0, Z=100
- Scale: X=5, Y=5, Z=1

**Platform 2:**
- Add another Cube
- Location: X=-500, Y=300, Z=200
- Scale: X=5, Y=5, Z=1

**Platform 3:**
- Add another Cube
- Location: X=0, Y=-500, Z=150
- Scale: X=8, Y=3, Z=1

## Step 8: Add Boundary Walls

Keep the player from falling off:

Add four walls around the edges:
- North Wall: Location (0, 2500, 100), Scale (50, 1, 4)
- South Wall: Location (0, -2500, 100), Scale (50, 1, 4)
- East Wall: Location (2500, 0, 100), Scale (1, 50, 4)
- West Wall: Location (-2500, 0, 100), Scale (1, 50, 4)

## Step 9: Create Basic Materials

Let's add some color!

### Ground Material

1. Right-click in Content/Materials → Material
2. Name it `M_Ground`
3. Double-click to open
4. In the Material Editor:
   - Hold '3' and click to create a Constant3Vector (color)
   - Set color to a gray: (0.2, 0.2, 0.2)
   - Connect to Base Color
5. Click Save
6. Drag material onto the ground cube

### Platform Material

1. Create another material: `M_Platform`
2. Color: Blue (0.1, 0.1, 0.5)
3. Apply to all platforms

### Wall Material

1. Create: `M_Wall`
2. Color: Dark red (0.3, 0.1, 0.1)
3. Apply to walls

## Step 10: Test the Level

1. Click Play (green arrow) or press Alt+P
2. You should see the level but can't move (no player yet!)
3. Press Escape to stop

## Checkpoint

You should now have:
- [x] A new project named "Collector"
- [x] Organized folder structure
- [x] A level with ground, platforms, and walls
- [x] Basic materials applied
- [x] Project settings configured

Save everything! (Ctrl+Shift+S)

## What's Next

We have a level, but no player! Let's create a playable character.

**[Continue to Player Character →](02-player-character.md)**

---

# Part 2: Player Character

## Step 1: Create Character Blueprint

1. Content Browser → Blueprints/Characters
2. Right-click → Blueprint Class
3. Select **Character** as parent
4. Name it `BP_PlayerCharacter`
5. Double-click to open

## Step 2: Add Visual Representation

The Character class gives us:
- Capsule Component (collision)
- Arrow Component (forward direction)
- Character Movement Component (built-in movement)

Let's add a visible mesh:

1. In Components panel, click Add
2. Search for "Static Mesh"
3. Add Static Mesh Component
4. In Details, set Static Mesh to "Cube" (from Engine Content)
5. Adjust:
   - Location: Z = -90 (center in capsule)
   - Scale: X=0.5, Y=0.5, Z=1

Now we can see our player!

## Step 3: Add a Camera

1. Add Component → Camera
2. Set Location: X=-300, Y=0, Z=100
3. Set Rotation: Pitch=-20 (looks down slightly)

This creates a third-person view behind the player.

## Step 4: Add Spring Arm (Better Camera)

A Spring Arm prevents camera clipping through walls:

1. Delete the Camera component
2. Add Component → Spring Arm
3. Location: Z=50
4. Rotation: Pitch=-20
5. Target Arm Length: 300
6. Enable "Do Collision Test"
7. Add Component → Camera (as child of Spring Arm)

## Step 5: Setup Input

### Create Input Mapping

1. Edit → Project Settings → Input
2. Under Action Mappings, add:
   - **Jump**: Spacebar
3. Under Axis Mappings, add:
   - **MoveForward**: W (scale 1.0), S (scale -1.0)
   - **MoveRight**: D (scale 1.0), A (scale -1.0)
   - **Turn**: Mouse X (scale 1.0)
   - **LookUp**: Mouse Y (scale -1.0)

### Implement Movement in Blueprint

1. Open BP_PlayerCharacter
2. Go to Event Graph tab

**Add Movement Input:**

```
Event Tick
    │
    ▼
Get Input Axis Value "MoveForward"
    │
    ▼
Add Movement Input
├── World Direction: Get Actor Forward Vector
└── Scale Value: (from axis)
```

Do the same for MoveRight (using Get Actor Right Vector).

**Add Camera Control:**

```
Event Tick
    │
    ▼
Get Input Axis Value "Turn"
    │
    ▼
Add Controller Yaw Input
```

```
Event Tick
    │
    ▼
Get Input Axis Value "LookUp"
    │
    ▼
Add Controller Pitch Input
```

**Add Jump:**

```
Input Action Jump (Pressed)
    │
    ▼
Jump
```

## Step 6: Configure Character Movement

Select the Character Movement Component and adjust:

- Max Walk Speed: 600
- Jump Z Velocity: 600
- Air Control: 0.2
- Gravity Scale: 2.0

These values feel good for a platformer-style game.

## Step 7: Configure Camera Settings

1. Select the Player Character
2. In Details, find "Pawn" section
3. Enable "Use Controller Rotation Yaw"
4. On Spring Arm, enable "Use Pawn Control Rotation"

## Step 8: Add Player Properties

We need health and score! Add variables:

1. In My Blueprint panel, under Variables, click +
2. Add:
   - **MaxHealth** (Float) = 100
   - **CurrentHealth** (Float) = 100
   - **Score** (Integer) = 0

Make them editable:
- Click the eye icon next to each to make them Blueprint Read/Write

## Step 9: Create Health Functions

### TakeDamage Function

1. In My Blueprint → Functions → + Add
2. Name: `TakeDamage`
3. Add Input: `DamageAmount` (Float)
4. Implementation:

```
TakeDamage(DamageAmount)
    │
    ▼
CurrentHealth = CurrentHealth - DamageAmount
    │
    ▼
Branch: CurrentHealth <= 0?
    │
    ├── True → Die (custom event)
    └── False → (end)
```

### Heal Function

1. Add Function: `Heal`
2. Input: `HealAmount` (Float)
3. Implementation:

```
Heal(HealAmount)
    │
    ▼
CurrentHealth = Min(CurrentHealth + HealAmount, MaxHealth)
```

### AddScore Function

1. Add Function: `AddScore`
2. Input: `Points` (Integer)
3. Implementation:

```
AddScore(Points)
    │
    ▼
Score = Score + Points
```

## Step 10: Use Our Character

### Set as Default Pawn

1. Edit → Project Settings → Maps & Modes
2. Under Default Modes:
   - Default Pawn Class: BP_PlayerCharacter

### Place Player Start

1. Make sure there's a Player Start in the level
2. Position it above the ground (so player doesn't spawn inside)

## Step 11: Test!

1. Press Play
2. You should be able to:
   - Move with WASD
   - Look around with mouse
   - Jump with Spacebar

## Troubleshooting

**Can't move?**
- Check Input mappings are correct
- Check Movement Input nodes are connected to Tick

**Camera spinning wildly?**
- Check "Use Controller Rotation" settings
- Check Mouse axis scale values (might need to be smaller)

**Falling through floor?**
- Check capsule collision is enabled
- Check floor has collision

## Checkpoint

You should now have:
- [x] A playable character with visible mesh
- [x] Third-person camera with spring arm
- [x] WASD movement and jumping
- [x] Health and score variables
- [x] Functions for taking damage, healing, adding score

Save everything!

## What's Next

Time to add things to collect!

**[Continue to Collectibles →](03-collectibles.md)**
