# Mathematical Analysis for Recursive and Non-Recursive Algorithms
### Design and Analysis of Algorithms (DAA) – Engineering Semester Notes

> **Goal:** Understand how to mathematically analyze recursive and non-recursive algorithms, determine their time and space complexities, identify when each approach is appropriate, and solve exam-oriented problems confidently.

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. Why Mathematical Analysis is Important](#2-why-mathematical-analysis-is-important)
- [3. Recursive Algorithms](#3-recursive-algorithms)
- [4. Non-Recursive Algorithms](#4-non-recursive-algorithms)
- [5. When to Use Each Approach](#5-when-to-use-each-approach)
- [6. Where They Are Used](#6-where-they-are-used)
- [7. How to Identify the Correct Approach](#7-how-to-identify-the-correct-approach)
- [8. Mathematical Analysis of Non-Recursive Algorithms](#8-mathematical-analysis-of-non-recursive-algorithms)
- [9. Mathematical Analysis of Recursive Algorithms](#9-mathematical-analysis-of-recursive-algorithms)
- [10. Solving Recurrence Relations](#10-solving-recurrence-relations)
- [11. Complexity Analysis](#11-complexity-analysis)
- [12. Implementation Examples](#12-implementation-examples)
- [13. Visual Explanation](#13-visual-explanation)
- [14. Edge Cases](#14-edge-cases)
- [15. Limitations](#15-limitations)
- [16. Comparison Table](#16-comparison-table)
- [17. Exam Tips](#17-exam-tips)
- [18. Frequently Asked Questions](#18-frequently-asked-questions)
- [19. Quick Revision Sheet](#19-quick-revision-sheet)

---

# 1. Introduction

Algorithm analysis determines **how efficiently an algorithm performs** without actually executing it.

The two primary categories are:

- **Recursive Algorithms**
  - Function calls itself.
  - Uses the system call stack.

- **Non-Recursive Algorithms**
  - Uses loops (`for`, `while`)
  - No self-calls.

The goal is to calculate:

- Running Time
- Memory Usage
- Growth Rate
- Scalability

---

# 2. Why Mathematical Analysis is Important

Without analysis:

- Cannot compare algorithms.
- Cannot estimate execution time.
- Difficult to optimize programs.
- Impossible to predict scalability.

Example:

Suppose two sorting algorithms exist.

Algorithm A:
- Time = O(n²)

Algorithm B:
- Time = O(n log n)

For n = 1,000,000

Algorithm A performs roughly:

```
1,000,000²
=
10¹² operations
```

Algorithm B performs roughly:

```
1,000,000 × log₂(1,000,000)

≈ 20,000,000 operations
```

Huge difference.

---

# 3. Recursive Algorithms

## Definition

A recursive algorithm solves a problem by solving smaller instances of the same problem.

Every recursive algorithm has:

- Base Case
- Recursive Case

Example:

```
Factorial(n)

if n==0
    return 1

return n × Factorial(n-1)
```

---

## Structure

```
Function

↓

Base Case?

↓

Yes → Stop

↓

No

↓

Recursive Call

↓

Return Result
```

---

# 4. Non-Recursive Algorithms

Uses loops instead of function calls.

Example:

```
sum = 0

for i = 1 to n
    sum += i
```

Execution happens sequentially.

No call stack is created.

---

# 5. When to Use Each Approach

## Use Recursion When

- Tree traversal
- Graph DFS
- Divide and Conquer
- Backtracking
- Dynamic Programming (Top Down)

Examples

- Merge Sort
- Quick Sort
- DFS
- Tower of Hanoi
- Binary Search (recursive version)

---

## Use Non-Recursion When

- Sequential processing
- Large datasets
- Memory efficiency matters
- Avoid stack overflow

Examples

- Array traversal
- Bubble Sort
- Selection Sort
- BFS
- Iterative Binary Search

---

# 6. Where They Are Used

| Domain | Recursive | Non Recursive |
|---------|-----------|---------------|
| Trees | Yes | Sometimes |
| Graphs | DFS | BFS |
| AI Search | Yes | Yes |
| File Systems | Yes | Yes |
| Operating Systems | Yes | Yes |
| Parsing | Yes | Rare |
| Databases | Sometimes | Yes |
| Machine Learning | Rare | Yes |

---

# 7. How to Identify the Correct Approach

Ask these questions:

```
Can the problem be divided into
smaller versions of itself?

↓

Yes

↓

Does each smaller problem have
the same structure?

↓

Yes

↓

Use Recursion
```

Otherwise

```
Simple repetition?

↓

Yes

↓

Use Loop
```

---

## Decision Tree

```
Problem

│

├── Repeated calculations?

│      │

│      ├── Yes

│      │      │

│      │      └── Loop

│

└── Smaller identical subproblems?

       │

       ├── Yes

       │      │

       │      └── Recursion

       │

       └── No

              │

              └── Loop
```

---

# 8. Mathematical Analysis of Non-Recursive Algorithms

---

## Case 1

```
for i=1 to n

    print(i)
```

Operations:

```
Executed n times

Time = n

O(n)
```

---

## Case 2

Nested Loop

```
for i=1 to n

    for j=1 to n

        print()
```

Operations

```
n × n

= n²

O(n²)
```

---

## Case 3

```
for i=1 to n

    for j=1 to i

        print()
```

Operations

```
1

+2

+3

...

+n
```

Mathematically

```
n(n+1)/2

≈ n²/2

O(n²)
```

---

## Case 4

```
i=1

while(i<n)

    i*=2
```

Values

```
1

2

4

8

16

32

...

n
```

Suppose

```
2^k=n

k=log₂n

Time

O(log n)
```

---

## Case 5

```
for i=1 to n

    j=1

    while(j<n)

        j*=2
```

Outer Loop

```
n
```

Inner

```
log n
```

Total

```
n log n
```

---

# 9. Mathematical Analysis of Recursive Algorithms

Recursive algorithms are analyzed using **recurrence relations**.

General Form

```
T(n)=Work Done

+

Recursive Calls
```

Example

```
Factorial

T(n)

=

T(n-1)

+

1
```

---

## Fibonacci

```
T(n)

=

T(n-1)

+

T(n-2)

+

1
```

This grows exponentially.

Complexity

```
O(2ⁿ)
```

---

## Binary Search

```
T(n)

=

T(n/2)

+

1
```

Complexity

```
O(log n)
```

---

## Merge Sort

```
T(n)

=

2T(n/2)

+

n
```

Complexity

```
O(n log n)
```

---

## Quick Sort (Average)

```
T(n)

=

2T(n/2)

+

n

=

O(n log n)
```

Worst

```
O(n²)
```

---

# 10. Solving Recurrence Relations

---

## Method 1

### Iteration Method

Example

```
T(n)

=

T(n-1)

+

1
```

Expand

```
=T(n-2)+1+1

=T(n-3)+1+1+1

...

=T(1)+n

O(n)
```

---

## Method 2

### Substitution Method

Guess solution.

Then prove.

---

## Method 3

### Recursion Tree

Example

```
           n

       /       \

    n/2       n/2

   /   \      /   \

 n/4 n/4   n/4 n/4
```

Every level costs

```
n
```

Levels

```
log n
```

Total

```
n log n
```

---

## Method 4

### Master Theorem

For

```
T(n)

=

aT(n/b)

+

f(n)
```

Three cases.

---

### Case 1

```
f(n)

smaller

than

n^(log_ba)
```

Answer

```
Θ(n^(log_ba))
```

---

### Case 2

Equal

```
Θ(n^(log_ba) log n)
```

---

### Case 3

Larger

```
Θ(f(n))
```

---

Example

```
2T(n/2)+n

a=2

b=2

f(n)=n
```

Answer

```
Θ(n log n)
```

---

# 11. Complexity Analysis

## Time Complexity

Measures running time.

Common Orders

| Complexity | Growth |
|------------|---------|
| O(1) | Constant |
| O(log n) | Logarithmic |
| O(n) | Linear |
| O(n log n) | Linearithmic |
| O(n²) | Quadratic |
| O(n³) | Cubic |
| O(2ⁿ) | Exponential |
| O(n!) | Factorial |

---

## Space Complexity

Measures memory usage.

Recursive algorithms require

```
Extra Call Stack
```

Example

```
Factorial

Depth=n

Space

O(n)
```

Iterative version

```
O(1)
```

---

# 12. Implementation Examples

## Recursive Factorial

```python
def factorial(n):
    if n == 0:
        return 1
    return n * factorial(n - 1)
```

Time

```
O(n)
```

Space

```
O(n)
```

---

## Iterative Factorial

```python
def factorial(n):
    ans = 1

    for i in range(1, n + 1):
        ans *= i

    return ans
```

Time

```
O(n)
```

Space

```
O(1)
```

---

## Recursive Binary Search

```python
def binary_search(arr, left, right, target):
    if left > right:
        return -1

    mid = (left + right) // 2

    if arr[mid] == target:
        return mid

    if arr[mid] > target:
        return binary_search(arr, left, mid - 1, target)

    return binary_search(arr, mid + 1, right, target)
```

Complexity

```
Time = O(log n)

Space = O(log n)
```

---

## Iterative Binary Search

```python
def binary_search(arr, target):
    left = 0
    right = len(arr)-1

    while left <= right:
        mid = (left+right)//2

        if arr[mid] == target:
            return mid

        elif arr[mid] < target:
            left = mid + 1

        else:
            right = mid - 1

    return -1
```

Time

```
O(log n)
```

Space

```
O(1)
```

---

# 13. Visual Explanation

## Recursive Execution

Example

```
factorial(4)
```

Call Stack

```
factorial(4)

↓

factorial(3)

↓

factorial(2)

↓

factorial(1)

↓

factorial(0)

↓

Return 1

↓

Return 1

↓

Return 2

↓

Return 6

↓

Return 24
```

---

## Stack Visualization

```
Top

────────────

factorial(0)

factorial(1)

factorial(2)

factorial(3)

factorial(4)

────────────

Bottom
```

Then pops upward.

---

## Iterative Execution

```
result=1

↓

i=1

↓

result=1

↓

i=2

↓

result=2

↓

i=3

↓

result=6

↓

i=4

↓

result=24

↓

End
```

---

## Loop Flow

```
Start

↓

Initialize

↓

Condition

↓

Body

↓

Increment

↓

Condition

↓

...

↓

False

↓

Stop
```

---

# 14. Edge Cases

## Recursion Without Base Case

```python
def fun(n):
    return fun(n-1)
```

Produces

```
Infinite recursion

↓

Stack Overflow
```

---

## Incorrect Base Case

```
if n==2

instead of

if n==0
```

Produces wrong answers.

---

## Deep Recursion

Example

```
factorial(100000)
```

Most programming languages exceed recursion depth.

---

## Empty Input

```
Factorial(0)

Answer=1
```

---

## Binary Search Edge Cases

- Empty array
- One element
- Duplicate values
- Target not present
- First element
- Last element

---

## Loop Edge Cases

```
n=0

Loop never executes.
```

---

```
Negative n

May create infinite loop.
```

---

# 15. Limitations

## Recursive Algorithms

- Stack overflow
- More memory usage
- Function call overhead
- Harder debugging
- Slower for simple tasks

---

## Non-Recursive Algorithms

- Can become lengthy
- Difficult for recursive structures
- Manual stack management
- Reduced readability in tree problems

---

# 16. Comparison Table

| Feature | Recursive | Non-Recursive |
|----------|-----------|---------------|
| Uses Function Calls | Yes | No |
| Uses Loops | No | Yes |
| Stack Required | Yes | No |
| Memory Usage | Higher | Lower |
| Readability | Better for recursive problems | Better for iterative problems |
| Speed | Slightly slower | Usually faster |
| Risk of Stack Overflow | Yes | No |
| Easy for Trees | Excellent | Moderate |
| Easy for Arrays | Moderate | Excellent |
| Debugging | Harder | Easier |
| Time Complexity | Depends on recurrence | Depends on loops |
| Space Complexity | Usually Higher | Usually Lower |
| Examples | Merge Sort, DFS | BFS, Bubble Sort |

---

# 17. Exam Tips

## Remember

For Loops

```
One Loop

↓

O(n)
```

Nested Loops

```
O(n²)
```

Halving

```
O(log n)
```

Divide and Conquer

```
Usually

O(n log n)
```

---

## Common Recurrence Relations

| Recurrence | Complexity |
|------------|------------|
| T(n)=T(n−1)+1 | O(n) |
| T(n)=T(n−1)+n | O(n²) |
| T(n)=T(n/2)+1 | O(log n) |
| T(n)=2T(n/2)+n | O(n log n) |
| T(n)=2T(n−1)+1 | O(2ⁿ) |

---

# 18. Frequently Asked Questions

### Q1. Why does recursion use more memory?

Because every recursive call creates a new stack frame until the base case is reached.

---

### Q2. Can every recursive algorithm be converted into an iterative one?

Yes. By using loops and, when necessary, an explicit stack or queue.

---

### Q3. Which is faster?

Iterative solutions are generally faster because they avoid function call overhead.

---

### Q4. What is a recurrence relation?

A mathematical equation that expresses the running time of a recursive algorithm in terms of smaller input sizes.

---

### Q5. Why is Binary Search O(log n)?

Each step discards half of the remaining search space.

---

# 19. Quick Revision Sheet

## Recursive Algorithm

- Function calls itself
- Requires base case
- Uses call stack
- Easy for divide-and-conquer
- May cause stack overflow

---

## Non-Recursive Algorithm

- Uses loops
- No recursion
- Memory efficient
- Faster in most practical cases

---

## Complexity Summary

| Pattern | Complexity |
|---------|------------|
| Single Loop | O(n) |
| Nested Loops | O(n²) |
| Triple Loops | O(n³) |
| Halving Loop | O(log n) |
| Loop + Halving | O(n log n) |
| Binary Search | O(log n) |
| Merge Sort | O(n log n) |
| Quick Sort (Average) | O(n log n) |
| Quick Sort (Worst) | O(n²) |
| Recursive Factorial | O(n) |
| Fibonacci (Naive) | O(2ⁿ) |

---

## Formula Cheat Sheet

```text
1 + 2 + 3 + ... + n
= n(n+1)/2
≈ O(n²)

2^k = n
⇒ k = log₂n

Master Theorem

T(n) = aT(n/b) + f(n)

Case 1:
f(n) < n^(log_b a)
⇒ Θ(n^(log_b a))

Case 2:
f(n) = n^(log_b a)
⇒ Θ(n^(log_b a) log n)

Case 3:
f(n) > n^(log_b a)
⇒ Θ(f(n))
```

---

## One-Line Summary

> **Non-recursive algorithms are analyzed by counting loop iterations, while recursive algorithms are analyzed by forming and solving recurrence relations. Understanding these analyses enables comparison of algorithms, prediction of scalability, and selection of the most efficient solution for a given problem.**
