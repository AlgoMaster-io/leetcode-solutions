# [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

## Approach 1: Sorting

### Solution
csharp
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(1)
using System;

public class Solution {
    public int FindKthLargest(int[] nums, int k) {
        Array.Sort(nums); // Sort the array in ascending order
        return nums[nums.Length - k]; // Return the kth largest element
    }
}
```

## Approach 2: Using a Min-Heap

### Solution
csharp
```csharp
// Time Complexity: O(n log k)
// Space Complexity: O(k)
using System;
using System.Collections.Generic;

public class Solution {
    public int FindKthLargest(int[] nums, int k) {
        PriorityQueue<int, int> minHeap = new PriorityQueue<int, int>(); // Min-heap to keep the top k elements

        foreach (int num in nums) {
            minHeap.Enqueue(num, num); // Add the current number to the heap
            if (minHeap.Count > k) {
                minHeap.Dequeue(); // Remove the smallest element if the heap size exceeds k
            }
        }

        return minHeap.Peek(); // The root of the heap is the kth largest element
    }
}
```

## Approach 3: Quickselect (Partitioning)

### Solution
csharp
```csharp
// Time Complexity: O(n) on average, O(n^2) in the worst case
// Space Complexity: O(1)
using System;

public class Solution {
    public int FindKthLargest(int[] nums, int k) {
        int n = nums.Length;
        return QuickSelect(nums, 0, n - 1, n - k);
    }

    private int QuickSelect(int[] nums, int left, int right, int k) {
        if (left == right) {
            return nums[left]; // Base case: only one element
        }

        var random = new Random();
        int pivotIndex = left + random.Next(right - left + 1);

        pivotIndex = Partition(nums, left, right, pivotIndex);

        if (pivotIndex == k) {
            return nums[k]; // Found the kth largest element
        } else if (pivotIndex < k) {
            return QuickSelect(nums, pivotIndex + 1, right, k); // Search in the right part
        } else {
            return QuickSelect(nums, left, pivotIndex - 1, k); // Search in the left part
        }
    }

    private int Partition(int[] nums, int left, int right, int pivotIndex) {
        int pivotValue = nums[pivotIndex];
        Swap(nums, pivotIndex, right); // Move pivot to the end
        int storeIndex = left;

        for (int i = left; i < right; i++) {
            if (nums[i] < pivotValue) {
                Swap(nums, storeIndex, i);
                storeIndex++;
            }
        }

        Swap(nums, storeIndex, right); // Move pivot to its final position
        return storeIndex;
    }

    private void Swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

