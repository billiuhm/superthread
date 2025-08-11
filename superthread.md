# SuperThread
SuperThread is a lightweight thread pool, used for parallel computing without the complexity of creating a thread-pool from scratch. 

## Use
Superthread is easy to use in C++, to do it, enter `#include <tpool.hpp>` at the top of any .cpp file, and place `tpool.hpp` into the same folder.
Then preferably in the global namespace, type `threadpool pool`, the `pool` part being completely customisable.

### addTask
`addTask` is used to add a function to the queue. An example being `addTask(foo, var1, var2, var3);`.

`addTask` is capable of returning variables (e.g. `int x = addTask(foo)`) because `addTask` is an `auto` variable type.

To join the thread (like STL's `thread.join()` function), there is no explicit function, but it can be done using `future`.
To help understand:
```cpp
    std::vector<std::future<void>> futures;
    for (size_t i = 0; i < std::thread::hardware_concurrency(); ++i) {
        futures.emplace_back(pool.addTask([i] { particle[i].loop(); }));
    }

    for (auto& future : futures) {
        future.get();
    }
```

## Compilation
For compilation, C++20 is preferred, but C++17 paired with gcc works.