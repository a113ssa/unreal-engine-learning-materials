## Templates

**Templates** in C++ allow you to write generic and reusable code. They enable functions and classes to operate with generic types.

The expression ```template <typename T>``` is a part of C++ template programming. It defines a template for a function or class, allowing it to work with any data type. This makes the code more flexible and reusable.

**Breakdown of ```template <typename T>```**
```template```: This keyword tells the compiler that the following code is a template.
```<typename T>```: This specifies that T is a placeholder for a data type. You can use any identifier instead of T, but T is conventional.

- **Function Templates**: Create functions that can operate on different data types.
   ```cpp
   template <typename T>
   T GetMax(T a, T b) {
       return (a > b) ? a : b;
   }

   int main() {
       int i = GetMax<int>(1, 2);    // Calls GetMax with int
       float f = GetMax<float>(1.0f, 2.0f); // Calls GetMax with float
       return 0;
   }
   ```

- **Class Templates**: Define a class that can handle any data type.
   ```cpp
   template <typename T>
   class MyContainer {
   private:
       T value;
   public:
       MyContainer(T v) : value(v) {}
       T getValue() { return value; }
   };

   int main() {
       MyContainer<int> intContainer(5);
       MyContainer<float> floatContainer(5.5f);
       return 0;
   }
   ```

- **Template Specialization**: Customize the implementation of a template for a specific type.
   ```cpp
   template <> // Specialize for char*
   class MyContainer<char*> {
   private:
       char* value;
   public:
       MyContainer(char* v) : value(v) {}
       char* getValue() { return value; }
   };
   ```

**Why Use Templates?**
- **Reusability**: Templates allow you to write code that can be reused for different data types.
- **Type Safety**: They provide compile-time type checking.
- **Performance**: Templates can be more efficient than other forms of polymorphism because they are resolved at compile time.

## STL (Standard Template Library)

The **Standard Template Library (STL)** provides a set of common classes and functions for data structures and algorithms. It includes containers, iterators, and algorithms.

#### Containers:
Store collections of objects. Common containers include `vector`, `list`, `map`, and `set` ([*what are vector, list, map and set*](./vector_list_map_set.md))
   ```cpp
   #include <vector>
   #include <map>
   #include <iostream>

   int main() {
       std::vector<int> vec = {1, 2, 3, 4};
       std::map<int, std::string> myMap;
       myMap[1] = "One";
       myMap[2] = "Two";

       for (int val : vec) {
           std::cout << val << " ";
       }

       for (auto& pair : myMap) {
           std::cout << pair.first << ": " << pair.second << " ";
       }

       return 0;
   }
   ```

#### Iterators:
Provide a way to traverse containers.

Here are examples for each type of iterator in C++:

- **Input iterators** are used to read data from a sequence. They support operations like `++` (increment) and `*` (dereference).

    **Example**: Reading from `std::istream_iterator`

    ```cpp
    #include <iostream>
    #include <iterator>
    #include <vector>

    int main() {
        std::cout << "Enter integers (end with EOF, e.g., Ctrl+D): ";

        // Create an input stream iterator for std::cin
        std::istream_iterator<int> inputIt(std::cin);
        std::istream_iterator<int> endOfStream;

        std::vector<int> vec(inputIt, endOfStream);

        std::cout << "You entered: ";
        for (auto& val : vec) {
            std::cout << val << " ";
        }
        std::cout << std::endl;

        return 0;
    }
    ```

- **Output iterators** are used to write data to a sequence. They support operations like `++` (increment) and `*` (dereference) to assign values.

    **Example**: Writing to `std::ostream_iterator`

    ```cpp
    #include <iostream>
    #include <iterator>
    #include <vector>

    int main() {
        std::vector<int> vec = {1, 2, 3, 4, 5};

        // Create an output stream iterator for std::cout
        std::ostream_iterator<int> outputIt(std::cout, " ");

        std::cout << "Vector elements: ";
        std::copy(vec.begin(), vec.end(), outputIt); // Copy elements to output

        std::cout << std::endl;

        return 0;
    }
    ```

- **Forward iterators** can read and write data, and they support operations like `++` (increment) and `*` (dereference). They guarantee single-pass traversal.

    **Example**: Using `std::forward_list`

    ```cpp
    #include <iostream>
    #include <forward_list>

    int main() {
        std::forward_list<int> flist = {1, 2, 3, 4, 5};

        std::cout << "Forward list elements: ";
        for (auto it = flist.begin(); it != flist.end(); ++it) {
            std::cout << *it << " ";
        }
        std::cout << std::endl;

        return 0;
    }
    ```

- **Bidirectional iterators** can read and write data, and they support operations like `++` (increment) and `--` (decrement). They allow traversal in both directions.

    **Example**: Using `std::list`

    ```cpp
    #include <iostream>
    #include <list>

    int main() {
        std::list<int> lst = {1, 2, 3, 4, 5};

        std::cout << "List elements forward: ";
        for (auto it = lst.begin(); it != lst.end(); ++it) {
            std::cout << *it << " ";
        }
        std::cout << std::endl;

        std::cout << "List elements backward: ";
        for (auto it = lst.rbegin(); it != lst.rend(); ++it) {
            std::cout << *it << " ";
        }
        std::cout << std::endl;

        return 0;
    }
    ```

