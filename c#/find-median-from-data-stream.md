# [Leetcode 295: Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach with Heaps](#optimized-approach-with-heaps)

---

### Brute Force Approach

In this approach, the idea is to maintain a list of numbers and compute the median whenever required by sorting the list.

#### Intuition

- We keep a list to store the numbers as they arrive.
- Every time we need to find the median, we sort the list and then find the middle element(s).
- If the number of elements is odd, the median is the middle element. If even, it is the average of the two middle elements.

#### Implementation

```csharp
public class MedianFinder {
    private List<int> nums;
        
    /** initialize your data structure here. */
    public MedianFinder() {
        nums = new List<int>(); // Initialize the list to store numbers
    }
    
    public void AddNum(int num) {
        nums.Add(num); // Add the new number to the list
    }
    
    public double FindMedian() {
        nums.Sort(); // Sort the list

        int n = nums.Count;
        if (n % 2 == 1) { // If the number of elements is odd
            return nums[n / 2]; // Return the middle element
        } else { // If the number of elements is even
            return (nums[(n / 2) - 1] + nums[n / 2]) / 2.0; // Return the average of the two middle elements
        }
    }
}
```

#### Time Complexity

- `AddNum`: O(1)
- `FindMedian`: O(n log n) because of the sorting operation

#### Space Complexity

- O(n) for storing the numbers

---

### Optimized Approach with Heaps

To efficiently find the median of a data stream, we can use a max-heap and a min-heap.

#### Intuition

- Use two heaps: a max-heap for the lower half of the numbers and a min-heap for the upper half.
- The max-heap contains the smallest half of the numbers but in reverse order (so we have access to the largest of the smallest).
- The min-heap contains the largest half of the numbers.
- The median can be found in O(1) time by inspecting the tops of the heaps:
  - If both heaps are of equal size, the median is the average of the tops.
  - If one heap is larger, the median is the top of that heap.

#### Implementation

```csharp
using System.Collections.Generic;

public class MedianFinder {
    private PriorityQueue<int, int> minHeap; // Min-heap for the larger half
    private PriorityQueue<int, int> maxHeap; // Max-heap for the smaller half

    /** Initialize your data structure here. */
    public MedianFinder() {
        minHeap = new PriorityQueue<int, int>(); // Min-heap by default
        maxHeap = new PriorityQueue<int, int>(Comparer<int>.Create((a, b) => b.CompareTo(a))); // Max-heap, we use reverse comparison
    }
    
    public void AddNum(int num) {
        if (maxHeap.Count == 0 || num <= maxHeap.Peek()) {
            maxHeap.Enqueue(num, num); // Add to the max-heap
        } else {
            minHeap.Enqueue(num, num); // Add to the min-heap
        }

        // Balance the heaps if necessary
        if (maxHeap.Count > minHeap.Count + 1) {
            minHeap.Enqueue(maxHeap.Dequeue(), maxHeap.Dequeue());
        } else if (minHeap.Count > maxHeap.Count) {
            maxHeap.Enqueue(minHeap.Dequeue(), minHeap.Dequeue());
        }
    }
    
    public double FindMedian() {
        if (maxHeap.Count == minHeap.Count) { // If both heaps are of the same size
            return (maxHeap.Peek() + minHeap.Peek()) / 2.0; // Median is the average of the two heap tops
        } else {
            return maxHeap.Peek(); // Max-heap has more elements, so the median is its top
        }
    }
}
```

#### Time Complexity

- `AddNum`: O(log n) due to heap operations
- `FindMedian`: O(1)

#### Space Complexity

- O(n) for storing the numbers in the heaps

This approach is optimal for streaming data as it balances insertion time complexity with quick median access.

