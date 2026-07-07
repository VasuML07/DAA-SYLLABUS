# Divide and Conquer Algorithms

> **Note:** This is a GitHub-ready study guide covering the Divide and
> Conquer paradigm, Merge Sort, Quick Sort, and Binary Search.

## Table of Contents

-   [Divide and Conquer](#divide-and-conquer)
-   [Merge Sort](#merge-sort)
-   [Quick Sort](#quick-sort)
-   [Binary Search](#binary-search)
-   [Complexity Comparison](#complexity-comparison)
-   [Exam Tips](#exam-tips)

# Divide and Conquer

## Definition

Divide and Conquer is an algorithmic paradigm that:

1.  Divide the problem into smaller subproblems.
2.  Conquer each subproblem recursively.
3.  Combine results to obtain the final answer.

```{=html}
<!-- -->
```
                  Problem
                     │
          ┌──────────┴──────────┐
          │                     │
     Subproblem A        Subproblem B
          │                     │
       Solve                Solve
          └──────────┬──────────┘
                     │
                  Combine
                     │
                  Solution

### Advantages

-   Reduces large problems into manageable parts.
-   Enables recursive solutions.
-   Often reduces time complexity dramatically.
-   Suitable for parallel execution.

### General Complexity

T(n)=aT(n/b)+f(n)

Master Theorem is commonly used to analyze such recurrences.

------------------------------------------------------------------------

# Merge Sort

## Why it's used

Efficient stable sorting with guaranteed O(n log n).

## When it's used

-   Large datasets
-   External sorting
-   Stable sorting requirement

## Where it's used

-   Database systems
-   External file sorting
-   Java stable sorting implementations for objects

## Locate

-   Java: Arrays.sort(Object\[\])
-   Collections.sort()

## Working

    38 27 43 3 9 82 10

            Divide
                │
     ┌──────────┴──────────┐
    38 27 43 3      9 82 10
         ...
                │
              Merge
                │
    3 9 10 27 38 43 82

## Algorithm

1.  Divide array into halves.
2.  Recursively sort both halves.
3.  Merge sorted halves.

## Java Implementation

``` java
public class MergeSort {

    public static void mergeSort(int[] arr, int left, int right) {
        if (left >= right) return;

        int mid = left + (right - left) / 2;

        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);

        merge(arr, left, mid, right);
    }

    private static void merge(int[] arr, int left, int mid, int right) {

        int[] temp = new int[right - left + 1];

        int i = left;
        int j = mid + 1;
        int k = 0;

        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j])
                temp[k++] = arr[i++];
            else
                temp[k++] = arr[j++];
        }

        while (i <= mid)
            temp[k++] = arr[i++];

        while (j <= right)
            temp[k++] = arr[j++];

        for (int x = 0; x < temp.length; x++)
            arr[left + x] = temp[x];
    }
}
```

### Complexity

  Case      Time
  --------- ------------
  Best      O(n log n)
  Average   O(n log n)
  Worst     O(n log n)
  Space     O(n)

### Limitations

-   Extra memory required
-   Slower than Quick Sort on many in-memory datasets

------------------------------------------------------------------------

# Quick Sort

## Why it's used

Fast practical sorting with excellent cache performance.

## When it's used

-   In-memory arrays
-   Competitive programming
-   High-performance software

## Where it's used

-   Language libraries
-   Embedded systems
-   Search engines

## Locate

Historically used by primitive sorting implementations.

## Visualization

    Pivot = 7

    8 2 9 4 1 7 6

            ↓

    2 4 1 6 |7| 8 9

## Steps

1.  Select pivot.
2.  Partition.
3.  Recursively sort left.
4.  Recursively sort right.

## Java

``` java
public class QuickSort {

    public static void quickSort(int[] arr, int low, int high) {

        if (low < high) {

            int pivotIndex = partition(arr, low, high);

            quickSort(arr, low, pivotIndex - 1);
            quickSort(arr, pivotIndex + 1, high);
        }
    }

    private static int partition(int[] arr, int low, int high) {

        int pivot = arr[high];
        int i = low - 1;

        for (int j = low; j < high; j++) {

            if (arr[j] <= pivot) {
                i++;

                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }

        int temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;

        return i + 1;
    }
}
```

### Complexity

  Case      Time
  --------- ------------
  Best      O(n log n)
  Average   O(n log n)
  Worst     O(n²)

### Edge Cases

    Already Sorted
    1 2 3 4 5

    Bad pivot every time

    ↓

    O(n²)

------------------------------------------------------------------------

# Binary Search

## Why it's used

Searches sorted arrays efficiently.

## When

-   Sorted data
-   Large databases
-   Dictionaries

## Where

-   STL
-   Java Arrays.binarySearch()
-   Databases

## Visualization

    1 3 5 7 9 11 13

    Target = 9

    Low       Mid      High
     |---------|---------|
              7

    Move Right

            Mid

              9 ✓

## Steps

1.  Find middle.
2.  Compare.
3.  Search left/right half.

## Java

``` java
public class BinarySearch {

    public static int binarySearch(int[] arr, int target) {

        int low = 0;
        int high = arr.length - 1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            if (arr[mid] == target)
                return mid;

            if (arr[mid] < target)
                low = mid + 1;
            else
                high = mid - 1;
        }

        return -1;
    }
}
```

### Complexity

  Case      Time
  --------- ----------
  Best      O(1)
  Average   O(log n)
  Worst     O(log n)

### Limitations

-   Requires sorted data
-   Doesn't work efficiently on linked lists

------------------------------------------------------------------------

# Complexity Comparison

  Algorithm       Best         Average      Worst        Space      Stable
  --------------- ------------ ------------ ------------ ---------- --------
  Merge Sort      O(n log n)   O(n log n)   O(n log n)   O(n)       Yes
  Quick Sort      O(n log n)   O(n log n)   O(n²)        O(log n)   No
  Binary Search   O(1)         O(log n)     O(log n)     O(1)       N/A

# Exam Tips

-   Merge Sort guarantees O(n log n).
-   Quick Sort is usually fastest in practice.
-   Binary Search requires sorted input.
-   Divide and Conquer = Divide → Conquer → Combine.
