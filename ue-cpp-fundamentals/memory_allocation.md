## Memory Allocation

### `malloc`

`malloc` is a standard C library function that allocates a specified amount of memory and returns a pointer to it. The allocated memory is uninitialized. `malloc` stands for "memory allocation."

**Syntax**:
```c
void* malloc(size_t size);
```

- `size`: The number of bytes to allocate.
- Returns a pointer to the allocated memory or `NULL` if the allocation fails.

**Example**:
```c
#include <stdlib.h> // For malloc and free
#include <stdio.h>

int main() {
    // Allocate memory for an array of 10 integers
    int* array = (int*)malloc(10 * sizeof(int));

    if (array == NULL) {
        printf("Memory allocation failed\n");
        return 1;
    }

    // Use the allocated memory
    for (int i = 0; i < 10; ++i) {
        array[i] = i;
        printf("%d ", array[i]);
    }
    printf("\n");

    // Free the allocated memory
    free(array);

    return 0;
}
```

### `free`

`free` is a standard C library function that deallocates memory previously allocated by `malloc`, `calloc`, or `realloc`.

**Syntax**:
```c
void free(void* ptr);
```

- `ptr`: Pointer to the memory to be deallocated.

**Example**:
```c
int* array = (int*)malloc(10 * sizeof(int));
// Use the array
free(array); // Deallocate the memory
```

### `calloc` (Contiguous Allocation)

`calloc` is similar to `malloc` but initializes the allocated memory to zero. It allocates memory for an array of elements, initializes all bytes to zero, and returns a pointer to the allocated memory.

**Syntax**:
```c
void* calloc(size_t num, size_t size);
```

- `num`: Number of elements.
- `size`: Size of each element.

**Example**:
```c
int* array = (int*)calloc(10, sizeof(int));
```

### `realloc` (Reallocation)

`realloc` changes the size of previously allocated memory. It can expand or shrink the memory block.

**Syntax**:
```c
void* realloc(void* ptr, size_t size);
```

- `ptr`: Pointer to the memory block to be resized.
- `size`: New size in bytes.

**Example**:
```c
int* array = (int*)malloc(10 * sizeof(int));
array = (int*)realloc(array, 20 * sizeof(int));
```

### `new` and `delete`

In C++, `new` and `delete` are operators used for dynamic memory management.

- **`new`**: Allocates memory and calls the constructor.
- **`delete`**: Deallocates memory and calls the destructor.

**Example**:
```cpp
int* array = new int[10]; // Allocate memory for an array of 10 integers
delete[] array; // Deallocate memory
```

### Unreal Engine Memory Management

Unreal Engine provides its own memory management functions and utilities, built on top of standard C++ mechanisms, tailored for game development. **Use Unreal's memory management utilities instead of manual malloc and free**.

#### `FMemory::Malloc` and `FMemory::Free`

Unreal Engine provides `FMemory::Malloc` and `FMemory::Free` for memory allocation and deallocation.

**Syntax**:
```cpp
void* FMemory::Malloc(SIZE_T Count, uint32 Alignment = DEFAULT_ALIGNMENT);
void FMemory::Free(void* Original);
```

- `Count`: Number of bytes to allocate.
- `Alignment`: Alignment of the allocated memory.

**Example**:
```cpp
void* Memory = FMemory::Malloc(1024);
FMemory::Free(Memory);
```

#### Smart Pointers (`TSharedPtr`, `TWeakPtr`, `TUniquePtr`)

Unreal Engine provides smart pointers to manage object lifetimes automatically.

**Example**:
```cpp
TSharedPtr<MyClass> SharedPtr = MakeShared<MyClass>();
TWeakPtr<MyClass> WeakPtr = SharedPtr;
TUniquePtr<MyClass> UniquePtr = MakeUnique<MyClass>();
```

#### `NewObject` and `SpawnActor`

For `UObject`-derived classes, use `NewObject` and `SpawnActor` to create instances that are managed by Unreal's garbage collector.

**Example**:
```cpp
UMyObject* MyObject = NewObject<UMyObject>(this);
AActor* MyActor = GetWorld()->SpawnActor<AMyActor>(AMyActor::StaticClass());
```
