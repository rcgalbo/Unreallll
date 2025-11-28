# Algorithm Basics

Algorithms are step-by-step procedures for solving problems. Understanding common algorithms helps you write efficient code and recognize when to use existing solutions.

## What You'll Learn

- How to analyze algorithm efficiency
- Common searching algorithms
- Common sorting algorithms
- Pathfinding basics
- When to use which algorithm

## Algorithm Efficiency: Big O Notation

Big O describes how an algorithm's time (or space) grows with input size.

### Common Complexities

```
O(1)       Constant    - Same time regardless of size
O(log n)   Logarithmic - Time grows slowly
O(n)       Linear      - Time grows with size
O(n log n) Linearithmic- Common for good sorting
O(n²)      Quadratic   - Time grows with square of size
O(2ⁿ)      Exponential - Time doubles with each element
```

### Visual Comparison

For n = 1000 elements:

| Complexity | Operations |
|------------|------------|
| O(1) | 1 |
| O(log n) | ~10 |
| O(n) | 1,000 |
| O(n log n) | ~10,000 |
| O(n²) | 1,000,000 |
| O(2ⁿ) | More than atoms in universe |

### Why It Matters

```cpp
// O(n) - Fine for 1000 enemies
for (enemy in enemies) {
    Update(enemy);
}

// O(n²) - Disaster for 1000 enemies (1,000,000 operations!)
for (enemy1 in enemies) {
    for (enemy2 in enemies) {
        CheckCollision(enemy1, enemy2);
    }
}
```

## Searching Algorithms

### Linear Search

Check each element one by one.

```cpp
int LinearSearch(int[] array, int target) {
    for (int i = 0; i < array.length; i++) {
        if (array[i] == target) {
            return i;  // Found at index i
        }
    }
    return -1;  // Not found
}
```

**Complexity:** O(n)
**Use when:** Data is unsorted, small dataset, one-time search

### Binary Search

Repeatedly divide sorted data in half.

```cpp
int BinarySearch(int[] sortedArray, int target) {
    int left = 0;
    int right = sortedArray.length - 1;

    while (left <= right) {
        int mid = (left + right) / 2;

        if (sortedArray[mid] == target) {
            return mid;  // Found
        }
        else if (sortedArray[mid] < target) {
            left = mid + 1;  // Search right half
        }
        else {
            right = mid - 1;  // Search left half
        }
    }
    return -1;  // Not found
}
```

**How it works:**
```
Looking for 7 in [1, 3, 5, 7, 9, 11, 13]

Step 1: mid = 7, target = 7 → Found!

Looking for 9:
Step 1: mid = 7, 9 > 7 → search [9, 11, 13]
Step 2: mid = 11, 9 < 11 → search [9]
Step 3: mid = 9, 9 = 9 → Found!
```

**Complexity:** O(log n)
**Use when:** Data is sorted, frequent searches

### Comparison

| Searching 1 million items |
|---------------------------|
| Linear: Up to 1,000,000 checks |
| Binary: Up to 20 checks |

## Sorting Algorithms

### Why Sorting Matters

Many operations are faster on sorted data:
- Binary search requires sorted data
- Finding min/max is instant
- Detecting duplicates is easier

### Bubble Sort (Simple but Slow)

Repeatedly swap adjacent elements if in wrong order.

```cpp
void BubbleSort(int[] array) {
    for (int i = 0; i < array.length; i++) {
        for (int j = 0; j < array.length - 1; j++) {
            if (array[j] > array[j + 1]) {
                // Swap
                int temp = array[j];
                array[j] = array[j + 1];
                array[j + 1] = temp;
            }
        }
    }
}
```

**Complexity:** O(n²)
**Use when:** Learning, tiny datasets, nearly sorted data

### Selection Sort (Simple, Predictable)

Find minimum, put it first, repeat.

```cpp
void SelectionSort(int[] array) {
    for (int i = 0; i < array.length; i++) {
        int minIndex = i;
        for (int j = i + 1; j < array.length; j++) {
            if (array[j] < array[minIndex]) {
                minIndex = j;
            }
        }
        // Swap minimum with current position
        int temp = array[i];
        array[i] = array[minIndex];
        array[minIndex] = temp;
    }
}
```

