# Control Flow

So far, code runs from top to bottom, line by line. But real programs need to make decisions and repeat actions. Control flow is how you control which code runs and when.

## What You'll Learn

- How to make decisions with conditions
- How to repeat code with loops
- How to combine conditions
- Common patterns used in games

## Conditions: If/Else

The most fundamental control structure: "If this is true, do that."

### Basic If Statement

```
if (condition is true) {
    do this code
}
```

Example:
```cpp
if (health <= 0) {
    GameOver();
}
```

Translation: "If health is 0 or less, run the GameOver function."

### If/Else

What if we want to do something different when the condition is false?

```
if (condition is true) {
    do this
} else {
    do that instead
}
```

Example:
```cpp
if (hasKey) {
    OpenDoor();
} else {
    DisplayMessage("You need a key!");
}
```

### If/Else If/Else

Multiple conditions:

```cpp
if (health > 75) {
    healthColor = Green;
} else if (health > 25) {
    healthColor = Yellow;
} else {
    healthColor = Red;
}
```

The computer checks each condition in order and runs the first one that's true.

### Condition Order Matters

```cpp
// Wrong order
if (score > 0) {
    rank = "Bronze";
} else if (score > 100) {  // Never reached! Any score > 100 is also > 0
    rank = "Silver";
}

// Correct order
if (score > 100) {
    rank = "Silver";
} else if (score > 0) {
    rank = "Bronze";
}
```

Check specific conditions before general ones.

## Comparison Operators

These are used to compare values:

| Operator | Meaning | Example |
|----------|---------|---------|
| `==` | Equal to | `health == 100` |
| `!=` | Not equal to | `lives != 0` |
| `>` | Greater than | `score > 1000` |
| `<` | Less than | `ammo < 10` |
| `>=` | Greater than or equal | `level >= 5` |
| `<=` | Less than or equal | `health <= 0` |

### == vs =

This is a common source of bugs:
- `=` **assigns** a value
- `==` **compares** values

```cpp
if (health = 0)   // BUG! This sets health to 0, then checks if 0 is truthy
if (health == 0)  // Correct! This checks if health equals 0
```

## Logical Operators

Combine multiple conditions:

### AND (&&)

Both conditions must be true:
```cpp
if (hasKey && doorIsLocked) {
    UnlockDoor();
}
```
Translation: "If we have the key AND the door is locked, unlock it."

### OR (||)

At least one condition must be true:
```cpp
if (health <= 0 || timeExpired) {
    GameOver();
}
```
Translation: "If health is 0 or less, OR time is up, game over."

### NOT (!)

Inverts a condition:
```cpp
if (!isPaused) {
    UpdateGame();
}
```
Translation: "If the game is NOT paused, update it."

### Combining Operators

```cpp
if ((hasKey || hasLockpick) && !enemyNearby) {
    OpenDoor();
}
```
Use parentheses to be clear about order.

## Truth Tables

How logical operators work:

### AND (&&)
| A | B | A && B |
|---|---|--------|
| true | true | **true** |
| true | false | false |
| false | true | false |
| false | false | false |

Both must be true.

### OR (||)
| A | B | A \|\| B |
|---|---|--------|
| true | true | **true** |
| true | false | **true** |
| false | true | **true** |
| false | false | false |

At least one must be true.

### NOT (!)
| A | !A |
|---|---|
| true | false |
| false | true |

Flips the value.

## Loops: Repeating Code

Loops run code multiple times. Without loops, you'd write:

```cpp
// Without loops (tedious and limiting)
SpawnEnemy();
SpawnEnemy();
SpawnEnemy();
SpawnEnemy();
SpawnEnemy();
```

With loops:
```cpp
// With loops
for (int i = 0; i < 5; i++) {
    SpawnEnemy();
}
```

### While Loops

"While this condition is true, keep doing this."

```cpp
while (playerIsAlive) {
    UpdateGame();
    RenderFrame();
}
```

The loop checks the condition, runs the code, checks again, runs again, until the condition becomes false.

**Danger: Infinite Loops**
```cpp
while (true) {
    DoSomething();  // This runs forever! Computer freezes.
}
```

Always make sure a while loop can eventually end.

### For Loops

"Do this a specific number of times."

```cpp
for (int i = 0; i < 10; i++) {
    SpawnEnemy();
}
```

Breaking this down:
- `int i = 0` - Start with i equal to 0
- `i < 10` - Keep going while i is less than 10
- `i++` - After each iteration, add 1 to i

This runs 10 times (i = 0, 1, 2, 3, 4, 5, 6, 7, 8, 9).

### Loop Counter

The `i` variable (or whatever you name it) counts iterations:

```cpp
for (int i = 0; i < 5; i++) {
    print(i);  // Prints 0, 1, 2, 3, 4
}
```

You can use this counter:
```cpp
for (int i = 1; i <= 3; i++) {
    SpawnEnemyAtWave(i);  // Spawns at wave 1, 2, 3
}
```

### For Each Loops

"Do something for each item in a collection."

```cpp
for (Enemy enemy : allEnemies) {
    enemy.TakeDamage(10);
}
```

