---
id: CPP_Multithreading
aliases:
  - Multithreading
tags: []
---

# Multithreading


## Concurrency

- Concurrency means multiple threads running at the same time without sharing any resources.
- Commonly used for splitting tasks between threads that can be worked independently.
- the `int main()` runs the main thread, you need to use `join()` to attach the rest of the threads to the main thread so that the program does not exit before the secondary threads end.

Basic Usage:

```cpp
#include <thread>
#include <iostream>

void workFunc(int* ptr, size_t times)
{
	while(times--) {
		*ptr += 1;
	}
}

int main(int argc, char const *argv[])
{
	int* numbers = new int[3];

	std::thread t1(workFunc, numbers, 500);
	std::thread t2(workFunc, numbers + 1, 600);
	std::thread t3(workFunc, numbers + 2, 700);

	t1.join();
	t2.join();
	t3.join();

	for(int i = 0; i < 3; ++i)
		std::cout << "Number Slot " << (int)i << " is " << (int)numbers[i] << std::endl;

	delete[] numbers;
	return 0;
}
```

## Shared Resources

- A resource where two different threads can access the same memory address is called a shared resource.
- Shared resources can be managed between threads using `mutex`. They are like batons which indicate that only when you have the baton, can you edit the shared data.

Example:

```cpp
#include <thread>
#include <mutex>
#include <queue>

template<class T>
class SafeQueue {
public:
    void push(const T& val) {
        std::lock_guard<std::mutex> lock(_m);
        _q.push(val);
    }

    bool pop(T& val) {
        std::lock_guard<std::mutex> lock(_m);
        if (!_q.empty()) {
            val = _q.front();
            _q.pop();
            return true;
        } else {
            return false;
        }
    }
private:
    std::mutex _m;
    std::queue<T> _q;
};
```

- `std::lock_guard` is a new way of holding mutex as it self-releases when you are out of scope. This helps against accidentally not releasing the mutex when the thread scope is finished.
- `std::conditional_variable` can be used to introduce synchronization between threads.

## Resources

- [Concurrency support library (since C++11) - cppreference.com](https://en.cppreference.com/w/cpp/thread)
- [std::condition_variable - cppreference.com](https://en.cppreference.com/w/cpp/thread/condition_variable)
- [C++ Multithreading, the simple way](https://medium.com/codex/c-multithreading-the-simple-way-95aa1f7304a2)
- [Multithreading in C++ - GeeksforGeeks](https://www.geeksforgeeks.org/multithreading-in-cpp/)
- [When to use multithreading in C++?](https://stackoverflow.com/questions/22548759/when-to-use-multithreading-in-c)
- [C++ Advanced Multithreading Guide - Mutexes, Asyncs, Futures](https://www.srcmake.com/home/cpp-advanced-multithreading)
- [A tutorial on modern multithreading and concurrency in C++](https://www.educative.io/blog/modern-multithreading-and-concurrency-in-cpp)
- [An Introduction to Multithreading in C++20 - Anthony Williams - CppCon 2022](https://youtu.be/A7sVFJLJM-A?si=iiRbhIDXTd1PGO92)
