# The Game Loop

Every game runs a continuous loop that processes input, updates the world, and renders frames. Understanding this loop is key to understanding how games work.

## What You'll Learn

- What the game loop is and why it matters
- How Unreal processes each frame
- What Delta Time is and why it's important
- How to think about frame-based programming

## What Is the Game Loop?

The game loop is the heartbeat of your game. It runs continuously:

```
while (game is running) {
    ProcessInput();       // Check keyboard, mouse, controller
    UpdateGame();         // Move objects, run AI, physics
    Render();            // Draw everything to screen
}
```

This happens 30, 60, or more times per second.

### Why a Loop?

Games are **real-time simulations**. Unlike a calculator that waits for input, games must:
- Keep animating even when player isn't pressing buttons
- Update AI continuously
- Simulate physics
- Keep the world alive

## Frame Rate

**Frame rate** (FPS - Frames Per Second) is how many times the loop runs per second:

| FPS | Frame Time | Feel |
|-----|-----------|------|
| 30 | 33.3 ms | Acceptable for some games |
| 60 | 16.7 ms | Smooth, standard target |
| 120 | 8.3 ms | Very smooth, competitive gaming |
| 144+ | <7 ms | Ultra smooth |

Higher FPS = smoother motion = better feel.

### Frame Time Budget

At 60 FPS, everything must complete in 16.7 milliseconds:

```
┌─────────────────────────────────────────────────────┐
│ Input │ Physics │ AI │ Game Logic │ Rendering │ Spare │
│  1ms  │   3ms   │ 2ms │    4ms    │    5ms    │ 1.7ms │
└─────────────────────────────────────────────────────┘
                    Total: 16.7ms
```

If you exceed the budget, frame rate drops.

## Unreal's Game Loop

Unreal's loop is more detailed:

```
Frame Start
    │
    ▼
Input Processing
    │
    ▼
World Tick (calls Actor Tick functions)
    │
    ▼
Physics Simulation
    │
    ▼
Camera Updates
    │
    ▼
Rendering
    │
    ▼
Frame End → Back to Start
```

### Tick Functions

**Tick** is how Actors participate in the game loop:

```cpp
void AMyActor::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

    // This runs every frame
    MoveForward(Speed * DeltaTime);
}
```

In Blueprints, use the "Event Tick" node.

### Tick Groups

Unreal processes ticks in groups:

```
1. TG_PrePhysics      - Before physics
2. TG_StartPhysics    - Physics begins
3. TG_DuringPhysics   - During physics
4. TG_EndPhysics      - Physics ends
5. TG_PostPhysics     - After physics
6. TG_PostUpdateWork  - After everything
```

Most Actors tick in PostPhysics (default).

You can change tick group:
```cpp
PrimaryActorTick.TickGroup = TG_PrePhysics;
```

## Delta Time

**Delta Time** is the time since the last frame (in seconds).

### Why Delta Time Matters

Without Delta Time (wrong):
```cpp
void Tick()
{
    Position += 10;  // Move 10 units per frame
}
```

Problem: At 60 FPS, moves 600 units/second. At 30 FPS, moves 300 units/second. Speed depends on frame rate!

With Delta Time (correct):
```cpp
void Tick(float DeltaTime)
{
    Position += 100 * DeltaTime;  // Move 100 units per second
}
```

Now movement is consistent regardless of frame rate.

### Delta Time Math

```
At 60 FPS: DeltaTime ≈ 0.0167 seconds
At 30 FPS: DeltaTime ≈ 0.0333 seconds

Movement = Speed * DeltaTime

60 FPS: 100 * 0.0167 = 1.67 units per frame
        1.67 * 60 frames = ~100 units per second ✓

30 FPS: 100 * 0.0333 = 3.33 units per frame
        3.33 * 30 frames = ~100 units per second ✓
```

Same result regardless of frame rate!

### When to Use Delta Time

**Use Delta Time for:**
- Movement
- Timers
- Interpolation
- Anything that should be consistent across frame rates

**Don't need Delta Time for:**
- One-time events (button pressed)
- Physics (engine handles it)
- Counting frames specifically

## Frame-Independent Programming

### Movement

```cpp
// Wrong - frame-dependent
Position.X += 5;

// Correct - frame-independent
Position.X += Speed * DeltaTime;
```

### Rotation

```cpp
// Wrong
Rotation.Yaw += 1;

// Correct
Rotation.Yaw += RotationSpeed * DeltaTime;
```

### Timers

```cpp
// Wrong - counts frames, not time
frameCount++;
if (frameCount > 60) { /* ... */ }

// Correct - counts actual time
elapsedTime += DeltaTime;
if (elapsedTime > 1.0f) { /* ... */ }
```

### Interpolation

```cpp
// Smooth movement toward target
FVector NewPosition = FMath::VInterpTo(
    CurrentPosition,
    TargetPosition,
    DeltaTime,
    InterpSpeed
);
```

## Tick vs Timers

### Tick

Runs every frame. Use for:
- Continuous movement
- Always-on behavior
- Real-time responses

```cpp
void Tick(float DeltaTime)
{
    // Runs 60+ times per second
    CheckForNearbyEnemies();
}
```

### Timers

Run after a delay. Use for:
- Delayed actions
- Periodic checks (not every frame)
- Cooldowns

