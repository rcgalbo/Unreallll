# Actors and Components

Actors and Components are the building blocks of everything in Unreal Engine. Understanding them is essential for building games.

## What You'll Learn

- What Actors are and how they work
- What Components are and the types available
- The Actor lifecycle
- How to design with composition

## What Is an Actor?

An **Actor** is any object that can be placed in a level. Everything you see (and many things you don't) are Actors:

- Characters
- Lights
- Cameras
- Sounds
- Triggers
- Particle effects
- The floor, walls, everything

### Actor Basics

Every Actor has:

**Transform**: Position, rotation, and scale in the world
```
Position: X=100, Y=200, Z=50
Rotation: Pitch=0, Yaw=45, Roll=0
Scale: X=1, Y=1, Z=1
```

**Unique ID**: Every Actor has a unique identifier

**Lifespan**: Actors are spawned and destroyed

**Components**: Actors contain components that provide functionality

### Actor Without Components

An Actor with no components does almost nothing:
- Has a transform (exists somewhere in space)
- Can receive events
- Can be spawned/destroyed

Components give Actors their actual capabilities.

## What Is a Component?

A **Component** is a piece of functionality that attaches to an Actor. Think of them as LEGO pieces:

```
Character Actor
├── Capsule Component      (collision shape)
├── Skeletal Mesh Component (the 3D model)
├── Camera Component       (what player sees)
├── Movement Component     (handles walking/jumping)
└── Audio Component        (plays sounds)
```

### Component Philosophy

Unreal uses **composition over inheritance**:

**Inheritance approach (limited):**
```
Character
└── FlyingCharacter
    └── FlyingShootingCharacter
        └── FlyingShootingHealingCharacter  // Gets messy!
```

**Composition approach (flexible):**
```
Character
├── Add FlyingComponent    → Can fly
├── Add ShootingComponent  → Can shoot
└── Add HealingComponent   → Can heal
```

Mix and match components to create any combination.

## Common Components

### Scene Component

The base for all components with transforms. Provides:
- Position, rotation, scale
- Attachment to other components
- Hierarchical transforms

```
Root (Scene Component)
├── Mesh
├── Camera
│   └── Light (attached to camera)
└── Trigger
```

### Primitive Components

Components that can be rendered or have physics:

**Static Mesh Component**
- Displays a 3D model
- Used for objects that don't animate

**Skeletal Mesh Component**
- Displays animated 3D models
- Used for characters, creatures

**Collision Components**
- Capsule, Box, Sphere
- Define collision shapes
- Can be visible or invisible

### Movement Components

Handle how Actors move:

**Character Movement Component**
- Walking, running, jumping
- Swimming, flying
- Crouching
- Built-in networking

**Projectile Movement Component**
- For bullets, rockets
- Handles trajectory
- Gravity, homing

**Floating Pawn Movement**
- Simple floating movement
- Good for flying or swimming

### Utility Components

**Camera Component**
- Defines view perspective
- Can attach to any Actor

**Spring Arm Component**
- Camera boom
- Handles camera collision
- Smooth following

**Audio Component**
- Plays sounds
- 3D spatialization
- Looping, volume control

**Particle System Component**
- Visual effects
- Fire, smoke, magic

**Light Components**
- Point, spot, directional lights
- Attach lights to Actors

## Actor Lifecycle

Actors go through specific phases:

```
Constructor
    │
    ▼
PostInitializeComponents
    │
    ▼
BeginPlay    ← Game starts, Actor is ready
    │
    ▼
Tick         ← Called every frame (if enabled)
    │
    ▼
EndPlay      ← Actor is being removed
    │
    ▼
Destroyed
```

### Key Events

**Constructor**
- Called when Actor is created
- Set up components here
- Don't access other Actors yet

**BeginPlay**
- Called when game starts
- All Actors exist
- Safe to reference other Actors

**Tick**
- Called every frame
- Main game logic goes here
- Can be disabled for performance

**EndPlay**
- Called when Actor is removed
- Clean up references
- Called before Destroy

### In Blueprints

```
Event BeginPlay → Your startup logic
Event Tick → Your per-frame logic
Event EndPlay → Your cleanup logic
```

### In C++

```cpp
void AMyActor::BeginPlay()
{
    Super::BeginPlay();  // Always call parent!
    // Your code here
}

void AMyActor::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);
    // Your per-frame code here
}
```

## Creating Actors

### In the Editor

1. Content Browser → Right-click → Blueprint Class
2. Choose parent class (Actor, Character, etc.)
3. Open Blueprint
4. Add components in Components panel
5. Add logic in Event Graph

### In C++

```cpp
// Header (.h)
UCLASS()
class AMyActor : public AActor
{
    GENERATED_BODY()

public:
    AMyActor();

    UPROPERTY(VisibleAnywhere)
    UStaticMeshComponent* Mesh;

    UPROPERTY(VisibleAnywhere)
    USphereComponent* CollisionSphere;
};

// Source (.cpp)
AMyActor::AMyActor()
{
    // Create components in constructor
    CollisionSphere = CreateDefaultSubobject<USphereComponent>(TEXT("CollisionSphere"));
    RootComponent = CollisionSphere;

    Mesh = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("Mesh"));
    Mesh->SetupAttachment(RootComponent);
}
```

## Spawning Actors

### In Blueprints

Use the "Spawn Actor from Class" node:
- Select class to spawn
- Set spawn transform
- Get reference to new Actor

### In C++

```cpp
// Simple spawn
AMyActor* NewActor = GetWorld()->SpawnActor<AMyActor>(
    AMyActor::StaticClass(),
    SpawnLocation,
    SpawnRotation
);

// With parameters
FActorSpawnParameters SpawnParams;
SpawnParams.Owner = this;
AMyActor* NewActor = GetWorld()->SpawnActor<AMyActor>(
    AMyActor::StaticClass(),
    SpawnLocation,
    SpawnRotation,
    SpawnParams
);
```

## Component Attachment

Components form hierarchies:

```cpp
// Root component
RootComponent = CreateDefaultSubobject<USceneComponent>(TEXT("Root"));

// Mesh attached to root
Mesh = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("Mesh"));
Mesh->SetupAttachment(RootComponent);

// Light attached to mesh
Light = CreateDefaultSubobject<UPointLightComponent>(TEXT("Light"));
Light->SetupAttachment(Mesh);
```

When parent moves, children move with it.

### Attachment Rules

```cpp
// Keep world position
Component->AttachToComponent(Parent, FAttachmentTransformRules::KeepWorldTransform);

// Snap to parent's position
Component->AttachToComponent(Parent, FAttachmentTransformRules::SnapToTargetNotIncludingScale);
```

## Finding Actors and Components

### Get Components

```cpp
// Get single component of type
UStaticMeshComponent* Mesh = GetComponentByClass<UStaticMeshComponent>();

// Get all components of type
TArray<UAudioComponent*> AudioComponents;
GetComponents<UAudioComponent>(AudioComponents);
```

### Find Actors

```cpp
// Find by class
TArray<AActor*> FoundActors;
UGameplayStatics::GetAllActorsOfClass(GetWorld(), AEnemy::StaticClass(), FoundActors);

// Find by tag
TArray<AActor*> TaggedActors;
UGameplayStatics::GetAllActorsWithTag(GetWorld(), FName("Pickup"), TaggedActors);
```

## Design Patterns

### Pattern 1: Component for Reusable Behavior

Create a component for behavior you want on multiple Actors:

```cpp
// HealthComponent - add to anything that has health
UCLASS()
class UHealthComponent : public UActorComponent
{
    UPROPERTY(EditAnywhere)
    float MaxHealth = 100.0f;

    float CurrentHealth;

    void TakeDamage(float Amount);
    void Heal(float Amount);
};
```

Now any Actor can have health by adding this component.

### Pattern 2: Interface for Common Actions

Define an interface that multiple Actor types implement:

```cpp
// Interface
UINTERFACE()
class UDamageable : public UInterface { };

class IDamageable
{
public:
    virtual void TakeDamage(float Amount) = 0;
};

// Implementation
class AEnemy : public ACharacter, public IDamageable
{
    virtual void TakeDamage(float Amount) override;
};

class ADestructibleWall : public AActor, public IDamageable
{
    virtual void TakeDamage(float Amount) override;
};
```

Now you can damage enemies and walls with the same code.

### Pattern 3: Child Actor Component

Spawn another Actor as a component:

```cpp
UPROPERTY(VisibleAnywhere)
UChildActorComponent* WeaponActor;

// In constructor
WeaponActor = CreateDefaultSubobject<UChildActorComponent>(TEXT("Weapon"));
WeaponActor->SetChildActorClass(AWeapon::StaticClass());
```

Useful when the "component" needs to be a full Actor.

## Exercises

### Exercise 1: Build an Actor

Create an Actor (Blueprint) with:
- A static mesh (cube)
- A point light
- A sphere collision trigger

### Exercise 2: Component Hierarchy

Set up this hierarchy:
```
Root
├── Body (mesh)
│   └── Head (mesh)
│       └── Hat (mesh)
└── Weapon (mesh)
```

Move the Body and observe what moves with it.

### Exercise 3: Lifecycle Events

Create a Blueprint that:
1. Prints "Created!" in BeginPlay
2. Prints "Frame!" every tick (then disable tick)
3. Prints "Destroyed!" in EndPlay

Spawn and destroy it to see the events.

## Key Takeaways

1. **Actors** are objects in the world
2. **Components** give Actors functionality
3. **Composition** is preferred over inheritance
4. **Lifecycle**: Constructor → BeginPlay → Tick → EndPlay
5. **Components can attach** to form hierarchies
6. **Reusable components** let you share behavior

## What's Next

Now let's understand how the game loop works - how Unreal processes each frame.

**[Continue to Game Loop](04-game-loop.md)**

---

## Quick Reference

| Component Type | Purpose |
|---------------|---------|
| Static Mesh | Display 3D model |
| Skeletal Mesh | Animated model |
| Capsule/Box/Sphere | Collision |
| Character Movement | Walking, jumping |
| Camera | Player view |
| Spring Arm | Camera boom |
| Audio | Play sounds |
| Point/Spot Light | Lighting |

| Lifecycle Event | When |
|-----------------|------|
| Constructor | Actor created |
| BeginPlay | Game starts |
| Tick | Every frame |
| EndPlay | Actor removed |
