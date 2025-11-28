# Blueprints vs C++

Unreal Engine offers two ways to program: Blueprints (visual scripting) and C++. This guide helps you understand both and when to use each.

## What You'll Learn

- What Blueprints are and how they work
- What C++ offers in Unreal
- When to use which
- How they work together

## What Are Blueprints?

**Blueprints** are a visual scripting system. Instead of writing text code, you connect nodes with wires.

### How Blueprints Look

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│ Event BeginPlay│────▶│ Print String │────▶│ Set Health  │
└─────────────┘     │ "Game Start" │     │ Value: 100  │
                    └─────────────┘     └─────────────┘
```

The same logic in C++:
```cpp
void AMyActor::BeginPlay()
{
    Super::BeginPlay();
    UE_LOG(LogTemp, Warning, TEXT("Game Start"));
    Health = 100;
}
```

### Blueprint Advantages

**1. Visual and Intuitive**
- See the flow of logic
- No syntax errors
- Easier for beginners

**2. Fast Iteration**
- Changes apply immediately
- No compilation wait
- Quick prototyping

**3. Designer-Friendly**
- Artists and designers can use them
- No programming background needed
- Direct manipulation of game objects

**4. Built-in Safety**
- Can't crash as easily
- Type-checked connections
- No memory management

### Blueprint Disadvantages

**1. Performance**
- 10x slower than C++ for complex logic
- Not suitable for heavy computation
- Can impact frame rate

**2. Complexity Limits**
- Large Blueprints become unreadable
- Hard to version control (binary files)
- Limited algorithm implementation

**3. Features**
- Some engine features require C++
- Less control over low-level systems
- Can't create new engine types

## What Is C++ in Unreal?

Unreal uses C++ with special macros and conventions.

### How Unreal C++ Looks

```cpp
// Header file (.h)
UCLASS()
class MYGAME_API AMyCharacter : public ACharacter
{
    GENERATED_BODY()

public:
    UPROPERTY(EditAnywhere, BlueprintReadWrite)
    float Health = 100.0f;

    UFUNCTION(BlueprintCallable)
    void TakeDamage(float Amount);
};

// Source file (.cpp)
void AMyCharacter::TakeDamage(float Amount)
{
    Health -= Amount;
    if (Health <= 0)
    {
        Die();
    }
}
```

### C++ Advantages

**1. Performance**
- Full native speed
- Essential for complex systems
- Better for CPU-intensive tasks

**2. Full Engine Access**
- Access to all engine features
- Create new asset types
- Modify engine behavior

**3. Code Management**
- Text files work with version control
- Easier to review and diff
- Better organization for large projects

**4. Industry Standard**
- Professional game development uses C++
- Transferable skills
- More job opportunities

### C++ Disadvantages

**1. Complexity**
- Steeper learning curve
- Memory management
- More ways to create bugs

**2. Iteration Speed**
- Must compile changes
- Longer feedback loop
- Can't change during play

**3. Setup**
- Need Visual Studio
- More configuration
- Larger project files

## When to Use Which

### Use Blueprints For:

- **Prototyping** - Test ideas quickly
- **Simple logic** - Basic game rules
- **UI** - Menu systems, HUD
- **Level scripting** - Triggers, doors, events
- **Animation** - State machines, blending
- **Designer work** - Tuning values, behaviors

### Use C++ For:

- **Core systems** - Inventory, combat, AI
- **Performance-critical code** - Pathfinding, physics queries
- **Base classes** - Character, enemy base classes
- **Complex algorithms** - Procedural generation
- **Engine extensions** - New editor tools
- **Networking** - Multiplayer logic

### The Best Approach: Both

Professional Unreal development uses both:

```
C++ Base Class
├── Core functionality
├── Performance-critical code
└── Properties exposed to Blueprints
         │
         ▼
Blueprint Child Class
├── Design tweaks
├── Visual effects
├── Sound triggers
└── Simple game logic
```

**Example: Enemy Character**

C++ (base class):
```cpp
UCLASS()
class AEnemyBase : public ACharacter
{
    UPROPERTY(EditDefaultsOnly)
    float MaxHealth = 100.0f;

    UPROPERTY(BlueprintReadOnly)
    float CurrentHealth;

    UFUNCTION(BlueprintCallable)
    void TakeDamage(float Amount);

    UFUNCTION(BlueprintImplementableEvent)
    void OnDeath();  // Blueprint fills this in
};
```

Blueprint (child class):
- Set MaxHealth to 50 for weak enemies
- Set MaxHealth to 200 for bosses
- Implement OnDeath to play death animation
- Add particle effects

## Blueprint-C++ Integration

### Exposing C++ to Blueprints

**Properties:**
```cpp
UPROPERTY(EditAnywhere, BlueprintReadWrite)
float Health;  // Editable in editor and Blueprint
```

**Functions:**
```cpp
UFUNCTION(BlueprintCallable)
void Attack();  // Can be called from Blueprint

