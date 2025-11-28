# Implementation Plan - SimpleRunner

We will build a simple "Endless Runner" style game where platforms are generated procedurally. The difficulty will increase by making platforms smaller and the gap between them larger as the player progresses.

## Goal Description
Create a C++ based game in Unreal Engine 5.
-   **Player**: A simple character that can move left/right and jump. Forward movement will be automatic.
-   **Level**: Platforms spawned dynamically ahead of the player.
-   **Progression**: Difficulty increases over time/distance.

## User Review Required
> [!IMPORTANT]
> You must have Unreal Engine 5 installed and a C++ project named `SimpleRunner` created before we can proceed with the code.

## Proposed Changes

We will create the following C++ classes in the `Source/SimpleRunner` directory.

### Core Gameplay

#### [NEW] [SimpleRunnerCharacter](file:///home/rcgalbo/unreallll/SimpleRunner/Source/SimpleRunner/SimpleRunnerCharacter.h)
-   **Base Class**: `ACharacter`
-   **Purpose**: The player character.
-   **Functionality**:
    -   Automatic forward movement.
    -   Input handling for Left/Right movement and Jump.
    -   Camera setup (Third Person view).

#### [NEW] [PlatformActor](file:///home/rcgalbo/unreallll/SimpleRunner/Source/SimpleRunner/PlatformActor.h)
-   **Base Class**: `AActor`
-   **Purpose**: The ground the player runs on.
-   **Functionality**:
    -   A simple Static Mesh component (Cube).
    -   Logic to destroy itself when it falls behind the player (optimization).

#### [NEW] [LevelGenerator](file:///home/rcgalbo/unreallll/SimpleRunner/Source/SimpleRunner/LevelGenerator.h)
-   **Base Class**: `AActor`
-   **Purpose**: Spawns platforms.
-   **Functionality**:
    -   Spawns initial set of platforms.
    -   Spawns new platforms as the player moves.
    -   **Difficulty Logic**: Adjusts platform size and gap based on total platforms spawned or distance traveled.

#### [NEW] [SimpleRunnerGameMode](file:///home/rcgalbo/unreallll/SimpleRunner/Source/SimpleRunner/SimpleRunnerGameMode.h)
-   **Base Class**: `AGameModeBase`
-   **Purpose**: Sets default classes.
-   **Functionality**:
    -   Sets `DefaultPawnClass` to `ASimpleRunnerCharacter`.

## Verification Plan

### Automated Tests
-   We will rely on manual verification in the Unreal Editor as automated testing for gameplay requires complex setup.

### Manual Verification
1.  **Compile**: Run the build task in VS Code or compile from the Editor.
2.  **Play**: Press "Play" in the editor.
3.  **Verify**:
    -   Character moves forward automatically.
    -   Character can jump and move sideways.
    -   Platforms appear ahead.
    -   Platforms disappear behind.
    -   Gaps get wider after running for a while.
