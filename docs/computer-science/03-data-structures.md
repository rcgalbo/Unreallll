# Data Structures

Data structures are ways of organizing data. Choosing the right structure makes your code faster and simpler.

## What You'll Learn

- Common data structures and when to use them
- How each structure works internally
- Performance characteristics
- Practical game development examples

## Why Data Structures Matter

Different structures are good at different things:

```
Need to:                      Use:
Look up by name              → Dictionary/Map
Keep items in order          → Array/List
Process in order received    → Queue
Process most recent first    → Stack
Fast add/remove anywhere     → Linked List
```

Choosing wrong can make your code 100x slower.

## Arrays

The most basic structure: a fixed-size sequence of elements.

### How Arrays Work

Elements are stored consecutively in memory:

```
Index:    0     1     2     3     4
        ┌─────┬─────┬─────┬─────┬─────┐
Value:  │  10 │  20 │  30 │  40 │  50 │
        └─────┴─────┴─────┴─────┴─────┘
Address: 100   104   108   112   116
```

### Array Operations

| Operation | Time | Explanation |
|-----------|------|-------------|
| Access by index | O(1) | Direct calculation: address = start + (index × size) |
| Search | O(n) | Must check each element |
| Insert at end | O(1) | If space available |
| Insert in middle | O(n) | Must shift elements |
| Delete | O(n) | Must shift elements |

### When to Use Arrays

**Good for:**
- Known, fixed number of elements
- Frequent access by index
- Iterating through all elements

**Bad for:**
- Frequently adding/removing elements
- Unknown number of elements
- Searching for specific values

### Game Example: Inventory Slots

```cpp
// Fixed 10 inventory slots
Item inventory[10];

// Access slot 3 (instant)
Item item = inventory[3];

// Check all slots
for (int i = 0; i < 10; i++) {
    if (inventory[i].name == "Sword") {
        // Found it
    }
}
```

## Dynamic Arrays (Lists)

Like arrays, but can grow and shrink.

### How They Work

Start with small array, grow when needed:

```
Initial:  [_, _, _, _]     (capacity 4)
Add 5 items → must grow!
New:      [1, 2, 3, 4, 5, _, _, _]  (capacity 8, doubled)
```

When array is full, allocate new larger array and copy everything.

### Dynamic Array Operations

| Operation | Time | Note |
|-----------|------|------|
| Access by index | O(1) | Same as array |
| Add at end | O(1)* | *Amortized - usually fast, occasionally slow |
| Add in middle | O(n) | Must shift elements |
| Remove | O(n) | Must shift elements |
| Search | O(n) | Must check each |

### When to Use

**Good for:**
- Unknown number of elements
- Mostly adding at the end
- Frequent index access

**Bad for:**
- Frequent insertions/deletions in the middle
- Real-time code where even occasional slowdowns matter

### In Unreal Engine

```cpp
// TArray is Unreal's dynamic array
TArray<AActor*> Enemies;

Enemies.Add(enemy1);       // Add to end
Enemies[0];                // Access by index
Enemies.Remove(enemy1);    // Remove (shifts others)
Enemies.Num();             // Get count
```

## Linked Lists

Elements connected by pointers, not stored consecutively.

### How They Work

```
┌───────────┐    ┌───────────┐    ┌───────────┐
│ Value: 10 │───▶│ Value: 20 │───▶│ Value: 30 │───▶ null
│ Next: ────│    │ Next: ────│    │ Next: ────│
└───────────┘    └───────────┘    └───────────┘
```

Each element (node) contains data and a pointer to the next element.

### Linked List Operations

| Operation | Time | Explanation |
|-----------|------|-------------|
| Access by index | O(n) | Must traverse from start |
| Search | O(n) | Must traverse |
| Insert at start | O(1) | Just update pointers |
| Insert in middle | O(1)* | *If you have the position |
| Delete | O(1)* | *If you have the position |

### When to Use

**Good for:**
- Frequent insert/delete at known positions
- Don't need random access

**Bad for:**
- Frequent access by index
- Memory efficiency (extra pointer per element)

### Game Example: Undo System

```cpp
// Each action points to previous
Action* currentAction;

void PerformAction(Action* newAction) {
    newAction->previous = currentAction;
    currentAction = newAction;
}

void Undo() {
    currentAction->Revert();
    currentAction = currentAction->previous;
}
```

## Stacks

Last In, First Out (LIFO) - like a stack of plates.

### How They Work

```
Push 10: [10]
Push 20: [10, 20]
Push 30: [10, 20, 30]
Pop:     [10, 20]      → Returns 30
Pop:     [10]          → Returns 20
```

### Stack Operations

| Operation | Time |
|-----------|------|
| Push (add to top) | O(1) |
| Pop (remove from top) | O(1) |
| Peek (view top) | O(1) |

### When to Use

- Undo/redo systems
- Parsing expressions
- Tracking state that can be reversed
- Depth-first search

### Game Example: Menu Navigation

```cpp
Stack<Menu> menuStack;

void OpenMenu(Menu menu) {
    menuStack.Push(menu);
    menu.Show();
}

void GoBack() {
    menuStack.Pop().Hide();
    menuStack.Peek().Show();  // Show previous menu
}
```

## Queues

First In, First Out (FIFO) - like a line at a store.

### How They Work

```
Enqueue 10: [10]
Enqueue 20: [10, 20]
Enqueue 30: [10, 20, 30]
Dequeue:    [20, 30]      → Returns 10
Dequeue:    [30]          → Returns 20
```

### Queue Operations

| Operation | Time |
|-----------|------|
| Enqueue (add to back) | O(1) |
| Dequeue (remove from front) | O(1) |
| Peek (view front) | O(1) |

### When to Use

- Processing in order received
- Task scheduling
- Breadth-first search
- Event systems

