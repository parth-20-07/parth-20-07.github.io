---
id: sw_essentials
aliases:
  - Data Structures and Algorithms
tags:
---

# Data Structures and Algorithms

## Basics

### Heap Memory

- Heap Memory is different from Heap data type. Here, heap means a
simply a pile of memory space available for programmers to allocate and
deallocate.
- Memory in Heap takes time to access.
- The reference to heap memory on generation is stored in Stack
Memory.
- Data is visible to all the threads here, so it can be changed by
some other process which is risky.
- De-Allocation is not automatic and thus needs to be performed
manually everytime.

### Accessing Heap

- Heap can be accessed using **malloc()** function or
**new** operator. Though, keep in mind that the return
types are different for each.
    - malloc() returns a *null* pointer and hence needs to be
    converted to suitable data type.
    - new operator returns the pointer of exact data type and needs to
    be stored accordingly.

### Allocating Memory in Heap

**How to store pointer to an array with 5 elements in
heap** - malloc() function:

```
```cpp
int *p = (int *)malloc(5*sizeof(int));
```
```

- new operator
    
    ```cpp
    int *p = new int[5];
    ```
    

In both cases, though the data is stored in heap, the pointer is
allocated in the stack memory for easy access.

**Create a pointer to a *struct* in a heap**

```cpp
#include <iostream>#include<stdlib.h>using namespace std;struct Rectangle{    int length;    int breadth;};int main(){    struct Rectangle *ptr = (struct Rectangle*)malloc(sizeof(struct Rectangle));    ptr->length = 10;    ptr->breadth = 15;    printf("Length: %d\nBreadth: %d",ptr->length, ptr->breadth);    return 0;}
```

### References in C++

Referencing is the method of providing multiple names to a single
memory cell/data. This is essential when building small function where
pointers are not that essential. Referencing can be done by:

```cpp
int a = 10;int &r = a;
```

Assume that Address of **a** is *200/201*. When
we reference **r** to **a**, now the address
of **r** is also *200/201*. This means that the same
block of memory has multiple names to access it. All the data
manipulation methods work on the referenced block as original.

### Abstract Data Type

Abstract Data type (ADT) is a type (or class) for objects whose
behavior is defined by a set of values and a set of operations.

E.g. Adding the functionality of *pop()*, *push()* can
convert the linked list to either a stack or a queue data type.

## Physical Data Structures

Physical data structures are the actual implementations of data
structures in memory. They determine the performance and efficiency of
algorithms that use the data structure, by determining how the data is
stored and accessed in memory.

