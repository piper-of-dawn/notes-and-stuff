`const` in C++ is a type qualifier used to indicate that an entity (like a variable, pointer, or member function) is intended to be "read-only" or immutable after its initial definition. It acts as a contract enforced by the compiler, enhancing code safety, clarity, and enabling potential optimizations by signaling invariant properties.

**What is a `const` variable?**
A `const` variable is one whose value cannot be modified after initialization. The compiler prevents any attempts to assign a new value to it. This guarantees the variable retains its initial state throughout its scope, making the code easier to reason about.

```cpp
const int MAX_SIZE = 100;
// MAX_SIZE = 200; // Compile-time error: assignment of read-only variable 'MAX_SIZE'
```

**What is a pointer to `const` data?**
This declares a pointer (`const T* ptr` or `T const* ptr`) through which the pointed-to data cannot be modified. The pointer itself *can* be changed to point to a different location, but the data accessed *via this pointer* is treated as constant.

```cpp
int x = 10;
const int* p = &x; // p points to x, treats x as const via p
// *p = 20;         // Compile-time error: cannot assign through pointer-to-const
int y = 30;
p = &y;            // OK: p can point elsewhere (to y, treated as const via p)
```

**What is a `const` pointer?**
This declares a pointer (`T* const ptr`) that cannot be changed to point to a different memory location after initialization. However, the data *it points to* can still be modified through this pointer (unless the data itself is also `const`).

```cpp
int x = 10;
int y = 20;
int* const q = &x; // q is a const pointer, always points to x
*q = 30;           // OK: modifies the value of x through q
// q = &y;          // Compile-time error: cannot change where q points
```

**What is a `const` pointer to `const` data?**
This (`const T* const ptr` or `T const* const ptr`) combines both restrictions: neither the pointer itself can be changed to point elsewhere, nor can the data it points to be modified *through this pointer*. It represents a fixed pointer to immutable data.

```cpp
int x = 10;
const int* const r = &x; // r always points to x, treats x as const via r
// *r = 20;         // Compile-time error: cannot modify data via r
// int y = 30;
// r = &y;          // Compile-time error: cannot change where r points
```

**What is a `const` reference?**
A `const` reference (`const T& ref`) is an alias for an object that cannot be modified *through that reference*. Like a pointer-to-const, it provides read-only access, but with reference semantics (must be initialized, cannot be null, cannot be reseated).

```cpp
int x = 10;
const int& ref = x; // ref is an alias for x, treating it as const
// ref = 20;        // Compile-time error: assignment of read-only reference 'ref'
```

**What is a `const` member function?**
A member function declared with `const` after its parameter list (`ReturnType ClassName::func() const;`) promises not to modify any non-static member variables of the object it's called on. This allows the function to be called on `const` objects of the class, ensuring read-only access preserves the object's state.

```cpp
class MyClass {
    int value;
public:
    MyClass(int v) : value(v) {}
    int getValue() const { // Promises not to change 'value'
        // value = 10; // Compile-time error inside const member function
        return value;
    }
    void setValue(int v) { // Non-const, can modify 'value'
        value = v;
    }
};
const MyClass obj(5);
int v = obj.getValue(); // OK: calling const member function on const object
// obj.setValue(10);   // Compile-time error: calling non-const member function on const object
```