# Part 7: Polish

A functional game becomes a GOOD game with polish. Let's add sound, effects, and feel.

## Step 1: Add Sound Effects

### Sound Files You Need

For this game, you'll want:
- **Jump sound** - Whoosh or springy sound
- **Land sound** - Thud or impact
- **Collect sound** - Positive chime or ding
- **Damage sound** - Hit or pain sound
- **Win jingle** - Victory fanfare
- **Lose sound** - Sad trombone or defeat

### Finding Free Sounds

- [Freesound.org](https://freesound.org) - Free sound effects
- [OpenGameArt.org](https://opengameart.org) - Game assets
- Unreal Marketplace - Free monthly content

### Importing Sounds

1. Drag audio files into Content/Audio folder
2. Unreal imports them automatically
3. WAV format works best

## Step 2: Add Player Sounds

### Jump Sound

In BP_PlayerCharacter, when jump input is pressed:

```
Input Action Jump (Pressed)
    │
    ▼
Play Sound at Location
├── Sound: JumpSound
└── Location: Get Actor Location
    │
    ▼
Jump
```

### Landing Sound

1. In Character Movement Component, there's an event for landing
2. Or detect when player hits ground:

```
Event On Landed
    │
    ▼
Play Sound at Location
├── Sound: LandSound
└── Location: Get Actor Location
```

### Footstep Sounds

For walking sounds:
```
Variables:
- FootstepInterval (Float) = 0.4
- TimeSinceFootstep (Float) = 0

Event Tick (if moving and on ground):
    │
    ▼
TimeSinceFootstep += DeltaTime
    │
    ▼
Branch: TimeSinceFootstep >= FootstepInterval?
    │
    ├── True →
    │   Play Footstep Sound
    │   Reset TimeSinceFootstep
    │
    └── False → Continue
```

## Step 3: Add Collection Effects

### Sound

Already added in collectible:
```
Play Sound at Location (PickupSound)
```

### Particle Effect

1. Create or import a particle effect
2. On collection:

```
Spawn Emitter at Location
├── Emitter: CollectionParticle
└── Location: Collectible Location
```

### Score Pop Animation

In UI, when score changes:
1. Play scale animation on score text
2. Brief scale up then back to normal
3. Maybe change color briefly (gold flash)

## Step 4: Add Damage Effects

### Camera Shake

1. Create a Camera Shake Blueprint:
   - Content Browser → Blueprint Class → Camera Shake Base
   - Name: `BP_DamageShake`
2. Configure:
   - Oscillation Duration: 0.3
   - Rotation: Small pitch/yaw oscillation

3. Play when damaged:
```
Get Player Camera Manager → Start Camera Shake (BP_DamageShake)
```

### Screen Flash

Already added in UI section - the red flash overlay.

### Sound

```
Play Sound at Location (DamageSound)
```

## Step 5: Add Environmental Effects

### Ambient Sound

1. Place an "Ambient Sound" actor in level
2. Set to looping background music or ambience
3. Attenuation: None (plays everywhere)

### Lighting Polish

1. Adjust the directional light for dramatic shadows
2. Add point lights near collectibles (makes them glow)
3. Add fog for atmosphere

### Skybox

1. Select the Sky Sphere
2. Adjust colors for mood
3. Or use an HDRI skybox for realism

## Step 6: Add Game Feel

"Game feel" or "juice" makes games satisfying.

### Collectible Attraction

Make collectibles slightly pull toward player when close:

```
Event Tick:
    │
    ▼
Get Distance to Player
    │
    ▼
Branch: Distance < 200?
    │
    ├── True →
    │   Lerp position toward player (slowly)
    │   Speed increases as player gets closer
    │
    └── False → Normal behavior
```

This creates a satisfying "magnet" effect.

### Squash and Stretch

When player lands:
1. Briefly squash the player mesh (scale Y down, X/Z up)
2. Return to normal over 0.1 seconds

```
On Landed:
    │
    ▼
Play Timeline (SquashStretch)
├── 0.0s: Scale (1.2, 0.8, 1.2)
├── 0.1s: Scale (0.9, 1.1, 0.9)
└── 0.15s: Scale (1, 1, 1)
```

### Speed Lines

When moving fast:
1. Enable a radial blur post-process effect
2. Intensity based on speed
3. Creates sense of motion

## Step 7: Add Hazard Effects

### Visual Warning

Before hazard activates:
- Flash red
- Play warning sound
- Particle effect

### Impact Effect

When player hits hazard:
- Spawn impact particles (sparks, blood, etc.)
- Brief slow motion (time dilation)
- Camera shake

## Step 8: Win/Lose Sequences

### Win Sequence

```
Win Game:
    │
    ▼
Play Win Jingle
    │
    ▼
Spawn Confetti Particles
    │
    ▼
Slow Motion (Set Global Time Dilation = 0.5)
    │
    ▼
Wait 1 second (real time)
    │
    ▼
Show Win Screen
```

### Lose Sequence

```
Player Dies:
    │
    ▼
Play Death Sound
    │
    ▼
Camera Shake (big)
    │
    ▼
Ragdoll Player (or death animation)
    │
    ▼
Fade to Black
    │
    ▼
Show Game Over Screen
```

## Step 9: Main Menu

Create a proper start screen:

### Main Menu Widget

```
┌─────────────────────────────────────┐
│                                     │
│           COLLECTOR                 │
│                                     │
│            [PLAY]                   │
│                                     │
│           [OPTIONS]                 │
│                                     │
│            [QUIT]                   │
│                                     │
└─────────────────────────────────────┘
```

### Main Menu Level

1. Create new level: "MainMenu"
2. Simple scene with camera pointing at logo
3. Widget displayed on start
4. Play button opens "MainLevel"

### Set Starting Level

Project Settings → Maps & Modes → Game Default Map → MainMenu

## Step 10: Final Testing

### Test Checklist

- [ ] Can complete the game (collect all items)
- [ ] Can lose the game (health reaches 0)
- [ ] All sounds play correctly
- [ ] UI updates properly
- [ ] No crashes
- [ ] Performance is acceptable (60 FPS)
- [ ] Win/lose screens work
- [ ] Pause menu works
- [ ] Main menu works

### Get Feedback

Have someone else play:
- Watch them play (don't help!)
- Note where they get confused
- Ask what they liked/disliked
- Iterate based on feedback

## Checkpoint

You should now have:
- [x] Sound effects for all actions
- [x] Particle effects for feedback
- [x] Camera shake for impacts
- [x] Visual feedback for damage
- [x] Polished win/lose sequences
- [x] Main menu

## Polish Checklist

Ask yourself:
- Does every action have feedback?
- Is the game satisfying to play?
- Would I want to play this again?
- Is anything confusing?
- Does it feel "good"?

## What's Next

Congratulations! You've built a complete game. Let's talk about where to go from here.

**[Continue to Next Steps →](08-next-steps.md)**
