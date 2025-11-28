# Thinking Like a Programmer

Before writing any code, you need to understand how programmers approach problems. This isn't about memorizing syntax - it's about developing a way of thinking.

## What You'll Learn

- How to break down problems into smaller parts
- How to think in steps (algorithms)
- How to read and understand code
- How to approach errors and bugs

## The Programmer's Mindset

Programming is **problem solving**. The code is just how you express your solution.

When you see a programmer working, most of their time isn't spent typing. They're:
- Understanding the problem
- Planning a solution
- Testing ideas
- Debugging when things don't work

The typing part is often the smallest piece.

## Breaking Down Problems

The most important skill in programming: **decomposition** - breaking big problems into smaller ones.

### Example: Making a Sandwich

Let's say you need to write instructions for a robot to make a peanut butter sandwich. A human would just say "make a sandwich." But a robot (or computer) needs exact steps:

**Too vague:**
```
Make a peanut butter sandwich
```

**Better:**
```
1. Get bread from cabinet
2. Get peanut butter from cabinet
3. Get knife from drawer
4. Open bread bag
5. Remove two slices of bread
6. Open peanut butter jar
7. Use knife to scoop peanut butter
8. Spread peanut butter on one slice
9. Place second slice on top
10. Put away ingredients
```

**Even better (handling edge cases):**
```
1. Check if we have bread
   - If no bread, go to store or report error
2. Check if we have peanut butter
   - If no peanut butter, go to store or report error
3. Get bread from cabinet
...and so on
```

### Why This Matters for Games

Making a game character jump seems simple. But decomposed:

1. Detect when player presses jump button
2. Check if character is on the ground
3. If on ground, apply upward force
4. Play jump animation
5. Play jump sound
6. While in air, apply gravity
7. Detect when character hits ground
8. Stop upward movement
9. Play landing animation
10. Allow jumping again

Each of these steps can be broken down further. That's how all games are built.

## Thinking in Steps (Algorithms)

An **algorithm** is just a sequence of steps to accomplish a task. You use algorithms every day:

- Recipe for cooking
- Directions to a location
- Morning routine

In programming, we write algorithms that computers can follow.

### Key Properties of Good Algorithms

**1. Precise** - Each step is clearly defined
```
Bad:  "Move forward a bit"
Good: "Move forward 10 units"
```

**2. Unambiguous** - Only one way to interpret each step
```
Bad:  "If the player is close, attack"
Good: "If the player is within 5 meters, attack"
```

**3. Finite** - It eventually ends
```
Bad:  "Keep looking until you find it" (what if it doesn't exist?)
Good: "Look for 10 seconds, then give up if not found"
```

**4. Complete** - Handles all cases
```
Bad:  "Move forward" (what if there's a wall?)
Good: "If path is clear, move forward. Otherwise, stop."
```

### Example: Finding the Largest Number

Problem: Given three numbers, find the largest one.

**Algorithm:**
```
1. Assume the first number is the largest
2. Compare with the second number
   - If second is larger, it becomes the new largest
3. Compare current largest with third number
   - If third is larger, it becomes the new largest
4. The current largest is your answer
```

**Walking through it with numbers 5, 12, 8:**
```
1. Assume 5 is largest (largest = 5)
2. Compare 5 with 12 → 12 is larger (largest = 12)
3. Compare 12 with 8 → 12 is still larger (largest = 12)
4. Answer: 12
```

This algorithm works for ANY three numbers. That's the power of algorithmic thinking.

## Reading Code

Before writing code, learn to read it. Code is written to be read by humans (computers don't care about formatting).

### Read Top to Bottom

Code generally executes from top to bottom:

```
line 1 runs first
line 2 runs second
line 3 runs third
```

### Follow the Flow

Some code changes the order of execution:

```
if it's raining:
    bring umbrella
    wear boots
otherwise:
    wear sunglasses

always lock the door
```

The code takes different paths depending on conditions, then continues.

### Understand One Line at a Time

Don't try to understand everything at once. Take it line by line:

```cpp
int health = 100;        // Create a number called health, set it to 100
health = health - 20;    // Subtract 20 from health (now it's 80)
if (health <= 0) {       // If health is 0 or less...
    GameOver();          // ...run the GameOver function
}
```

### Look for Patterns

As you read more code, you'll recognize common patterns:

- "If this, then that" (conditions)
- "Do this 10 times" (loops)
- "Remember this value" (variables)
- "Do this calculation" (functions)

## How to Approach Errors

Errors are not failures - they're information. Every programmer encounters errors constantly.

### Types of Errors

**Syntax Errors** - You wrote something the computer doesn't understand
```
Like writing "teh" instead of "the"
The computer tells you exactly where the mistake is
Usually easy to fix
```

**Logic Errors** - Your code runs but does the wrong thing
```
Like giving someone directions that lead to the wrong place
The computer doesn't know it's wrong
Harder to find - requires testing
```

**Runtime Errors** - Code crashes while running
```
Like dividing by zero
Computer can't continue
Often gives a clue about what went wrong
```

### Debugging Mindset

When something doesn't work:

**1. Don't panic.** Errors are normal.

**2. Read the error message.** It usually tells you:
   - What went wrong
   - Where it went wrong (file name, line number)
   - Sometimes why

**3. Reproduce the problem.** Can you make it happen again?

**4. Isolate the issue.** What's the smallest amount of code that causes the problem?

**5. Check your assumptions.** What do you THINK is happening vs. what's ACTUALLY happening?

**6. Make one change at a time.** Change something, test, repeat.

**7. Take breaks.** Fresh eyes often spot problems immediately.

### The Rubber Duck Method

Explain your code, line by line, to an inanimate object (traditionally a rubber duck). The act of explaining often reveals the problem.

This sounds silly but it works. When you explain something out loud, you process it differently than when you just read it.

## Exercises

### Exercise 1: Decompose a Task

Break down "make the player character attack an enemy" into steps. Consider:
- How do you know the player wants to attack?
- How do you determine which enemy to attack?
- What happens during the attack?
- What happens to the enemy?

### Exercise 2: Write an Algorithm

Write step-by-step instructions for:
"Determine if a number is even or odd"

Remember: be precise, unambiguous, and complete.

### Exercise 3: Trace Through Code

For each line, write what happens:

```
lives = 3
score = 0
score = score + 100
if lives > 0:
    play_game()
```

### Exercise 4: Find the Error

What's wrong with this algorithm for "check if player won"?

```
1. If player has all gems, player wins
2. Display "You Win!"
```

(Hint: What if the player doesn't have all gems?)

## Key Takeaways

1. **Decomposition** - Break big problems into small ones
2. **Algorithms** - Precise, step-by-step instructions
3. **Reading code** - One line at a time, follow the flow
4. **Errors are information** - Read them, don't fear them
5. **The computer does exactly what you tell it** - If it's wrong, your instructions are wrong

## Common Beginner Mistakes

- Trying to understand everything at once
- Jumping straight to coding without planning
- Ignoring error messages
- Changing multiple things at once when debugging
- Not testing after each small change

## What's Next

Now that you understand how to think about problems, let's learn about the basic building blocks of code: variables and data.

**[Continue to Variables and Data](02-variables-and-data.md)**

---

## Quick Reference

| Concept | Description |
|---------|-------------|
| Decomposition | Breaking big problems into smaller ones |
| Algorithm | Step-by-step instructions to solve a problem |
| Syntax error | Code the computer can't understand |
| Logic error | Code that runs but does the wrong thing |
| Runtime error | Code that crashes while running |
| Debugging | Finding and fixing errors |
