# 480. [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approach: Two Heaps (Max-Heap and Min-Heap)

### Solution
csharp
```csharp
// Time Complexity: O(n * log(k)), where n is the size of nums and k is the size of the window
// Space Complexity: O(k), for storing elements in the heaps
using System;
using System.Collections.Generic;

public class Solution {
    public double[] MedianSlidingWindow(int[] nums, int k) {
        int n = nums.Length;
        double[] result = new double[n - k + 1];
        
        // Max-heap for the smaller half
        var maxHeap = new SortedSet<(int value, int index)>(Comparer<(int, int)>.Create((a, b) => a.value != b.value ? b.value.CompareTo(a.value) : a.index.CompareTo(b.index)));
        // Min-heap for the larger half
        var minHeap = new SortedSet<(int value, int index)>();
        
        for (int i = 0; i < n; i++) {
            // Add the new element into the appropriate heap
            if (maxHeap.Count == 0 || nums[i] <= maxHeap.Min.value) {
                maxHeap.Add((nums[i], i));
            } else {
                minHeap.Add((nums[i], i));
            }
            
            // Rebalance the heaps if necessary
            if (maxHeap.Count > minHeap.Count + 1) {
                var max = maxHeap.Min;
                maxHeap.Remove(max);
                minHeap.Add(max);
            } else if (minHeap.Count > maxHeap.Count) {
                var min = minHeap.Min;
                minHeap.Remove(min);
                maxHeap.Add(min);
            }
            
            // Remove the element that is sliding out of the window
            if (i >= k) {
                var elementToRemove = nums[i - k];
                if (elementToRemove <= maxHeap.Min.value) {
                    maxHeap.Remove((elementToRemove, i - k));
                } else {
                    minHeap.Remove((elementToRemove, i - k));
                }
                
                // Rebalance the heaps after removal
                if (maxHeap.Count > minHeap.Count + 1) {
                    var max = maxHeap.Min;
                    maxHeap.Remove(max);
                    minHeap.Add(max);
                } else if (minHeap.Count > maxHeap.Count) {
                    var min = minHeap.Min;
                    minHeap.Remove(min);
                    maxHeap.Add(min);
                }
            }
            
            // Add the median to the result when the window is fully formed
            if (i >= k - 1) {
                if (maxHeap.Count == minHeap.Count) {
                    result[i - k + 1] = ((double) maxHeap.Min.value + minHeap.Min.value) / 2;
                } else {
                    result[i - k + 1] = maxHeap.Min.value;
                }
            }
        }
        
        return result;
    }
}
```

