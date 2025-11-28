# How Computers Work

Understanding how computers actually work helps you write better code and debug problems. This isn't about memorizing specifications - it's about building a mental model.

## What You'll Learn

- How computers execute instructions
- What the CPU, memory, and storage do
- How code becomes something the computer can run
- Why this matters for game development

## The Basic Model

At its core, a computer does three things:

```
1. INPUT:   Receive data (keyboard, mouse, files)
2. PROCESS: Transform data (calculations, logic)
3. OUTPUT:  Send results (screen, speakers, files)
```

Everything a computer does is some combination of these.

## The CPU: The Brain

The **CPU** (Central Processing Unit) is where actual computation happens.

### What the CPU Does

The CPU executes **instructions** - simple operations like:
- Add two numbers
- Compare two values
- Copy data from one place to another
- Jump to a different instruction

That's it. Everything your computer does - games, videos, AI - is built from these simple operations.

### How Fast?

A modern CPU runs BILLIONS of instructions per second (measured in GHz - gigahertz).

```
1 GHz = 1,000,000,000 instructions per second
3 GHz = 3,000,000,000 instructions per second
```

Your game running at 60 FPS has about 50 million CPU cycles per frame. That's a LOT of instructions.

### Multiple Cores

Modern CPUs have multiple **cores** - essentially multiple CPUs in one chip:

```
Single-core: Can do one thing at a time
Quad-core:   Can do four things simultaneously
8-core:      Can do eight things simultaneously
```

Games use multiple cores for:
- Main game logic
- Physics calculations
- AI processing
- Audio
- Loading data

### The CPU and Games

The CPU handles:
- Game logic (if player presses jump, apply force)
- AI decisions (should enemy attack or flee?)
- Physics calculations (where does this object move?)
- Coordinating everything

When people say a game is "CPU-bound," it means the CPU is the bottleneck.

## Memory (RAM): Short-Term Storage

**RAM** (Random Access Memory) is where the computer stores data it's actively using.

### Why RAM Exists

The CPU is fast but can only hold tiny amounts of data itself. RAM is slower than the CPU but can hold gigabytes of data.

```
CPU registers: ~100 bytes, instant access
RAM:          8-64 GB, fast access
Storage:      500 GB - 2 TB, slow access
```

### How RAM Works

Think of RAM as a giant array of numbered boxes:

```
Address | Value
--------|-------
0       | 42
1       | 100
2       | "hello"
3       | 3.14159
...     | ...
```

Every piece of data has an **address** (the box number).

When you create a variable:
```cpp
int health = 100;
```

The computer:
1. Finds an empty spot in RAM (say, address 1000)
2. Stores the value 100 there
3. Remembers that "health" means "look at address 1000"

### RAM Is Volatile

When you turn off the computer, RAM is erased. That's why you need to save your game - to copy data from RAM to storage.

### RAM and Games

Games load lots of data into RAM:
- Textures and models (hundreds of MB)
- Sound files
- Level data
- Game state (player position, enemy states, etc.)

When you see "loading..." screens, the game is copying data from storage into RAM where the CPU can use it quickly.

### Not Enough RAM

If a game needs more RAM than you have:
- It has to constantly swap data with storage (slow!)
- This causes stuttering and long load times
- This is why games have minimum RAM requirements

## Storage: Long-Term Memory

**Storage** (HDD or SSD) keeps data permanently.

### HDD vs SSD

**HDD (Hard Disk Drive)**:
- Spinning magnetic disk
- Slower (100-200 MB/s)
- Cheaper per GB
- Moving parts can fail

**SSD (Solid State Drive)**:
- Electronic chips (like a big USB drive)
- Much faster (500-7000 MB/s)
- More expensive per GB
- No moving parts, more reliable

### Why SSDs Matter for Games

Games constantly load data from storage:
- Level changes
- Streaming new areas as you move
- Loading save files

With an SSD:
- Load times are much shorter
- Open-world games stream data faster (less pop-in)
- Overall smoother experience

This is why modern consoles (PS5, Xbox Series X) use SSDs.

## The GPU: Graphics Processor

The **GPU** (Graphics Processing Unit) specializes in rendering graphics.

### Why a Separate Chip?

Drawing graphics requires doing the same calculation millions of times (once per pixel). The GPU has thousands of small cores designed for this:

```
CPU: 8 powerful cores (complex tasks)
GPU: 4000+ simple cores (repetitive tasks)
```

### What the GPU Does