This damages every enemy in the allEnemies collection. (We'll cover collections later.)

### Breaking Out of Loops

Sometimes you need to exit a loop early:

```cpp
for (int i = 0; i < 100; i++) {
    if (FoundTarget()) {
        break;  // Exit the loop immediately
    }
    CheckNextLocation();
}
```

### Skipping Iterations

Sometimes you want to skip to the next iteration:

```cpp
for (Enemy enemy : allEnemies) {
    if (enemy.IsDead()) {
        continue;  // Skip dead enemies
    }
    enemy.Update();  // Only update living enemies
}
```

## Nested Control Flow

You can put conditions and loops inside each other:

```cpp
for (int x = 0; x < 10; x++) {
    for (int y = 0; y < 10; y++) {
        // This runs 100 times (10 * 10)
        PlaceTile(x, y);
    }
}
```

```cpp
if (gameStarted) {
    if (playerAlive) {
        if (!isPaused) {
            UpdateGame();
        }
    }
}
```

But deeply nested code is hard to read. Often you can simplify:

```cpp
if (gameStarted && playerAlive && !isPaused) {
    UpdateGame();
}
```

## Switch Statements

When you have many specific cases:

```cpp
// Using if/else (verbose)
if (direction == "north") {
    MoveNorth();
} else if (direction == "south") {
    MoveSouth();
} else if (direction == "east") {
    MoveEast();
} else if (direction == "west") {
    MoveWest();
}

// Using switch (cleaner)
switch (direction) {
    case "north":
        MoveNorth();
        break;
    case "south":
        MoveSouth();
        break;
    case "east":
        MoveEast();
        break;
    case "west":
        MoveWest();
        break;
    default:
        StandStill();
        break;
}
```

The `break` prevents "falling through" to the next case.

## Common Game Patterns

### The Game Loop

Every game has a main loop:

```cpp
while (gameRunning) {
    ProcessInput();       // Check for player input
    UpdateGameState();    // Update positions, check collisions, etc.
    RenderFrame();        // Draw everything to screen
}
```

This runs 30-60+ times per second.

### State Machines

Games often have states:

```cpp
switch (gameState) {
    case MainMenu:
        ShowMenu();
        if (playerPressedStart) {
            gameState = Playing;
        }
        break;

    case Playing:
        UpdateGame();
        if (playerDied) {
            gameState = GameOver;
        }
        break;

    case GameOver:
        ShowGameOver();
        if (playerPressedRestart) {
            gameState = Playing;
            ResetGame();
        }
        break;
}
```

### Collision Checking

Check each object against others:

```cpp
for (Bullet bullet : allBullets) {
    for (Enemy enemy : allEnemies) {
        if (BulletHitsEnemy(bullet, enemy)) {
            enemy.TakeDamage(bullet.damage);
            bullet.Destroy();
            break;  // Bullet is gone, stop checking this bullet
        }
    }
}
```

### Input Response

```cpp
if (IsKeyPressed(SPACE)) {
    if (canJump) {
        Jump();
        canJump = false;
    }
}

if (IsOnGround()) {
    canJump = true;
}
```

## Exercises

### Exercise 1: Trace the Output

What gets printed?

```cpp
int x = 5;
if (x > 3) {
    print("A");
}
if (x > 7) {
    print("B");
}
if (x == 5) {
    print("C");
}
```

### Exercise 2: Fix the Logic

This code should only open the door if the player has the key AND no enemies are nearby:

```cpp
if (hasKey || !enemyNearby) {
    OpenDoor();
}
```

What's wrong? Fix it.

### Exercise 3: Loop Practice

Write a loop that prints numbers 1 through 10.
Then modify it to print only even numbers.

### Exercise 4: Grade Calculator

Write if/else statements to assign a letter grade:
- 90-100: A
- 80-89: B
- 70-79: C
- 60-69: D
- Below 60: F

### Exercise 5: Find the Bug

```cpp
int countdown = 10;
while (countdown > 0) {
    print(countdown);
}
print("Blast off!");
```

Why doesn't this work?

## Key Takeaways

1. **If statements** let you make decisions
2. **Comparison operators** compare values (==, !=, <, >, <=, >=)
3. **Logical operators** combine conditions (&&, ||, !)
4. **While loops** repeat while a condition is true
5. **For loops** repeat a specific number of times
6. **Break** exits a loop early
7. **Continue** skips to the next iteration

## Common Mistakes

- Using `=` instead of `==`
- Infinite loops (forgetting to change the condition)
- Off-by-one errors (loop runs one too many or few times)
- Checking conditions in wrong order
- Forgetting `break` in switch statements

## What's Next

Now you can make decisions and repeat code. Next, we'll learn about functions - reusable blocks of code.

**[Continue to Functions](04-functions.md)**

---

## Quick Reference

| Concept | Syntax |
|---------|--------|
| If | `if (condition) { code }` |
| If/Else | `if (condition) { } else { }` |
| While loop | `while (condition) { code }` |
| For loop | `for (init; condition; update) { code }` |
| And | `&&` |
| Or | `\|\|` |
| Not | `!` |
| Equal | `==` |
| Not equal | `!=` |
| Break | `break;` |
| Continue | `continue;` |
