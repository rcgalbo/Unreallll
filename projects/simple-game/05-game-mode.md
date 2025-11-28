# Part 5: Game Mode

The Game Mode defines the rules of your game. Let's create proper win/lose conditions.

## Step 1: Create Game Mode Blueprint

1. Content Browser → Blueprints
2. Right-click → Blueprint Class
3. Parent: **Game Mode Base**
4. Name: `BP_CollectorGameMode`
5. Double-click to open

## Step 2: Configure Default Classes

In the Class Defaults:

- Default Pawn Class: `BP_PlayerCharacter`
- HUD Class: None (we'll set this later)
- Player Controller Class: Default
- Game State Class: Default

## Step 3: Set as Default Game Mode

1. Edit → Project Settings → Maps & Modes
2. Default GameMode: `BP_CollectorGameMode`

Or set per-level:
1. Window → World Settings
2. GameMode Override: `BP_CollectorGameMode`

## Step 4: Add Game State Variables

In BP_CollectorGameMode:

```
Variables:
- TotalCollectibles (Integer) = 0
- CollectedCount (Integer) = 0
- GameActive (Boolean) = true
- GameWon (Boolean) = false
```

## Step 5: Count Collectibles on Start

```
Event BeginPlay
    │
    ▼
Get All Actors of Class (BP_Collectible)
    │
    ▼
Get Array Length
    │
    ▼
Set TotalCollectibles
```

Now we know how many collectibles exist in the level.

## Step 6: Create Collection Reporting

When a collectible is picked up, it should tell the Game Mode.

### In BP_Collectible

After player collects:
```
Get Game Mode → Cast to BP_CollectorGameMode
    │
    ▼
Call "OnCollectibleCollected" (custom function)
```

### In BP_CollectorGameMode

```
Function: OnCollectibleCollected
    │
    ▼
CollectedCount = CollectedCount + 1
    │
    ▼
Branch: CollectedCount >= TotalCollectibles?
    │
    ├── True → Call WinGame
    └── False → Continue
```

## Step 7: Win Condition

```
Function: WinGame
    │
    ▼
Set GameActive = false
Set GameWon = true
    │
    ▼
Print String "YOU WIN!"
    │
    ▼
Get Player Controller → Set Show Mouse Cursor = true
    │
    ▼
Set Timer (3 seconds) → Show Win Screen (UI, later)
```

## Step 8: Lose Condition

When player dies, inform Game Mode.

### In BP_PlayerCharacter (Die event)

```
Custom Event: Die
    │
    ▼
Get Game Mode → Cast to BP_CollectorGameMode
    │
    ▼
Call "OnPlayerDied"
```

### In BP_CollectorGameMode

```
Function: OnPlayerDied
    │
    ▼
Set GameActive = false
Set GameWon = false
    │
    ▼
Print String "GAME OVER"
    │
    ▼
Get Player Controller → Set Show Mouse Cursor = true
    │
    ▼
Set Timer (3 seconds) → Show Game Over Screen (UI, later)
```

## Step 9: Restart Function

```
Function: RestartGame
    │
    ▼
Open Level (by Name) → "MainLevel"
```

Alternative using console command:
```
Execute Console Command → "RestartLevel"
```

## Step 10: Pause Game (Optional)

### Add Pause Input

Project Settings → Input → Action Mapping:
- **Pause**: Escape key

### In BP_CollectorGameMode or PlayerController

```
Input Action Pause (Pressed)
    │
    ▼
Branch: Is Game Paused?
    │
    ├── True →
    │   Set Game Paused = false
    │   Hide Pause Menu
    │
    └── False →
        Set Game Paused = true
        Show Pause Menu
```

## Step 11: Track High Score (Optional)

Save the best score between sessions:

### Save Score

```
Function: SaveHighScore
    │
    ▼
Branch: Current Score > Saved High Score?
    │
    ├── True →
    │   Create Save Game Object
    │   Set High Score property
    │   Save Game to Slot "HighScore" Index 0
    │
    └── False → Skip
```

### Load Score

```
Event BeginPlay
    │
    ▼
Does Save Game Exist ("HighScore", 0)?
    │
    ├── True →
    │   Load Game from Slot
    │   Get High Score property
    │
    └── False →
        High Score = 0
```

## Step 12: Test Game Flow

1. Press Play
2. Collect all items → Should see "YOU WIN!"
3. Get hit by hazards until death → Should see "GAME OVER"
4. Verify restart works

## Game Mode Best Practices

**Single Responsibility:**
- Game Mode handles rules
- Player handles player stuff
- Keep them separate

**Communication:**
- Actors tell Game Mode about events
- Game Mode tells UI about state changes
- Use events/delegates for loose coupling

**State Management:**
- Clear game states (Playing, Won, Lost, Paused)
- Prevent invalid state transitions

## Checkpoint

You should now have:
- [x] Custom Game Mode
- [x] Win condition (collect all items)
- [x] Lose condition (health reaches 0)
- [x] Game state tracking
- [x] Restart functionality

## Full Game Flow

```
Game Start
    │
    ▼
Spawn Player at Player Start
    │
    ▼
Game Mode counts collectibles
    │
    ▼
┌─────────────────────────────┐
│         GAMEPLAY            │
│  - Collect items            │
│  - Avoid hazards            │
│  - Score increases          │
│  - Health decreases         │
└───────────────┬─────────────┘
                │
        ┌───────┴───────┐
        │               │
        ▼               ▼
  All Collected    Health = 0
        │               │
        ▼               ▼
     YOU WIN!      GAME OVER
        │               │
        └───────┬───────┘
                │
                ▼
            Restart?
                │
        ┌───────┴───────┐
        │               │
        ▼               ▼
       Yes              No
        │               │
        ▼               ▼
    Restart         Main Menu
```

## What's Next

Time to show the player what's happening with a User Interface!

**[Continue to User Interface →](06-user-interface.md)**
