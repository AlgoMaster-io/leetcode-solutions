# 295. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Approach: Two Heaps

### Solution
```java
// Time Complexity: O(log(n)) for adding a number, O(1) for finding the median
// Space Complexity: O(n), where n is the number of elements in the data stream
import java.util.PriorityQueue;

class MedianFinder {
    private PriorityQueue<Integer> maxHeap; // To store the smaller half of the numbers
    private PriorityQueue<Integer> minHeap; // To store the larger half of the numbers

    public MedianFinder() {
        maxHeap = new PriorityQueue<>((a, b) -> b - a); // Max-heap
        minHeap = new PriorityQueue<>(); // Min-heap
    }

    public void addNum(int num) {
        // Add to maxHeap, then balance by adding the largest of maxHeap to minHeap
        maxHeap.add(num);
        minHeap.add(maxHeap.poll());

        // Ensure maxHeap always has the same number or one more element than minHeap
        if (maxHeap.size() < minHeap.size()) {
            maxHeap.add(minHeap.poll());
        }
    }

    public double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.peek(); // Odd number of elements
        }
        return (maxHeap.peek() + minHeap.peek()) / 2.0; // Even number of elements
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```