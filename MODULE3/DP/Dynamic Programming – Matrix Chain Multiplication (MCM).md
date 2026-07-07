# Dynamic Programming – Matrix Chain Multiplication (MCM) | Complete Engineering Semester Notes

> **Subject:** Design and Analysis of Algorithms (DAA)  
> **Topic:** Dynamic Programming – Matrix Chain Multiplication (MCM)  
> **Difficulty:** Engineering Semester Exam Level  
> **Language:** Java

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. What is Matrix Chain Multiplication?](#2-what-is-matrix-chain-multiplication)
- [3. Why Matrix Chain Multiplication Matters](#3-why-matrix-chain-multiplication-matters)
- [4. Why Dynamic Programming?](#4-why-dynamic-programming)
- [5. Applications](#5-applications)
- [6. Mathematical Formulation](#6-mathematical-formulation)
- [7. Optimal Substructure](#7-optimal-substructure)
- [8. Overlapping Subproblems](#8-overlapping-subproblems)
- [9. Dynamic Programming Approach](#9-dynamic-programming-approach)
- [10. Recurrence Relation](#10-recurrence-relation)
- [11. Step-by-Step DP Construction](#11-step-by-step-dp-construction)
- [12. Complete Worked Example](#12-complete-worked-example)
- [13. DP Table Visualization](#13-dp-table-visualization)
- [14. Parenthesization Table](#14-parenthesization-table)
- [15. Algorithm](#15-algorithm)
- [16. Java Implementation](#16-java-implementation)
- [17. Complexity Analysis](#17-complexity-analysis)
- [18. Edge Cases](#18-edge-cases)
- [19. Limitations](#19-limitations)
- [20. Advantages](#20-advantages)
- [21. Exam Tips](#21-exam-tips)
- [22. Frequently Asked Viva Questions](#22-frequently-asked-viva-questions)
- [23. Summary](#23-summary)

---

# 1. Introduction

Matrix Chain Multiplication (MCM) is one of the **most important Dynamic Programming problems**. The goal is **not to multiply matrices**, but to determine **the best order of multiplication** that minimizes the total number of scalar multiplications.

The matrices themselves cannot be reordered because matrix multiplication is **associative** but **not commutative**.

Example:

```
A × B × C

Possible:

(A × B) × C
A × (B × C)

Both produce the same result,
but the computational cost may differ significantly.
```

---

# 2. What is Matrix Chain Multiplication?

Suppose we have matrices:

```
A1, A2, A3, ... , An
```

Each matrix has compatible dimensions.

Example

```
A1 = 10 × 30
A2 = 30 × 5
A3 = 5 × 60
```

Question:

> In which order should we multiply these matrices to minimize computation?

---

# 3. Why Matrix Chain Multiplication Matters

Large-scale software performs millions of matrix multiplications.

Examples include:

- Machine Learning
- Artificial Intelligence
- Graphics Rendering
- Image Processing
- Robotics
- Scientific Computing
- Big Data Analytics

Finding an efficient multiplication order can reduce computation time dramatically.

---

# 4. Why Dynamic Programming?

A brute-force solution tries **every possible parenthesization**.

Number of parenthesizations grows according to the **Catalan Numbers**.

Example

| Number of Matrices | Parenthesizations |
|-------------------|------------------|
| 3 | 2 |
| 4 | 5 |
| 5 | 14 |
| 6 | 42 |
| 10 | 4862 |

Clearly impossible for large N.

Dynamic Programming works because:

- Optimal Substructure
- Overlapping Subproblems

---

# 5. Applications

Matrix Chain Multiplication is used in:

- Query optimization in DBMS
- Compiler optimization
- Computer graphics
- Machine learning
- Neural network optimization
- Scientific simulations
- Linear algebra libraries
- Robotics

---

# 6. Mathematical Formulation

Suppose dimensions are stored as

```
p = [p0, p1, p2, ... pn]
```

Then

```
Matrix A1 = p0 × p1
Matrix A2 = p1 × p2
Matrix A3 = p2 × p3
```

Example

```
p = [10,30,5,60]

A1 = 10×30
A2 = 30×5
A3 = 5×60
```

---

# 7. Optimal Substructure

Suppose the best solution is

```
(A1 A2)(A3 A4)
```

Then

```
(A1 A2)

must itself be optimal.

Similarly

(A3 A4)

must also be optimal.
```

Visual

```
Whole Problem

          A1 A2 A3 A4
               |
     -----------------------
     |                     |
 A1 A2                 A3 A4
     |                     |
 Optimal               Optimal
```

---

# 8. Overlapping Subproblems

While solving

```
A1...A6
```

We repeatedly compute

```
A2...A4

A3...A5

A2...A5
```

Instead of recomputing, DP stores answers.

---

# 9. Dynamic Programming Approach

We create two tables.

```
Cost Table (m)

Stores minimum multiplication cost.

Split Table (s)

Stores where to split.
```

---

# 10. Recurrence Relation

For matrices i through j

```
m[i][j] =
minimum over all k

(
m[i][k]
+
m[k+1][j]
+
p[i-1] × p[k] × p[j]
)
```

Visual

```
Matrices

Ai ...... Ak | Ak+1 ...... Aj
        Split Here

Cost

Left Cost
+
Right Cost
+
Final Multiplication
```

---

# 11. Step-by-Step DP Construction

Example

```
Dimensions

p = [30,35,15,5,10,20,25]
```

Matrices

```
A1 = 30×35
A2 = 35×15
A3 = 15×5
A4 = 5×10
A5 = 10×20
A6 = 20×25
```

---

### Step 1

Length = 1

```
Every single matrix

Cost = 0
```

---

### Step 2

Length = 2

Compute

```
A1 A2

A2 A3

A3 A4

...
```

---

### Step 3

Length = 3

Compute

```
A1 A2 A3

A2 A3 A4

...
```

Continue until

```
Length = n
```

---

# 12. Complete Worked Example

Example

```
p = [10,30,5,60]
```

Matrices

```
A1 = 10×30

A2 = 30×5

A3 = 5×60
```

---

## Option 1

```
(A1 A2) A3
```

Cost

```
A1×A2

10×30×5

=1500

Result matrix

10×5

Now

10×5×60

=3000

Total

1500+3000

=4500
```

---

## Option 2

```
A1 (A2 A3)
```

Cost

```
30×5×60

=9000

Result

30×60

Now

10×30×60

=18000

Total

27000
```

---

Minimum

```
4500
```

Optimal Parenthesization

```
(A1A2)A3
```

---

# 13. DP Table Visualization

Cost Table

```
       1      2      3

1      0    1500   4500

2             0    9000

3                    0
```

Meaning

```
dp[1][3]

Minimum cost

4500
```

---

# 14. Parenthesization Table

```
       1    2    3

1      -    1    2

2           -    2

3                -
```

Interpretation

```
Split A1...A3

After Matrix 2

(A1A2)(A3)
```

---

# 15. Algorithm

```
Initialize DP table

Diagonal = 0

For chain length =2 to n

    For every possible i

        j=i+length-1

        Initialize infinity

        Try every split k

            Compute cost

            Update minimum

Return dp[1][n]
```

---

### Flow Diagram

```
Start

↓

Initialize DP

↓

Length = 2

↓

Try Every Split

↓

Store Minimum

↓

Increase Length

↓

Done

↓

Answer
```

---

# 16. Java Implementation

```java
import java.util.Arrays;

public class MatrixChainMultiplication {

    /**
     * Computes the minimum number of scalar multiplications required
     * to multiply a chain of matrices.
     *
     * @param dimensions Array of dimensions where matrix Ai has size
     *                   dimensions[i-1] x dimensions[i]
     * @return Minimum multiplication cost
     */
    public static int matrixChainOrder(int[] dimensions) {

        if (dimensions == null || dimensions.length < 2) {
            return 0;
        }

        int n = dimensions.length - 1;

        if (n == 1) {
            return 0;
        }

        int[][] dp = new int[n][n];

        for (int[] row : dp) {
            Arrays.fill(row, 0);
        }

        // chainLength = number of matrices in current chain
        for (int chainLength = 2; chainLength <= n; chainLength++) {

            for (int i = 0; i <= n - chainLength; i++) {

                int j = i + chainLength - 1;

                dp[i][j] = Integer.MAX_VALUE;

                for (int k = i; k < j; k++) {

                    long cost =
                            (long) dp[i][k]
                          + dp[k + 1][j]
                          + (long) dimensions[i]
                          * dimensions[k + 1]
                          * dimensions[j + 1];

                    if (cost < dp[i][j]) {
                        dp[i][j] = (int) cost;
                    }
                }
            }
        }

        return dp[0][n - 1];
    }

    public static void main(String[] args) {

        int[] dimensions = {30, 35, 15, 5, 10, 20, 25};

        int minimumCost = matrixChainOrder(dimensions);

        System.out.println("Minimum Cost = " + minimumCost);
    }
}
```

Output

```
Minimum Cost = 15125
```

---

## Printing the Optimal Parenthesization (Optional Extension)

To reconstruct the optimal multiplication order, maintain a second table `split[i][j]`:

- Whenever a lower cost is found, store the corresponding split index `k`.
- After filling the DP table, recursively print:
  - `Ai` if `i == j`
  - Otherwise print:
    ```
    (
      left-subchain
      right-subchain
    )
    ```
    using the stored split.

This reconstruction takes **O(n)** time after the DP tables are built.

---

# 17. Complexity Analysis

| Operation | Complexity |
|------------|------------|
| DP Table Construction | O(n³) |
| Space | O(n²) |
| Parenthesization Recovery | O(n) |

---

### Why O(n³)?

Three nested loops

```
Length

↓

Start Matrix

↓

Split Position
```

---

# 18. Edge Cases

| Case | Example | Output |
|-------|----------|--------|
| Single Matrix | 10×20 | 0 |
| Two Matrices | 10×20,20×30 | 6000 |
| Three Matrices | Standard | Compare both orders |
| Very Large Dimensions | 10000×10000 | May require `long` to avoid overflow |
| Null Array | null | 0 |
| Invalid Dimensions | length < 2 | 0 |

Example

```
Only one matrix

A1

No multiplication

Cost = 0
```

---

# 19. Limitations

| Limitation | Explanation |
|------------|-------------|
| O(n³) Time | Slow for very large chains |
| O(n²) Memory | Large memory consumption |
| Does Not Multiply Matrices | Finds only the optimal order |
| Assumes Compatible Dimensions | Input must represent a valid chain |
| Integer Overflow | Use `long` for extremely large dimensions |

---

# 20. Advantages

- Guarantees the optimal solution.
- Eliminates repeated computation.
- Much faster than brute force.
- Classical Dynamic Programming problem.
- Widely used in optimization.

---

# 21. Exam Tips

### Remember the recurrence

```
m[i][j]

=

minimum

m[i][k]

+

m[k+1][j]

+

p[i-1]×p[k]×p[j]
```

---

### Steps to solve manually

1. Draw the DP table.
2. Fill the diagonal with `0`.
3. Process chains of length `2`.
4. Continue increasing chain length.
5. Evaluate every split `k`.
6. Record the minimum cost.
7. Use the split table to recover the parenthesization.

---

### Common Mistakes

- Confusing the number of matrices with the number of dimensions.
- Forgetting that matrix multiplication is associative, not commutative.
- Using incorrect indices in the recurrence.
- Omitting the scalar multiplication cost `p[i-1] * p[k] * p[j]`.
- Not handling the single-matrix case.

---

# 22. Frequently Asked Viva Questions

### Q1. What is Matrix Chain Multiplication?

It is the problem of determining the most efficient order to multiply a sequence of matrices while preserving the original order.

---

### Q2. Does MCM multiply matrices?

No. It computes the minimum scalar multiplication cost and the optimal parenthesization.

---

### Q3. Why is Dynamic Programming suitable?

Because the problem has **optimal substructure** and **overlapping subproblems**.

---

### Q4. What is stored in the DP table?

`dp[i][j]` stores the minimum cost to multiply matrices `Ai` through `Aj`.

---

### Q5. Why is the diagonal initialized to zero?

A single matrix requires no multiplication.

---

### Q6. Time Complexity?

```
O(n³)
```

---

### Q7. Space Complexity?

```
O(n²)
```

---

### Q8. What additional table is often maintained?

A **split (parenthesization)** table that records the best split index for reconstructing the optimal multiplication order.

---

# 23. Summary

- Matrix Chain Multiplication is a classic Dynamic Programming optimization problem.
- The objective is to **minimize scalar multiplication cost**, not to change the matrix order.
- Dynamic Programming is effective because of **optimal substructure** and **overlapping subproblems**.
- The DP solution constructs a cost table in increasing chain lengths using the recurrence:
  ```
  dp[i][j] = min(
      dp[i][k] +
      dp[k+1][j] +
      p[i-1] * p[k] * p[j]
  )
  ```
- The algorithm runs in **O(n³)** time and uses **O(n²)** space.
- A second split table enables reconstruction of the optimal parenthesization.
- MCM is a foundational algorithm with applications in compilers, databases, scientific computing, graphics, and machine learning.

---

## One-Line Exam Definition

> **Matrix Chain Multiplication is a Dynamic Programming algorithm that determines the optimal parenthesization of a chain of compatible matrices to minimize the total number of scalar multiplications without changing the order of the matrices.**
