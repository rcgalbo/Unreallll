# Variables and Data

Every program works with data - numbers, text, true/false values, and more. Variables are how we store and manipulate this data.

## What You'll Learn

- What variables are and why we need them
- Different types of data
- How to name variables well
- How variables work in memory

## What Is a Variable?

A **variable** is a named container that holds a value.

Think of it like a labeled box:
- The **label** (variable name) lets you find the box
- The **contents** (value) is what's inside
- You can look inside, change the contents, or use what's inside

### Real-World Analogy

Imagine you're tracking a game character:

```
Box labeled "health":     contains 100
Box labeled "score":      contains 0
Box labeled "playerName": contains "Alex"
Box labeled "isAlive":    contains true
```

In code (pseudocode - not any specific language):

```
health = 100
score = 0
playerName = "Alex"
isAlive = true
```

## Why Variables Matter

Without variables, you'd have to hardcode every value:

**Without variables (bad):**
```
display "Player 1 has 100 health"
# Later, if health changes, you'd need a completely different line
display "Player 1 has 80 health"
```

**With variables (good):**
```
health = 100
display "Player 1 has " + health + " health"
health = 80
display "Player 1 has " + health + " health"
```

The same display line works with any health value.

## Types of Data

Computers need to know what KIND of data you're storing. This is called the **data type**.

### Numbers: Integers

**Integers** are whole numbers (no decimal point):
```
age = 25
lives = 3
score = 0
damage = -10
```

Used for: counting, indexing, scores, health points

### Numbers: Floating Point

**Floats** (or **doubles**) are numbers with decimals:
```
speed = 5.5
percentage = 99.9
pi = 3.14159
temperature = -40.0
```

Used for: positions, physics calculations, percentages

### Why Two Number Types?

- **Integers** are faster and use less memory
- **Floats** can represent more values but have precision limits
- Use integers when you don't need decimals
- Use floats when precision matters

**Gotcha:** Float math can be imprecise:
```
0.1 + 0.2 might equal 0.30000000000000004
```
This is a fundamental computer science issue, not a bug.

### Text: Strings

**Strings** are sequences of characters (text):
```
playerName = "Alex"
greeting = "Hello, World!"
emptyString = ""
sentence = "The score is 100"
```

Note the quotation marks - they tell the computer "this is text, not code."

### True/False: Booleans

**Booleans** can only be `true` or `false`:
```
isAlive = true
hasKey = false
gameOver = false
isPaused = true
```

Used for: conditions, flags, on/off states

Named after George Boole, who invented Boolean algebra.

### Summary of Basic Types

| Type | What It Holds | Examples |
|------|--------------|----------|
| Integer | Whole numbers | 0, 42, -17, 1000000 |
| Float/Double | Decimal numbers | 3.14, -0.5, 100.0 |
| String | Text | "hello", "Player 1", "" |
| Boolean | True or false | true, false |

## Declaring Variables

**Declaring** a variable means creating it and giving it a name.

### In Unreal (Blueprints)

You create a variable in the Variables panel, choose its type, and give it a name.

### In C++ (Unreal's Code Language)

```cpp
int health = 100;           // Integer
float speed = 5.5f;         // Float (note the 'f')
FString name = "Player";    // String (Unreal's string type)
bool isAlive = true;        // Boolean
```

### In Python (Simpler Syntax)

```python
health = 100        # Python figures out it's an integer
speed = 5.5         # Python figures out it's a float
name = "Player"     # Python figures out it's a string
is_alive = True     # Python figures out it's a boolean
```

## Naming Variables

Good variable names make code readable. Bad names make code confusing.

### Rules (Most Languages)

1. Start with a letter or underscore (not a number)
2. Can contain letters, numbers, underscores
3. Cannot contain spaces
4. Cannot be reserved words (like `if`, `else`, `for`)
5. Case-sensitive (`Health` and `health` are different)

### Good Names

```
playerHealth      // Clear what it represents
currentScore      // 'current' adds useful context
isGameOver        // 'is' prefix indicates boolean
enemyCount        // Clear and specific
maxSpeed          // 'max' indicates it's a limit
```

### Bad Names

```
x                 // What is x?
data              // Too vague
temp              // Temporary what?
thing             // Meaningless
myVariable        // Says nothing useful
a1                // Cryptic
```

### Naming Conventions

Different languages/teams use different styles:

