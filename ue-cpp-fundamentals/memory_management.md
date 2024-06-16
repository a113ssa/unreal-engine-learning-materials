## Memory Management

Memory management is a critical aspect of C++ development, especially in game development with Unreal Engine. Here's a comprehensive guide on how to manage memory effectively in Unreal Engine C++:

### Basics of Memory Management in C++

#### Manual Memory Management
- **Allocation**: Using `new` to allocate memory.
- **Deallocation**: Using `delete` to free memory.
- **Arrays**: Use `new[]` and `delete[]` for dynamic arrays.
- **Dangling Pointers**: Pointers that reference freed memory.
- **Memory Leaks**: Allocated memory that is not freed.

```cpp
int* p = new int;    // Allocate memory
*p = 10;            // Use the memory
delete p;           // Free the memory
p = nullptr;        // Avoid dangling pointer

int* arr = new int[10];  // Allocate array
delete[] arr;            // Free array
arr = nullptr;           // Avoid dangling pointer
```

Manual memory management techniques, such as using `new` and `delete` directly, **should generally be avoided in Unreal Engine code**. Unreal Engine provides a range of higher-level memory management tools and practices that are more suited to game development within its framework. These tools help ensure safer, more efficient, and more manageable code

### Memory Management in Unreal Engine

#### Smart Pointers

Unreal Engine provides its own smart pointers to manage memory automatically. They help avoid common issues like memory leaks and dangling pointers.

- **TSharedPtr**: Reference-counted smart pointer. When the last `TSharedPtr` owning an object is destroyed, the object is deleted.

  ```cpp
  TSharedPtr<MyClass> Ptr = MakeShared<MyClass>();
  ```

- **TWeakPtr**: Non-owning reference to an object managed by `TSharedPtr`. Used to avoid circular references.

  ```cpp
  TSharedPtr<MyClass> SharedPtr = MakeShared<MyClass>();
  TWeakPtr<MyClass> WeakPtr = SharedPtr;
  ```

- **TUniquePtr**: Unique ownership of an object. When the `TUniquePtr` is destroyed, the object is deleted.

  ```cpp
  TUniquePtr<MyClass> UniquePtr = MakeUnique<MyClass>();
  ```

#### UObjects and Garbage Collection

Unreal Engine uses a garbage collector to manage the memory of `UObject`-derived classes.

- **UCLASS, UPROPERTY, UFUNCTION**: Macros to declare classes, properties, and functions that participate in Unreal's reflection system and garbage collection.

  ```cpp
  UCLASS()
  class MYGAME_API AMyActor : public AActor
  {
      GENERATED_BODY()

  public:
      UPROPERTY()
      UObject* MyObject;
  };
  ```

- **Garbage Collection**: Unreal's garbage collector automatically tracks and cleans up `UObject` instances.

#### Object Lifetimes

- **New Objects**: Use `NewObject` to create `UObject` instances.

  ```cpp
  UMyObject* MyObject = NewObject<UMyObject>(this);
  ```

- **Actors**: Use `SpawnActor` to create actors.

  ```cpp
  AActor* MyActor = GetWorld()->SpawnActor<AMyActor>(AMyActor::StaticClass());
  ```

- **Destruction**: Use `Destroy` to destroy actors.

  ```cpp
  MyActor->Destroy();
  ```

### Tips and Tricks

**Use Smart Pointers**: Prefer `TSharedPtr`, `TWeakPtr`, and `TUniquePtr` over raw pointers.
**Avoid `new` and `delete`**: Use Unreal's memory management functions (`NewObject`, `SpawnActor`, etc.).
**Use `UPROPERTY`**: Mark pointers with `UPROPERTY` to ensure they are tracked by the garbage collector.
**Manage Object Lifetimes**: Understand object lifetimes and ensure objects are created and destroyed properly.
**Avoid Circular References**: Use `TWeakPtr` to break circular references in smart pointers.
**Monitor Memory Usage**: Use Unreal's profiling tools to monitor memory usage and identify leaks.

### Practical Example

Here's a practical example that demonstrates proper memory management practices in Unreal Engine:

**MyActor.h**

```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "MyActor.generated.h"

UCLASS()
class MYGAME_API AMyActor : public AActor
{
    GENERATED_BODY()

public:
    AMyActor();

protected:
    virtual void BeginPlay() override;

public:
    virtual void Tick(float DeltaTime) override;

    UPROPERTY()
    UObject* MyObject;

    TSharedPtr<class MyClass> MySharedPtr;
};
```

**MyActor.cpp**

```cpp
#include "MyActor.h"
#include "Engine/World.h"

AMyActor::AMyActor()
{
    PrimaryActorTick.bCanEverTick = true;

    // Use MakeShared to create a TSharedPtr
    MySharedPtr = MakeShared<MyClass>();
}

void AMyActor::BeginPlay()
{
    Super::BeginPlay();

    // Use NewObject to create a UObject instance
    MyObject = NewObject<UMyObject>(this);
}

void AMyActor::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

    // Safe usage of MySharedPtr
    if (MySharedPtr.IsValid())
    {
        MySharedPtr->DoSomething();
    }
}
```

