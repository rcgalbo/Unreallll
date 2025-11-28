# Problem Solving

Programming is problem solving. The code is just how you express your solution. This guide teaches you how to approach problems systematically.

## What You'll Learn

- A step-by-step problem-solving process
- How to break down complex problems
- Common problem-solving patterns
- How to get unstuck

## The Problem-Solving Process

### Step 1: Understand the Problem

Before writing ANY code, make sure you understand what you're solving.

**Ask yourself:**
- What is the input? (What do I start with?)
- What is the output? (What do I need to produce?)
- What are the constraints? (What rules must I follow?)
- What are edge cases? (What unusual situations might occur?)

**Example Problem:** "Make the player jump"

**Understanding it:**
- Input: Player presses jump button
- Output: Character moves upward, then falls back down
- Constraints: Can only jump when on ground, jump has max height
- Edge cases: What if player is already jumping? What if there's a ceiling?

### Step 2: Plan Your Approach

Don't start coding immediately. Plan first.

**Write out the steps in plain English:**
```
1. Check if jump button is pressed
2. Check if player is on the ground
3. If both true:
   a. Apply upward velocity
   b. Set "is jumping" to true
   c. Play jump animation
   d. Play jump sound
4. While in air:
   a. Apply gravity (reduce upward velocity)
   b. Check for ceiling collision
5. When player hits ground:
   a. Set "is jumping" to false
   b. Allow jumping again
```

### Step 3: Divide and Conquer

Break the problem into smaller sub-problems:

```
"Make player jump" becomes:
├── Detect jump input
├── Check if grounded
├── Apply physics
│   ├── Initial upward force
│   └── Gravity
├── Handle collisions
│   ├── Ground detection
│   └── Ceiling detection
└── Play feedback
    ├── Animation
    └── Sound
```

Solve each small problem individually.

### Step 4: Start Small

Don't try to build everything at once:

```
Version 1: Press button → character moves up instantly
Version 2: Add gravity so character falls back down
Version 3: Only allow jump when on ground
Version 4: Add jump animation
Version 5: Add sound
Version 6: Handle ceiling collision
```

Get something working, then improve it.

### Step 5: Test Frequently

After each change:
1. Test that the new thing works
2. Test that old things still work
3. Test edge cases

Don't write 100 lines then test. Write 5 lines, test, repeat.

### Step 6: Debug Systematically

When something doesn't work:

1. **Reproduce** - Can you make it happen consistently?
2. **Isolate** - What's the smallest code that shows the problem?
3. **Hypothesize** - What do you THINK is wrong?
4. **Test** - Check if your hypothesis is correct
5. **Fix** - Make ONE change
6. **Verify** - Did the fix work? Did it break anything else?

## Problem-Solving Patterns

### Pattern 1: Input → Process → Output

Most problems follow this structure:

```
Input:    Raw data (player presses button)
Process:  Transform/calculate (check conditions, apply physics)
Output:   Result (character jumps)
```

Identify each part for your problem.

### Pattern 2: State Machines

Many problems involve states and transitions:

```
States:     [Idle] [Walking] [Jumping] [Falling] [Dead]
Transitions:
  Idle → Walking:  Movement input
  Idle → Jumping:  Jump input
  Jumping → Falling: Reached peak height
  Falling → Idle:  Hit ground
  Any → Dead:      Health <= 0
```

Draw the states and transitions before coding.

### Pattern 3: Search

Finding something in a collection:

```
For each item in collection:
    If item matches what we're looking for:
        Return item (found it!)
Return "not found"
```

Example: Find nearest enemy
```
nearestEnemy = null
nearestDistance = infinity

For each enemy:
    distance = DistanceTo(enemy)
    If distance < nearestDistance:
        nearestDistance = distance
        nearestEnemy = enemy

Return nearestEnemy
```

### Pattern 4: Accumulation

Building up a result:

```
result = starting value

For each item:
    Update result based on item

Return result
```

Example: Calculate total damage
```
totalDamage = 0

For each attack in combo:
    totalDamage = totalDamage + attack.damage

Return totalDamage
```

### Pattern 5: Filtering

Keeping only items that match criteria:

```
result = empty list

For each item:
    If item matches criteria:
        Add item to result

Return result
```

Example: Get all enemies in range
```
enemiesInRange = empty list

For each enemy:
    If DistanceTo(enemy) < attackRange:
        Add enemy to enemiesInRange

Return enemiesInRange
```

### Pattern 6: Transformation

Converting each item to something else:

```
result = empty list

For each item:
    newItem = Transform(item)
    Add newItem to result

Return result
```

Example: Get all enemy positions
```
positions = empty list

For each enemy:
    position = enemy.GetPosition()
    Add position to positions

Return positions
```

## How to Get Unstuck

Everyone gets stuck. Here's how to get moving again:

### 1. Rubber Duck Debugging

Explain your problem out loud, line by line, to an inanimate object. The act of explaining often reveals the issue.

### 2. Take a Break

Walk away. Get water. Sleep on it. Your brain processes problems in the background.

### 3. Simplify

Remove complexity until it works:
- Comment out code until the bug disappears
- Use hardcoded values instead of variables
- Test with the simplest possible input

### 4. Check Your Assumptions