For each frame (60 times per second):
1. Receives geometry (triangles that make up 3D objects)
2. Transforms triangles to screen position
3. Determines which pixels each triangle covers
4. Calculates color for each pixel (lighting, textures, shadows)
5. Outputs final image to display

This is millions of calculations per frame.

### GPU and Games

When people say a game is "GPU-bound," the graphics card is the bottleneck. Solutions:
- Lower resolution
- Reduce graphics quality settings
- Upgrade GPU

## How Code Runs

You write code in a programming language. The computer only understands binary (1s and 0s). Something has to translate.

### Compilation

**Compiled languages** (C++, Rust) translate code BEFORE running:

```
Source Code → Compiler → Machine Code → CPU
   (text)      (tool)     (binary)    (runs)
```

- Translation happens once
- Result runs directly on CPU
- Fast execution
- Unreal Engine uses C++

### Interpretation

**Interpreted languages** (Python, JavaScript) translate code WHILE running:

```
Source Code → Interpreter → CPU
   (text)      (reads and     (runs)
                executes)
```

- Translation happens every time you run
- Slower execution
- More flexible
- Blueprints work similarly (though they're also compiled)

### Why Compiled Languages for Games?

Games need maximum performance. Compiled code runs faster because:
- No translation overhead at runtime
- Compiler can optimize the code
- Direct control over memory and hardware

That's why Unreal uses C++.

## The Game Loop

Every game runs a continuous loop:

```cpp
while (gameIsRunning) {
    ProcessInput();      // Check keyboard, mouse, controller
    UpdateGame();        // Move objects, run AI, physics
    RenderFrame();       // Draw everything to screen
}
```

This runs 30-60+ times per second.

### Frame Time Budget

At 60 FPS, each frame has 16.67 milliseconds:

```
1 second = 1000 milliseconds
1000 ms / 60 frames = 16.67 ms per frame
```

Everything must complete in that time:
- Input processing: ~1 ms
- Game logic: ~3-5 ms
- Physics: ~2-3 ms
- AI: ~2-3 ms
- Rendering: ~5-8 ms

If you exceed 16.67 ms, the frame rate drops.

## Why This Matters

Understanding hardware helps you:

### Write Efficient Code

```cpp
// Bad: Searches all enemies every frame
for (enemy in allEnemies) {
    if (Distance(player, enemy) < 10) {
        // Found nearby enemy
    }
}

// Better: Only check enemies in current region
for (enemy in currentRegion.enemies) {
    if (Distance(player, enemy) < 10) {
        // Found nearby enemy
    }
}
```

### Understand Performance Problems

"Why is my game stuttering?"
- CPU overloaded? (check game logic, AI)
- GPU overloaded? (check graphics settings)
- RAM insufficient? (check what's loaded)
- Storage slow? (upgrade to SSD)

### Make Better Design Decisions

"Should I use high-res textures?"
- They use more RAM
- They use more storage
- They take longer to load
- They use more GPU memory
- Trade-off: visual quality vs performance

## Practical Examples

### Example 1: Open World Games

Challenge: Massive world can't fit in RAM

Solution: **Streaming**
- Only load the area around the player
- As player moves, load new areas, unload old ones
- SSD makes this faster and smoother

### Example 2: Multiplayer Games

Challenge: Multiple players, can't predict their actions

Solution: **Client-Server**
- Server runs authoritative game state (prevents cheating)
- Clients send input, receive state updates
- Prediction smooths out network delay

### Example 3: Physics-Heavy Games

Challenge: Simulating thousands of objects

Solution: **GPU Physics**
- Move physics calculations to GPU
- GPU's parallel processing handles many objects well
- CPU freed up for other tasks

## Key Takeaways

1. **CPU** executes instructions - the brain
2. **RAM** stores active data - short-term memory
3. **Storage** keeps permanent data - long-term memory
4. **GPU** renders graphics - specialized for visuals
5. **Games run in a loop** - 60 times per second
6. **Performance is about trade-offs** - balance quality and speed

## What's Next

Now that you understand the hardware, let's look at how we manage memory and why it matters for performance.

**[Continue to Memory and Performance](02-memory-and-performance.md)**

---

## Quick Reference

| Component | Purpose | Speed | Size |
|-----------|---------|-------|------|
| CPU | Execute instructions | Fastest | Tiny |
| RAM | Active data storage | Fast | 8-64 GB |
| Storage | Permanent storage | Slow (HDD) / Medium (SSD) | 500GB-2TB |
| GPU | Graphics rendering | Fast for parallel tasks | 4-24 GB |