In this example:
- **Smart Pointers**: `TSharedPtr` is used to manage `MyClass` instances.
- **UObjects**: `UPROPERTY` is used to ensure `MyObject` is tracked by the garbage collector.
- **Memory Management Functions**: `MakeShared` and `NewObject` are used for memory allocation.

### Advanced Practices for Memory Management

#### Object Pools

Object pools are a way to manage objects that are frequently created and destroyed. Instead of allocating and deallocating memory repeatedly, you maintain a pool of reusable objects.

- **Advantages**: Reduces memory fragmentation, improves performance, and lowers overhead.
- **Implementation**: Create a pool manager that keeps a collection of pre-allocated objects.

```cpp
template <typename T>
class ObjectPool {
public:
    T* Acquire() {
        if (!Pool.empty()) {
            T* obj = Pool.back();
            Pool.pop_back();
            return obj;
        }
        return new T();
    }

    void Release(T* obj) {
        Pool.push_back(obj);
    }

private:
    std::vector<T*> Pool;
};
```

**Usage Example**:
```cpp
ObjectPool<MyClass> Pool;
MyClass* Obj = Pool.Acquire();
// Use Obj
Pool.Release(Obj);
```

#### Custom Allocators

Unreal Engine allows you to create custom memory allocators for specific types of objects. This can be useful for managing memory more efficiently.
[*What is memory allocator*](./memory_allocation.md)

**Example**: Creating a custom allocator for a class.

```cpp
class MyClass {
public:
    void* operator new(size_t Size) {
        return FMemory::Malloc(Size);
    }

    void operator delete(void* Ptr) {
        FMemory::Free(Ptr);
    }
};
```

#### Using Memory Pools

Unreal Engine has built-in support for memory pools, which can be used to allocate and deallocate memory blocks efficiently.

```cpp
// Declare a memory pool
FMemStackBase MemoryPool;

// Allocate memory from the pool
void* Memory = MemoryPool.Alloc(1024);

// Use the memory
// ...

// Free the memory (automatically when the pool is reset or destroyed)
MemoryPool.Free(Memory);
```

#### Arena Allocators

Arena allocators allow you to allocate a large block of memory up front and then manage individual allocations within that block.

- **Advantages**: Reduces fragmentation, improves allocation speed.
- **Implementation**: Create an arena allocator that manages a single large block of memory.

```cpp
class ArenaAllocator {
public:
    ArenaAllocator(size_t size) : Size(size), Offset(0) {
        Memory = (char*)FMemory::Malloc(Size);
    }

    ~ArenaAllocator() {
        FMemory::Free(Memory);
    }

    void* Alloc(size_t size) {
        if (Offset + size <= Size) {
            void* ptr = Memory + Offset;
            Offset += size;
            return ptr;
        }
        return nullptr;
    }

    void Reset() {
        Offset = 0;
    }

private:
    size_t Size;
    size_t Offset;
    char* Memory;
};
```


[*The Diffrence between Object Pool, Memory Pool, Custom Allocators, and Arena Allocators*](./memory_management_practices_differences.md)

#### Avoiding Memory Fragmentation

Memory fragmentation can degrade performance over time. Here are some techniques to minimize fragmentation:

- **Use Fixed-size Allocations**: Allocate memory in fixed-size blocks.
- **Group Similar Objects**: Group objects of the same size together to reduce fragmentation.
- **Recycle Memory**: Reuse memory blocks whenever possible.

#### Profiling and Debugging Tools

Use Unreal Engine's built-in profiling and debugging tools to monitor memory usage and detect memory leaks.

- **Unreal Insights**: Provides detailed memory usage statistics.
- **Stat Commands**: Use commands like `stat memory`, `stat slate`, `stat game` to profile different aspects of your game.
- **Memory Report**: Generate a memory report to analyze memory usage.

```cpp
// In the console
stat memory
stat slate
stat game
```

#### Thread Safety

When managing memory in a multi-threaded environment, ensure thread safety to avoid race conditions and data corruption.

- **Use Synchronization Primitives**: Use mutexes, locks, and atomic operations to manage access to shared memory.
- **Thread-safe Containers**: Use thread-safe containers provided by Unreal Engine.

#### Custom Garbage Collection Rules

If your game has specific needs, you can implement custom garbage collection rules to optimize memory management.

- **Marking and Sweeping**: Customize the marking and sweeping phases to suit your game's requirements.
- **Custom Object Lifetimes**: Implement custom logic to manage object lifetimes and references.

#### Efficient Use of Textures and Assets

Large textures and assets can consume significant memory. Optimize their usage to manage memory efficiently.

- **Texture Compression**: Use texture compression to reduce memory footprint.
- **Streaming Assets**: Stream assets on demand to minimize memory usage.
- **Level of Detail (LOD)**: Use LOD to load lower-resolution assets at a distance.

#### Best Practices for Performance

- **Avoid Unnecessary Copies**: Use references and pointers to avoid copying large objects.
- **Minimize Dynamic Allocations**: Prefer stack allocations or pre-allocated memory pools.
- **Cache Frequently Used Data**: Cache data that is accessed frequently to reduce memory access time.
- **Profile Regularly**: Regularly profile your game to identify and address memory issues early.
