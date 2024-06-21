**Memory Management in C++**

Memory management is a fundamental aspect of C++ programming. It involves allocating and deallocating memory for variables and objects during program execution. C++ provides both manual and automatic memory management techniques:

- **Manual Memory Management:**
  - Uses pointers to allocate memory dynamically using `new` and `delete` operators.
  - Requires careful management to prevent memory leaks (unused allocated memory) and dangling pointers (pointers pointing to deallocated memory).
- **Smart Pointers (Automatic):**
  - Provide safer alternatives to raw pointers by managing memory automatically.
  - Common smart pointers include:
    - `unique_ptr`: Ensures a single owner of memory, automatically deallocated upon destruction.
    - `shared_ptr`: Allows multiple owners of memory, deallocated when the last owner goes out of scope.
    - `weak_ptr`: Can be used with `shared_ptr` to avoid cyclic references and memory leaks.
- **RAII (Resource Acquisition Is Initialization):**
  - An approach where resources (including memory) are acquired during object initialization and automatically released during object destruction.
  - Common in C++ libraries to ensure proper resource management.

Effective memory management is crucial for C++ programs to run efficiently and avoid memory-related issues. Understanding both manual and automatic techniques empowers you to write robust and reliable code.

### code examples demonstrating different memory management techniques in C++:

**1. Manual Memory Management with Raw Pointers:**

```c++
#include <iostream>

int main() {
  // Allocate memory for an integer using new
  int* ptr = new int;

  // Assign a value to the allocated memory
  *ptr = 42;

  // Access the value using the pointer
  std::cout << "Value: " << *ptr << std::endl;

  // Important: Deallocate memory using delete to prevent memory leaks
  delete ptr;

  // Don't access the memory after deletion (dangling pointer)
  // std::cout << "Value after delete: " << *ptr << std::endl; // Potential error

  return 0;
}
```

**2. Using Smart Pointers (`unique_ptr`):**

```c++
#include <iostream>
#include <memory>

int main() {
  // Create a unique_ptr that automatically manages memory
  std::unique_ptr<int> ptr(new int);

  // Assign a value like before
  *ptr = 42;

  std::cout << "Value: " << *ptr << std::endl;

  // No need to explicitly deallocate, unique_ptr handles it on destruction
}
```

**3. Using Smart Pointers (`shared_ptr`):**

```c++
#include <iostream>
#include <memory>

class MyClass {
public:
  MyClass() {
    std::cout << "MyClass object created." << std::endl;
  }

  ~MyClass() {
    std::cout << "MyClass object destroyed." << std::endl;
  }
};

int main() {
  // Create a shared_ptr pointing to a new MyClass object
  std::shared_ptr<MyClass> ptr1(new MyClass);

  // Another shared_ptr can share ownership (reference counting)
  std::shared_ptr<MyClass> ptr2 = ptr1;

  std::cout << "Number of owners: " << ptr1.use_count() << std::endl; // Outputs 2

  // When both ptr1 and ptr2 go out of scope, MyClass object is deleted
}
```

**4. RAII (Resource Acquisition Is Initialization):**

```c++
#include <iostream>
#include <fstream>

class FileHandle {
private:
  std::ifstream file;

public:
  // Constructor opens the file (resource acquisition)
  FileHandle(const std::string& filename) : file(filename) {
    if (!file.is_open()) {
      std::cerr << "Error opening file!" << std::endl;
    }
  }

  // Destructor closes the file (resource release) automatically
  ~FileHandle() {
    if (file.is_open()) {
      file.close();
      std::cout << "File closed." << std::endl;
    }
  }

  // Methods to operate on the file (assuming appropriate methods)
  // ...
};

int main() {
  // FileHandle object automatically manages file opening and closing
  FileHandle handle("my_file.txt");

  // ... (use handle methods to access the file)
}
```

These are basic examples, and memory management in C++ can be quite complex. Remember to choose the appropriate technique based on your needs and ensure proper resource management to prevent memory leaks and other issues.
