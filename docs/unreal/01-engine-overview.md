# Unreal Engine Overview

Unreal Engine is one of the most powerful game engines available. This guide explains how it's organized and how the pieces fit together.

## What You'll Learn

- What a game engine does
- Unreal Engine's core architecture
- Key concepts and terminology
- How to navigate the editor

## What Is a Game Engine?

A **game engine** is a framework for building games. It provides:

- **Rendering**: Drawing 3D graphics to the screen
- **Physics**: Simulating gravity, collisions, movement
- **Audio**: Playing sounds and music
- **Input**: Handling keyboard, mouse, controller
- **Networking**: Multiplayer support
- **Asset Management**: Loading models, textures, sounds
- **Tools**: Editors for building content

Without an engine, you'd build all of this yourself. Engines let you focus on your actual game.

## Unreal's Architecture

### High-Level Structure

```
Unreal Engine
├── Core Systems
│   ├── Object System (UObject)
│   ├── Memory Management
│   ├── Reflection System
│   └── Serialization
│
├── Game Framework
│   ├── Game Mode
│   ├── Game State
│   ├── Player Controller
│   ├── Pawn/Character
│   └── HUD
│
├── World & Actors
│   ├── World
│   ├── Levels
│   ├── Actors
│   └── Components
│
├── Rendering
│   ├── Materials
│   ├── Lighting
│   ├── Post Processing
│   └── Cameras
│
├── Physics
│   ├── Collision
│   ├── Physics Bodies
│   └── Constraints
│
└── Tools
    ├── Blueprint Editor
    ├── Material Editor
    ├── Level Editor
    └── Sequencer
```

### The World

Everything in your game exists in a **World**. The World contains:

- **Levels**: Containers for game content
- **Actors**: Objects that exist in the world
- **Systems**: Physics, rendering, audio, etc.

```
World
├── Persistent Level
│   ├── Player Character
│   ├── Enemies
│   ├── Items
│   └── Environment
├── Streaming Level 1 (loaded/unloaded as needed)
└── Streaming Level 2
```

### Levels

A **Level** is a collection of Actors - think of it as a container or scene.

**Persistent Level**: Always loaded
**Streaming Levels**: Load/unload dynamically (for large worlds)

You can have one big level or many small ones that stream in.

### Actors

An **Actor** is anything that can be placed in a level:
- Characters
- Lights
- Cameras
- Triggers
- Static meshes
- Sound emitters
- Literally anything in the world

Actors are the fundamental building blocks of Unreal games.

### Components

**Components** are pieces that attach to Actors:

```
Player Character (Actor)
├── Skeletal Mesh Component (the 3D model)
├── Camera Component (player's view)
├── Capsule Component (collision)
├── Character Movement Component (handles walking, jumping)
└── Audio Component (footstep sounds)
```

Components give Actors their functionality. An Actor without components doesn't do much.

## Key Classes

### UObject

The base class for almost everything in Unreal:
- Provides memory management
- Enables reflection (runtime type info)
- Supports serialization (saving/loading)
- Integrates with garbage collection

### AActor

Base class for objects in the world:
- Has a transform (position, rotation, scale)
- Can have components
- Participates in gameplay
- Can be spawned and destroyed

### APawn

An Actor that can be "possessed" (controlled):
- Can receive input
- Represents a player or AI entity
- Base for characters and vehicles

### ACharacter

A Pawn specialized for humanoid characters:
- Built-in movement (walking, jumping, swimming)
- Skeletal mesh support
- Animation system integration

### APlayerController

Handles player input and camera:
- Receives input from player
- Possesses a Pawn
- Manages HUD
- Each player has one controller

### AGameModeBase

Defines the rules of your game:
- What classes to spawn
- Win/lose conditions
- Player spawning
- Game flow

Only exists on the server in multiplayer.

## The Game Framework

Unreal has a specific framework for organizing game logic:

```
                    ┌─────────────┐
                    │  GameMode   │  Rules, spawning
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │  GameState  │  Shared state (score, time)
                    └──────┬──────┘
                           │
         ┌─────────────────┼─────────────────┐
         │                 │                 │
    ┌────▼────┐       ┌────▼────┐      ┌────▼────┐
    │ Player  │       │ Player  │      │ Player  │
    │Controller│       │Controller│      │Controller│
    └────┬────┘       └────┬────┘      └────┬────┘
         │                 │                 │
    ┌────▼────┐       ┌────▼────┐      ┌────▼────┐
    │  Pawn   │       │  Pawn   │      │  Pawn   │
    └─────────┘       └─────────┘      └─────────┘
```

### GameMode

