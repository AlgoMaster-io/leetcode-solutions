# [1499. Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approach 1: Using a Priority Queue (Heap-Based)

### Solution
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int FindMaxValueOfEquation(int[][] points, int k) {
        SortedSet<Tuple<int, int>> maxHeap = new SortedSet<Tuple<int, int>>(Comparer<Tuple<int, int>>.Create((a, b) => b.Item1 != a.Item1 ? b.Item1.CompareTo(a.Item1) : b.Item2.CompareTo(a.Item2))); // Max-heap: {y - x, x}
        int maxValue = int.MinValue;

        foreach (var point in points) {
            int x = point[0], y = point[1];

            // Remove points outside the range of k
            while (maxHeap.Count > 0 && x - maxHeap.Max.Item2 > k) {
                maxHeap.Remove(maxHeap.Max);
            }

            // If the heap is not empty, calculate the current value of the equation
            if (maxHeap.Count > 0) {
                maxValue = Math.Max(maxValue, maxHeap.Max.Item1 + y + x);
            }

            // Add the current point to the heap
            maxHeap.Add(Tuple.Create(y - x, x));
        }

        return maxValue;
    }
}
```

## Approach 2: Using a Deque (Optimal Solution)

### Solution
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int FindMaxValueOfEquation(int[][] points, int k) {
        LinkedList<Tuple<int, int>> deque = new LinkedList<Tuple<int, int>>(); // Stores pairs {y - x, x}
        int maxValue = int.MinValue;

        foreach (var point in points) {
            int x = point[0], y = point[1];

            // Remove points outside the range of k
            while (deque.Count > 0 && x - deque.First.Value.Item2 > k) {
                deque.RemoveFirst();
            }

            // If the deque is not empty, calculate the current value of the equation
            if (deque.Count > 0) {
                maxValue = Math.Max(maxValue, deque.First.Value.Item1 + y + x);
            }

            // Remove points from the back of the deque that have a smaller y - x value
            while (deque.Count > 0 && deque.Last.Value.Item1 <= y - x) {
                deque.RemoveLast();
            }

            // Add the current point to the deque
            deque.AddLast(Tuple.Create(y - x, x));
        }

        return maxValue;
    }
}
```

