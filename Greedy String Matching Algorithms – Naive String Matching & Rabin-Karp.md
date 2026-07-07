# Greedy String Matching Algorithms – Naive String Matching & Rabin-Karp
### Design and Analysis of Algorithms (DAA) – Engineering Semester Exam Notes

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. Why These Algorithms Matter](#2-why-these-algorithms-matter)
- [3. When to Use Them](#3-when-to-use-them)
- [4. Where They're Applied](#4-where-theyre-applied)
- [5. How to Use Them](#5-how-to-use-them)
- [6. Naive String Matching Algorithm](#6-naive-string-matching-algorithm)
  - [Concept](#concept)
  - [Working Process](#working-process)
  - [Visual Example](#visual-example)
  - [Algorithm](#algorithm)
  - [Correctness](#correctness)
  - [Complexity Analysis](#complexity-analysis)
  - [Java Implementation](#java-implementation)
- [7. Rabin-Karp Algorithm](#7-rabin-karp-algorithm)
  - [Concept](#concept-1)
  - [Rolling Hash](#rolling-hash)
  - [Visual Example](#visual-example-1)
  - [Algorithm](#algorithm-1)
  - [Correctness](#correctness-1)
  - [Complexity Analysis](#complexity-analysis-1)
  - [Java Implementation](#java-implementation-1)
- [8. Limitations](#8-limitations)
- [9. Edge Cases](#9-edge-cases)
- [10. Comparison Table](#10-comparison-table)
- [11. Conclusion & Key Takeaways](#11-conclusion--key-takeaways)

---

# 1. Introduction

String Matching (Pattern Matching) is the process of finding one string (called the **pattern**) inside another string (called the **text**).

Example:

```
Text    : ABABDABACDABABCABAB
Pattern : ABABCABAB
```

The goal is to determine whether the pattern exists and, if so, at which positions.

Greedy string matching algorithms attempt to find the pattern efficiently by examining possible alignments.

The two fundamental algorithms are:

1. Naive String Matching
2. Rabin-Karp Algorithm

These algorithms form the foundation for understanding more advanced algorithms such as KMP and Boyer-Moore.

---

# 2. Why These Algorithms Matter

String searching is one of the most common operations in Computer Science.

Applications include:

| Application | Example |
|------------|----------|
| Search Engines | Searching keywords |
| Text Editors | Ctrl + F |
| DNA Matching | Genome sequence analysis |
| Cyber Security | Virus signature detection |
| Compiler Design | Token recognition |
| Data Compression | Pattern identification |
| Plagiarism Detection | Similar document matching |
| Log Analysis | Finding error messages |

---

# 3. When to Use Them

## Use Naive String Matching when

- Input size is small
- Simplicity is important
- Learning purposes
- One-time searching
- No preprocessing required

---

## Use Rabin-Karp when

- Multiple pattern searching
- Large text files
- Hashing already exists
- Need faster average performance
- Searching many patterns simultaneously

---

# 4. Where They're Applied

```
Text Editor
     │
     ▼
Find Button (Ctrl + F)
     │
     ▼
Pattern Matching Algorithm
     │
     ▼
Highlight Matches
```

---

Another example

```
DNA Database

Genome
ATCGGATCGATCG

Pattern
ATCG

↓

Pattern Matching

↓

Found Positions
0
5
9
```

---

# 5. How to Use Them

Suppose

```
Text = ABCABCABC

Pattern = ABC
```

General steps

```
Start

↓

Take first window

↓

Compare

↓

Match?

 ├── Yes → Report Position
 │
 └── No → Shift Window

↓

Repeat

↓

End
```

Naive shifts by **one character** every time.

Rabin-Karp shifts by **one character but compares hashes first**.

---

# 6. Naive String Matching Algorithm

## Concept

Naive algorithm compares the pattern with every possible substring of equal length.

It performs direct character-by-character comparison.

---

## Working Process

Example

```
Text

A B A A B A B A A B
0 1 2 3 4 5 6 7 8 9

Pattern

A B A
```

---

## Visual Example

### Window 1

```
Text

A B A A B A B A A B
↑ ↑ ↑

Pattern

A B A
```

Match ✔

---

### Window 2

```
A B A A B A B A A B
  ↑ ↑ ↑

Pattern

A B A
```

Mismatch ✘

---

### Window 3

```
A B A A B A B A A B
    ↑ ↑ ↑

Pattern

A B A
```

Mismatch ✘

---

### Window 4

```
A B A A B A B A A B
      ↑ ↑ ↑

Pattern

A B A
```

Match ✔

---

## Sliding Window

```
Window 1

ABCDEF

[ABC]

↓

Window 2

A[BCD]

↓

Window 3

AB[CDE]

↓

Window 4

ABC[DEF]
```

Moves only one character every iteration.

---

## Algorithm

```
For every position i

Compare Pattern

↓

All Characters Match?

├── Yes → Pattern Found

└── No → Shift by 1

↓

Repeat
```

---

## Correctness

The algorithm checks every possible alignment.

Since every possible starting position is examined, no occurrence can be missed.

Therefore, the algorithm is correct.

---

## Complexity Analysis

Let

```
n = Text Length

m = Pattern Length
```

### Best Case

```
O(n)
```

Mismatch occurs immediately.

---

### Worst Case

```
O(n × m)
```

Example

```
Text

AAAAAAAAAA

Pattern

AAAAAB
```

Every shift compares almost every character.

---

### Space Complexity

```
O(1)
```

---

## Java Implementation

```java
public class NaiveStringMatching {

    public static void search(String text, String pattern) {

        int n = text.length();
        int m = pattern.length();

        if (m == 0) {
            System.out.println("Empty pattern.");
            return;
        }

        if (m > n) {
            System.out.println("Pattern longer than text.");
            return;
        }

        for (int i = 0; i <= n - m; i++) {

            int j;

            for (j = 0; j < m; j++) {

                if (text.charAt(i + j) != pattern.charAt(j))
                    break;

            }

            if (j == m)
                System.out.println("Pattern found at index " + i);
        }
    }

    public static void main(String[] args) {

        String text = "ABABDABACDABABCABAB";
        String pattern = "ABABCABAB";

        search(text, pattern);
    }
}
```

### Time Complexity

```
Best      : O(n)

Worst     : O(n × m)

Space     : O(1)
```

---

# 7. Rabin-Karp Algorithm

## Concept

Rabin-Karp improves searching using **hash values**.

Instead of comparing every character immediately,

It compares

```
Hash(Text Window)

with

Hash(Pattern)
```

Only if both hashes are equal are characters compared.

---

## Rolling Hash

Instead of recalculating every window,

It updates

```
Old Hash

↓

Remove Left Character

↓

Multiply

↓

Add Right Character

↓

New Hash
```

This is called **Rolling Hash**.

---

## Visual Example

Text

```
A B C D A B C D
```

Pattern

```
A B C
```

Windows

```
ABC

BCD

CDA

DAB

ABC

BCD
```

Each window has its own hash.

---

### Hash Comparison

```
Pattern

ABC

Hash = 57

↓

Window

ABC

Hash = 57

↓

Equal

↓

Compare Characters

↓

Pattern Found
```

---

### Collision Example

```
Hash(Pattern)

↓

57

Window Hash

↓

57

↓

Characters

ABC

ABD

↓

Different

↓

False Match
```

This is called **Hash Collision**.

---

## Algorithm

```
Compute Pattern Hash

↓

Compute First Window Hash

↓

Hashes Equal?

├── Yes
│      Compare Characters
│
└── No
       Skip Comparison

↓

Compute Next Window Hash

↓

Repeat
```

---

## Correctness

The algorithm checks every possible window.

Hash comparison filters unnecessary comparisons.

Character verification removes false matches caused by collisions.

Thus, all true matches are detected correctly.

---

## Complexity Analysis

### Best Case

```
O(n)
```

---

### Average Case

```
O(n + m)
```

Very efficient.

---

### Worst Case

```
O(n × m)
```

Occurs when every window has identical hash.

---

### Space Complexity

```
O(1)
```

---

## Java Implementation

```java
public class RabinKarp {

    static final int d = 256;
    static final int q = 101;

    static void search(String pattern, String text) {

        int m = pattern.length();
        int n = text.length();

        if (m == 0) {
            System.out.println("Empty pattern.");
            return;
        }

        if (m > n) {
            System.out.println("Pattern longer than text.");
            return;
        }

        int p = 0;
        int t = 0;
        int h = 1;

        for (int i = 0; i < m - 1; i++)
            h = (h * d) % q;

        for (int i = 0; i < m; i++) {

            p = (d * p + pattern.charAt(i)) % q;
            t = (d * t + text.charAt(i)) % q;

        }

        for (int i = 0; i <= n - m; i++) {

            if (p == t) {

                int j;

                for (j = 0; j < m; j++) {

                    if (text.charAt(i + j) != pattern.charAt(j))
                        break;

                }

                if (j == m)
                    System.out.println("Pattern found at index " + i);

            }

            if (i < n - m) {

                t = (d * (t - text.charAt(i) * h)
                        + text.charAt(i + m)) % q;

                if (t < 0)
                    t += q;
            }
        }
    }

    public static void main(String[] args) {

        String text = "ABABDABACDABABCABAB";
        String pattern = "ABABCABAB";

        search(pattern, text);

    }
}
```

### Time Complexity

```
Best      : O(n)

Average   : O(n + m)

Worst     : O(n × m)

Space     : O(1)
```

---

# 8. Limitations

## Naive Algorithm

```
Text

AAAAAAAAAAAAAA

Pattern

AAAAAB

↓

Every Shift

↓

Almost Entire Pattern Compared

↓

Slow
```

Problems

- Large number of comparisons
- Slow for repetitive text
- No preprocessing
- Inefficient for huge files

---

## Rabin-Karp

```
Hash

57

↓

Window Hash

57

↓

Actual Characters

Different

↓

Extra Comparison
```

Problems

- Hash collisions
- Choosing good prime number
- Slightly complex implementation
- Worst case equals naive

---

# 9. Edge Cases

## Case 1

### Empty Pattern

```
Text

ABCDE

Pattern

""
```

Expected

```
Return immediately
```

---

## Case 2

Pattern Longer Than Text

```
Text

ABC

Pattern

ABCDE
```

Result

```
Impossible

No Match
```

---

## Case 3

Single Character

```
Text

AAAAAA

Pattern

A
```

Matches

```
0

1

2

3

4

5
```

---

## Case 4

Pattern Not Found

```
Text

ABCDEFG

Pattern

XYZ
```

```
No Match
```

---

## Case 5

Overlapping Pattern

```
Text

AAAAA

Pattern

AAA
```

Matches

```
Index

0

1

2
```

Visual

```
AAA

 AAA

  AAA
```

---

## Case 6

Special Characters

```
Text

abc@123#xyz

Pattern

123#
```

Works correctly because algorithms compare characters regardless of type.

---

## Case 7

Entire Text Equals Pattern

```
Text

HELLO

Pattern

HELLO
```

One match at

```
0
```

---

# 10. Comparison Table

| Feature | Naive | Rabin-Karp |
|---------|--------|------------|
| Idea | Direct Comparison | Hash Comparison |
| Preprocessing | No | Yes (Hash) |
| Uses Hashing | No | Yes |
| Best Case | O(n) | O(n) |
| Average Case | O(n×m) | O(n+m) |
| Worst Case | O(n×m) | O(n×m) |
| Space | O(1) | O(1) |
| Implementation | Very Easy | Moderate |
| Hash Collision | No | Yes |
| Suitable For | Small Inputs | Large Text & Multiple Patterns |
| Multiple Pattern Search | Poor | Excellent |
| Practical Speed | Slower | Faster on Average |

---

# 11. Conclusion & Key Takeaways

## Naive String Matching

✔ Simplest algorithm

✔ Easy to implement

✔ No preprocessing

✔ Suitable for small inputs

✔ Worst-case **O(n × m)**

---

## Rabin-Karp

✔ Uses hashing

✔ Rolling hash avoids recomputation

✔ Efficient average performance

✔ Excellent for searching multiple patterns

✔ May suffer from hash collisions

---

# Exam Revision Sheet

## Remember

| Topic | Must Memorize |
|---------|---------------|
| Naive Idea | Compare at every shift |
| Rabin-Karp Idea | Compare hashes first |
| Rolling Hash | Update hash in O(1) |
| Naive Worst Case | O(n × m) |
| Rabin-Karp Average | O(n + m) |
| Rabin-Karp Worst | O(n × m) |
| Space Complexity | O(1) |
| Collision | Same hash, different strings |
| Naive Advantage | Very simple |
| Rabin-Karp Advantage | Faster average performance, multiple pattern search |

---

# Quick Memory Trick

```
Naive

Compare Everything

↓

Simple

↓

Slow
```

```
Rabin-Karp

Compare Hash

↓

If Equal

↓

Compare Characters

↓

Fast on Average
```

---

> **Semester Exam Tip:** Questions commonly ask for the algorithm steps, time complexity, a dry run with an example, differences between Naive and Rabin-Karp, and the Java implementation. Be prepared to explain the sliding window in Naive matching and the rolling hash mechanism in Rabin-Karp with diagrams.
