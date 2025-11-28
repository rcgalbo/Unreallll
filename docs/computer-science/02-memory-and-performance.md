# Memory and Performance

Games are one of the most demanding applications for computers. Understanding memory and performance helps you build games that run smoothly.

## What You'll Learn

- How memory is organized and managed
- What causes performance problems
- How to think about optimization
- Common patterns for better performance

## Memory Layout

When your program runs, it uses memory in organized sections:

```
High Memory
┌──────────────────┐
│      Stack       │  ← Local variables, function calls
├──────────────────┤
│        ↓         │
│   (free space)   │
│        ↑         │
├──────────────────┤
│       Heap       │  ← Dynamically allocated objects
├──────────────────┤
│   Static Data    │  ← Global variables, constants
├──────────────────┤
│      Code        │  ← Your program instructions
└──────────────────┘
Low Memory
```

### The Stack

The **stack** stores:
- Local variables
- Function parameters
- Return addresses

It's automatically managed - when a function ends, its stack data is automatically cleaned up.

```cpp
void Attack(int damage) {        // 'damage' goes on stack
    int actualDamage = damage;   // 'actualDamage' goes on stack
    // When function ends, both are automatically removed
}
```

**Stack properties:**
- Very fast (just move a pointer)
- Limited size (usually 1-8 MB)
- Automatic cleanup
- LIFO (Last In, First Out)

### The Heap

The **heap** stores objects that:
- Need to persist beyond a function call
- Have dynamic size
- Are explicitly created and destroyed

```cpp
// Create object on heap
Enemy* enemy = new Enemy();

// ... enemy persists until explicitly deleted ...

// Must manually delete
delete enemy;
```

**Heap properties:**
- Slower than stack (must find free space)
- Much larger (limited by RAM)
- Manual management (or garbage collection)
- Can cause memory leaks if not cleaned up

### Stack vs Heap

| Aspect | Stack | Heap |
|--------|-------|------|
| Speed | Very fast | Slower |
| Size | Limited (~1-8 MB) | Large (GBs) |
| Management | Automatic | Manual (or GC) |
| Use for | Local variables | Objects, large data |

## Memory Problems

### Memory Leaks

A **memory leak** happens when you allocate memory but never free it:

```cpp
void SpawnEnemy() {
    Enemy* enemy = new Enemy();  // Allocate memory
    // ... forgot to delete or store reference ...
}  // Memory is now leaked - can't access or free it
```

Over time, leaked memory accumulates:
- Game uses more and more RAM
- Eventually runs out of memory
- Game slows down or crashes

### How to Avoid Leaks

