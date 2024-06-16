Let's dive into the details of some of the most commonly used containers in the C++ Standard Template Library (STL): `vector`, `list`, `map`, and `set`.

### `std::vector`

**Description**:
A `vector` is a dynamic array that can change size. It allows random access to elements and provides fast insertion and deletion at the end.

**Key Features**:
- Dynamic size.
- Fast random access (constant time complexity for access).
- Fast insertion and deletion at the end (amortized constant time complexity).
- Elements are stored contiguously in memory.

**Usage**:
```cpp
#include <vector>
#include <iostream>

int main() {
    std::vector<int> vec = {1, 2, 3, 4};

    // Add elements
    vec.push_back(5);
    vec.push_back(6);

    // Access elements
    std::cout << "Element at index 2: " << vec[2] << std::endl;

    // Iterate over elements
    for (const auto& val : vec) {
        std::cout << val << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

> **Note**: *What `const auto&` means in the range-based for loop*:
> - `const` ensures that the elements are not modified for read-only access
> - `auto` automatically deduces the type of the elements
> - `&` is a reference to avoid copying elements for efficiency (especially for large objects) because the range-based for loop creates a copy of each element by default

### `std::list`

**Description**:
- A `list` is a doubly linked list. It allows efficient insertion and deletion of elements anywhere in the list.

**Key Features**:
- Bidirectional iteration.
- Efficient insertion and deletion (constant time complexity) anywhere in the list.
- No direct random access (linear time complexity for access).

**Usage**:
```cpp
#include <list>
#include <iostream>

int main() {
    std::list<int> lst = {1, 2, 3, 4};

    // Add elements
    lst.push_back(5);
    lst.push_front(0);

    // Access elements (no direct random access)
    auto it = lst.begin();
    std::advance(it, 2); // Move iterator to the third element
    std::cout << "Third element: " << *it << std::endl;

    // Iterate over elements
    for (const auto& val : lst) {
        std::cout << val << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

### `std::map`

**Description**:
- A `map` is an associative container that stores key-value pairs. It maintains a sorted order of keys and allows fast retrieval of values based on keys.

**Key Features**:
- Sorted by keys.
- Unique keys.
- Logarithmic time complexity for insertion, deletion, and access.
- Implements a balanced binary tree (typically a Red-Black Tree).

**Usage**:
```cpp
#include <map>
#include <iostream>

int main() {
    std::map<int, std::string> myMap;

    // Add elements
    myMap[1] = "One";
    myMap[2] = "Two";
    myMap[3] = "Three";

    // Access elements
    std::cout << "Value for key 2: " << myMap[2] << std::endl;

    // Iterate over elements
    for (const auto& pair : myMap) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }

    return 0;
}
```


### `std::set`

**Description**:
- A `set` is a container that stores unique elements in a sorted order. It provides fast retrieval and ensures that each element is unique.

**Key Features**:
- Sorted elements.
- Unique elements.
- Logarithmic time complexity for insertion, deletion, and access.
- Typically implemented as a balanced binary tree (Red-Black Tree).

**Usage**:
```cpp
#include <set>
#include <iostream>

int main() {
    std::set<int> mySet = {1, 2, 3, 4};

    // Add elements
    mySet.insert(5);
    mySet.insert(1); // Duplicate, will not be added

    // Access elements
    if (mySet.find(3) != mySet.end()) {
        std::cout << "Element 3 found in the set" << std::endl;
    }

    // Iterate over elements
    for (const auto& val : mySet) {
        std::cout << val << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

### Comparison

| Type   | Pros                                                                   | Cons                                                 |
|--------|------------------------------------------------------------------------|------------------------------------------------------|
| **Vector** | Fast random access, efficient insertion at the end.                    | Inefficient insertion and deletion at arbitrary positions (except at the end). |
| **List**   | Efficient insertion and deletion at any position.                      | No random access, more memory overhead due to pointers. |
| **Map**    | Fast search, insertion, and deletion based on keys, sorted keys.       | More memory overhead due to tree structure.          |
| **Set**    | Unique, sorted elements, fast search, insertion, and deletion.         | No duplicate elements allowed, more memory overhead due to tree structure. |
