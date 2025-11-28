# Part 6: User Interface

Players need to see their health, score, and game state. Let's create a HUD!

## Step 1: Create HUD Widget

1. Content Browser → UI folder
2. Right-click → User Interface → Widget Blueprint
3. Name: `WBP_HUD`
4. Double-click to open

## Step 2: Design the HUD Layout

In the Widget Designer:

### Add Canvas Panel (Root)

Most widgets need a Canvas Panel as root for positioning.

### Add Health Display

1. From Palette, drag "Progress Bar" into Canvas
2. Position: Top-left (Anchor to top-left corner)
3. Size: 200 x 30
4. Rename to "HealthBar"
5. Set Fill Color to Red

Add a text label:
1. Drag "Text" widget
2. Position above or beside health bar
3. Text: "Health"
4. Rename to "HealthLabel"

### Add Score Display

1. Drag "Text" widget
2. Position: Top-right (Anchor to top-right corner)
3. Rename to "ScoreText"
4. Text: "Score: 0"
5. Make text larger (font size 24)

### Add Collection Counter

1. Drag "Text" widget
2. Position: Top-center
3. Text: "0 / 10"
4. Rename to "CollectionText"

## Step 3: Bind Health Bar

The health bar should reflect actual player health.

### Create Binding

1. Select HealthBar
2. In Details, find "Percent"
3. Click "Bind" → Create Binding
4. This creates a function that returns the percent value

### Binding Implementation

```
Get Health Percent
    │
    ▼
Get Player Character → Cast to BP_PlayerCharacter
    │
    ▼ (Success)
Get CurrentHealth / Get MaxHealth
    │
    ▼
Return (as float 0-1)
```

Now the health bar updates automatically!

## Step 4: Bind Score Text

1. Select ScoreText
2. Bind the "Text" property
3. Create binding function:

```
Get Score Text
    │
    ▼
Get Player Character → Cast to BP_PlayerCharacter
    │
    ▼
Get Score
    │
    ▼
Format Text: "Score: {0}"
    │
    ▼
Return as Text
```

## Step 5: Bind Collection Counter

1. Select CollectionText
2. Bind "Text" property:

```
Get Collection Text
    │
    ▼
Get Game Mode → Cast to BP_CollectorGameMode
    │
    ▼
Format Text: "{CollectedCount} / {TotalCollectibles}"
    │
    ▼
Return as Text
```

## Step 6: Display the HUD

### In BP_PlayerCharacter

```
Event BeginPlay
    │
    ▼
Create Widget (Class: WBP_HUD)
    │
    ▼
Add to Viewport
```

Or in the Player Controller for cleaner separation.

## Step 7: Create Win Screen

1. Create Widget: `WBP_WinScreen`
2. Design:
   - Large "YOU WIN!" text (centered)
   - Final score display
   - "Play Again" button
   - "Quit" button

### Win Screen Layout

```
┌─────────────────────────────────────┐
│                                     │
│            YOU WIN!                 │
│                                     │
│         Final Score: 1500           │
│                                     │
│    [Play Again]    [Quit]           │
│                                     │
└─────────────────────────────────────┘
```

### Button Events

**Play Again Button:**
1. Select button
2. In Details → Events → On Clicked
3. Create event:

```
On Clicked (PlayAgainButton)
    │
    ▼
Get Game Mode → Call RestartGame
```

**Quit Button:**
```
On Clicked (QuitButton)
    │
    ▼
Quit Game
```

## Step 8: Create Game Over Screen

1. Create Widget: `WBP_GameOverScreen`
2. Similar to Win Screen but:
   - "GAME OVER" text
   - "Try Again" button
   - Maybe show how close they were

## Step 9: Show End Screens

### In BP_CollectorGameMode

**Show Win Screen:**
```
Function: ShowWinScreen
    │
    ▼
Create Widget (WBP_WinScreen)
    │
    ▼
Add to Viewport
    │
    ▼
Set Input Mode UI Only
    │
    ▼
Get Player Controller → Set Show Mouse Cursor = true
```

**Show Game Over Screen:**
```
Function: ShowGameOverScreen
    │
    ▼
Create Widget (WBP_GameOverScreen)
    │
    ▼
Add to Viewport
    │
    ▼
Set Input Mode UI Only
    │
    ▼
Get Player Controller → Set Show Mouse Cursor = true
```

## Step 10: Create Pause Menu

1. Create Widget: `WBP_PauseMenu`
2. Layout:
   - "PAUSED" text
   - "Resume" button
   - "Restart" button
   - "Quit" button

### Toggle Pause Menu

In Player Controller:
```
Input Action Pause
    │
    ▼
Branch: PauseMenuVisible?
    │
    ├── True →
    │   Remove Pause Menu from Parent
    │   Set Game Paused = false
    │   Set Input Mode Game Only
    │   Hide Mouse Cursor
    │
    └── False →
        Create Widget (WBP_PauseMenu)
        Add to Viewport
        Set Game Paused = true
        Set Input Mode UI Only
        Show Mouse Cursor
```

## Step 11: Add Damage Flash

Visual feedback when player takes damage:

### In WBP_HUD

1. Add an Image widget covering entire screen
2. Name: "DamageFlash"
3. Color: Red with alpha 0 (transparent)
4. Set to "Hit Test Invisible" (doesn't block input)

### Create Flash Animation

1. Window → Animations (in Widget)
2. Create Animation: "DamageFlashAnim"
3. Add track for DamageFlash opacity
4. Keyframes:
   - 0.0s: Alpha = 0.5 (visible)
   - 0.3s: Alpha = 0 (invisible)

### Trigger Flash

Create function in WBP_HUD:
```
Function: PlayDamageFlash
    │
    ▼
Play Animation (DamageFlashAnim)
```

Call from player when damaged.

## Step 12: Test UI

1. Press Play
2. Verify:
   - Health bar updates when damaged
   - Score updates when collecting
   - Collection counter updates
   - Win screen appears when all collected
   - Game over screen appears on death
   - Pause menu works

## UI Best Practices

**Readability:**
- High contrast text
- Appropriate font sizes
- Clear visual hierarchy

**Responsiveness:**
- Use anchors for different screen sizes
- Test at different resolutions

**Feedback:**
- Animate changes (score pop, health bar color)
- Sound effects for UI interactions

## Checkpoint

You should now have:
- [x] HUD with health, score, collection counter
- [x] Win screen with buttons
- [x] Game over screen with buttons
- [x] Pause menu
- [x] Damage flash effect

## What's Next

Let's polish the game with sound and effects!

**[Continue to Polish →](07-polish.md)**