- Defines rules and spawning
- Server-only in multiplayer
- Examples:
  - "Players have 3 lives"
  - "Match lasts 10 minutes"
  - "Use this player class"

### GameState

- Holds state replicated to all players
- Examples:
  - Team scores
  - Round number
  - Match timer

### PlayerController

- One per player
- Handles input
- Possesses pawns
- Manages UI

### Pawn/Character

- The actual entity in the world
- What the player sees and controls
- Has components for mesh, collision, movement

## Editor Overview

### Main Windows

**Viewport**: 3D view of your level
- Navigate: Right-click + WASD
- Orbit: Alt + Left-click
- Zoom: Scroll wheel

**Content Browser**: All your assets
- Models, textures, sounds, Blueprints
- Organized in folders

**World Outliner**: List of all Actors in the level
- Hierarchical view
- Search and filter

**Details Panel**: Properties of selected object
- Transform, components, settings
- Where you configure everything

**Modes Panel**: Different editing modes
- Place mode (add objects)
- Paint mode (paint terrain)
- Landscape mode (create terrain)
- Foliage mode (place trees, grass)

### Key Menus

**File**: Project management, packaging
**Edit**: Undo, preferences, project settings
**Window**: Open/close panels
**Build**: Compile lighting, navigation, etc.
**Play**: Test your game

### Project Settings

`Edit > Project Settings` - Critical configuration:

- **Maps & Modes**: Default maps and game mode
- **Input**: Key bindings
- **Physics**: Gravity, collision defaults
- **Rendering**: Graphics settings
- **Packaging**: Build settings

## File Structure

Unreal projects have a standard structure:

```
MyProject/
├── Config/                  # Settings files
│   ├── DefaultEngine.ini
│   ├── DefaultGame.ini
│   └── DefaultInput.ini
│
├── Content/                 # All assets
│   ├── Characters/
│   ├── Blueprints/
│   ├── Materials/
│   ├── Maps/
│   └── ...
│
├── Source/                  # C++ code (if using)
│   └── MyProject/
│       ├── MyProject.h
│       ├── MyProject.cpp
│       └── ...
│
├── Intermediate/            # Build files (can delete)
├── Saved/                   # Logs, autosaves
└── MyProject.uproject       # Project file
```

### Important File Types

| Extension | Type |
|-----------|------|
| .uproject | Project file |
| .umap | Level/Map |
| .uasset | Asset (mesh, texture, Blueprint, etc.) |
| .cpp/.h | C++ source code |
| .ini | Configuration files |

## Coordinate System

Unreal uses a left-handed coordinate system:

```
        Z (Up)
        │
        │
        │
        └────────── X (Forward)
       /
      /
     Y (Right)
```

- **X**: Forward/Back
- **Y**: Left/Right
- **Z**: Up/Down

Units are in **centimeters**. A character is typically ~180 cm tall.

## Getting Help

### In-Editor

- **Hover** over any property for tooltip
- **F1** opens documentation for selected item
- **Right-click** often reveals options

### Documentation

- [Official Documentation](https://docs.unrealengine.com)
- Right-click any node in Blueprints → "View Documentation"

### Community

- Unreal Engine Forums
- Unreal Slackers Discord
- YouTube tutorials
- Stack Overflow

## Exercises

### Exercise 1: Explore the Editor

1. Create a new Third Person project
2. Find these panels: Viewport, Content Browser, World Outliner, Details
3. Select the player character in the World Outliner
4. Look at its components in the Details panel

### Exercise 2: Navigate the Viewport

Practice viewport navigation:
1. Right-click + WASD to fly around
2. Alt + Left-click to orbit
3. Find the player start location
4. Find where the floor ends

### Exercise 3: Examine the Game Framework

1. Open the Project Settings (Edit > Project Settings)
2. Find Maps & Modes
3. Note what GameMode is being used
4. Open that GameMode Blueprint (Content Browser > search)

## Key Takeaways

1. **Worlds** contain **Levels** contain **Actors** contain **Components**
2. **GameMode** defines rules, **GameState** holds shared data
3. **PlayerController** handles input, **Pawn/Character** is the player entity
4. **Everything inherits from UObject**
5. **The editor** is your main tool for building games

## What's Next

Now that you understand the overall structure, let's compare the two ways to program in Unreal: Blueprints and C++.

**[Continue to Blueprints vs C++](02-blueprints-vs-cpp.md)**

---

## Quick Reference

| Concept | Purpose |
|---------|---------|
| World | Container for everything |
| Level | A scene of Actors |
| Actor | Object in the world |
| Component | Piece of an Actor |
| GameMode | Game rules |
| PlayerController | Player input |
| Pawn/Character | Controllable entity |
