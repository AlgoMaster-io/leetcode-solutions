# 295. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Approach: Two Heaps

### Solution
csharp
```csharp
// Time Complexity: O(log(n)) for adding a number, O(1) for finding the median
// Space Complexity: O(n), where n is the number of elements in the data stream
using System;
using System.Collections.Generic;

public class MedianFinder {
    private PriorityQueue<int, int> maxHeap; // To store the smaller half of the numbers
    private PriorityQueue<int, int> minHeap; // To store the larger half of the numbers

    public MedianFinder() {
        maxHeap = new PriorityQueue<int, int>(Comparer<int>.Create((a, b) => b - a)); // Max-heap
        minHeap = new PriorityQueue<int, int>(); // Min-heap
    }

    public void AddNum(int num) {
        // Add to maxHeap, then balance by adding the largest of maxHeap to minHeap
        maxHeap.Enqueue(num, num);
        minHeap.Enqueue(maxHeap.Dequeue(), maxHeap.Peek());

        // Ensure maxHeap always has the same number or one more element than minHeap
        if (maxHeap.Count < minHeap.Count) {
            maxHeap.Enqueue(minHeap.Dequeue(), minHeap.Peek());
        }
    }

    public double FindMedian() {
        if (maxHeap.Count > minHeap.Count) {
            return maxHeap.Peek(); // Odd number of elements
        }
        return (maxHeap.Peek() + minHeap.Peek()) / 2.0; // Even number of elements
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.AddNum(num);
 * double param_2 = obj.FindMedian();
 */
```

