# Part 3: Collectibles

Now let's create items the player can collect for points.

## Step 1: Create Collectible Blueprint

1. Content Browser → Blueprints/Pickups
2. Right-click → Blueprint Class
3. Select **Actor** as parent
4. Name it `BP_Collectible`
5. Double-click to open

## Step 2: Add Components

### Collision Sphere

1. Add Component → Sphere Collision
2. Set as Root Component (drag onto DefaultSceneRoot)
3. Set Sphere Radius: 50
4. In Collision section:
   - Generate Overlap Events: Enabled
   - Collision Enabled: Query Only

### Visual Mesh

1. Add Component → Static Mesh
2. Set Static Mesh to "Sphere" (from Engine Content)
3. Scale: 0.5, 0.5, 0.5

### Rotating Movement

We'll make it spin to be eye-catching!

## Step 3: Create Collectible Material

1. Content/Materials → New Material → `M_Collectible`
2. Make it emissive (glowing):
   - Add Constant3Vector: Yellow (1, 0.8, 0)
   - Connect to Base Color
   - Add Constant: 5
   - Multiply Color × 5
   - Connect to Emissive Color
3. Apply to the collectible mesh

## Step 4: Add Spinning Motion

In BP_Collectible Event Graph:

```
Event Tick
    │
    ▼
Add Actor Local Rotation
├── Delta Rotation: (0, 0, 2)  [Z rotation]
└── Sweep: false
```

This spins the collectible 2 degrees per frame.

**Better (frame-independent):**

```
Event Tick (Delta Seconds)
    │
    ▼
Add Actor Local Rotation
├── Delta Rotation: (0, 0, RotationSpeed * DeltaSeconds * 100)
└── Sweep: false
```

Add variable: **RotationSpeed** (Float) = 1.0

## Step 5: Add Bobbing Motion

Make it float up and down:

1. Add variable: **StartLocation** (Vector)
2. Add variable: **RunningTime** (Float) = 0

```
Event BeginPlay
    │
    ▼
Set StartLocation = Get Actor Location
```

```
Event Tick (Delta Seconds)
    │
    ▼
RunningTime = RunningTime + DeltaSeconds
    │
    ▼
New Z = StartLocation.Z + (Sin(RunningTime * 2) * 20)
    │
    ▼
Set Actor Location (StartLocation.X, StartLocation.Y, New Z)
```

## Step 6: Handle Collection

When player overlaps, give points and destroy:

```
Event ActorBeginOverlap (Other Actor)
    │
    ▼
Cast to BP_PlayerCharacter
    │
    ▼ (Success)
Call AddScore(100) on player
    │
    ▼
Play Sound at Location (pickup sound)
    │
    ▼
Destroy Actor
```

## Step 7: Add Point Value Variable

Make the point value configurable:

1. Add variable: **PointValue** (Integer) = 100
2. Make it "Instance Editable" (click the eye icon)
3. Use PointValue instead of hardcoded 100

Now you can place collectibles worth different amounts!

## Step 8: Add Sound Effect

1. Import a sound file or use a placeholder
2. Add variable: **PickupSound** (Sound Base reference)
3. Make it Instance Editable
4. In the overlap event, use "Play Sound at Location"

## Step 9: Place Collectibles in Level

1. Drag BP_Collectible into the level
2. Position it somewhere reachable
3. Duplicate (Ctrl+D) and place more around the level
4. Vary the PointValue on some for bonus items

**Suggested placements:**
- A few on the ground (easy)
- Some on platforms (requires jumping)
- One in a tricky spot (high value)

## Step 10: Test Collection

1. Press Play
2. Walk into collectibles
3. Verify:
   - Collectible disappears
   - Score increases (check with Print String for now)
   - Sound plays (if added)

## Adding Visual Feedback

### Particle Effect on Collection

1. Add variable: **CollectionEffect** (Particle System reference)
2. Before Destroy Actor:

```
Spawn Emitter at Location
├── Emitter Template: CollectionEffect
└── Location: Get Actor Location
```

### Screen Flash (Optional)

We'll add this when we create the UI.

## Checkpoint

You should now have:
- [x] Collectible actors that spin and bob
- [x] Collision detection with player
- [x] Points added when collected
- [x] Collectibles destroyed after pickup
- [x] Multiple collectibles placed in level

## Challenge: Different Collectible Types

Create variations:
- **Gem**: 100 points, common
- **Star**: 500 points, rare
- **Crown**: 1000 points, one per level

Use different meshes and colors for each.

## What's Next

Now let's add some danger with hazards!

**[Continue to Hazards →](04-hazards.md)**