```cpp
// Call DoSomething() after 2 seconds
GetWorld()->GetTimerManager().SetTimer(
    TimerHandle,
    this,
    &AMyActor::DoSomething,
    2.0f,    // Delay
    false    // Don't loop
);

// Call repeatedly every 0.5 seconds
GetWorld()->GetTimerManager().SetTimer(
    TimerHandle,
    this,
    &AMyActor::DoSomething,
    0.5f,    // Interval
    true     // Loop
);
```

### When to Use Each

| Scenario | Use |
|----------|-----|
| Smooth movement | Tick |
| Check health every second | Timer |
| Respond to input | Tick |
| Spawn enemy waves | Timer |
| Physics interactions | Tick |
| Cooldown ability | Timer |

## Performance Considerations

### Disable Tick When Not Needed

Every ticking Actor has overhead. Disable when possible:

```cpp
// Disable tick
PrimaryActorTick.bCanEverTick = false;

// Or disable at runtime
SetActorTickEnabled(false);
```

### Reduce Tick Frequency

Not everything needs to tick every frame:

```cpp
// Tick every 0.1 seconds instead of every frame
PrimaryActorTick.TickInterval = 0.1f;
```

### Batch Operations

Instead of each Actor doing expensive work:

```cpp
// Bad: Each enemy pathfinds every frame
void AEnemy::Tick(float DeltaTime)
{
    FindPathToPlayer();  // Expensive!
}

// Better: Manager distributes work across frames
void AEnemyManager::Tick(float DeltaTime)
{
    // Only update a few enemies per frame
    for (int i = 0; i < EnemiesPerFrame; i++)
    {
        Enemies[CurrentIndex]->UpdatePath();
        CurrentIndex = (CurrentIndex + 1) % Enemies.Num();
    }
}
```

## Fixed Timestep vs Variable Timestep

### Variable Timestep (Unreal Default)

- Delta Time varies each frame
- Smoother visuals
- Can have physics inconsistencies at low FPS

### Fixed Timestep

- Physics always simulates at fixed rate (e.g., 60 Hz)
- More consistent physics
- Visual interpolation between physics states

Unreal uses variable timestep for gameplay but can be configured for physics:

```
Project Settings → Physics → Substepping
```

## Practical Examples

### Example 1: Moving Platform

```cpp
void AMovingPlatform::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

    // Move back and forth
    FVector NewLocation = GetActorLocation();
    NewLocation.X += Direction * Speed * DeltaTime;

    // Reverse at bounds
    if (NewLocation.X > MaxX || NewLocation.X < MinX)
    {
        Direction *= -1;
    }

    SetActorLocation(NewLocation);
}
```

### Example 2: Cooldown System

```cpp
void APlayer::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

    // Reduce cooldown over time
    if (AbilityCooldown > 0)
    {
        AbilityCooldown -= DeltaTime;
    }
}

void APlayer::UseAbility()
{
    if (AbilityCooldown <= 0)
    {
        PerformAbility();
        AbilityCooldown = 5.0f;  // 5 second cooldown
    }
}
```

### Example 3: Smooth Camera Follow

```cpp
void AGameCamera::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

    // Smoothly follow target
    FVector TargetLocation = Player->GetActorLocation() + Offset;
    FVector NewLocation = FMath::VInterpTo(
        GetActorLocation(),
        TargetLocation,
        DeltaTime,
        FollowSpeed
    );

    SetActorLocation(NewLocation);
}
```

## Exercises

### Exercise 1: Frame Rate Test

Create an Actor that:
1. Moves right continuously
2. First without Delta Time, then with Delta Time
3. Limit frame rate (t.MaxFPS console command) and observe

### Exercise 2: Timer Practice

Create a spawner that:
1. Waits 3 seconds after game start
2. Spawns an enemy
3. Continues spawning every 2 seconds

### Exercise 3: Cooldown Implementation

Create an ability that:
1. Does something when activated
2. Has a 5-second cooldown
3. Shows remaining cooldown time

## Key Takeaways

1. **Game loop** runs continuously, processing input, updating, rendering
2. **Delta Time** makes code frame-rate independent
3. **Always multiply by Delta Time** for movement and time-based logic
4. **Use Timers** for delayed/periodic actions
5. **Disable Tick** when not needed for performance
6. **Think in seconds**, not frames

## What's Next

You now understand how Unreal Engine works! It's time to put it all together and build a simple game.

**[Continue to Simple Game Project](../../projects/simple-game/README.md)**

---

## Quick Reference

| Concept | Formula/Usage |
|---------|---------------|
| Frame-independent movement | `Position += Speed * DeltaTime` |
| Frame-independent rotation | `Rotation += RotSpeed * DeltaTime` |
| Timer (one-shot) | `SetTimer(Handle, Object, Function, Delay, false)` |
| Timer (looping) | `SetTimer(Handle, Object, Function, Interval, true)` |
| Disable tick | `PrimaryActorTick.bCanEverTick = false` |
| Tick interval | `PrimaryActorTick.TickInterval = 0.1f` |

| Frame Rate | Delta Time |
|------------|------------|
| 30 FPS | ~0.033 sec |
| 60 FPS | ~0.017 sec |
| 120 FPS | ~0.008 sec |
