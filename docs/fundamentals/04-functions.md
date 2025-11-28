# Functions

Functions are reusable blocks of code. Instead of writing the same code over and over, you write it once as a function and call it whenever needed.

## What You'll Learn

- What functions are and why they matter
- How to create and use functions
- Parameters and return values
- How to think about breaking code into functions

## What Is a Function?

A **function** is a named block of code that performs a specific task.

Think of it like a machine:
1. You give it inputs (or nothing)
2. It does something
3. It gives you an output (or nothing)

### Real-World Analogy

A coffee machine is like a function:

```
Function: MakeCoffee
Inputs: coffee beans, water
Process: grinds beans, heats water, brews
Output: cup of coffee
```

You don't need to know HOW it works internally. You just:
1. Put in the inputs
2. Press the button
3. Get the output

That's **abstraction** - hiding complexity behind a simple interface.

## Why Use Functions?

### 1. Avoid Repetition (DRY - Don't Repeat Yourself)

**Without functions:**
```cpp
// Damage enemy 1
enemy1Health = enemy1Health - 10;
if (enemy1Health <= 0) {
    enemy1Health = 0;
    DestroyEnemy1();
}

// Damage enemy 2
enemy2Health = enemy2Health - 10;
if (enemy2Health <= 0) {
    enemy2Health = 0;
    DestroyEnemy2();
}

// Damage enemy 3... and so on
```

**With functions:**
```cpp
DamageEnemy(enemy1, 10);
DamageEnemy(enemy2, 10);
DamageEnemy(enemy3, 10);
```

### 2. Organization

Functions break code into logical pieces:
```cpp
void GameLoop() {
    HandleInput();
    UpdatePhysics();
    CheckCollisions();
    UpdateAI();
    RenderGraphics();
    PlaySounds();
}
```

Much clearer than 500 lines of mixed code.

### 3. Testability

You can test functions individually:
```cpp
// Test the CalculateDamage function with different inputs
result = CalculateDamage(100, 20);  // Should return 80
result = CalculateDamage(10, 20);   // Should return 0 (not negative)
```

### 4. Reusability

Write once, use everywhere:
```cpp
// Same function used in many places
PlayerTakeDamage(damage);
EnemyTakeDamage(damage);
BarrelTakeDamage(damage);
```

## Creating Functions

### Basic Function (No Inputs, No Output)

```cpp
void SayHello() {
    print("Hello, World!");
}
```

- `void` means "no output" (returns nothing)
- `SayHello` is the function name
- `()` holds parameters (empty = no inputs)
- `{ }` contains the code

### Calling a Function

```cpp
SayHello();  // Prints "Hello, World!"
SayHello();  // Prints "Hello, World!" again
```

The parentheses are required even with no inputs.

## Parameters (Inputs)

Parameters let you pass data INTO a function.

### Single Parameter

```cpp
void Greet(string name) {
    print("Hello, " + name + "!");
}

Greet("Alice");  // Prints "Hello, Alice!"
Greet("Bob");    // Prints "Hello, Bob!"
```

- `string name` declares a parameter named `name` of type `string`
- When called, the argument ("Alice") is copied into `name`

### Multiple Parameters

```cpp
void Attack(Enemy target, int damage) {
    target.health = target.health - damage;
    print("Dealt " + damage + " damage!");
}

Attack(goblin, 25);   // Attacks goblin for 25 damage
Attack(dragon, 100);  // Attacks dragon for 100 damage
```

Parameters are separated by commas.

### Parameter vs Argument

- **Parameter**: The variable in the function definition (`damage`)
- **Argument**: The value you pass when calling (`25`)

```cpp
void Heal(int amount) {      // 'amount' is a parameter
    health = health + amount;
}

Heal(50);                     // 50 is an argument
```

## Return Values (Outputs)

Functions can give back a result.

### Returning a Value

```cpp
int Add(int a, int b) {
    return a + b;
}

int result = Add(3, 5);  // result is 8
print(Add(10, 20));      // Prints 30
```

- `int` before the function name means it returns an integer
- `return` sends a value back and exits the function

### Using Return Values

```cpp
int CalculateDamage(int baseDamage, int armor) {
    int actualDamage = baseDamage - armor;
    if (actualDamage < 0) {
        actualDamage = 0;
    }
    return actualDamage;
}

// Use the returned value
int damageDealt = CalculateDamage(50, 30);  // damageDealt is 20
enemy.health = enemy.health - damageDealt;
```

### Return Ends the Function

```cpp
int GetSign(int number) {
    if (number > 0) {
        return 1;      // Function ends here if positive
    }
    if (number < 0) {
        return -1;     // Function ends here if negative
    }
    return 0;          // Only reached if zero
}
```

Code after `return` doesn't run.

### Void Functions

`void` means no return value:
```cpp
void PrintScore(int score) {
    print("Score: " + score);
    // No return statement needed (or use 'return;' to exit early)
}
```

## Scope: Where Variables Exist

Variables inside a function only exist inside that function.

### Local Variables

```cpp
void MyFunction() {
    int localVar = 10;  // Only exists inside this function
    print(localVar);    // Works fine
}

// Outside the function
print(localVar);        // ERROR! localVar doesn't exist here
```

### Parameter Scope

Parameters are also local:

