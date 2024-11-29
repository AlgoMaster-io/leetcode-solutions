# 703. [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approach: Min-Heap

### Solution
csharp
```csharp
// Time Complexity: O(n * log(k)) for initialization and O(log(k)) for each add operation
// Space Complexity: O(k), where k is the size of the heap
using System.Collections.Generic;

public class KthLargest {
    private PriorityQueue<int, int> minHeap;
    private int k;

    public KthLargest(int k, int[] nums) {
        this.k = k;
        minHeap = new PriorityQueue<int, int>(Comparer<int>.Create((a, b) => a.CompareTo(b)));

        // Add all elements to the heap and maintain only k elements
        foreach (int num in nums) {
            Add(num);
        }
    }

    public int Add(int val) {
        minHeap.Enqueue(val, val); // Add the new value to the heap

        // Remove the smallest element if the heap exceeds size k
        if (minHeap.Count > k) {
            minHeap.Dequeue();
        }

        return minHeap.Peek(); // The kth largest element is the root of the heap
    }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest obj = new KthLargest(k, nums);
 * int param_1 = obj.Add(val);
 */
```

