---
title: How to return an Array?
---

In C++, arrays declared inside a function like `int arr[5];` live on the **stack**—a fast, temporary memory region tied to the function's lifetime. When the function returns, the stack frame is destroyed, and `arr` no longer exists. Returning it like this:

```cpp
int* makeArray() {
    int arr[5];       // lives on stack
    return arr;       // ❌ returns pointer to invalid memory
}
```

...leads to undefined behavior, because you're returning a pointer to memory that was auto-deleted.

To return an array properly, allocate it on the **heap**, or better yet, use `std::vector`. Heap memory stays valid until you explicitly free it:

```cpp
std::vector<int> makeArray() {
    std::vector<int> arr(5);  // lives on heap, managed
    return arr;               // ✅ safe to return
}
```

**Stack = automatic, fast, short-lived. Heap = manual, persistent.** Use stack for temporary work, heap (or `std::vector`) for anything that needs to survive outside a function.