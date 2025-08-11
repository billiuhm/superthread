# SuperThread
SuperThread is a lightweight thread pool, used for parallel computing without the complexity of creating a thread-pool from scratch. 

## Compilation &amp; dependencies
For compilation, C++20 is preferred, but C++17 paired with gcc works. There is no external libraries needed (except for `STL`), and `threadPool` is not OS-Specific.

## Use
Superthread is easy to use in C++, to do it, enter `#include <tpool.hpp>` at the top of any .cpp file, and place `tpool.hpp` into the same folder.
Then preferably in the global namespace, type `threadPool pool`, the `pool` part being completely customisable.

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

### addTaskBatch
`addTaskBatch` is used when something needs to be parallelised efficiently without the overhead of running `addTask` over and over again, although it has it's own set of limits.

`addTaskBatch` has 3 arguments, `count`, `function`, `args`.
* `count` - the amount of times the function needs to be ran (`size_t`)
* `function` - the function to be ran (e.g. `foo()`)
* `args` - the parameters (e.g. `foo(1, 2, 3)`)

To return the outputs, you will need something along the lines of:
```cpp
std::vector<std::future<int>> results = pool.addTaskBatch(quantity, foo, argument);
```

## Testing performance
To test this library's efficiency, I will put it through tests, only using this compile command:
`g++ -O0 -o main.exe main.cpp`

### Memory usage
To test memory usage, I will set it up for 8 threads and try get minimal overhead.
``` cpp
#include "tpool.hpp"
#include <thread>
#include <chrono>

int main() {
    threadPool pool(8);

    while (true) {
        
    }
}
```

This returns 584 KB via `taskmgr.exe`'s details tab, and measuring specifically the `Memory (active private working set)`.

## Summary
Superthread is a simple and lightweight thread-pool for easy &amp; performant use written in C++ that has:
* Low memory use (584 kilobytes with 8 threads)
* Low CPU use
* Fast compilation times
* Quick setup times (drag-and-drop)
* Fast writing times
* No ties to an OS
* Works in low-level enviroments easily
