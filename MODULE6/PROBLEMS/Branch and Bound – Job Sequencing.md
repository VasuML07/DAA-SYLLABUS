# Branch and Bound – Job Sequencing
### Design and Analysis of Algorithms (DAA) – Complete Exam Study Guide

> **Study Goal:** Understand the Branch and Bound approach for solving the Job Sequencing problem, including the search tree, bounding function, pruning, implementation, complexity analysis, limitations, and exam-oriented concepts.

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. Why Branch and Bound is Used](#2-why-branch-and-bound-is-used)
- [3. When Branch and Bound is Used](#3-when-branch-and-bound-is-used)
- [4. Where Branch and Bound is Used](#4-where-branch-and-bound-is-used)
- [5. How Branch and Bound Works](#5-how-branch-and-bound-works)
- [6. Understanding the Job Sequencing Problem](#6-understanding-the-job-sequencing-problem)
- [7. Mathematical Representation](#7-mathematical-representation)
- [8. State Space Tree](#8-state-space-tree)
- [9. Branching Process](#9-branching-process)
- [10. Bounding Concept](#10-bounding-concept)
- [11. Pruning Strategy](#11-pruning-strategy)
- [12. Decision Making During Search](#12-decision-making-during-search)
- [13. Worked Example](#13-worked-example)
- [14. Limitations](#14-limitations)
- [15. Edge Cases](#15-edge-cases)
- [16. Java Implementation](#16-java-implementation)
- [17. Complexity Analysis](#17-complexity-analysis)
- [18. Comparison with Other Approaches](#18-comparison-with-other-approaches)
- [19. Exam Tips](#19-exam-tips)
- [20. Viva Questions](#20-viva-questions)
- [21. Summary](#21-summary)

---

# 1. Introduction

Job Sequencing is one of the classical optimization problems studied in **Design and Analysis of Algorithms (DAA)**.

The objective is to determine the **best order of executing jobs** so that an optimization objective is achieved.

Depending on the problem, the objective may be:

- Maximize profit
- Minimize completion time
- Minimize lateness
- Minimize processing cost
- Meet deadlines
- Reduce resource utilization

While Greedy algorithms solve some versions efficiently, many practical scheduling problems become **NP-Hard**, where Greedy methods cannot guarantee the optimal solution.

For such problems, **Branch and Bound** is one of the most powerful exact optimization techniques.

Instead of checking every possible schedule, Branch and Bound intelligently removes schedules that can never become optimal.

---

# 2. Why Branch and Bound is Used

## The Problem

Suppose there are **N jobs**.

Every possible ordering is a different schedule.

For example:

```
Jobs:

A
B
C
D
```

Possible schedules:

```
ABCD
ABDC
ACBD
ACDB
ADBC
ADCB
BACD
...
```

Number of schedules:

```
N!
```

Example:

| Jobs | Possible Orders |
|------|-----------------|
|2|2|
|3|6|
|4|24|
|5|120|
|6|720|
|7|5040|
|8|40320|
|10|3,628,800|

Clearly,

```
Brute Force

↓

Generate every schedule

↓

Calculate cost

↓

Return best schedule
```

This becomes impossible for large values of **N**.

---

## Why Not Brute Force?

Brute Force explores every schedule.

```
Root

├── Schedule 1
├── Schedule 2
├── Schedule 3
├── ...
└── Schedule N!
```

Time complexity:

```
O(N!)
```

Even for 15 jobs,

```
15!

=

1,307,674,368,000
```

More than one trillion possibilities.

Impossible in practical systems.

---

## Why Not Greedy?

Greedy makes the locally best decision.

Example:

```
Choose highest profit first.
```

or

```
Choose shortest job first.
```

Unfortunately,

Local optimum

≠

Global optimum

Example:

```
Job

Profit

Deadline

A

100

1

B

95

2

C

90

2
```

Greedy may select

```
A

↓

B
```

But another ordering might produce a larger overall objective depending on constraints.

Greedy cannot explore alternatives.

---

## Why Not Dynamic Programming?

Dynamic Programming requires:

- Overlapping subproblems
- Optimal substructure
- Manageable state space

Job sequencing with multiple constraints often produces an exponential DP table.

Example constraints:

- Deadlines
- Machine assignment
- Processing time
- Resource limits
- Precedence constraints

State space becomes enormous.

DP becomes impractical.

---

## Why Branch and Bound?

Branch and Bound combines

```
Systematic Search

+

Mathematical Bounding

+

Pruning
```

Instead of exploring everything,

it explores only promising schedules.

---

### Basic Idea

```
Generate schedule

↓

Estimate best possible future result

↓

Can this schedule beat current best?

↓

YES

↓

Explore

------------------

NO

↓

Discard immediately
```

This is called

```
Pruning
```

---

## Major Advantages

| Advantage | Explanation |
|------------|-------------|
|Optimal solution|Always returns the best solution|
|Search reduction|Avoids exploring useless branches|
|Memory efficient (DFS version)|Stores only active path|
|General technique|Works for many NP-Hard problems|
|Mathematical guarantee|Never removes potentially optimal solutions|

---

## Visual Overview

```
Without Branch and Bound


                 Root
          /   /   |   \
         *    *   *    *
       / | \ /|\ /|\ /|\
      * * * * * * * * * *

Explore everything.
```

---

```
With Branch and Bound


                 Root
          /   /   |   \
         *    X   *    X
       / |\
      *  X *

Only promising nodes survive.
```

---

# 3. When Branch and Bound is Used

Branch and Bound is suitable whenever:

- Exact optimal solution is required.
- Brute Force is too expensive.
- Search space can be represented as a tree.
- Upper or lower bounds can be computed.
- Poor solutions can be eliminated early.

---

## Conditions

### Condition 1

Many possible solutions.

```
Schedule 1

Schedule 2

Schedule 3

...

Schedule N!
```

---

### Condition 2

Need optimal answer.

Approximation is not acceptable.

Example:

```
Military scheduling

Satellite scheduling

Medical scheduling

Air traffic scheduling
```

---

### Condition 3

Bounding function exists.

Example:

```
Current Profit

+

Maximum Remaining Profit

=

Upper Bound
```

If

```
Upper Bound

<

Best Profit
```

Discard.

---

### Condition 4

State space forms a tree.

```
Root

↓

Choose Job

↓

Choose Next Job

↓

Choose Next Job
```

Every level represents one decision.

---

## Suitable Problems

Branch and Bound is widely used for:

| Problem | Suitable |
|----------|-----------|
|Job Sequencing|Yes|
|Traveling Salesman|Yes|
|Assignment Problem|Yes|
|Knapsack|Yes|
|Scheduling Problems|Yes|
|Integer Programming|Yes|
|Machine Allocation|Yes|
|Resource Scheduling|Yes|

---

## Situations Where It Is Not Ideal

| Situation | Reason |
|------------|---------|
|Very small input|Brute Force is simpler|
|Need only approximate answer|Greedy is faster|
|Real-time constraints|Search may still be expensive|
|No bounding function|Cannot prune|

---

# 4. Where Branch and Bound is Used

Branch and Bound is widely used in industries where **finding the best solution is more important than obtaining a quick approximate answer**.

---

## Manufacturing

Factory scheduling.

```
Machine 1

↓

Machine 2

↓

Machine 3
```

Goal:

- Minimize production time
- Reduce idle machines
- Maximize throughput

---

## Cloud Computing

Assign jobs to servers.

```
Incoming Jobs

↓

Server Selection

↓

Execution Schedule
```

Objectives:

- Reduce response time
- Increase utilization
- Balance workload

---

## Operating Systems

CPU scheduling.

```
Processes

↓

Scheduler

↓

CPU
```

Optimization:

- Waiting time
- Turnaround time
- Deadline satisfaction

---

## Airline Scheduling

```
Flights

↓

Aircraft

↓

Crew

↓

Runways
```

Branch and Bound helps optimize:

- Flight assignments
- Crew schedules
- Maintenance windows

---

## Logistics

Package delivery.

```
Warehouse

↓

Vehicle

↓

Delivery Order
```

Objective:

- Minimize travel cost
- Reduce delivery time

---

## Robotics

Task scheduling.

```
Robot

↓

Task Selection

↓

Execution Order
```

---

## Artificial Intelligence

Planning problems.

```
Goal

↓

Possible Actions

↓

Best Action Sequence
```

---

## Project Management

Scheduling project activities.

```
Task

↓

Resource

↓

Timeline
```

---

## Data Centers

Efficient allocation of:

- CPUs
- GPUs
- Storage
- Memory
- Virtual machines

---

## Banking

Batch transaction scheduling.

```
Transactions

↓

Priority Queue

↓

Execution Order
```

---

## Real-World Summary

| Industry | Example Use |
|-----------|-------------|
|Manufacturing|Machine scheduling|
|Cloud Computing|Server allocation|
|Operating Systems|CPU scheduling|
|Transportation|Vehicle routing|
|Robotics|Task planning|
|Artificial Intelligence|Search optimization|
|Finance|Transaction scheduling|
|Telecommunications|Resource allocation|

---

# 5. How Branch and Bound Works

Branch and Bound follows a structured search process.

Instead of checking every possible schedule, it repeatedly asks:

> **"Can this partial solution still become the optimal solution?"**

If the answer is **No**, that branch is discarded.

---

## Overall Workflow

```text
                Start
                  │
                  ▼
          Create Root Node
                  │
                  ▼
        Expand Possible Choices
                  │
                  ▼
         Compute Bound Value
                  │
        ┌─────────┴─────────┐
        │                   │
        ▼                   ▼
   Bound Promising?       Not Promising
        │                   │
        ▼                   ▼
 Expand Further         Prune Branch
        │
        ▼
 Continue Until
 Complete Schedule
```

---

## Step-by-Step Procedure

### Step 1 — Create the Root Node

Initially, no jobs are scheduled.

```text
Level 0

Root

Schedule = { }

Profit = 0
```

---

### Step 2 — Branch

Select one unscheduled job and place it next in the schedule.

Example:

```text
Root
├── A
├── B
├── C
└── D
```

Each child represents a different decision.

---

### Step 3 — Calculate the Bound

For every partial schedule, estimate the **best possible objective value** that can still be achieved if all remaining choices are ideal.

General idea:

```text
Bound

=

Current Objective

+

Maximum Possible Remaining Gain
```

The exact formula depends on the scheduling problem.

---

### Step 4 — Compare with Current Best

Suppose:

```text
Current Best Profit = 180
```

A node has:

```text
Upper Bound = 165
```

Since:

```text
165 < 180
```

No completion of that branch can beat the best solution already found.

Therefore:

```text
Discard Branch
```

This process is called **Pruning**.

---

### Step 5 — Continue Until Completion

The algorithm repeats:

1. Expand a promising node.
2. Compute its bound.
3. Prune if necessary.
4. Continue until all promising nodes are explored.

The final solution is guaranteed to be optimal because only branches that **cannot** improve the answer are removed.

---

# 6. Understanding the Job Sequencing Problem

Before studying the complete Branch and Bound search tree, it is important to understand the scheduling problem itself.

In a typical Job Sequencing problem:

- There are **N jobs**.
- Each job has associated information such as:
  - Profit
  - Deadline
  - Processing time (problem dependent)
- Only one job can execute at a time.
- The objective is to determine the **best execution order**.

Example dataset:

| Job | Profit | Deadline |
|------|--------|----------|
| J1 | 80 | 2 |
| J2 | 55 | 1 |
| J3 | 70 | 2 |
| J4 | 40 | 3 |

Possible schedules include:

```text
J1 → J2 → J3 → J4

J2 → J1 → J4 → J3

J3 → J4 → J1 → J2

...
```

Branch and Bound searches these schedules intelligently instead of evaluating all `N!` permutations.

---

# 7. Mathematical Representation

Let:

```text
J = {J1, J2, J3, ..., Jn}
```

A solution is an ordered sequence:

```text
S = (J2, J4, J1, J3, ...)
```

Each node in the search tree stores:

- Partial schedule
- Current objective value
- Bound estimate
- Remaining jobs

Conceptually:

```text
Node

=

(
Current Schedule,
Current Cost/Profit,
Bound,
Remaining Jobs
)
```

The algorithm always expands nodes that appear most promising according to their bound.

---

**End of Part 1**

The next part covers:

- Complete **State Space Tree**
- **Branching Process**
- **Bounding Calculation**
- **Pruning Strategy**
- **Decision Making**
- **Detailed Worked Example** with complete ASCII search trees and exam-style walkthrough.

# 8. State Space Tree

A **State Space Tree** is a graphical representation of **all possible partial and complete job schedules**.

- **Root Node** → No job has been selected.
- **Intermediate Nodes** → Partial schedules.
- **Leaf Nodes** → Complete schedules.

Each level of the tree represents **one scheduling decision**.

---

## General Structure

Suppose there are four jobs:

```
J1
J2
J3
J4
```

The state-space tree begins as:

```text
                           {}
                 /-----------|-----------|-----------\
               J1           J2          J3          J4
             / | \        / | \       / | \       / | \
           ... ... ...  ... ... ... ... ... ... ... ...
```

Here:

- `{}` = Empty schedule
- `J1` = Choose Job 1 first
- `J2` = Choose Job 2 first
- etc.

---

## Levels of the Tree

| Tree Level | Meaning |
|------------|---------|
| Level 0 | No job selected |
| Level 1 | First job fixed |
| Level 2 | First two jobs fixed |
| Level 3 | First three jobs fixed |
| Level N | Complete schedule |

Example:

```text
Level 0

{}

↓

Level 1

{J1}

↓

Level 2

{J1,J3}

↓

Level 3

{J1,J3,J2}

↓

Level 4

{J1,J3,J2,J4}
```

---

## Example Search Tree

Jobs:

```
A
B
C
```

Possible schedules:

```text
                      {}
                 /------|------\
                A       B       C
              /  \    /  \    /  \
            AB  AC  BA  BC  CA  CB
            |    |   |   |   |   |
          ABC ACB BAC BCA CAB CBA
```

Observe:

```
Leaf Nodes

=

3!

=

6
```

Branch and Bound usually explores **far fewer than all six leaves** because many branches are pruned.

---

## Node Representation

Each node stores important information.

Example:

```text
Node

Schedule : A → C

Profit   : 120

Bound    : 215

Remaining Jobs

B,D
```

Visual representation:

```text
+-----------------------+
| Schedule : A → C      |
| Profit   : 120        |
| Bound    : 215        |
| Remaining: B,D        |
+-----------------------+
```

---

## Parent–Child Relationship

Suppose:

Current node:

```text
A
```

Remaining jobs:

```
B
C
D
```

Children become:

```text
           A
      /-----|-----\
    AB     AC     AD
```

Every child represents choosing one additional job.

---

# 9. Branching Process

Branching means **generating new candidate schedules** from the current partial schedule.

Every branch corresponds to one possible decision.

---

## Step 1

Start.

```text
{}
```

Remaining jobs:

```
A
B
C
D
```

---

## Step 2

Generate children.

```text
{}
├── A
├── B
├── C
└── D
```

---

## Step 3

Suppose node **A** is expanded.

Remaining jobs:

```
B
C
D
```

Children:

```text
A
├── AB
├── AC
└── AD
```

---

## Step 4

Expand node:

```
AC
```

Remaining:

```
B
D
```

Children:

```text
AC
├── ACB
└── ACD
```

Continue until every job has been placed.

---

## General Rule

If:

```
Remaining Jobs = k
```

Then,

```
Children = k
```

Example:

| Remaining Jobs | Children Generated |
|---------------|-------------------|
|4|4|
|3|3|
|2|2|
|1|1|

---

## Branching Visualization

```text
Root

↓

Choose First Job

↓

Choose Second Job

↓

Choose Third Job

↓

Choose Fourth Job
```

Every decision creates additional branches.

---

# 10. Bounding Concept

Bounding is the **heart of the Branch and Bound algorithm**.

Instead of fully exploring a node, the algorithm estimates the **best possible outcome** that can still be achieved from that node.

If that estimate is not good enough, exploration stops immediately.

---

## Why Calculate a Bound?

Suppose current best solution:

```
Profit = 250
```

A partial schedule has:

```
Current Profit = 120
```

Maximum possible remaining profit:

```
90
```

Upper Bound:

```
120 + 90

=

210
```

Since

```
210 < 250
```

No continuation can beat the best solution.

Therefore:

```
Prune
```

---

## Bound Formula

The exact formula depends on the scheduling objective.

A common upper-bound idea is:

```text
Upper Bound

=

Current Profit

+

Maximum Possible Remaining Profit
```

For minimization problems:

```text
Lower Bound

=

Current Cost

+

Minimum Possible Remaining Cost
```

---

## Bounding Illustration

```text
Current Node

Profit = 90

Remaining Maximum = 140

↓

Upper Bound

=

230
```

If

```
Best Solution

=

210
```

then

```
230 > 210
```

This branch is still promising.

---

## Example

Current node:

```text
Schedule

A → C
```

Current profit:

```
110
```

Remaining jobs:

```
B
D
```

Maximum additional profit:

```
130
```

Upper Bound:

```
110

+

130

=

240
```

Suppose current best:

```
225
```

Since

```
240 > 225
```

Continue exploring.

---

## Another Example

Node:

```text
A → D
```

Current profit:

```
100
```

Maximum remaining:

```
80
```

Bound:

```
180
```

Best solution:

```
210
```

Since

```
180 < 210
```

Discard.

---

# 11. Pruning Strategy

Pruning means **removing branches that cannot produce an optimal solution**.

Instead of exploring every schedule:

```text
Explore

↓

Calculate Bound

↓

Can this node beat the best answer?

↓

YES

↓

Continue

----------------

NO

↓

Prune
```

---

## Example Search Tree

```text
                     {}
              /-------|-------\
             A        B        C
            / \      / \      / \
          AB  AC   BA  BC   CA  CB
              X          X
```

Nodes marked **X** are pruned.

Their children are never generated.

---

## Why Pruning is Safe

Suppose:

```
Current Best Profit

=

260
```

Node:

```
Upper Bound

=

245
```

Even under the most optimistic conditions:

```
Maximum Possible Profit

=

245
```

It can never reach 260.

Therefore:

```
Discard Entire Subtree
```

---

## Search Reduction

Without pruning:

```text
Root

↓

4

↓

12

↓

24

↓

24
```

Every node is explored.

---

With pruning:

```text
Root

↓

4

↓

7

↓

5

↓

2
```

Only promising branches survive.

---

## Advantages of Pruning

| Advantage | Benefit |
|-----------|---------|
|Reduces search|Fewer nodes explored|
|Improves speed|Avoids unnecessary computation|
|Guarantees optimality|Never removes potentially optimal solutions|
|Scales better|Works well for many NP-hard problems|

---

# 12. Decision Making During Search

At every node, the algorithm performs the same sequence of decisions.

---

## Decision Flow

```text
Generate Node
      │
      ▼
Calculate Bound
      │
      ▼
Compare with Best Solution
      │
 ┌────┴────┐
 │         │
 ▼         ▼
Expand   Prune
```

---

## Decision Table

| Current Profit | Bound | Best Profit | Decision |
|---------------|------:|------------:|----------|
|100|250|180|Expand|
|90|170|190|Prune|
|140|270|240|Expand|
|160|210|230|Prune|
|180|290|260|Expand|

---

## Priority of Expansion

Many Branch and Bound implementations use a **priority queue**.

Nodes with **better bounds** are explored first.

Example:

| Node | Bound |
|------|------:|
|A|250|
|B|190|
|C|275|
|D|210|

Expansion order:

```text
C

↓

A

↓

D

↓

B
```

This often finds strong solutions earlier, increasing pruning efficiency.

---

# 13. Worked Example

Consider four jobs.

| Job | Profit |
|------|--------|
|A|80|
|B|70|
|C|60|
|D|40|

Assume the algorithm is maximizing total profit while respecting scheduling constraints.

---

## Initial State

```text
Schedule

{}

Profit = 0
```

Possible first choices:

```text
{}
├── A
├── B
├── C
└── D
```

---

## Bound Calculation

Suppose:

| Node | Current Profit | Upper Bound |
|------|---------------:|------------:|
|A|80|250|
|B|70|230|
|C|60|180|
|D|40|150|

Current best:

```
0
```

All nodes remain candidates initially.

---

## Expand the Best Bound

Choose:

```
A
```

Children:

```text
A
├── AB
├── AC
└── AD
```

Suppose their bounds are:

| Node | Bound |
|------|------:|
|AB|245|
|AC|220|
|AD|180|

Expand **AB** first.

---

## Continue Expansion

```text
AB
├── ABC
└── ABD
```

Suppose:

| Node | Profit | Bound |
|------|--------|------:|
|ABC|210|245|
|ABD|190|215|

Eventually, the complete schedule:

```text
A → B → C → D
```

produces:

```
Profit = 235
```

Set:

```text
Best Profit = 235
```

---

## Pruning

Return to unexplored nodes.

Node:

```
AC
```

Upper Bound:

```
220
```

Since:

```text
220 < 235
```

Prune.

Similarly:

```
AD

Bound = 180
```

Prune immediately.

---

## Final Search Tree

```text
                          {}
                 /---------|---------\
                A          B          C
              / | \       ...        ...
            AB AC AD
            |
          ABC
            |
         ABCD ✔

AC  → Pruned
AD  → Pruned
Many branches under B and C may also be pruned if their bounds are below 235.
```

---

## Key Observations

- The algorithm **does not examine every possible schedule**.
- Good solutions found early improve pruning.
- Better bounds lead to fewer explored nodes.
- The optimal solution is still guaranteed because pruning only removes branches that cannot outperform the current best.

---

**End of Part 2**

**Part 3** includes:

- Limitations
- All edge cases with diagrams
- Complete Java implementation (fully commented)
- Sample input/output
- Dry run
- Complexity analysis
- Comparison with Greedy, Dynamic Programming, and Brute Force
- Exam tips
- Viva questions
- Summary

# 14. Limitations

Although **Branch and Bound** is one of the most powerful exact optimization techniques, it is **not a polynomial-time algorithm**. Its performance depends heavily on the quality of the bounding function.

---

## 14.1 Exponential Worst-Case Time

In the worst case, Branch and Bound explores almost every possible schedule.

For **n** jobs:

```text
Possible schedules = n!
```

Example:

| Jobs | Possible Schedules |
|------|-------------------:|
|3|6|
|4|24|
|5|120|
|6|720|
|7|5,040|
|8|40,320|
|10|3,628,800|

If very few branches can be pruned, the algorithm approaches brute-force performance.

---

## 14.2 Performance Depends on the Bound

A weak bound removes very few nodes.

```text
Weak Bound

↓

Less Pruning

↓

More Search

↓

Slower Algorithm
```

A strong bound greatly reduces the search space.

```text
Strong Bound

↓

More Pruning

↓

Less Search

↓

Faster Algorithm
```

---

## 14.3 Memory Usage

Best-first Branch and Bound typically stores many live nodes.

```text
Priority Queue

↓

Node 1

Node 2

Node 3

...

Node k
```

Large problems may require significant memory.

---

## 14.4 Large Inputs

As the number of jobs increases, the search tree grows rapidly.

```text
Level 0 : 1

Level 1 : n

Level 2 : n(n−1)

Level 3 : n(n−1)(n−2)

...

Leaf Nodes : n!
```

Even with pruning, very large instances remain computationally expensive.

---

## 14.5 Complex Bound Computation

Some scheduling problems require sophisticated mathematical bounds.

If computing the bound itself is expensive, the algorithm's total running time increases.

---

## 14.6 Summary of Limitations

| Limitation | Effect |
|------------|--------|
|Worst-case exponential time|May explore nearly all schedules|
|Depends on bound quality|Weak bounds reduce efficiency|
|High memory usage|Especially in best-first search|
|Not ideal for huge inputs|Search tree becomes enormous|
|Complex implementation|Requires careful node and bound management|

---

# 15. Edge Cases

Understanding edge cases is useful for exams and interviews.

---

## Case 1 – Single Job

Input:

| Job | Profit |
|------|--------|
|A|100|

Search Tree:

```text
{}

↓

A
```

Only one possible schedule exists.

Result:

```text
Optimal Schedule

A
```

No branching or pruning is required.

---

## Case 2 – Two Jobs

Jobs:

| Job | Profit |
|------|--------|
|A|50|
|B|80|

Possible schedules:

```text
AB

BA
```

Search Tree:

```text
      {}
     /  \
    A    B
    |    |
   AB   BA
```

The algorithm evaluates the promising branch first according to its bound.

---

## Case 3 – Identical Processing Times

Jobs:

| Job | Time |
|------|------|
|A|5|
|B|5|
|C|5|

Since every job takes the same amount of time, many partial schedules may have identical bounds.

Example:

```text
      {}
    /  |  \
   A   B   C
```

Many nodes receive equal priority.

The algorithm still guarantees the optimal schedule.

---

## Case 4 – Equal Profits

| Job | Profit |
|------|--------|
|A|100|
|B|100|
|C|100|

Many schedules produce identical objective values.

Example:

```text
ABC

ACB

BAC

BCA

CAB

CBA
```

The algorithm may return **any optimal schedule**.

---

## Case 5 – Impossible Scheduling

Example:

| Job | Deadline | Time |
|------|----------|------|
|A|1|5|
|B|1|6|

Both jobs cannot meet their deadlines.

Search Tree:

```text
{}

↓

A

↓

Invalid
```

```text
{}

↓

B

↓

Invalid
```

Every branch becomes infeasible.

Final Result:

```text
No Feasible Schedule
```

---

## Case 6 – No Feasible Solution

Suppose every partial schedule violates constraints.

```text
              {}

          /    |    \

         X     X     X
```

Every node is pruned.

Algorithm returns:

```text
No Solution Exists
```

---

## Case 7 – Maximum Pruning

Suppose current best profit:

```
300
```

Remaining branches:

| Node | Bound |
|------|------:|
|A|180|
|B|220|
|C|170|

Since every bound is below 300:

```text
Root

├── A ✗

├── B ✗

└── C ✗
```

Entire search terminates immediately.

---

## Case 8 – Minimum Pruning

If every node has a promising bound:

```text
Root

├── A

├── B

├── C

└── D
```

No branches are removed.

The algorithm explores almost the complete tree.

Worst-case performance approaches:

```text
O(n!)
```

---

# 16. Java Implementation

> **Note:** The classical **Job Sequencing with Deadlines** problem is optimally solved using a Greedy algorithm. The following implementation demonstrates the **Branch and Bound search technique** for educational purposes by exploring job orders and pruning using an upper bound.

```java
import java.util.*;

class Job {
    String id;
    int profit;

    Job(String id, int profit) {
        this.id = id;
        this.profit = profit;
    }
}

class Node {
    List<Integer> schedule;
    boolean[] used;
    int currentProfit;
    int bound;

    Node(List<Integer> schedule, boolean[] used,
         int currentProfit, int bound) {

        this.schedule = schedule;
        this.used = used;
        this.currentProfit = currentProfit;
        this.bound = bound;
    }
}

public class BranchAndBoundJobSequencing {

    static int bestProfit = Integer.MIN_VALUE;
    static List<Integer> bestSchedule = new ArrayList<>();

    static int calculateBound(Job[] jobs,
                              boolean[] used,
                              int currentProfit) {

        int bound = currentProfit;

        for (int i = 0; i < jobs.length; i++) {
            if (!used[i])
                bound += jobs[i].profit;
        }

        return bound;
    }

    static void branchAndBound(Job[] jobs,
                               List<Integer> schedule,
                               boolean[] used,
                               int currentProfit) {

        if (schedule.size() == jobs.length) {

            if (currentProfit > bestProfit) {
                bestProfit = currentProfit;
                bestSchedule = new ArrayList<>(schedule);
            }

            return;
        }

        int bound = calculateBound(jobs, used, currentProfit);

        if (bound <= bestProfit)
            return;

        for (int i = 0; i < jobs.length; i++) {

            if (!used[i]) {

                used[i] = true;
                schedule.add(i);

                branchAndBound(
                        jobs,
                        schedule,
                        used,
                        currentProfit + jobs[i].profit
                );

                schedule.remove(schedule.size() - 1);
                used[i] = false;
            }
        }
    }

    public static void main(String[] args) {

        Job[] jobs = {

                new Job("A", 80),
                new Job("B", 70),
                new Job("C", 60),
                new Job("D", 40)

        };

        boolean[] used = new boolean[jobs.length];

        branchAndBound(
                jobs,
                new ArrayList<>(),
                used,
                0
        );

        System.out.println("Best Profit = " + bestProfit);

        System.out.print("Schedule = ");

        for (int index : bestSchedule) {
            System.out.print(jobs[index].id + " ");
        }
    }
}
```

---

## Sample Output

```text
Best Profit = 250

Schedule = A B C D
```

> The exact schedule depends on the objective function and constraints used. This simplified implementation demonstrates the Branch and Bound framework rather than a specific industrial scheduling model.

---

## Dry Run (Simplified)

Jobs:

| Job | Profit |
|------|--------|
|A|80|
|B|70|
|C|60|

Initial state:

```text
Schedule = {}

Profit = 0

Bound = 210
```

Expand:

```text
A

↓

Profit = 80

Bound = 210
```

Expand:

```text
AB

↓

Profit = 150

Bound = 210
```

Complete:

```text
ABC

↓

Profit = 210
```

Update:

```text
Best Profit = 210
```

Any future node whose bound is **≤ 210** is pruned.

---

# 17. Complexity Analysis

## Time Complexity

### Best Case

Excellent pruning removes most branches.

```text
Much less than n!
```

There is no fixed polynomial bound.

---

### Average Case

Depends on:

- Quality of the bound
- Job ordering
- Problem constraints

Usually much better than brute force.

---

### Worst Case

Very little pruning occurs.

Every permutation is explored.

```text
Time Complexity

O(n!)
```

---

## Space Complexity

Depth-first implementation:

```text
O(n)
```

for recursion stack (excluding storage of solutions).

Best-first implementation:

```text
O(number of live nodes)
```

which may become exponential.

---

## Complexity Table

| Case | Time Complexity |
|------|-----------------|
|Best|Problem dependent (heavy pruning)|
|Average|Much better than brute force|
|Worst|`O(n!)`|

| Memory | Complexity |
|---------|-----------|
|DFS Branch and Bound|`O(n)` recursion stack|
|Best-First Branch and Bound|Can be exponential|

---

## Comparison with Other Approaches

| Method | Optimal | Time Complexity | Notes |
|--------|---------|----------------|------|
|Brute Force|Yes|`O(n!)`|Explores all schedules|
|Greedy|Not always|Usually polynomial|Fast but may be suboptimal|
|Dynamic Programming|Problem dependent|Varies|Works only for suitable state formulations|
|Branch and Bound|Yes|Worst case `O(n!)`|Uses pruning to reduce search|

---

## Visual Comparison

```text
Brute Force

████████████████████████████

Branch and Bound

██████████

Greedy

██

(Conceptual comparison only)
```

---

# 18. Exam Tips

## Remember the Four Core Steps

```text
Branch

↓

Bound

↓

Compare

↓

Prune
```

---

## Keywords to Include in Answers

- State Space Tree
- Live Node
- Dead Node
- Upper Bound
- Lower Bound
- Pruning
- Optimal Solution
- Search Space
- Bounding Function

---

## Common Exam Questions

1. Define Branch and Bound.
2. Explain Job Sequencing.
3. Draw a State Space Tree.
4. Explain Bound Calculation.
5. Explain Pruning with an example.
6. Write the algorithm.
7. Write the Java implementation.
8. Compare Branch and Bound with Greedy.
9. Compare Branch and Bound with Brute Force.
10. Analyze the time complexity.

---

# 19. Viva Questions

### Q1. What is Branch and Bound?

An exact optimization technique that systematically searches the solution space while pruning branches that cannot improve the current best solution.

---

### Q2. What is a State Space Tree?

A tree representing all possible partial and complete solutions.

---

### Q3. What is a Live Node?

A node that may still produce a better solution and can be expanded.

---

### Q4. What is a Dead Node?

A node that has been pruned or fully explored.

---

### Q5. What is a Bound?

An estimate of the best possible objective value achievable from a partial solution.

---

### Q6. Why is pruning safe?

Because only branches that **cannot outperform the current best solution** are removed.

---

### Q7. Does Branch and Bound always produce the optimal solution?

Yes, provided the bound is valid and pruning is performed correctly.

---

### Q8. Is Branch and Bound always fast?

No. In the worst case, it may still examine nearly all possible solutions.

---

# 20. Summary

## Key Points

- Branch and Bound is an **exact optimization algorithm**.
- It organizes the search space as a **State Space Tree**.
- **Branching** generates new partial solutions.
- **Bounding** estimates the best possible outcome from a node.
- **Pruning** removes nodes that cannot improve the current best solution.
- It guarantees the **optimal solution** while often exploring far fewer nodes than brute force.
- The effectiveness of the algorithm depends heavily on the quality of the **bounding function**.
- Worst-case complexity remains exponential, but practical performance is often much better due to pruning.

---

# Quick Revision Sheet

| Topic | One-Line Revision |
|------|--------------------|
|Branch|Generate child nodes|
|Bound|Estimate maximum possible future value|
|Prune|Discard non-promising branches|
|State Space Tree|Represents all possible schedules|
|Live Node|Eligible for expansion|
|Dead Node|Pruned or already explored|
|Goal|Find the optimal schedule|
|Worst Time|`O(n!)`|
|Space (DFS)|`O(n)` recursion stack|
|Guarantee|Always finds the optimal solution if implemented correctly|

---

# Final Exam Mnemonic

Remember the sequence:

```text
Build

↓

Branch

↓

Bound

↓

Best Comparison

↓

Prune

↓

Repeat

↓

Optimal Solution
```

This six-step workflow captures the essence of the **Branch and Bound algorithm** and is useful for quickly recalling the algorithm during exams.

---
**End of Study Guide**
