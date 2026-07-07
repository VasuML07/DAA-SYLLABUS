# Stable Marriage Problem (Gale–Shapley Algorithm) – Complete Study Guide
### Design and Analysis of Algorithms (DAA) | Engineering Semester Exam Notes

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. Why It Matters](#2-why-it-matters)
- [3. When & Where It's Used](#3-when--where-its-used)
- [4. How to Find & Solve It](#4-how-to-find--solve-it)
- [5. Visual Explanation of the Algorithm](#5-visual-explanation-of-the-algorithm)
- [6. Java Implementation](#6-java-implementation)
- [7. Limitations & Edge Cases](#7-limitations--edge-cases)
- [8. Complexity Analysis](#8-complexity-analysis)
- [9. Exam Tips](#9-exam-tips)
- [10. Conclusion](#10-conclusion)

---

# 1. Introduction

The **Stable Marriage Problem (SMP)** is one of the most famous matching problems in computer science.

It was introduced by **David Gale** and **Lloyd Shapley** in **1962**, leading to the famous **Gale-Shapley Stable Matching Algorithm**.

The objective is:

> Match **n men** and **n women** such that **no unstable pair exists.**

A matching is **stable** if there does **not** exist:

- A man who prefers another woman over his current partner
- AND that woman also prefers him over her current partner

Such a pair is called a **blocking pair**.

---

## Example

Men

```
M1
M2
M3
```

Women

```
W1
W2
W3
```

Possible Matching

```
M1 — W2
M2 — W3
M3 — W1
```

If no man and woman would both rather be with each other than with their assigned partners,

the matching is **Stable**.

---

# 2. Why It Matters

Stable matching avoids conflicts and dissatisfaction.

Without stability:

```
M1 ---- W1
M2 ---- W2
```

Suppose

```
M1 prefers W2
W2 prefers M1
```

Then

```
M1 ---- W1
M2 ---- W2

      ↓

M1 and W2 leave their partners
```

Current matching breaks.

Hence it is unstable.

---

## Importance

- Fair allocation
- No participant has incentive to cheat
- Prevents rematching
- Efficient solution
- Polynomial time algorithm

---

## Computer Science Significance

Stable Matching is used in

- Matching Theory
- Game Theory
- Market Design
- Optimization
- Resource Allocation
- Distributed Systems

---

# 3. When & Where It's Used

---

## 1. Medical Residency Matching

Doctors rank hospitals.

Hospitals rank doctors.

Algorithm finds stable assignments.

Example

```
Doctor A → Hospital X
Doctor B → Hospital Y
```

Used in:

- NRMP (USA)

---

## 2. College Admissions

Students rank colleges.

Colleges rank students.

Stable admission avoids conflicts.

---

## 3. Job Recruitment

Applicants rank companies.

Companies rank applicants.

Produces stable hiring.

---

## 4. School Admission

Students

↓

Schools

↓

Stable Allocation

---

## 5. Internship Matching

Students

↓

Companies

↓

Internships

---

## 6. Organ Donation Matching

Patients

↓

Donors

↓

Stable pairing

---

## 7. Cloud Resource Allocation

Applications

↓

Servers

↓

Stable resource allocation

---

## 8. Dating Platforms

Users rank preferred matches.

Stable matching reduces conflicts.

---

# 4. How to Find & Solve It

The Gale-Shapley algorithm is an **iterative improvement algorithm** because each proposal improves the quality of the current matching until stability is reached.

---

## Algorithm Idea

Initially

```
Everyone is free.
```

Repeat

```
A free man proposes
↓

Highest-ranked woman not yet proposed

↓

If woman is free

Accept

Else

Compare partners

↓

Keep better partner

Reject other
```

Repeat until

```
No free men remain.
```

---

## Flowchart

```
          Start

             │

             ▼

      All men are free

             │

             ▼

 Select a free man

             │

             ▼

Propose to highest-ranked
woman not yet proposed

             │

      +------+------+

      |             |

 Woman free?      Engaged?

      |             |

     Yes            Yes

      |             |

 Accept         Compare partners

                   │

        Better new proposer?

          /              \

        Yes              No

        │                 │

Replace partner       Reject proposal

        │

Old partner becomes free

        │

Repeat until all matched

        │

       End
```

---

## Step-by-Step Algorithm

### Step 1

All participants are free.

```
M1
M2
M3
```

```
W1
W2
W3
```

---

### Step 2

Choose a free man.

Suppose

```
M1
```

---

### Step 3

He proposes to his first preference.

```
M1 → W2
```

If W2 is free

```
Accepted
```

---

### Step 4

Choose another free man.

```
M2 → W2
```

Now

```
W2 compares

Current : M1

New : M2
```

Suppose W2 prefers M2.

```
Reject M1

Accept M2
```

Now

```
M1 becomes free again.
```

---

### Step 5

M1 proposes next woman.

Continue until everyone matched.

---

# 5. Visual Explanation of the Algorithm

## Example

Men Preferences

| Man | Preference |
|------|------------|
| M1 | W1 W2 W3 |
| M2 | W2 W1 W3 |
| M3 | W2 W3 W1 |

Women Preferences

| Woman | Preference |
|--------|------------|
| W1 | M2 M1 M3 |
| W2 | M1 M2 M3 |
| W3 | M1 M2 M3 |

---

## Iteration 1

```
M1 → W1

Accepted
```

Matching

```
M1 — W1
```

---

## Iteration 2

```
M2 → W2

Accepted
```

Matching

```
M1 — W1

M2 — W2
```

---

## Iteration 3

```
M3 → W2
```

Current partner

```
M2
```

Preference

```
W2 likes

M1

M2

M3
```

She prefers M2.

```
Reject M3
```

---

## Iteration 4

```
M3 → W3

Accepted
```

Final Matching

```
M1 — W1

M2 — W2

M3 — W3
```

---

## Another Example Showing Partner Replacement

Preferences

```
M1 : W1 W2 W3

M2 : W1 W2 W3

M3 : W2 W3 W1
```

Women

```
W1 : M2 M1 M3

W2 : M3 M1 M2

W3 : M1 M2 M3
```

---

Iteration

```
M1 → W1

Accepted
```

```
M2 → W1

W1 prefers M2

Reject M1

Accept M2
```

```
M1 → W2

Accepted
```

```
M3 → W2

W2 prefers M3

Reject M1

Accept M3
```

```
M1 → W3

Accepted
```

Final

```
M2 — W1

M3 — W2

M1 — W3
```

Notice how engagements improve over iterations until no blocking pair exists.

---

# 6. Java Implementation

```java
import java.util.Arrays;

public class StableMarriage {

    // Number of men/women
    static final int N = 4;

    /**
     * Returns true if woman prefers man1 over man2.
     */
    static boolean womanPrefersNewMan(
            int[][] womenPref,
            int woman,
            int newMan,
            int currentMan) {

        for (int i = 0; i < N; i++) {
            if (womenPref[woman][i] == newMan)
                return true;

            if (womenPref[woman][i] == currentMan)
                return false;
        }

        return false;
    }

    static void stableMarriage(int[][] menPref,
                               int[][] womenPref) {

        // womanPartner[w] = man currently engaged
        int[] womanPartner = new int[N];

        Arrays.fill(womanPartner, -1);

        // true if man is free
        boolean[] manFree = new boolean[N];
        Arrays.fill(manFree, true);

        // next woman each man should propose to
        int[] nextProposal = new int[N];

        int freeMen = N;

        while (freeMen > 0) {

            // Find first free man
            int man = 0;

            while (!manFree[man])
                man++;

            // Propose until engaged
            while (manFree[man]) {

                int woman = menPref[man][nextProposal[man]];
                nextProposal[man]++;

                // Woman is free
                if (womanPartner[woman] == -1) {

                    womanPartner[woman] = man;
                    manFree[man] = false;
                    freeMen--;

                } else {

                    int currentMan = womanPartner[woman];

                    if (womanPrefersNewMan(
                            womenPref,
                            woman,
                            man,
                            currentMan)) {

                        womanPartner[woman] = man;

                        manFree[man] = false;

                        manFree[currentMan] = true;

                    }
                }
            }
        }

        System.out.println("Final Stable Matching\n");

        for (int woman = 0; woman < N; woman++) {

            System.out.println(
                    "Woman W" + woman +
                    "  <-->  Man M" +
                    womanPartner[woman]);
        }
    }

    public static void main(String[] args) {

        int[][] menPreference = {

                {0,1,2,3},
                {1,0,2,3},
                {1,2,0,3},
                {0,1,2,3}
        };

        int[][] womenPreference = {

                {1,0,2,3},
                {0,1,2,3},
                {0,1,2,3},
                {0,1,2,3}
        };

        stableMarriage(menPreference, womenPreference);
    }
}
```

---

## Explanation of Variables

| Variable | Purpose |
|-----------|----------|
| womanPartner[] | Stores woman's current partner |
| manFree[] | Indicates whether a man is free |
| nextProposal[] | Tracks the next woman to propose to |
| menPreference[][] | Men's ranked preferences |
| womenPreference[][] | Women's ranked preferences |

---

# 7. Limitations & Edge Cases

---

## 1. Preference Ties

Standard Gale-Shapley assumes **strict preference order**.

Example

```
W1

M1 = M2
```

Tie

↓

Multiple stable solutions possible.

---

## 2. Unequal Number of Participants

Example

```
Men = 4

Women = 3
```

One man remains unmatched.

```
M1 → W1

M2 → W2

M3 → W3

M4 → None
```

Requires algorithm modification.

---

## 3. Incomplete Preference Lists

Example

```
M1

W1

W2
```

Does not rank

```
W3
```

Algorithm must handle missing preferences carefully.

---

## 4. Multiple Stable Matchings

Example

```
Stable Matching A

M1-W1

M2-W2
```

Also

```
Stable Matching B

M1-W2

M2-W1
```

More than one stable answer may exist.

The algorithm returns one based on the proposing side.

---

## 5. Proposer Bias

Men propose.

Result

```
Best possible for men

Worst stable for women
```

If women propose

```
Best for women

Worst stable for men
```

Visualization

```
Men propose

↓

Men-optimal

Women-pessimal


Women propose

↓

Women-optimal

Men-pessimal
```

---

## 6. Blocking Pair (Unstable Matching)

Matching

```
M1 — W1

M2 — W2
```

Preferences

```
M1 prefers W2

W2 prefers M1
```

Diagram

```
M1 ----- W1

M2 ----- W2

 \       /

  \_____/

Blocking Pair
```

This matching is **not stable**.

---

# 8. Complexity Analysis

| Operation | Complexity |
|------------|------------|
| Worst-case proposals | n² |
| Time Complexity | **O(n²)** |
| Space Complexity | **O(n)** |

---

## Why O(n²)?

Each man can propose to each woman at most once.

```
n men

×

n women

=

n² proposals
```

Therefore,

```
Time Complexity = O(n²)
```

---

# 9. Exam Tips

### Definition

A stable matching has **no blocking pair**.

---

### Algorithm Name

**Gale-Shapley Stable Marriage Algorithm**

---

### Key Idea

```
Free man proposes

↓

Woman keeps better proposal

↓

Rejected man tries again

↓

Repeat until everyone matched
```

---

### Important Points

- Greedy iterative improvement algorithm
- Produces a stable matching
- No blocking pair exists
- Men-proposing version is men-optimal
- Women-proposing version is women-optimal
- Guaranteed termination
- Polynomial-time algorithm
- Time complexity: **O(n²)**

---

### Frequently Asked Exam Questions

1. Define Stable Marriage Problem.
2. What is a blocking pair?
3. Explain the Gale-Shapley algorithm with an example.
4. Trace the algorithm for given preference lists.
5. Prove why the algorithm always terminates.
6. Why is the resulting matching stable?
7. Discuss proposer-optimality.
8. Write the Java implementation.
9. State the time and space complexities.
10. Explain limitations and edge cases.

---

# 10. Conclusion

The **Stable Marriage Problem** is a classic matching problem that demonstrates how iterative improvement can produce globally stable outcomes through a sequence of local decisions. The **Gale-Shapley algorithm** repeatedly allows free participants to propose, temporarily accepts the best available proposal, and replaces inferior matches whenever a better proposal arrives. This process continues until every participant is matched and no blocking pair exists.

For semester examinations, remember these key points:

- The objective is to produce a **stable matching**, not necessarily a unique one.
- Stability means **no blocking pair** exists.
- The algorithm terminates after at most **n² proposals**.
- Time complexity is **O(n²)** and space complexity is **O(n)**.
- The side that proposes obtains its **optimal stable matching**, while the opposite side receives its least favorable stable matching among all stable solutions.
- The algorithm is widely applied in residency matching, college admissions, job allocation, and other real-world assignment problems.

Understanding the proposal-and-replacement process, tracing iterations, and being able to explain why the final matching is stable are the most important concepts for both theory questions and algorithm-tracing problems in DAA exams.