**Complexity:** O(n²)
**Use when:** Small datasets, minimizing swaps matters

### Quick Sort (Fast, Widely Used)

Pick a "pivot," partition around it, recursively sort partitions.

```cpp
void QuickSort(int[] array, int low, int high) {
    if (low < high) {
        int pivotIndex = Partition(array, low, high);
        QuickSort(array, low, pivotIndex - 1);
        QuickSort(array, pivotIndex + 1, high);
    }
}

int Partition(int[] array, int low, int high) {
    int pivot = array[high];
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (array[j] < pivot) {
            i++;
            Swap(array, i, j);
        }
    }
    Swap(array, i + 1, high);
    return i + 1;
}
```

**Complexity:** O(n log n) average, O(n²) worst
**Use when:** General purpose, large datasets

### Merge Sort (Stable, Predictable)

Divide in half, sort each half, merge.

```cpp
void MergeSort(int[] array, int left, int right) {
    if (left < right) {
        int mid = (left + right) / 2;
        MergeSort(array, left, mid);
        MergeSort(array, mid + 1, right);
        Merge(array, left, mid, right);
    }
}
```

**Complexity:** O(n log n) always
**Use when:** Stability matters, guaranteed performance needed

### Sorting Summary

| Algorithm | Best | Average | Worst | Stable? |
|-----------|------|---------|-------|---------|
| Bubble | O(n) | O(n²) | O(n²) | Yes |
| Selection | O(n²) | O(n²) | O(n²) | No |
| Quick | O(n log n) | O(n log n) | O(n²) | No |
| Merge | O(n log n) | O(n log n) | O(n log n) | Yes |

**Stable** means equal elements keep their original order.

### In Practice

Use your language's built-in sort - it's optimized:

```cpp
// Unreal Engine
TArray<int32> Numbers;
Numbers.Sort();  // Uses IntroSort (hybrid of QuickSort)

// With custom comparison
Enemies.Sort([](const AEnemy& A, const AEnemy& B) {
    return A.Health < B.Health;
});
```

## Pathfinding Algorithms

Essential for game AI - finding routes from A to B.

### The Problem

```
S = Start, E = End, # = Wall

S . . . .
. # # # .
. # E . .
. . . . .
```

Find the shortest path from S to E.

### Breadth-First Search (BFS)

Explore all neighbors, then neighbors' neighbors.

```cpp
Path BFS(Grid grid, Point start, Point end) {
    Queue<Point> queue;
    Map<Point, Point> cameFrom;

    queue.Enqueue(start);
    cameFrom[start] = null;

    while (!queue.IsEmpty()) {
        Point current = queue.Dequeue();

        if (current == end) {
            return ReconstructPath(cameFrom, end);
        }

        for (Point neighbor : grid.GetNeighbors(current)) {
            if (!cameFrom.ContainsKey(neighbor)) {
                queue.Enqueue(neighbor);
                cameFrom[neighbor] = current;
            }
        }
    }
    return null;  // No path found
}
```

**Complexity:** O(V + E) where V = nodes, E = connections
**Finds:** Shortest path (in terms of steps)
**Use when:** Unweighted graphs, need shortest path

### Dijkstra's Algorithm

Like BFS but handles different movement costs.

```
. = 1 cost
~ = 3 cost (water)
# = impassable

S . ~ ~ E
. # # ~ .
. # . . .
```

Finds path with lowest total cost, not fewest steps.

**Complexity:** O((V + E) log V) with priority queue
**Use when:** Different terrain costs, weighted graphs

### A* (A-Star)

Dijkstra's + heuristic to guide search toward goal.

