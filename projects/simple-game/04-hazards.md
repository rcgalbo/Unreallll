# Part 4: Hazards

Let's add danger to our game! Hazards damage the player on contact.

## Step 1: Create Hazard Blueprint

1. Content Browser → Blueprints/Hazards
2. Right-click → Blueprint Class
3. Parent: **Actor**
4. Name: `BP_Hazard`
5. Double-click to open

## Step 2: Add Components

### Collision Box

1. Add Component → Box Collision
2. Make it Root Component
3. Box Extent: X=50, Y=50, Z=10 (flat hazard, like spikes)
4. Generate Overlap Events: Enabled

### Visual Mesh

1. Add Component → Static Mesh
2. Use "Cube" from Engine Content
3. Scale to match collision roughly

## Step 3: Create Hazard Material

1. Content/Materials → New Material → `M_Hazard`
2. Color: Red (0.8, 0.1, 0.1)
3. Add slight emissive glow
4. Apply to hazard mesh

## Step 4: Add Damage Variable

1. Add variable: **DamageAmount** (Float) = 25
2. Make Instance Editable

## Step 5: Handle Player Overlap

```
Event ActorBeginOverlap (Other Actor)
    │
    ▼
Cast to BP_PlayerCharacter
    │
    ▼ (Success)
Call TakeDamage(DamageAmount) on player
    │
    ▼
Play Sound at Location (damage sound)
```

## Step 6: Prevent Repeated Damage

Without prevention, standing on a hazard damages every frame!

### Option 1: Cooldown

```
Variables:
- CanDamage (Boolean) = true
- DamageCooldown (Float) = 1.0

Event ActorBeginOverlap (Other Actor)
    │
    ▼
Branch: CanDamage?
    │
    ├── True →
    │   Cast to Player
    │   Call TakeDamage
    │   Set CanDamage = false
    │   Set Timer (DamageCooldown) → Set CanDamage = true
    │
    └── False → (nothing)
```

### Option 2: Damage on Enter Only

The overlap event only fires once when entering, so the basic setup works. But for continuous damage (like fire), use Option 1.

## Step 7: Visual Feedback

### Flash Player Red

We'll implement this in the UI section, but the concept:
1. When damaged, trigger a red screen flash
2. Player character broadcasts a "damaged" event

### Knockback

Push the player away from the hazard:

```
After TakeDamage:
    │
    ▼
Get direction from Hazard to Player
    │
    ▼
Launch Character
├── Velocity: Direction * 500 + (0, 0, 300)  [Add upward]
└── Override: true
```

## Step 8: Create Different Hazard Types

### Spike Hazard (Instant Damage)

- What we built above
- One-time damage on touch
- Use for floor spikes

### Fire Hazard (Continuous Damage)

1. Duplicate BP_Hazard → `BP_FireHazard`
2. Use continuous damage with cooldown
3. Add particle effect (fire)
4. Smaller damage but repeating

### Moving Hazard

1. Duplicate BP_Hazard → `BP_MovingHazard`
2. Add movement like the collectible bob:

```
Variables:
- StartLocation (Vector)
- MoveDistance (Float) = 200
- MoveSpeed (Float) = 1.0
- RunningTime (Float) = 0

Event Tick:
    │
    ▼
RunningTime += DeltaSeconds
NewX = StartLocation.X + Sin(RunningTime * MoveSpeed) * MoveDistance
Set Actor Location (NewX, StartLocation.Y, StartLocation.Z)
```

## Step 9: Place Hazards in Level

Add hazards strategically:

**Ground Level:**
- Spike patches between collectibles
- Force player to go around

**Platform Challenges:**
- Moving hazard between two platforms
- Requires timing

**Risk/Reward:**
- High-value collectible surrounded by hazards
- Worth the danger?

## Step 10: Test Hazards

1. Press Play
2. Walk into hazards
3. Verify:
   - Health decreases
   - Knockback works (if implemented)
   - Cooldown prevents rapid damage
   - Game doesn't crash when health reaches 0

## Handling Death

When health reaches 0, the player should die. In BP_PlayerCharacter:

### Create Die Event

```
Custom Event: Die
    │
    ▼
Print String "You Died!"  (temporary feedback)
    │
    ▼
Disable Input
    │
    ▼
Set Timer (3 seconds) → Restart Level
```

### Restart Level Function

```
Open Level (Get Current Level Name)
```

Or use the GameMode (next section).

## Checkpoint

You should now have:
- [x] Hazards that damage the player
- [x] Cooldown to prevent instant death
- [x] Knockback feedback
- [x] Different hazard types placed in level
- [x] Death handling (basic)

## Design Tips

**Fair Challenge:**
- Player should see hazards coming
- Give reaction time
- Don't place hazards at spawn

**Clear Communication:**
- Hazards look dangerous (red, spiky)
- Different hazard types look different
- Sound cues for nearby danger

**Risk vs Reward:**
- High-value pickups near hazards
- Safe paths for cautious players
- Shortcuts for skilled players

## What's Next

Time to create proper game rules with a Game Mode!

**[Continue to Game Mode →](05-game-mode.md)**
