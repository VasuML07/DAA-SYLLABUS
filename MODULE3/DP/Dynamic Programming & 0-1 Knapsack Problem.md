# Dynamic Programming & 0/1 Knapsack Problem (Design and Analysis of Algorithms)

> **Engineering Exam Study Guide**
> Complete GitHub-compatible Markdown Notes

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why Dynamic Programming?](#2-why-dynamic-programming)
* [3. When to Use Dynamic Programming](#3-when-to-use-dynamic-programming)
* [4. Where Dynamic Programming is Applied](#4-where-dynamic-programming-is-applied)
* [5. Understanding the 0/1 Knapsack Problem](#5-understanding-the-01-knapsack-problem)
* [6. Step-by-Step Solution using Dynamic Programming](#6-step-by-step-solution-using-dynamic-programming)
* [7. Visual Explanation](#7-visual-explanation)
* [8. Algorithm](#8-algorithm)
* [9. Java Implementation](#9-java-implementation)
* [10. Complexity Analysis](#10-complexity-analysis)
* [11. Limitations and Edge Cases](#11-limitations-and-edge-cases)
* [12. 0/1 vs Unbounded Knapsack](#12-01-vs-unbounded-knapsack)
* [13. Advantages and Disadvantages](#13-advantages-and-disadvantages)
* [14. Exam Tips](#14-exam-tips)
* [15. Practice Questions](#15-practice-questions)
* [16. Summary](#16-summary)

---

# 1. Introduction

## What is Dynamic Programming?

Dynamic Programming (DP) is an algorithmic technique used to solve optimization and decision-making problems by dividing them into smaller overlapping subproblems and storing the results so they are solved only once.

Instead of repeatedly solving identical subproblems, DP remembers previously computed answers.

This significantly reduces computation time.

---

## Main Idea

```
Large Problem
      │
      ▼
Break into Smaller Problems
      │
      ▼
Store Solutions (Memoization/Table)
      │
      ▼
Reuse Stored Solutions
      │
      ▼
Optimal Final Answer
```

---

## What is the Knapsack Problem?

The Knapsack Problem is one of the most famous applications of Dynamic Programming.

Imagine:

You have a bag with limited capacity.

Several items are available.

Each item has

* Weight
* Value

Goal:

Select items that maximize total value without exceeding the bag capacity.

---

## Example

| Item | Weight | Value |
| ---- | ------ | ----- |
| A    | 1      | 1     |
| B    | 3      | 4     |
| C    | 4      | 5     |
| D    | 5      | 7     |

Knapsack Capacity = **7**

Question:

Which items should be selected to maximize value?

---

# 2. Why Dynamic Programming?

Suppose there are **n** items.

Each item has two choices:

* Take
* Don't take

Total possibilities:

```
2^n
```

This becomes extremely slow.

Example:

```
10 items  → 1024 subsets

20 items  → 1,048,576 subsets

30 items  → 1 Billion+ subsets
```

Clearly impossible for large inputs.

---

## Brute Force

```
Generate all subsets

Evaluate every subset

Choose best
```

Time Complexity

```
O(2^n)
```

Very slow.

---

## Simple Recursion

```
Knapsack(i,w)

├── Take Item
└── Skip Item
```

Problem:

Same states appear many times.

Example:

```
K(4,10)

├── K(3,10)
│
└── K(3,6)

Both later call

K(2,6)

Repeated computation
```

---

## Memoization

Store every computed state.

```
If already computed

↓

Return stored answer
```

This avoids recomputation.

---

## Bottom-Up DP

Instead of recursion,

fill a table.

```
Small Answers

↓

Larger Answers

↓

Final Answer
```

---

## Comparison

| Method       | Time  | Space |
| ------------ | ----- | ----- |
| Brute Force  | O(2ⁿ) | O(1)  |
| Recursion    | O(2ⁿ) | O(n)  |
| Memoization  | O(nW) | O(nW) |
| Bottom-Up DP | O(nW) | O(nW) |

Where

* n = number of items
* W = capacity

---

# 3. When to Use Dynamic Programming

DP is useful only if two important properties exist.

---

## 1. Optimal Substructure

An optimal solution contains optimal solutions to smaller subproblems.

Example:

Best solution using first 5 items depends on

Best solution using first 4 items.

---

Diagram

```
Best Solution

      │

      ▼

Best Smaller Solution

      │

      ▼

Best Even Smaller Solution
```

---

## 2. Overlapping Subproblems

Same subproblem appears repeatedly.

```
K(5,10)

├── K(4,10)

└── K(4,7)

       │

       ▼

     K(3,7)

appears again
```

DP stores

```
K(3,7)

only once.
```

---

If a problem has both

✔ Optimal Substructure

✔ Overlapping Subproblems

Then Dynamic Programming is likely the right approach.

---

# 4. Where Dynamic Programming is Applied

Dynamic Programming is used in many real-world optimization problems.

| Area            | Application            |
| --------------- | ---------------------- |
| Finance         | Budget optimization    |
| Manufacturing   | Resource allocation    |
| Logistics       | Cargo optimization     |
| Networking      | Routing                |
| Robotics        | Path planning          |
| AI              | Decision making        |
| Cloud Computing | Resource scheduling    |
| Genetics        | DNA sequence alignment |
| Gaming          | Strategy optimization  |
| Compiler Design | Optimization           |

---

Knapsack specifically appears in

* Budget allocation
* Cargo loading
* Memory allocation
* Investment selection
* Portfolio optimization
* Resource scheduling

---

# 5. Understanding the 0/1 Knapsack Problem

## Problem Statement

Given

```
N items

Each item has

Weight

Value
```

Capacity

```
W
```

Find maximum value.

Constraint

Each item can be chosen

```
0 times

or

1 time
```

Hence the name

```
0/1 Knapsack
```

---

## Example

```
Capacity = 7
```

Items

| Item | Weight | Value |
| ---- | ------ | ----- |
| 1    | 1      | 1     |
| 2    | 3      | 4     |
| 3    | 4      | 5     |
| 4    | 5      | 7     |

---

Possible selections

| Items | Weight | Value   |
| ----- | ------ | ------- |
| 1+2   | 4      | 5       |
| 2+3   | 7      | 9 ✅     |
| 1+4   | 6      | 8       |
| 3+4   | 9      | Invalid |

Best answer

```
Value = 9
```

---

# 6. Step-by-Step Solution using Dynamic Programming

---

## Step 1

Define DP table.

```
Rows

↓

Items

Columns

↓

Capacity
```

```
dp[i][w]

=

Maximum value

using first i items

within capacity w
```

---

## Step 2

Base Cases

```
No items

↓

Value = 0

No capacity

↓

Value = 0
```

```
dp[0][w]=0

dp[i][0]=0
```

---

## Step 3

Recurrence Relation

If item cannot fit

```
weight[i] > w

↓

Skip item

dp[i][w]=dp[i-1][w]
```

Otherwise

Choose maximum

```
Skip item

or

Take item
```

Formula

```
dp[i][w]

=

max(

dp[i-1][w],

value[i]

+

dp[i-1][w-weight[i]]

)
```

---

## Step 4

Fill table row by row.

```
Small capacities

↓

Large capacities

↓

Final Answer
```

---

# 7. Visual Explanation

## Example

Capacity

```
7
```

Weights

```
1 3 4 5
```

Values

```
1 4 5 7
```

---

## Initial Table

```
        Capacity

      0 1 2 3 4 5 6 7

Item0 0 0 0 0 0 0 0 0
```

---

### After Item 1

```
Weight=1 Value=1

      0 1 2 3 4 5 6 7

0     0 0 0 0 0 0 0 0

1     0 1 1 1 1 1 1 1
```

---

### After Item 2

```
Weight=3 Value=4

      0 1 2 3 4 5 6 7

0     0 0 0 0 0 0 0 0

1     0 1 1 1 1 1 1 1

2     0 1 1 4 5 5 5 5
```

---

### After Item 3

```
Weight=4 Value=5

      0 1 2 3 4 5 6 7

0     0 0 0 0 0 0 0 0

1     0 1 1 1 1 1 1 1

2     0 1 1 4 5 5 5 5

3     0 1 1 4 5 6 6 9
```

---

### After Item 4

```
Weight=5 Value=7

      0 1 2 3 4 5 6 7

0     0 0 0 0 0 0 0 0

1     0 1 1 1 1 1 1 1

2     0 1 1 4 5 5 5 5

3     0 1 1 4 5 6 6 9

4     0 1 1 4 5 7 8 9
```

Final Answer

```
9
```

---

## Cell Decision Example

Suppose

```
dp[3][7]
```

Item

```
Weight = 4

Value = 5
```

Decision

```
Skip

↓

dp[2][7]=5

Take

↓

5 + dp[2][3]

=

5+4

=

9

Choose

max(5,9)

=

9
```

---

## Traceback

Start

```
dp[4][7]=9
```

Compare

```
dp[4][7]

==

dp[3][7]

Yes

↓

Item4 NOT selected
```

Move upward

```
dp[3][7]=9

!=

dp[2][7]=5

↓

Item3 selected
```

Remaining capacity

```
7-4=3
```

Move

```
dp[2][3]=4

!=

dp[1][3]

↓

Item2 selected
```

Remaining capacity

```
0
```

Selected Items

```
Item2

Item3
```

Weight

```
3+4=7
```

Value

```
4+5=9
```

---

# 8. Algorithm

```
Create DP table

Initialize first row and column to zero

For every item

    For every capacity

        If item fits

            choose maximum

        Else

            copy above value

Answer = bottom-right cell

Trace backward

If current cell differs from above

    item selected

Else

    item skipped
```

---

# 9. Java Implementation

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class KnapsackDP {

    public static int knapsack(int[] weights, int[] values, int capacity) {

        int n = weights.length;

        // DP table
        int[][] dp = new int[n + 1][capacity + 1];

        // Build the DP table
        for (int i = 1; i <= n; i++) {
            for (int w = 0; w <= capacity; w++) {

                // Item cannot fit
                if (weights[i - 1] > w) {
                    dp[i][w] = dp[i - 1][w];
                } else {

                    // Skip the item
                    int skip = dp[i - 1][w];

                    // Take the item
                    int take = values[i - 1]
                            + dp[i - 1][w - weights[i - 1]];

                    dp[i][w] = Math.max(skip, take);
                }
            }
        }

        System.out.println("Maximum Value = " + dp[n][capacity]);

        // Traceback to find selected items
        List<Integer> selectedItems = new ArrayList<>();

        int w = capacity;

        for (int i = n; i > 0; i--) {

            // If value changed, this item was selected
            if (dp[i][w] != dp[i - 1][w]) {

                selectedItems.add(i);

                w -= weights[i - 1];
            }
        }

        Collections.reverse(selectedItems);

        System.out.println("Selected Items:");

        for (int item : selectedItems) {
            System.out.println(
                    "Item " + item +
                    " Weight=" + weights[item - 1] +
                    " Value=" + values[item - 1]
            );
        }

        return dp[n][capacity];
    }

    public static void main(String[] args) {

        int[] weights = {1, 3, 4, 5};

        int[] values = {1, 4, 5, 7};

        int capacity = 7;

        knapsack(weights, values, capacity);
    }
}
```

---

## Sample Output

```
Maximum Value = 9

Selected Items:

Item 2 Weight=3 Value=4

Item 3 Weight=4 Value=5
```

---

# 10. Complexity Analysis

| Operation             | Complexity |
| --------------------- | ---------- |
| DP Table Construction | O(nW)      |
| Traceback             | O(n)       |
| Space                 | O(nW)      |

Where

```
n

=

Number of items

W

=

Capacity
```

---

# 11. Limitations and Edge Cases

## Large Capacity

DP depends on

```
Capacity

not

input size only.
```

Example

```
Capacity = 1,000,000

DP table becomes enormous.
```

Memory

```
Rows = n

Columns = W

Huge memory usage
```

---

## Time Complexity

```
O(nW)
```

If

```
n=5000

W=100000

Very slow
```

---

## Zero Capacity

```
Capacity = 0
```

Result

```
Maximum Value = 0
```

DP Table

```
Capacity

0

Item0 0

Item1 0

Item2 0

Item3 0
```

---

## Empty Item List

```
No items
```

Answer

```
0
```

---

## All Items Too Heavy

Example

```
Capacity=2

Weights

5 6 7
```

Table

```
      0 1 2

0     0 0 0

1     0 0 0

2     0 0 0

3     0 0 0
```

Nothing fits.

---

## Integer Overflow

If values are extremely large

```
2,000,000,000

+

2,000,000,000
```

May exceed

```
Integer.MAX_VALUE
```

Solution

Use

```java
long
```

instead of

```java
int
```

when necessary.

---

## Duplicate Weights

Perfectly valid.

DP still works correctly.

---

## Space Optimization

Instead of

```
n × W
```

Store only one row.

```
Current Row

Previous Row
```

or

```
Single 1D Array
```

Space

```
O(W)
```

---

# 12. 0/1 vs Unbounded Knapsack

| Feature        | 0/1 Knapsack   | Unbounded Knapsack       |
| -------------- | -------------- | ------------------------ |
| Item selection | Once           | Unlimited                |
| Repeat allowed | No             | Yes                      |
| DP Relation    | Previous row   | Same row allowed         |
| Applications   | Laptop packing | Coin Change, Rod Cutting |

---

Illustration

### 0/1

```
Item A

Take

OR

Skip
```

Cannot take twice.

---

### Unbounded

```
Item A

↓

Take

↓

Take Again

↓

Take Again

↓

Unlimited
```

---

# 13. Advantages and Disadvantages

## Advantages

* Eliminates repeated calculations
* Guarantees optimal solution
* Faster than brute force
* Widely applicable
* Systematic table-based solution

---

## Disadvantages

* High memory usage
* Capacity-dependent complexity
* Not every problem satisfies DP properties
* Large DP tables may be impractical

---

# 14. Exam Tips

Remember these formulas.

Recurrence:

```
dp[i][w]

=

max(

dp[i-1][w],

value[i]

+

dp[i-1][w-weight[i]]

)
```

Base Cases

```
dp[0][w]=0

dp[i][0]=0
```

Complexity

```
Time

O(nW)

Space

O(nW)
```

---

# 15. Practice Questions

### Short Questions

1. Define Dynamic Programming.
2. What are overlapping subproblems?
3. What is optimal substructure?
4. Why is Knapsack solved using DP?
5. State the recurrence relation.

---

### Long Questions

1. Explain Dynamic Programming with suitable examples.
2. Explain the 0/1 Knapsack Problem using DP.
3. Construct the DP table for a given example.
4. Write Java code for the 0/1 Knapsack Problem.
5. Compare 0/1 and Unbounded Knapsack.
6. Explain traceback with an example.

---

# 16. Summary

Dynamic Programming is an optimization technique that solves problems by storing solutions to overlapping subproblems and reusing them.

The **0/1 Knapsack Problem** is a classic Dynamic Programming problem where each item may either be selected once or not selected at all. A bottom-up DP table stores the best achievable value for every combination of items and capacities. The final answer is found in the last cell of the table, and traceback identifies the selected items.

Key points to remember:

* DP is applicable when **optimal substructure** and **overlapping subproblems** exist.
* Recurrence:

  ```
  dp[i][w] = max(dp[i-1][w],
                 value[i] + dp[i-1][w-weight[i]])
  ```
* Base cases:

  ```
  dp[0][w] = 0
  dp[i][0] = 0
  ```
* Time Complexity:

  ```
  O(nW)
  ```
* Space Complexity:

  ```
  O(nW)
  ```
* Space can be optimized to:

  ```
  O(W)
  ```

This completes the Dynamic Programming and 0/1 Knapsack study guide for Design and Analysis of Algorithms.
