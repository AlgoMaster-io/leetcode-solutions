# 973. [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approach 1: Max-Heap

### Solution
csharp
```csharp
// Time Complexity: O(n * log(k)), where n is the number of points
// Space Complexity: O(k), for the max-heap
using System;
using System.Collections.Generic;

public class Solution {
    public int[][] KClosest(int[][] points, int k) {
        // Max-Heap to store the k closest points
        var maxHeap = new SortedSet<(int Distance, int[] Point)>(Comparer<(int, int[])>.Create((a, b) => {
            int comparison = b.Distance.CompareTo(a.Distance);
            if (comparison == 0) {
                return a.Point[0].CompareTo(b.Point[0]);
            }
            return comparison;
        }));

        // Add points to the heap and maintain only the k closest points
        foreach (var point in points) {
            maxHeap.Add((Distance(point), point));
            if (maxHeap.Count > k) {
                maxHeap.Remove(maxHeap.Max); // Remove the farthest point
            }
        }

        // Extract the k closest points from the heap
        var result = new int[k][];
        int i = 0;
        foreach (var entry in maxHeap) {
            result[i] = entry.Point;
            i++;
        }

        return result;
    }

    // Helper method to calculate the squared distance from the origin
    private int Distance(int[] point) {
        return point[0] * point[0] + point[1] * point[1];
    }
}
```

## Approach 2: Quickselect

### Solution
csharp
```csharp
// Time Complexity: O(n) on average, O(n^2) in the worst case
// Space Complexity: O(1), as it modifies the input array in-place
using System;

public class Solution {
    public int[][] KClosest(int[][] points, int k) {
        QuickSelect(points, 0, points.Length - 1, k);
        var result = new int[k][];
        Array.Copy(points, result, k);
        return result;
    }

    private void QuickSelect(int[][] points, int left, int right, int k) {
        if (left >= right) return;

        int pivotIndex = Partition(points, left, right);
        if (pivotIndex == k) {
            return;
        } else if (pivotIndex < k) {
            QuickSelect(points, pivotIndex + 1, right, k);
        } else {
            QuickSelect(points, left, pivotIndex - 1, k);
        }
    }

    private int Partition(int[][] points, int left, int right) {
        int[] pivot = points[right];
        int pivotDistance = Distance(pivot);
        int i = left;

        for (int j = left; j < right; j++) {
            if (Distance(points[j]) < pivotDistance) {
                Swap(points, i, j);
                i++;
            }
        }
        Swap(points, i, right);
        return i;
    }

    private void Swap(int[][] points, int i, int j) {
        int[] temp = points[i];
        points[i] = points[j];
        points[j] = temp;
    }

    private int Distance(int[] point) {
        return point[0] * point[0] + point[1] * point[1];
    }
}
```