```cpp
void DoubleIt(int number) {
    number = number * 2;
    print(number);  // Prints the doubled value
}

int myNumber = 5;
DoubleIt(myNumber);  // Prints 10
print(myNumber);      // Still prints 5! Original unchanged.
```

The parameter `number` is a COPY of `myNumber`, not the same variable.

### Why This Matters

Local scope:
- Prevents accidental changes to other code
- Makes functions independent and reusable
- Keeps variables organized

## Pure Functions

A **pure function**:
1. Only depends on its inputs
2. Always returns the same output for the same inputs
3. Doesn't modify anything outside itself

```cpp
// Pure function
int Add(int a, int b) {
    return a + b;  // Only uses inputs, always same result
}

// Not pure (modifies external state)
void AddToScore(int points) {
    globalScore = globalScore + points;  // Modifies global variable
}
```

Pure functions are easier to understand and test. Use them when possible.

## Function Design Tips

### 1. Do One Thing

Each function should have a single responsibility:

```cpp
// Bad: Does too much
void HandlePlayerDeath() {
    player.health = 0;
    player.lives--;
    SaveHighScore();
    PlayDeathAnimation();
    PlayDeathSound();
    ShowGameOverScreen();
    ResetLevel();
}

// Better: Split into focused functions
void KillPlayer() {
    player.health = 0;
    player.lives--;
}

void HandleGameOver() {
    SaveHighScore();
    ShowGameOverScreen();
}

// Then combine as needed
void OnPlayerDeath() {
    KillPlayer();
    PlayDeathAnimation();
    PlayDeathSound();
    if (player.lives <= 0) {
        HandleGameOver();
    } else {
        ResetLevel();
    }
}
```

### 2. Name Functions Clearly

Function names should describe what they do:

```cpp
// Bad names
void DoIt();
void Process();
void HandleStuff();

// Good names
void CalculateDamage();
void SpawnEnemy();
void SaveGame();
void IsPlayerAlive();  // 'Is' prefix suggests boolean return
void GetPlayerHealth();  // 'Get' prefix suggests it returns a value
```

### 3. Keep Functions Short

If a function is getting long, break it into smaller functions:

```cpp
// Instead of 100 lines in one function
void UpdateGame() {
    UpdatePlayer();
    UpdateEnemies();
    UpdateProjectiles();
    CheckCollisions();
    UpdateUI();
}
```

### 4. Limit Parameters

Too many parameters is a sign the function does too much:

```cpp
// Hard to use and understand
void CreateCharacter(string name, int health, int strength, int speed,
                     int defense, int luck, int level, string class,
                     int x, int y, string sprite);

// Better: Group related data
void CreateCharacter(CharacterStats stats, Position pos, string sprite);
```

## Functions in Unreal Engine

### Blueprints

In Blueprints, functions appear as nodes:
- Create functions in the "My Blueprint" panel
- Add input pins (parameters) and output pins (return values)
- Call functions by dragging from the function list

### C++

```cpp
// In the header file (.h)
UFUNCTION(BlueprintCallable)  // Makes it available in Blueprints
int CalculateDamage(int BaseDamage, int Armor);

// In the source file (.cpp)
int AMyCharacter::CalculateDamage(int BaseDamage, int Armor) {
    int ActualDamage = BaseDamage - Armor;
    if (ActualDamage < 0) {
        ActualDamage = 0;
    }
    return ActualDamage;
}
```

## Exercises

### Exercise 1: Write a Function

Write a function called `Max` that takes two integers and returns the larger one.

```cpp
int result1 = Max(5, 3);   // Should be 5
int result2 = Max(10, 20); // Should be 20
int result3 = Max(7, 7);   // Should be 7
```

### Exercise 2: Identify the Bug

```cpp
void AddBonus(int score) {
    score = score + 100;
}

int playerScore = 500;
AddBonus(playerScore);
print(playerScore);  // What does this print? Why?
```

### Exercise 3: Refactor This Code

Turn this repetitive code into functions:

```cpp
// Player attacks enemy
int damage = playerStrength - enemyDefense;
if (damage < 0) damage = 0;
enemyHealth = enemyHealth - damage;

// Enemy attacks player
int damage2 = enemyStrength - playerDefense;
if (damage2 < 0) damage2 = 0;
playerHealth = playerHealth - damage2;
```

### Exercise 4: Function Tracing

What does this print?

```cpp
int Mystery(int x) {
    if (x <= 1) {
        return 1;
    }
    return x * Mystery(x - 1);
}

print(Mystery(4));
```

(This is a recursive function - a function that calls itself!)

## Key Takeaways

1. **Functions** are reusable blocks of code
2. **Parameters** pass data into functions
3. **Return values** pass data out of functions
4. **Scope** determines where variables exist
5. **Good functions** do one thing and do it well
6. **Naming matters** - be clear about what functions do

## What's Next

You now understand the building blocks of programming: variables, control flow, and functions. Next, let's put it all together and learn how to solve problems.

**[Continue to Problem Solving](05-problem-solving.md)**

---

## Quick Reference

| Concept | Example |
|---------|---------|
| Function (no return) | `void DoThing() { }` |
| Function (with return) | `int GetValue() { return 5; }` |
| Parameters | `void Greet(string name) { }` |
| Calling | `Greet("Alice");` |
| Return | `return value;` |
| Local variable | Only exists inside its function |