Write down what you THINK is happening. Then verify each assumption:
- "I think this variable is 10" → Add print statement to check
- "I think this function is being called" → Add print statement
- "I think this condition is true" → Print the condition's value

### 5. Read the Error Message

Really read it. Error messages often tell you:
- What went wrong
- Where it went wrong (file and line number)
- Sometimes why

### 6. Search for the Error

Copy the error message and search online. Someone else has probably had the same problem.

### 7. Ask for Help

When asking for help, provide:
- What you're trying to do
- What you expected to happen
- What actually happened
- What you've already tried
- Relevant code

Bad: "It doesn't work"
Good: "I'm trying to make the player jump. I expected pressing Space to move the character up, but nothing happens. I've verified the input is being detected (I added a print statement). Here's my jump code: [code]"

## Worked Example: Health System

Let's solve a problem step by step.

**Problem:** Create a health system where:
- Player starts with 100 health
- Player can take damage
- Player can heal
- Health can't go below 0 or above 100
- When health reaches 0, player dies

### Step 1: Understand

- Input: Damage amounts, heal amounts
- Output: Current health, death state
- Constraints: Health between 0-100
- Edge cases: Damage > current health, healing when full

### Step 2: Plan

```
1. Store current health (start at 100)
2. Store max health (100)
3. TakeDamage function:
   - Subtract damage from health
   - If health < 0, set to 0
   - If health == 0, trigger death
4. Heal function:
   - Add healing to health
   - If health > max, set to max
5. IsDead function:
   - Return true if health <= 0
```

### Step 3: Divide

Sub-problems:
- Store health value ✓
- TakeDamage function
- Heal function
- Clamp health to valid range
- Detect death

### Step 4: Implement (Start Small)

**Version 1: Just store health**
```cpp
int health = 100;
int maxHealth = 100;
```

**Version 2: Add TakeDamage**
```cpp
void TakeDamage(int amount) {
    health = health - amount;
    print("Health: " + health);
}
```

Test: `TakeDamage(30)` → Health should be 70 ✓

**Version 3: Clamp minimum**
```cpp
void TakeDamage(int amount) {
    health = health - amount;
    if (health < 0) {
        health = 0;
    }
    print("Health: " + health);
}
```

Test: `TakeDamage(150)` → Health should be 0, not -50 ✓

**Version 4: Add death detection**
```cpp
void TakeDamage(int amount) {
    health = health - amount;
    if (health <= 0) {
        health = 0;
        Die();
    }
}

void Die() {
    print("Player died!");
    // Handle death...
}
```

**Version 5: Add Heal**
```cpp
void Heal(int amount) {
    health = health + amount;
    if (health > maxHealth) {
        health = maxHealth;
    }
}
```

Test: When health is 50, `Heal(30)` → 80 ✓
Test: When health is 90, `Heal(30)` → 100 (not 120) ✓

**Final Version:**
```cpp
int health = 100;
int maxHealth = 100;

void TakeDamage(int amount) {
    health = health - amount;
    if (health <= 0) {
        health = 0;
        Die();
    }
}

void Heal(int amount) {
    health = health + amount;
    if (health > maxHealth) {
        health = maxHealth;
    }
}

bool IsDead() {
    return health <= 0;
}

void Die() {
    // Handle player death
}
```

## Exercises

### Exercise 1: Plan First

Before coding, write out the steps (in plain English) for:
"Display a countdown timer that goes from 10 to 0, then shows 'GO!'"

### Exercise 2: Break It Down

Break this problem into smaller sub-problems:
"Create an inventory system where players can pick up items, drop items, and see what they're carrying"

### Exercise 3: Identify the Pattern

Which pattern (search, accumulation, filtering, transformation) would you use for:
1. Find the enemy with the most health
2. Calculate total score from all coins collected
3. Get a list of all unlocked achievements
4. Convert player names to uppercase

### Exercise 4: Debug This

This code should print numbers 1 to 5, but it doesn't work:
```cpp
int i = 1;
while (i < 5) {
    print(i);
}
```
What's wrong? How would you find the bug?

## Key Takeaways

1. **Understand before coding** - Know the inputs, outputs, and constraints
2. **Plan your approach** - Write steps in plain English first
3. **Divide and conquer** - Break big problems into small ones
4. **Start small** - Get something working, then improve
5. **Test frequently** - Don't write too much before testing
6. **Debug systematically** - Reproduce, isolate, hypothesize, test, fix
7. **Learn patterns** - Recognize common problem structures

## What's Next

You now have a foundation in programming thinking. Next, let's dive deeper into computer science concepts that will help you understand how games work under the hood.

**[Continue to How Computers Work](../computer-science/01-how-computers-work.md)**

---

## Quick Reference: Problem-Solving Checklist

- [ ] Do I understand what the problem is asking?
- [ ] Do I know the inputs and expected outputs?
- [ ] Have I identified edge cases?
- [ ] Have I written out the steps in plain English?
- [ ] Have I broken it into smaller sub-problems?
- [ ] Am I starting with the simplest version?
- [ ] Am I testing after each small change?
- [ ] If stuck: Have I tried rubber duck debugging?
- [ ] If stuck: Have I checked my assumptions?
- [ ] If stuck: Have I read the error message carefully?
