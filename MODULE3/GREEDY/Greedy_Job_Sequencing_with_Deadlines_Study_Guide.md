# Greedy Job Sequencing with Deadlines Study Guide

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Why, When, and Where](#2-why-when-and-where)
- [3. Problem Statement](#3-problem-statement)
- [4. Greedy Strategy](#4-greedy-strategy)
- [5. Step-by-Step Approach](#5-step-by-step-approach)
- [6. Visual Walkthrough](#6-visual-walkthrough)
- [7. Java Implementation](#7-java-implementation)
- [8. Time and Space Complexity](#8-time-and-space-complexity)
- [9. Limitations and Edge Cases](#9-limitations-and-edge-cases)
- [10. Exam Revision Notes](#10-exam-revision-notes)

# 1. Introduction

The **Job Sequencing with Deadlines** problem is a classic greedy algorithm problem.

Each job:
- Takes exactly **1 unit of time**
- Has a **deadline**
- Has a **profit**
- Gives profit only if completed on or before its deadline.

Goal:
> Maximize total profit.

---

# 2. Why, When, and Where

## Why?

To maximize profit while satisfying deadline constraints.

## When?

Use when:
- Every task takes equal time.
- Only one task can execute at a time.
- Missing deadline gives zero profit.

## Where?

Examples:
- CPU scheduling
- Manufacturing
- Advertisement scheduling
- Cloud resource allocation
- Exam timetable slot assignment

---

# 3. Problem Statement

Example:

| Job | Deadline | Profit |
|------|----------|---------|
| A | 2 | 100 |
| B | 1 | 19 |
| C | 2 | 27 |
| D | 1 | 25 |
| E | 3 | 15 |

Maximum deadline = 3

Available slots:

```
Slot1   Slot2   Slot3
[ ]     [ ]     [ ]
```

---

# 4. Greedy Strategy

Rule:

1. Sort jobs by decreasing profit.
2. For each job:
   - Try to place it in the latest available slot before its deadline.
3. If slot unavailable, discard it.

Reason:

Placing a profitable job as late as possible leaves earlier slots for other jobs.

---

# 5. Step-by-Step Approach

Sorted:

| Job | Deadline | Profit |
|------|----------|---------|
| A |2|100|
| C |2|27|
| D |1|25|
| B |1|19|
| E |3|15|

Initial:

```
Slot1 Slot2 Slot3
 -     -     -
```

### Insert A

Latest ≤2 = Slot2

```
Slot1 Slot2 Slot3
 -      A     -
```

### Insert C

Slot2 occupied.

Try Slot1.

```
Slot1 Slot2 Slot3
 C      A     -
```

### Insert D

Slot1 occupied.

Discard.

### Insert B

Slot1 occupied.

Discard.

### Insert E

Deadline 3

```
Slot1 Slot2 Slot3
 C      A      E
```

Final:

|Slot|Job|
|---|---|
|1|C|
|2|A|
|3|E|

Profit = 27 + 100 + 15 = **142**

---

# 6. Visual Walkthrough

```
Jobs
 │
 ▼
Sort by Profit
 │
 ▼
For every Job
 │
 ▼
Find latest free slot
 │
 ├── Found
 │      │
 │      ▼
 │   Schedule Job
 │
 └── Not Found
        │
        ▼
     Reject Job
```

---

# 7. Java Implementation

```java
import java.util.*;

class Job {
    char id;
    int deadline;
    int profit;

    Job(char id, int deadline, int profit) {
        this.id = id;
        this.deadline = deadline;
        this.profit = profit;
    }
}

public class JobSequencing {

    public static void main(String[] args) {

        Job[] jobs = {
            new Job('A',2,100),
            new Job('B',1,19),
            new Job('C',2,27),
            new Job('D',1,25),
            new Job('E',3,15)
        };

        Arrays.sort(jobs,(a,b)->b.profit-a.profit);

        int maxDeadline = 0;
        for(Job j: jobs)
            maxDeadline = Math.max(maxDeadline,j.deadline);

        char[] slot = new char[maxDeadline];
        boolean[] occupied = new boolean[maxDeadline];

        int totalProfit = 0;

        for(Job job: jobs){

            // Search backward for latest available slot
            for(int j=Math.min(maxDeadline,job.deadline)-1;j>=0;j--){

                if(!occupied[j]){
                    occupied[j]=true;
                    slot[j]=job.id;
                    totalProfit+=job.profit;
                    break;
                }
            }
        }

        System.out.println("Selected Jobs:");

        for(int i=0;i<slot.length;i++)
            if(occupied[i])
                System.out.println("Slot "+(i+1)+" : "+slot[i]);

        System.out.println("Profit = "+totalProfit);
    }
}
```

### Variable Explanation

|Variable|Purpose|
|---|---|
|jobs|Stores all jobs|
|maxDeadline|Number of slots|
|slot|Scheduled jobs|
|occupied|Checks free slot|
|totalProfit|Final answer|

---

# 8. Time and Space Complexity

Without Disjoint Set Union:

|Operation|Complexity|
|---|---|
|Sorting|O(n log n)|
|Slot search|O(n × d)|
|Total|O(n log n + n×d)|

Worst case:

```
d ≈ n

Total = O(n²)
```

Optimized with Disjoint Set Union:

```
O(n log n)
```

---

# 9. Limitations and Edge Cases

## Limitation 1

Jobs must require equal execution time.

Counterexample:

|Job|Time|
|---|---|
|A|3|
|B|1|

Greedy fails.

---

## Limitation 2

Cannot maximize weighted completion time.

Needs Dynamic Programming or other optimization.

---

## Limitation 3

Negative Profit

|Job|Profit|
|---|---|
|A|-20|

Better to skip.

---

## Edge Cases

### Single Job

```
A
Deadline=1
Profit=50
```

Schedule A.

---

### Same Deadline

|Job|Deadline|Profit|
|---|---|---|
|A|2|40|
|B|2|90|

Choose B first.

---

### Deadline Greater Than Slots

Slots are created using maximum deadline.

---

### No Jobs

```
Jobs = {}
Profit = 0
```

---

### All Slots Filled

Remaining jobs are rejected.

---

# 10. Exam Revision Notes

## Key Points

- Greedy algorithm.
- Sort by descending profit.
- Place job in latest free slot.
- Every job requires one unit time.
- Profit earned only if deadline satisfied.

## Frequently Asked Questions

**Why latest slot?**

To preserve earlier slots for other jobs.

**Why sort by profit?**

Highest-profit jobs get priority.

**Time Complexity?**

O(n²) basic implementation.

O(n log n) with DSU.

## Common Exam Questions

1. Explain greedy choice.
2. Trace execution.
3. Write Java code.
4. Dry run on sample input.
5. Derive time complexity.
6. Mention limitations.
