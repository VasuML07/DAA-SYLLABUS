# Longest Common Subsequence (LCS) – Complete Dynamic Programming Study Guide (Engineering Semester Exam)

> **Course:** Design and Analysis of Algorithms (DAA)
> **Topic:** Dynamic Programming – Longest Common Subsequence (LCS)

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why LCS is Used](#2-why-lcs-is-used)
* [3. When to Use LCS](#3-when-to-use-lcs)
* [4. Where LCS is Applied](#4-where-lcs-is-applied)
* [5. Dynamic Programming Behind LCS](#5-dynamic-programming-behind-lcs)
* [6. How to Find the LCS](#6-how-to-find-the-lcs)
* [7. LCS Algorithm Explanation](#7-lcs-algorithm-explanation)
* [8. Recurrence Relation](#8-recurrence-relation)
* [9. DP Table Construction](#9-dp-table-construction)
* [10. Step-by-Step Example](#10-step-by-step-example)
* [11. Traceback to Find the LCS](#11-traceback-to-find-the-lcs)
* [12. Complete Java Implementation](#12-complete-java-implementation)
* [13. Complexity Analysis](#13-complexity-analysis)
* [14. Limitations and Edge Cases](#14-limitations-and-edge-cases)
* [15. Applications](#15-applications)
* [16. Advantages and Disadvantages](#16-advantages-and-disadvantages)
* [17. Exam Tips](#17-exam-tips)
* [18. Frequently Asked Viva Questions](#18-frequently-asked-viva-questions)
* [19. Summary](#19-summary)

---

# 1. Introduction

The **Longest Common Subsequence (LCS)** is one of the most important Dynamic Programming problems.

Given two sequences (usually strings), the objective is to find the **longest sequence that appears in both strings in the same relative order**.

The characters **do not have to be adjacent**, but their order must remain the same.

Example:

```
String 1 = ABCDGH
String 2 = AEDFHR

LCS = ADH
Length = 3
```

Notice:

```
ABCDGH
| |  |
A D  H

Characters remain in order.
```

---

# 2. Why LCS is Used

Many real-world systems need to compare two sequences.

Examples:

* Comparing two documents
* Comparing DNA sequences
* Finding similarities in source code
* Detecting plagiarism
* Version control systems
* Bioinformatics

Without Dynamic Programming, the number of possible subsequences grows exponentially.

Dynamic Programming reduces repeated work and provides an optimal solution efficiently.

---

# 3. When to Use LCS

Use LCS when:

* Order matters
* Elements may be skipped
* Matching characters need not be adjacent
* Maximum similarity is required

Use LCS for problems involving:

* String comparison
* Sequence comparison
* Similarity measurement
* Edit operations
* File comparison

---

# 4. Where LCS is Applied

## DNA Sequence Matching

```
DNA 1
A T G C T A

DNA 2
A G C A

LCS identifies common genes.
```

---

## Git Version Control

Old File

```
Hello World
Welcome User
Good Morning
```

New File

```
Hello World
Welcome Student
Good Morning
```

Git internally uses sequence comparison techniques related to LCS.

---

## Plagiarism Detection

Original

```
Algorithms are important for computer science.
```

Copied

```
Algorithms remain important in computer science.
```

LCS measures similarity.

---

## Spell Checking

Finding similarities between words.

Example

```
INTENTION
EXECUTION
```

---

## Bioinformatics

Comparing:

* DNA
* RNA
* Protein sequences

---

## Text Editors

Programs like:

* VS Code
* IntelliJ
* Eclipse

use sequence comparison for diff operations.

---

# 5. Dynamic Programming Behind LCS

The problem has:

## Optimal Substructure

Optimal solution depends on optimal solutions of smaller subproblems.

Example

```
LCS("ABC","ABD")

↓

LCS("AB","AB")
```

---

## Overlapping Subproblems

The same smaller LCS problems are computed repeatedly.

```
LCS(ABC,ABD)

↓

LCS(AB,AB)

↓

LCS(A,A)

↓

Computed many times
```

Dynamic Programming stores answers.

---

# 6. How to Find the LCS

Suppose

```
X = ABCBDAB
Y = BDCABA
```

Create a DP table.

Rows:

```
X
```

Columns:

```
Y
```

Add one extra row and column containing zeros.

```
      B D C A B A
    0 0 0 0 0 0 0
A
B
C
B
D
A
B
```

Each cell stores:

```
Length of LCS up to that point.
```

---

# 7. LCS Algorithm Explanation

For every character:

```
Compare X[i-1]
and
Y[j-1]
```

Two cases exist.

---

## Case 1

Characters match.

```
A == A
```

Then

```
DP[i][j]

=

DP[i-1][j-1] + 1
```

Visual

```
Diagonal

↖

+1
```

---

## Case 2

Characters do not match.

```
A != C
```

Take maximum.

```
max(

Top,

Left

)
```

Visual

```
      ↑

← Cell

Take maximum
```

---

# 8. Recurrence Relation

If

```
X[i-1] == Y[j-1]
```

Then

```
DP[i][j]
=
DP[i-1][j-1]+1
```

Otherwise

```
DP[i][j]
=
max(

DP[i-1][j],

DP[i][j-1]

)
```

ASCII Representation

```
Match

↖ +1

Mismatch

↑
←

Take maximum
```

---

# 9. DP Table Construction

Example

```
X = ABC

Y = AC
```

Initial table

|   |   | A | C |
| - | - | - | - |
| 0 | 0 | 0 | 0 |
| A | 0 |   |   |
| B | 0 |   |   |
| C | 0 |   |   |

---

## Fill Cell (A,A)

Characters equal

```
1
```

|   | 0 | A | C |
| - | - | - | - |
| 0 | 0 | 0 | 0 |
| A | 0 | 1 | 1 |
| B | 0 | 1 | 1 |
| C | 0 | 1 | 2 |

---

Final Table

|   | 0 | A | C |
| - | - | - | - |
| 0 | 0 | 0 | 0 |
| A | 0 | 1 | 1 |
| B | 0 | 1 | 1 |
| C | 0 | 1 | 2 |

Bottom-right value

```
2
```

Length of LCS.

---

# 10. Step-by-Step Example

Example

```
X = ABCD

Y = ACBD
```

---

## Step 1

Compare

```
A

A
```

Match

```
1
```

---

## Step 2

Compare

```
A

C
```

No match

Take

```
max(left,top)
```

---

## Step 3

Continue

Eventually

|   | 0 | A | C | B | D |
| - | - | - | - | - | - |
| 0 | 0 | 0 | 0 | 0 | 0 |
| A | 0 | 1 | 1 | 1 | 1 |
| B | 0 | 1 | 1 | 2 | 2 |
| C | 0 | 1 | 2 | 2 | 2 |
| D | 0 | 1 | 2 | 2 | 3 |

Final answer

```
3
```

---

# 11. Traceback to Find the LCS

Finding the length is not enough.

We reconstruct the sequence.

Start

```
Bottom-right
```

Move:

```
Diagonal

if match

↑

if top larger

←

if left larger
```

Visual

```
          End

      *

← ← *

↑

*

↖

Match

Store character
```

Example

```
ABCD

ACBD

↓

Trace

D

↓

B

↓

A

Reverse

ABD
```

---

# 12. Complete Java Implementation

```java
public class LongestCommonSubsequence {

    // Method to compute and print the LCS
    public static String findLCS(String s1, String s2) {

        int m = s1.length();
        int n = s2.length();

        // DP table
        int[][] dp = new int[m + 1][n + 1];

        // Build DP table
        for (int i = 1; i <= m; i++) {

            for (int j = 1; j <= n; j++) {

                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {

                    dp[i][j] = dp[i - 1][j - 1] + 1;

                } else {

                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);

                }
            }
        }

        // Traceback to reconstruct the LCS
        StringBuilder lcs = new StringBuilder();

        int i = m;
        int j = n;

        while (i > 0 && j > 0) {

            if (s1.charAt(i - 1) == s2.charAt(j - 1)) {

                lcs.append(s1.charAt(i - 1));

                i--;
                j--;

            } else if (dp[i - 1][j] > dp[i][j - 1]) {

                i--;

            } else {

                j--;
            }
        }

        return lcs.reverse().toString();
    }

    public static void main(String[] args) {

        String s1 = "ABCBDAB";
        String s2 = "BDCABA";

        String answer = findLCS(s1, s2);

        System.out.println("Longest Common Subsequence : " + answer);
        System.out.println("Length : " + answer.length());
    }
}
```

---

# 13. Complexity Analysis

| Operation             | Complexity |
| --------------------- | ---------- |
| DP Table Construction | O(m × n)   |
| Traceback             | O(m+n)     |
| Total Time            | O(m × n)   |
| Space                 | O(m × n)   |

Where

```
m = length of String 1

n = length of String 2
```

---

## Visual Complexity

```
Strings

100 × 100

↓

10,000 Cells

Fill each once

↓

O(mn)
```

---

# 14. Limitations and Edge Cases

## Limitation 1

Large memory usage.

```
10000 × 10000

↓

100 Million cells
```

Consumes significant memory.

---

## Limitation 2

Not suitable for extremely long sequences without optimization.

---

## Limitation 3

Only compares order.

It ignores:

* Meaning
* Context
* Synonyms

---

# Edge Cases

## Empty String

```
A = ""

B = ABC
```

Result

```
Length = 0
```

Visualization

```
""


ABC

↓

No subsequence
```

---

## Identical Strings

```
HELLO

HELLO
```

Result

```
HELLO
```

DP Table

Diagonal increases continuously.

```
1

2

3

4

5
```

---

## No Common Characters

```
ABC

XYZ
```

Result

```
Length = 0
```

Table

```
0 0 0

0 0 0

0 0 0
```

---

## Single Character Match

```
A

A
```

Answer

```
A
```

---

## Repeated Characters

```
AAAA

AA
```

LCS

```
AA
```

The algorithm correctly handles duplicates while preserving order.

---

# 15. Applications

| Application                 | Purpose                   |
| --------------------------- | ------------------------- |
| DNA Matching                | Compare genes             |
| Version Control             | File difference detection |
| Plagiarism Detection        | Measure similarity        |
| Bioinformatics              | Protein analysis          |
| Spell Checker               | Word comparison           |
| Text Editors                | Document comparison       |
| Data Synchronization        | Detect common content     |
| Natural Language Processing | Sentence similarity       |

---

# 16. Advantages and Disadvantages

## Advantages

* Optimal solution
* Eliminates repeated computation
* Uses Dynamic Programming efficiently
* Widely used in industry
* Easy to implement
* Guaranteed correct answer

---

## Disadvantages

* High memory consumption
* Slower for very large inputs
* Requires O(m × n) table
* Cannot understand semantic similarity

---

# 17. Exam Tips

Remember these points:

### Definition

> Longest sequence appearing in both strings while preserving order.

---

### Recurrence

```
Match

DP[i][j]
=
DP[i-1][j-1]+1

Else

max(top,left)
```

---

### Complexity

```
Time

O(mn)

Space

O(mn)
```

---

### Traceback

```
Match

Diagonal

No Match

Move toward larger value

Reverse collected characters
```

---

### Common Mistake

Do **not** confuse:

```
Substring

↓

Continuous

Subsequence

↓

Need NOT be continuous
```

Example

```
ABCDE

ACE

Valid subsequence

Not substring
```

---

# 18. Frequently Asked Viva Questions

### Q1. What is a subsequence?

A sequence obtained by deleting zero or more characters without changing the order of the remaining characters.

---

### Q2. Is every substring a subsequence?

Yes.

---

### Q3. Is every subsequence a substring?

No.

---

### Q4. Why use Dynamic Programming?

Because LCS has:

* Optimal Substructure
* Overlapping Subproblems

---

### Q5. Which value gives the answer?

```
DP[m][n]
```

---

### Q6. Can multiple LCSs exist?

Yes. Different subsequences may have the same maximum length.

Example:

```
String 1: ABCBDAB
String 2: BDCABA

Possible LCSs:
BCBA
BDAB
```

Both have the same maximum length.

---

# 19. Summary

```
Problem

↓

Find longest common subsequence

↓

Dynamic Programming

↓

Build DP Table

↓

Use recurrence

↓

Trace back

↓

Reverse result

↓

Output LCS
```

## Quick Revision Sheet

| Topic                 | Key Point                                                         |
| --------------------- | ----------------------------------------------------------------- |
| Technique             | Dynamic Programming                                               |
| Problem Type          | Sequence Comparison                                               |
| Recurrence (Match)    | DP[i−1][j−1] + 1                                                  |
| Recurrence (Mismatch) | max(DP[i−1][j], DP[i][j−1])                                       |
| Time Complexity       | O(m × n)                                                          |
| Space Complexity      | O(m × n)                                                          |
| Answer Location       | DP[m][n]                                                          |
| Traceback             | Diagonal on match; otherwise move to the larger neighbor          |
| Common Applications   | DNA analysis, Git diff, plagiarism detection, spell checking, NLP |
| Common Confusion      | Subsequence ≠ Substring                                           |

---

**End of Study Guide**