**camelCase** - First word lowercase, others capitalized
```
playerHealth
currentScore
isGameOver
```

**PascalCase** - Every word capitalized (common for class names)
```
PlayerHealth
CurrentScore
IsGameOver
```

**snake_case** - Words separated by underscores
```
player_health
current_score
is_game_over
```

Pick one style and stick with it. Unreal Engine typically uses PascalCase for most things.

## Changing Variables

Variables can change (that's why they're called variables).

### Assignment

The `=` sign assigns a value:
```
health = 100      // health is now 100
health = 80       // health is now 80 (the 100 is gone)
```

### Updating Based on Current Value

Often you want to change a variable relative to its current value:

```
score = 0
score = score + 10    // score is now 10
score = score + 10    // score is now 20
score = score * 2     // score is now 40
```

The right side is calculated first, then assigned to the left side.

### Shorthand (Many Languages)

```
score += 10    // Same as: score = score + 10
health -= 20   // Same as: health = health - 20
speed *= 2     // Same as: speed = speed * 2
lives++        // Same as: lives = lives + 1
lives--        // Same as: lives = lives - 1
```

## How Variables Work in Memory

Understanding this helps you understand how computers think.

### Memory as Boxes

Computer memory is like a huge wall of numbered boxes:

```
Address   | Value
----------|--------
1000      | 100      <- health lives here
1001      | 0        <- score lives here
1002      | "Alex"   <- playerName lives here
1003      | true     <- isAlive lives here
```

When you write `health = 100`:
1. Computer finds an empty box (address 1000)
2. Puts the value 100 in it
3. Remembers "health" means "look in box 1000"

When you write `health = 80`:
1. Computer looks up "health" → address 1000
2. Replaces 100 with 80
3. The 100 is gone forever

### Variables vs. Values

The variable is the NAME, not the value:
```
a = 5
b = a      // b gets the VALUE of a (5), not linked to a
a = 10     // a is now 10, b is still 5
```

Think of it as copying the contents of one box into another - they're not connected.

## Constants

Sometimes you want a value that NEVER changes. These are **constants**:

```cpp
const float PI = 3.14159f;
const int MAX_LIVES = 3;
const FString GAME_TITLE = "My Awesome Game";
```

The `const` keyword tells the computer "don't let this change."

Why use constants?
- Prevents accidental changes
- Makes code more readable
- Easy to update in one place

## Common Mistakes

### Using Before Declaring

```cpp
health = health - 10;  // Error! health doesn't exist yet
int health = 100;
```

Always declare variables before using them.

### Type Mismatch

```cpp
int health = "one hundred";  // Error! Can't put text in an integer
```

The type must match the value.

### Confusing = and ==

```
=  means "assign this value"
== means "check if equal" (we'll cover this later)
```

```cpp
health = 100;     // Sets health to 100
health == 100;    // Checks if health equals 100 (doesn't change anything)
```

## Exercises

### Exercise 1: Choose the Type

What data type would you use for:
1. Number of enemies on screen
2. Player's current position (x coordinate)
3. Player's name
4. Whether the game is paused
5. Current level number
6. Player's high score
7. An item's description

### Exercise 2: Trace the Values

What is the value of each variable after this code runs?

```
a = 10
b = 5
c = a + b
a = a - 3
b = c
c = a * 2
```

### Exercise 3: Name These Variables

Come up with good variable names for:
1. The player's remaining ammunition
2. Whether the player has collected the red key
3. The enemy's movement speed
4. The total number of coins in the level
5. The time remaining in the round

### Exercise 4: Fix the Errors

What's wrong with each line?

```
1. 2ndPlayer = "Bob"
2. int score = 100.5
3. string message = Hello World
4. bool hasWon = "true"
```

## Key Takeaways

1. **Variables store data** with a name and value
2. **Types matter** - integers, floats, strings, booleans
3. **Good names** make code readable
4. **Variables can change** - that's their purpose
5. **Memory** holds variables at addresses

## What's Next

Now you know how to store data. Next, let's learn how to make decisions and control what code runs.

**[Continue to Control Flow](03-control-flow.md)**

---

## Quick Reference

| Concept | Description |
|---------|-------------|
| Variable | Named container for a value |
| Integer | Whole number |
| Float | Decimal number |
| String | Text |
| Boolean | True or false |
| Declaration | Creating a variable |
| Assignment | Giving a variable a value |
| Constant | Variable that cannot change |