```cpp
Path AStar(Grid grid, Point start, Point end) {
    PriorityQueue<Point> openSet;
    Map<Point, int> gScore;  // Cost from start
    Map<Point, int> fScore;  // gScore + heuristic

    openSet.Enqueue(start, 0);
    gScore[start] = 0;
    fScore[start] = Heuristic(start, end);

    while (!openSet.IsEmpty()) {
        Point current = openSet.Dequeue();

        if (current == end) {
            return ReconstructPath(cameFrom, end);
        }

        for (Point neighbor : grid.GetNeighbors(current)) {
            int tentativeG = gScore[current] + MoveCost(current, neighbor);

            if (tentativeG < gScore.GetOrDefault(neighbor, INFINITY)) {
                cameFrom[neighbor] = current;
                gScore[neighbor] = tentativeG;
                fScore[neighbor] = tentativeG + Heuristic(neighbor, end);
                openSet.Enqueue(neighbor, fScore[neighbor]);
            }
        }
    }
    return null;
}
```

**The Heuristic:**
- Estimate of remaining distance
- Common: Manhattan distance (|x1-x2| + |y1-y2|)
- Must never overestimate for optimal path

**Complexity:** Depends on heuristic, often much faster than Dijkstra
**Use when:** Games! Most common pathfinding algorithm

### Pathfinding Summary

| Algorithm | Weighted? | Optimal? | Speed |
|-----------|-----------|----------|-------|
| BFS | No | Yes (unweighted) | Medium |
| Dijkstra | Yes | Yes | Slow |
| A* | Yes | Yes* | Fast |

*With admissible heuristic

## Game Algorithm Examples

### Collision Detection

**Brute Force (O(n²)):**
```cpp
for (obj1 in objects) {
    for (obj2 in objects) {
        if (Collides(obj1, obj2)) {
            HandleCollision(obj1, obj2);
        }
    }
}
```

**With Spatial Partitioning (Much Faster):**
```cpp
// Only check objects in same grid cell
for (cell in grid) {
    for (obj1 in cell.objects) {
        for (obj2 in cell.objects) {
            if (Collides(obj1, obj2)) {
                HandleCollision(obj1, obj2);
            }
        }
    }
}
```

### Finding Nearest Enemy

**Naive (O(n)):**
```cpp
Enemy nearest = null;
float nearestDist = INFINITY;

for (enemy in enemies) {
    float dist = Distance(player, enemy);
    if (dist < nearestDist) {
        nearestDist = dist;
        nearest = enemy;
    }
}
```

**With Spatial Structure (O(log n) average):**
```cpp
// K-D Tree or Quadtree
Enemy nearest = spatialTree.FindNearest(player.position);
```

### Random Selection with Weights

```cpp
// Loot table: sword (50%), shield (30%), potion (20%)
Item GetRandomLoot() {
    int roll = Random(0, 100);

    if (roll < 50) return SWORD;       // 0-49
    if (roll < 80) return SHIELD;      // 50-79
    return POTION;                      // 80-99
}
```

## Exercises

### Exercise 1: Complexity Analysis

What's the Big O complexity?

```cpp
void Mystery(int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            print(i, j);
        }
    }
}
```

### Exercise 2: Choose the Algorithm

What algorithm would you use for:
1. Finding if a player name exists in a sorted leaderboard
2. Sorting enemies by distance from player
3. Finding a path for AI to chase the player

### Exercise 3: Optimize This

```cpp
// This runs every frame. How can you make it faster?
Enemy FindWeakestEnemy() {
    SortByHealth(enemies);  // Sorts all enemies
    return enemies[0];      // Return first (lowest health)
}
```

### Exercise 4: Implement Binary Search

Write binary search for a sorted array of high scores to find where a new score should be inserted.

## Key Takeaways

1. **Big O** tells you how algorithms scale
2. **Binary search** is O(log n) - use for sorted data
3. **Good sorting** is O(n log n) - use built-in sorts
4. **A*** is the go-to for game pathfinding
5. **Reduce complexity** from O(n²) when possible
6. **Use appropriate data structures** to enable faster algorithms

## What's Next

You now have a foundation in computer science! Let's move on to understanding how Unreal Engine works specifically.

**[Continue to Engine Overview](../unreal/01-engine-overview.md)**

---

## Quick Reference

| Task | Algorithm | Complexity |
|------|-----------|------------|
| Search unsorted | Linear Search | O(n) |
| Search sorted | Binary Search | O(log n) |
| Sort | QuickSort/MergeSort | O(n log n) |
| Find path | A* | Varies |
| Find nearest | Spatial tree | O(log n) |