UFUNCTION(BlueprintPure)
bool IsAlive();  // Pure function (no side effects)
```

**Events:**
```cpp
UFUNCTION(BlueprintImplementableEvent)
void OnPickup();  // Blueprint provides implementation

UFUNCTION(BlueprintNativeEvent)
void OnHit();  // C++ default, Blueprint can override
```

### Common Specifiers

**UPROPERTY:**
| Specifier | Meaning |
|-----------|---------|
| EditAnywhere | Editable in editor |
| BlueprintReadOnly | Blueprint can read |
| BlueprintReadWrite | Blueprint can read/write |
| VisibleAnywhere | Visible but not editable |

**UFUNCTION:**
| Specifier | Meaning |
|-----------|---------|
| BlueprintCallable | Can be called from Blueprint |
| BlueprintPure | No side effects, can be used in expressions |
| BlueprintImplementableEvent | Blueprint must implement |
| BlueprintNativeEvent | C++ default, Blueprint can override |

## Learning Path

### Recommended Order:

1. **Start with Blueprints**
   - Learn Unreal's concepts
   - Understand game flow
   - Build simple projects

2. **Learn C++ Basics**
   - Variables, functions, classes
   - Pointers and references
   - Object-oriented programming

3. **Learn Unreal C++**
   - UCLASS, UPROPERTY, UFUNCTION
   - Actor lifecycle
   - Component system

4. **Combine Both**
   - C++ base classes
   - Blueprint child classes
   - Use each for its strengths

## Practical Example

Let's implement a simple pickup in both:

### Blueprint Version

```
Event ActorBeginOverlap
    │
    ▼
Cast to PlayerCharacter
    │
    ▼ (Success)
Add Health (50)
    │
    ▼
Play Sound
    │
    ▼
Destroy Actor
```

### C++ Version

```cpp
// Header
UCLASS()
class AHealthPickup : public AActor
{
    GENERATED_BODY()

public:
    UPROPERTY(EditAnywhere)
    float HealAmount = 50.0f;

    UPROPERTY(EditAnywhere)
    USoundBase* PickupSound;

private:
    UFUNCTION()
    void OnOverlap(AActor* Other);
};

// Source
void AHealthPickup::OnOverlap(AActor* Other)
{
    APlayerCharacter* Player = Cast<APlayerCharacter>(Other);
    if (Player)
    {
        Player->AddHealth(HealAmount);
        UGameplayStatics::PlaySoundAtLocation(this, PickupSound, GetActorLocation());
        Destroy();
    }
}
```

### Hybrid Version

C++ base:
```cpp
UCLASS()
class APickupBase : public AActor
{
    GENERATED_BODY()

protected:
    UFUNCTION(BlueprintImplementableEvent)
    void OnPickedUp(APlayerCharacter* Player);

private:
    void OnOverlap(AActor* Other);
};

void APickupBase::OnOverlap(AActor* Other)
{
    APlayerCharacter* Player = Cast<APlayerCharacter>(Other);
    if (Player)
    {
        OnPickedUp(Player);  // Blueprint handles specifics
        Destroy();
    }
}
```

Blueprint child handles:
- What effect to apply (health, ammo, etc.)
- Visual/audio feedback
- Specific amounts

## Exercises

### Exercise 1: Blueprint Practice

Create a Blueprint that:
1. Prints "Hello" when the game starts
2. Prints "Goodbye" when the game ends

### Exercise 2: Analyze Trade-offs

For each feature, would you use Blueprint, C++, or both?
1. Main menu UI
2. Pathfinding algorithm
3. Door that opens when player approaches
4. Inventory system
5. Particle effect on pickup

### Exercise 3: Read C++ Code

Look at this code and describe what it does:
```cpp
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Stats")
float MaxHealth = 100.0f;
```

## Key Takeaways

1. **Blueprints** are visual, fast to iterate, but slower performance
2. **C++** is powerful, performant, but has steeper learning curve
3. **Best practice**: Use both - C++ for systems, Blueprints for content
4. **Expose C++ to Blueprints** using UPROPERTY and UFUNCTION
5. **Start with Blueprints**, learn C++ as you advance

## What's Next

Now let's dive deeper into how Actors and Components work - the building blocks of everything in Unreal.

**[Continue to Actors and Components](03-actors-and-components.md)**

---

## Quick Reference

| Need | Use |
|------|-----|
| Prototype quickly | Blueprint |
| Performance-critical | C++ |
| Designer tuning | Blueprint |
| Core systems | C++ |
| Level scripting | Blueprint |
| Complex algorithms | C++ |
| Simple logic | Blueprint |
| Engine extensions | C++ |