### Game Example: Action Queue

```cpp
Queue<Action> actionQueue;

void QueueAction(Action action) {
    actionQueue.Enqueue(action);
}

void ProcessNextAction() {
    if (!actionQueue.IsEmpty()) {
        Action next = actionQueue.Dequeue();
        next.Execute();
    }
}
```

## Hash Maps (Dictionaries)

Store key-value pairs with fast lookup.

### How They Work

Keys are converted to array indices via a hash function:

```
Key "health" → hash("health") → 7
Key "score"  → hash("score")  → 3

Index:  0    1    2    3    4    5    6    7
      [   ][   ][   ][500][   ][   ][   ][100]
                      score          health
```

### Hash Map Operations

| Operation | Average Time | Worst Time |
|-----------|-------------|------------|
| Insert | O(1) | O(n) |
| Lookup | O(1) | O(n) |
| Delete | O(1) | O(n) |

Worst case happens when many keys hash to same index (collision).

### When to Use

**Good for:**
- Fast lookup by key
- Associating data with identifiers
- Counting occurrences

**Bad for:**
- Ordered data
- Finding min/max
- Iterating in order

### In Unreal Engine

```cpp
// TMap is Unreal's hash map
TMap<FString, int32> PlayerScores;

PlayerScores.Add("Alice", 1000);
PlayerScores.Add("Bob", 800);

int32* Score = PlayerScores.Find("Alice");  // O(1) lookup
```

### Game Example: Entity Lookup

```cpp
TMap<int32, AActor*> EntityById;

// Register entity
EntityById.Add(entity->GetId(), entity);

// Find entity by ID (fast!)
AActor* found = EntityById[targetId];
```

## Sets

Collection of unique elements.

### How They Work

Like a hash map but only stores keys, not values.

### Set Operations

| Operation | Time |
|-----------|------|
| Add | O(1) |
| Remove | O(1) |
| Contains | O(1) |

### When to Use

- Tracking unique items
- Fast membership testing
- Removing duplicates

### In Unreal Engine

```cpp
TSet<AActor*> VisitedRooms;

void EnterRoom(AActor* room) {
    if (!VisitedRooms.Contains(room)) {
        VisitedRooms.Add(room);
        GiveExplorationBonus();
    }
}
```

## Trees

Hierarchical structures with parent-child relationships.

### Binary Tree

Each node has at most two children:

```
        10
       /  \
      5    15
     / \   / \
    3   7 12  20
```

### Binary Search Tree (BST)

Ordered: left < parent < right

| Operation | Average | Worst |
|-----------|---------|-------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |

### When to Use Trees

- Hierarchical data (scene graphs, UI)
- Sorted data with fast search
- Spatial partitioning

### Game Example: Scene Graph

```
World
├── Level
│   ├── Player
│   │   ├── Camera
│   │   └── Weapon
│   ├── Enemy1
│   └── Enemy2
└── UI
    ├── HealthBar
    └── Minimap
```

Moving the Player also moves Camera and Weapon.

## Choosing the Right Structure

| Need | Use | Why |
|------|-----|-----|
| Fixed collection, fast access | Array | O(1) access by index |
| Growing collection | Dynamic Array | Flexible size, fast access |
| Fast insert/delete | Linked List | O(1) at known position |
| Process in order | Queue | FIFO order |
| Reverse order / undo | Stack | LIFO order |
| Fast lookup by key | Hash Map | O(1) average lookup |
| Unique elements | Set | O(1) contains check |
| Hierarchical data | Tree | Natural representation |
| Sorted data | BST / Sorted Array | Fast search |

## Practical Considerations

### Memory Cache

Arrays are faster in practice than their Big O suggests because of CPU caching:

```
Array:       [1][2][3][4][5]  ← Consecutive in memory, cache-friendly
Linked List: [1]→[2]→[3]     ← Scattered in memory, cache-unfriendly
```

Modern CPUs load chunks of memory at once. Arrays benefit; linked lists don't.

### Unreal's Containers

| Structure | Unreal Type |
|-----------|-------------|
| Dynamic Array | TArray |
| Hash Map | TMap |
| Set | TSet |
| Queue | TQueue |
| Linked List | TLinkedList |

## Exercises

### Exercise 1: Choose the Structure

What structure would you use for:
1. A list of high scores, sorted highest to lowest
2. Tracking which enemies the player has defeated
3. A conversation system where responses are processed in order
4. Looking up item stats by item name

### Exercise 2: Analyze This Code

What's the time complexity?

```cpp
bool Contains(int[] array, int value) {
    for (int i = 0; i < array.length; i++) {
        if (array[i] == value) return true;
    }
    return false;
}
```

How could you make it faster?

### Exercise 3: Implement a Stack

Using an array, implement:
- Push(value)
- Pop() → returns value
- IsEmpty() → returns bool

## Key Takeaways

1. **Arrays** for fast access by index
2. **Hash maps** for fast access by key
3. **Stacks** for LIFO (undo, parsing)
4. **Queues** for FIFO (processing order)
5. **Sets** for unique elements and membership testing
6. **Choose based on operations you need most**

## What's Next

Now that you understand how to organize data, let's look at algorithms - the procedures for processing that data efficiently.

**[Continue to Algorithm Basics](04-algorithms-basics.md)**

---

## Quick Reference

| Structure | Access | Insert | Delete | Search |
|-----------|--------|--------|--------|--------|
| Array | O(1) | O(n) | O(n) | O(n) |
| Dynamic Array | O(1) | O(1)* | O(n) | O(n) |
| Linked List | O(n) | O(1) | O(1) | O(n) |
| Hash Map | - | O(1) | O(1) | O(1) |
| BST | - | O(log n) | O(log n) | O(log n) |

*Amortized