1. **Match every `new` with `delete`**
2. **Use smart pointers** (automatically delete when done)
3. **Let the engine manage objects** (Unreal's UPROPERTY system)

### Dangling Pointers

A **dangling pointer** points to memory that was already freed:

```cpp
Enemy* enemy = new Enemy();
delete enemy;           // Memory freed
enemy->TakeDamage(10);  // BUG! enemy points to freed memory
```

This causes crashes or unpredictable behavior.

### Prevention

```cpp
Enemy* enemy = new Enemy();
delete enemy;
enemy = nullptr;        // Mark as invalid
if (enemy != nullptr) { // Check before using
    enemy->TakeDamage(10);
}
```

## Performance Basics

### What Is Performance?

Performance is about how efficiently your game runs:
- **Frame rate**: How many frames per second (FPS)
- **Frame time**: How long each frame takes (milliseconds)
- **Memory usage**: How much RAM you're using
- **Load time**: How long to start or load levels

### Frame Rate Math

```
60 FPS = 16.67 ms per frame
30 FPS = 33.33 ms per frame

If your frame takes 20 ms:
1000 ms / 20 ms = 50 FPS
```

### Where Time Goes

A typical game frame:

```
┌─────────────────────────────────────────┐
│ Input (1ms) │ Logic (4ms) │ Render (10ms) │
└─────────────────────────────────────────┘
              Total: 15ms = 66 FPS
```

If any section takes too long, frame rate drops.

## Common Performance Problems

### Problem 1: Doing Too Much Per Frame

```cpp
// Bad: Checking EVERY enemy EVERY frame
void Update() {
    for (enemy in allEnemies) {              // 1000 enemies
        for (other in allEnemies) {          // 1000 enemies
            CheckCollision(enemy, other);     // 1,000,000 checks!
        }
    }
}
```

**Solution:** Only check what's necessary
```cpp
// Better: Only check enemies near player
void Update() {
    nearbyEnemies = GetEnemiesInRange(player, 50);  // Maybe 20 enemies
    for (enemy in nearbyEnemies) {
        // Much fewer checks
    }
}
```

### Problem 2: Creating Objects Every Frame

```cpp
// Bad: Creating new object every frame
void Update() {
    Vector3 position = new Vector3(x, y, z);  // Allocation!
    // ... use position ...
}  // Memory allocated 60 times per second!
```

**Solution:** Reuse objects
```cpp
// Better: Reuse existing object
Vector3 position;  // Create once

void Update() {
    position.Set(x, y, z);  // Just update values
    // ... use position ...
}
```

### Problem 3: Loading During Gameplay

```cpp
// Bad: Loading when player opens door
void OpenDoor() {
    nextLevel = LoadLevel("level2.map");  // Takes 2 seconds!
    // Game freezes for 2 seconds
}
```

**Solution:** Load in advance
```cpp
// Better: Load before player reaches door
void Update() {
    if (PlayerNearDoor()) {
        StartAsyncLoad("level2.map");  // Load in background
    }
}

void OpenDoor() {
    if (levelLoaded) {
        SwitchToLevel();  // Instant!
    }
}
```

### Problem 4: Unnecessary Updates

```cpp
// Bad: Updating things that haven't changed
void Update() {
    for (object in allObjects) {
        object.RecalculateEverything();  // Even if nothing changed
    }
}
```

**Solution:** Only update what changed
```cpp
// Better: Track what needs updating
void Update() {
    for (object in dirtyObjects) {  // Only objects that changed
        object.Recalculate();
    }
    dirtyObjects.Clear();
}
```

## Optimization Mindset

### Rule 1: Measure First

Don't guess where the problem is. Use profiling tools to find the actual bottleneck.

```
"Premature optimization is the root of all evil"
- Donald Knuth
```

Optimize the slow parts, not everything.

### Rule 2: Big O Matters

How your code scales matters more than minor optimizations:

```
O(1)     - Constant: Same time regardless of size
O(n)     - Linear: Time grows with size
O(n²)    - Quadratic: Time grows with square of size
O(log n) - Logarithmic: Time grows slowly
```

Example:
```cpp
// O(n²) - Slow for large n
for (i in items) {
    for (j in items) {
        // n * n operations
    }
}

// O(n) - Much better
for (i in items) {
    // n operations
}
```

Changing O(n²) to O(n) has much more impact than micro-optimizations.

### Rule 3: Trade-Offs

Performance often trades against other things:
- **Memory vs Speed**: Cache results (use more memory, save calculation time)
- **Quality vs Speed**: Lower graphics settings for higher FPS
- **Complexity vs Speed**: More complex code might be faster

Choose based on your priorities.

## Practical Patterns

### Object Pooling

Instead of creating and destroying objects, reuse them:

```cpp
// Without pooling
void Shoot() {
    Bullet* bullet = new Bullet();  // Slow allocation
    // ... bullet flies ...
    delete bullet;                   // Slow deallocation
}

// With pooling
void Initialize() {
    for (i = 0; i < 100; i++) {
        bulletPool[i] = new Bullet();  // Create all upfront
        bulletPool[i].active = false;
    }
}

void Shoot() {
    Bullet* bullet = GetInactiveBullet();  // Reuse existing
    bullet.active = true;
    bullet.Reset();
}

void BulletHitsWall(Bullet* bullet) {
    bullet.active = false;  // Return to pool, don't delete
}
```

### Spatial Partitioning

Divide the world into regions to reduce checks:

```
┌─────┬─────┬─────┐
│  A  │  B  │  C  │   Instead of checking all objects
├─────┼─────┼─────┤   against all objects, only check
│  D  │  E  │  F  │   objects in the same (or adjacent)
├─────┼─────┼─────┤   grid cells.
│  G  │  H  │  I  │
└─────┴─────┴─────┘
```

### Level of Detail (LOD)

Use simpler versions for distant objects:

```
Distance < 10m:   High detail model (10,000 triangles)
Distance 10-50m:  Medium detail (2,000 triangles)
Distance > 50m:   Low detail (500 triangles)
```

Player doesn't notice, but GPU works much less.

### Culling

Don't process things the player can't see:

```cpp
// Frustum culling: Skip objects outside camera view
for (object in allObjects) {
    if (IsInCameraView(object)) {
        Render(object);
    }
}

// Occlusion culling: Skip objects behind walls
for (object in visibleObjects) {
    if (!IsBlockedByWall(object)) {
        Render(object);
    }
}
```

## Unreal Engine Specifics

### UPROPERTY

Unreal's UPROPERTY system automatically manages memory:

```cpp
UPROPERTY()
UObject* MyObject;  // Unreal tracks this, handles cleanup
```

Without UPROPERTY, you must manage memory yourself.

### Garbage Collection

Unreal periodically cleans up unreferenced objects. This can cause hitches if many objects are collected at once.

### Object Pooling in Unreal

Unreal has built-in pooling for projectiles, particles, and other frequently spawned objects.

## Exercises

### Exercise 1: Identify the Problem

What's wrong with this code?

```cpp
void Update() {
    for (int i = 0; i < 1000; i++) {
        String* message = new String("Hello");
        DisplayMessage(message);
    }
}
```

### Exercise 2: Calculate Complexity

What's the time complexity of this code?

```cpp
for (int i = 0; i < n; i++) {
    for (int j = i; j < n; j++) {
        DoSomething();
    }
}
```

### Exercise 3: Optimize This

How would you improve this enemy detection code?

```cpp
void FindNearestEnemy() {
    float nearestDist = 999999;
    for (enemy in allEnemies) {  // 500 enemies
        float dist = sqrt(
            pow(enemy.x - player.x, 2) +
            pow(enemy.y - player.y, 2)
        );
        if (dist < nearestDist) {
            nearestDist = dist;
            nearest = enemy;
        }
    }
}
```

(Hint: Do you need the actual distance, or just to compare?)

## Key Takeaways

1. **Stack is fast but small**, heap is large but slower
2. **Memory leaks** happen when you allocate but don't free
3. **Measure before optimizing** - find the real bottleneck
4. **Algorithm complexity matters** more than micro-optimizations
5. **Reuse objects** instead of constantly creating/destroying
6. **Don't process what you don't need** - cull invisible things

## What's Next

Now that you understand memory and performance, let's look at data structures - how we organize data for efficient access.

**[Continue to Data Structures](03-data-structures.md)**

---

## Quick Reference

| Problem | Solution |
|---------|----------|
| Memory leak | Match new with delete, use smart pointers |
| Slow collision | Spatial partitioning |
| Object creation | Object pooling |
| Too many calculations | Cull unnecessary work |
| Scaling issues | Better algorithms (reduce Big O) |