Types: - [Arrays](about:blank#arrays) - [Linked
Lists](about:blank#linked-list)

## Logical Data Structures

Logical Data Structures use the process of storing physical data
structures in memory and perform various operation to optimize time or
memory.

Types: - Linear Data Structures - [Stack](about:blank#stack) (LIFO) -
[Queue](about:blank#queue) (FIFO) - Non Linear Data Structures - [Trees](about:blank#trees) - [Graph](about:blank#graph) - Tabular Data
Structures - [Hash Table](about:blank#hash-table)

## Time and Space Complexity

### Constant Time: O(1)

When there is no dependence on the input size n, an algorithm is said
to have a constant time of order **O(1)**.

Example

```cpp
void example_function(int array[]):    print("First element of list: ", array[0])
```

The function above will require only one execution step whether the
above array contains 1, 100 or 1000 elements. As a result, the function
is in constant time with time complexity O(1).

### Linear Time: O(n)

Linear time is achieved when the running time of an algorithm
increases linearly with the length of the input. This means that when a
function runs for or iterates over an input size of n, it is said to
have a time complexity of order O(n).

Example

```python
def example_function(lst, size):
    for i in range(size):
        print("Element at index", i, " has value: ", lst[i])
```

The above function will take O(n) time (or “linear time”) to
complete, where n is the number of entries in the array. The function
will print 10 times if the given array has 10 entries, and 100 times if
the array has 100 entries.

*Note: Even if you iterate over half the array, the runtime still
depends on the input size, so it will be considered O(n).*

### Logarithm Time: O(log n)

When the size of the input data decreases in each step by a certain
factor, an algorithm will have logarithmic time complexity. This means
as the input size grows, the number of operations that need to be
executed grows comparatively much slower.

To better understand log n, let’s think of finding a word in a
dictionary. If you want to find a word with the letter “p”, you can go
through every alphabet and try finding the word, which is linear time
O(n). Another route you can take is to open the book to the exact center
page. If the word on the center page comes before “p”, you look for the
word in the right half. Otherwise, you look in the left half. In this
example, you are reducing your input size by half every time, so the
number of operations you will need to perform significantly reduces
compared to going through every letter. Thus you will have a time
complexity of O(log (n)).

Example Binary search and finding the largest/smallest element in a
binary tree are both examples of algorithms having logarithmic time
complexity.

Binary search comprises searching an array for a specified value by
splitting the array into two parts consistently and searching for the
element in only one of the two parts. This ensures that the operation is
not performed on every element of the input data.

```python
def binarySearch(lst, x):
    low = 0    high = len(lst)-1    # Repeat until the pointers low and high meet each other    while low <= high:
        mid = low + (high - low)//2        if lst[mid] == x:
            return mid
        elif lst[mid] < x:
            low = mid + 1        else:
            high = mid - 1    return -1
```

The Binary Search method takes a sorted list of elements and searches
through it for the element x. This is how the algorithm works:

1. Find the list’s midpoint.
2. Compare the target to the middle.
3. We’ve located our goal if our value and the target match.
4. If our value is lesser than the target, we focus on the list with
values ranging from the middle plus one to the highest.
5. If our value is greater than the target, we focus on the list
starting with the smallest value and ending with the midpoint minus one.
Continue until we locate the target or till we reach the last element,
which indicates that the element is not present in the list.

With every iteration, the size of our search list shrinks by half.
Therefore traversing and finding an entry in the list takes O(log(n))
time.

### Quadratic Time: O(n^2)

The performance of a quadratic time complexity algorithm is directly
related to the squared size of the input data collection. You will
encounter such time complexity in programs when you perform several
iterations on data sets.

Example

```python
def quadratic_function(lst, size):
    for i in range(size):
        for j in range(size):
            print("Iteration : " i, "Element of list at ", j, " is ", lst[j])
```

We have two nested loops in the example above. If the array has n
items, the outer loop will execute n times, and the inner loop will
execute n times for each iteration of the outer loop, resulting in n^2
prints. If the size of the array is 10, then the loop runs 10x10 times.
So the function ten will print 100 times. As a result, this function
will take O(n^2) time to complete.

### Exponential Time: O(2^n)

With each addition to the input (n), the growth rate doubles, and the
algorithm iterates across all subsets of the input elements. When an
input unit is increased by one, the number of operations executed is
doubled.

Example

```python
def fibonacci(n):
    if (n <= 1):
        return 1    else:
        return fibonacci(n - 2) + fibonacci(n - 1)
```

In the above example, we use recursion to calculate the Fibonacci
sequence. The algorithm O(2^n) specifies a growth rate that doubles
every time the input data set is added. An O(2^n) function’s exponential
growth curve starts shallow and then rises rapidly.

![https://misc-flexiple.s3.amazonaws.com/bon_cheat_sheet.jpg](https://misc-flexiple.s3.amazonaws.com/bon_cheat_sheet.jpg)

Time Notation

## Arrays

---

## Linked List

---

### Basics

---

- The head and the tail elements have the pointer value

***null***

.

---

### Pros

---

- Ease of insertion and deletion due to the fact that the list is
linked using pointers of the next dataset. - Inserts/Deletes are

*Constant Time Operations*

. - Random Access of data are

*Linear Time Operations*

. - Anytime we need to have “fast”
insertions and deletions, but random access is less important.

---

### Cons

---

- Since the data is linked via pointers and separated across memory,
it is slow to iterate through the whole list when needed. - Data is
always stored in Heap. - If you are looking for a particular element in
the list, the only possible option to reach that is go through every
single element through the list due to lack of fixes position of the
element. - You can only move in the forward direction when moving in the
linked list.

---

## Stack

## Trees

## Hash Table

## Resources

- Udemy: Mastering Data Structures and Algorithm using C and
C++:
    
    ![https://www.notion.soDSA-Udemy.jpeg](https://www.notion.soDSA-Udemy.jpeg)
    
- [neetcode.io/practice](https://neetcode.io/practice)
- [Big O
Notation Cheat Sheet](https://flexiple.com/algorithms/big-o-notation-cheat-sheet/)
