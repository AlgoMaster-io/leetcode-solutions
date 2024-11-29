# 373. [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Approach: Min-Heap

### Solution
csharp
```csharp
// Time Complexity: O(k * log(k)), where k is the number of pairs to find
// Space Complexity: O(k), for the heap
using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> KSmallestPairs(int[] nums1, int[] nums2, int k) {
        // Min-heap to store pairs along with their sums
        PriorityQueue<int[], int> minHeap = new PriorityQueue<int[], int>(Comparer<int>.Create((a, b) => 
            (a[0] + a[1]).CompareTo(b[0] + b[1]))
        );

        IList<IList<int>> result = new List<IList<int>>();

        // Initialize the heap with pairs (nums1[i], nums2[0]) for all i
        for (int i = 0; i < Math.Min(nums1.Length, k); i++) {
            minHeap.Enqueue(new int[] {nums1[i], nums2[0], 0}, nums1[i] + nums2[0]);
        }

        // Extract the smallest pairs until we have k pairs or the heap is empty
        while (minHeap.Count > 0 && k > 0) {
            int[] current = minHeap.Dequeue();
            result.Add(new List<int> { current[0], current[1] });
            k--;

            // If there are more pairs with the current nums1[i], add them to the heap
            if (current[2] + 1 < nums2.Length) {
                minHeap.Enqueue(new int[] {current[0], nums2[current[2] + 1], current[2] + 1}, current[0] + nums2[current[2] + 1]);
            }
        }

        return result;
    }
}
```