- **Random access iterators** can read and write data, and they support operations like `++` (increment), `--` (decrement), `+` (addition), `-` (subtraction), and `[]` (indexing). They allow direct access to any element in constant time.

    **Example**: Using `std::vector`

    ```cpp
    #include <iostream>
    #include <vector>

    int main() {
        std::vector<int> vec = {1, 2, 3, 4, 5};

        std::cout << "Vector elements using random access: ";
        for (size_t i = 0; i < vec.size(); ++i) {
            std::cout << vec[i] << " ";
        }
        std::cout << std::endl;

        std::cout << "Vector elements using iterator: ";
        for (auto it = vec.begin(); it != vec.end(); ++it) {
            std::cout << *it << " ";
        }
        std::cout << std::endl;

        return 0;
    }
    ```

**Summary**

- **Input Iterator**: Used to read data from a sequence (`std::istream_iterator`).
- **Output Iterator**: Used to write data to a sequence (`std::ostream_iterator`).
- **Forward Iterator**: Supports single-pass traversal with read and write capabilities (`std::forward_list`).
- **Bidirectional Iterator**: Supports traversal in both directions with read and write capabilities (`std::list`).
- **Random Access Iterator**: Supports direct access to any element in constant time (`std::vector`).

#### Algorithms
The C++ Standard Library provides a rich set of algorithms that operate on containers through iterators. These algorithms perform operations like searching, sorting, counting, manipulating, etc.

**Common Algorithms**:
- **Searching Algorithms**:
    `std::find`: Finds the first occurrence of a value in a range.
    `std::binary_search`: Checks if a value exists in a sorted range.

- **Sorting Algorithms**:
    `std::sort`: Sorts elements in a range.
    `std::stable_sort`: Sorts elements while preserving the relative order of equivalent elements.
- **Counting Algorithms**:
    `std::count`: Counts the number of elements equal to a value.
    `std::count_if`: Counts the number of elements satisfying a condition.
- **Manipulating Algorithms**:
    ``std::copy``: Copies elements from one range to another.
    ``std::transform``: Applies a function to a range and stores the result in another range.

**Examples of Using Algorithms**:
- **std::find**
    ```cpp
    #include <vector>
    #include <algorithm>
    #include <iostream>

    int main() {
        std::vector<int> vec = {1, 2, 3, 4, 5};

        auto it = std::find(vec.begin(), vec.end(), 3);

        if (it != vec.end()) {
            std::cout << "Element found: " << *it << std::endl;
        } else {
            std::cout << "Element not found" << std::endl;
        }

        return 0;
    }
    ```
- **std::sort**
    ```cpp
    #include <vector>
    #include <algorithm>
    #include <iostream>

    int main() {
        std::vector<int> vec = {5, 2, 3, 1, 4};

        std::sort(vec.begin(), vec.end());

        for (const auto& val : vec) {
            std::cout << val << " ";
        }
        std::cout << std::endl;

        return 0;
    }
    ```
- **std::count_if**
    ```cpp
    #include <vector>
    #include <algorithm>
    #include <iostream>

    int main() {
        std::vector<int> vec = {1, 2, 3, 4, 5};

        int count = std::count_if(vec.begin(), vec.end(), [](int val) {
            return val > 3;
        });

        std::cout << "Number of elements greater than 3: " << count << std::endl;

        return 0;
    }
    ```
- **std::transform**
    ```cpp
    #include <vector>
    #include <algorithm>
    #include <iostream>

    int main() {
        std::vector<int> vec = {1, 2, 3, 4, 5};
        std::vector<int> result(vec.size());

        std::transform(vec.begin(), vec.end(), result.begin(), [](int val) {
            return val * 2;
        });

        for (const auto& val : result) {
            std::cout << val << " ";
        }
        std::cout << std::endl;

        return 0;
    }
    ```
**Combining Iterators and Algorithms**:
By combining iterators and algorithms, you can perform complex operations on containers efficiently and concisely. Here’s a more complex example:

```cpp
#include <vector>
#include <algorithm>
#include <iostream>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};

    // Increment all elements by 1
    std::for_each(vec.begin(), vec.end(), [](int& val) {
        val += 1;
    });

    // Find if any element is greater than 5
    bool anyGreaterThanFive = std::any_of(vec.begin(), vec.end(), [](int val) {
        return val > 5;
    });

    std::cout << "Any element greater than 5: " << (anyGreaterThanFive ? "Yes" : "No") << std::endl;

    // Print all elements
    std::for_each(vec.begin(), vec.end(), [](int val) {
        std::cout << val << " ";
    });
    std::cout << std::endl;

    return 0;
}
```