## Unreal Engine Reflection System

Unreal Engine Reflection System is a powerful feature that enables the engine to introspect and manipulate objects and their properties at runtime. This system is essential for various engine functionalities, including serialization, garbage collection, and interaction with Blueprints and the editor

**Metadata and Introspection**:
Reflection allows the engine to retrieve information about classes, properties, and functions at runtime. This metadata includes names, types, and attributes of properties and methods.

**UObject and UCLASS**:
The base class for most Unreal Engine classes is `UObject`. Classes derived from `UObject` can utilize the reflection system.
The `UCLASS` macro is used to mark a class as part of the reflection system, enabling it to be used with Unreal’s introspection features.

**UPROPERTY and UFUNCTION**:
`UPROPERTY` and `UFUNCTION` macros expose class members and functions to the reflection system.
- `UPROPERTY` is used for properties that should be exposed to the editor, saved, or replicated over the network.
- `UFUNCTION` is used for functions that can be called from Blueprints, networked, or need other special handling.

### Benefits of the Reflection System

- **Editor Integration**:
Properties marked with `UPROPERTY` can be edited in the Unreal Editor. This makes it easy to tweak values and configure assets without modifying code.
**Example**:
  ```cpp
  UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Configuration")
  float MovementSpeed;
  ```
  [*What are argumets inside UPROPERTY?*](./specifiers.md)
- **Serialization**:
The reflection system handles the serialization and deserialization of objects. This is crucial for saving and loading game states, as well as for networking.
**Example**:
  ```cpp
  UPROPERTY(SaveGame)
  intPlayerScore;
  ```

- **Garbage Collection**:
Unreal Engine uses a garbage collector to manage memory for `UObject`-based classes. The reflection system helps track object references to ensure proper cleanup.
**Example:**
  ```cpp
  UPROPERTY()
  UObject* MyObjectReference;
  ```
  [*What is UObject\* (with asterisk)?*](./pointers.md)

- **Blueprint Integration**:
Functions marked with `UFUNCTION` can be called from Blueprints, enabling designers and artists to script gameplay behavior without writing C++ code.
**Example:**
  ```cpp
  UFUNCTION(BlueprintCallable, Category = "Gameplay")
  void Jump();
  ```

- **Network Replication**:
  Properties and functions can be replicated across the network using the reflection system. This is essential for multiplayer games.
  **Example:**
    ```cpp
    UPROPERTY(Replicated)
    bool bIsPlayerAlive;

    UFUNCTION(Server, Reliable, WithValidation)
    void Server_SetPlayerAlive(bool bAlive);
    ```

### Example of Reflection in Action

Here’s a simple example demonstrating how to use `UCLASS`, `UPROPERTY`, and `UFUNCTION`:

**MyActor.h**:
```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "MyActor.generated.h"

UCLASS()
class MYPROJECT_API AMyActor : public AActor
{
    GENERATED_BODY()

public:
    AMyActor();

protected:
    virtual void BeginPlay() override;

public:
    virtual void Tick(float DeltaTime) override;

    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Configuration")
    float MovementSpeed;

    UFUNCTION(BlueprintCallable, Category = "Movement")
    void MoveForward(float Value);
};
```

**MyActor.cpp**:
```cpp
#include "MyActor.h"

AMyActor::AMyActor()
{
    PrimaryActorTick.bCanEverTick = true;
    MovementSpeed = f;
}

void AMyActor::BeginPlay()
{
    Super::BeginPlay();
}

void AMyActor::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);
}

void AMyActor::MoveForward(float Value)
{
    FVector NewLocation = GetActorLocation() + FVector(Value * MovementSpeed * GetWorld()->DeltaTimeSeconds,  ;
    SetActorLocation(NewLocation);
}
```

### How It Works

- **`UCLASS()`**: Marks `AMyActor` as a class that can be introspected by the engine.
- **`UPROPERTY(EditAnywhere, BlueprintReadWrite)`**: Exposes `MovementSpeed` to the editor and allows it to be modified in Blueprints.
- **`UFUNCTION(BlueprintCallable)`**: Makes `MoveForward` callable from Blueprints.

### Practical Applications

 **Customizable Gameplay**: Designers can modify `MovementSpeed` directly in the editor or through Blueprints without changing code.
 **Runtime Adjustments**: Properties and functions can be adjusted at runtime using Blueprints or through the editor, facilitating rapid iteration and testing.
 **Data-Driven Development**: The reflection system supports a data-driven approach, where much of the game behavior can be defined in data (e.g., Blueprints, config files) rather than hard-coded.
