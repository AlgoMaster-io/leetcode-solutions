# [Leetcode 295: Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Two Heaps Approach](#two-heaps-approach)

---

## Brute Force Approach

### Intuition
In the brute force approach, every time we add a new number to our data structure, we will insert it into an existing list and sort it. This will allow us to easily compute the median by accessing the middle element(s) of the sorted list.

### Approach
1. Use a dynamic array to store the numbers.
2. `addNum(int num)`: Insert the incoming number into this array and then sort it.
3. `findMedian()`: Access the middle element or the average of two middle elements to get the median.
 
### Time Complexity
- Adding a number: O(n log n) due to the sorting after each insertion.
- Finding median: O(1).
 
### Space Complexity
- O(n) to store the numbers.

### Solution in Java
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class MedianFinder {
    private List<Integer> numbers;

    /** initialize your data structure here. */
    public MedianFinder() {
        numbers = new ArrayList<>();
    }
    
    public void addNum(int num) {
        // Add the number to the list
        numbers.add(num);
        // Sort the list to have numbers ordered
        Collections.sort(numbers);
    }
    
    public double findMedian() {
        int size = numbers.size();
        // If the size is odd, return the middle element
        if (size % 2 == 1) {
            return numbers.get(size / 2);
        } 
        // If the size is even, return the average of the two middle elements
        return (numbers.get(size / 2 - 1) + numbers.get(size / 2)) / 2.0;
    }
}
```

---

## Two Heaps Approach

### Intuition
To optimize the process, we can use two heaps:
- A max heap for the left part
- A min heap for the right part

The median can be quickly found by:
- Taking the top of one or both heaps, depending on the number of elements.

### Approach
1. Maintain two heaps:
   - A max heap to store the smaller half of the numbers.
   - A min heap to store the larger half of the numbers.
2. `addNum(int num)`:
   - If the new number is less than the max in max heap, add to max heap; otherwise, add to the min heap.
   - Balance the heaps so they contain equal numbers, or max heap has one extra.
3. `findMedian()`:
   - If both heaps have the same size, the median is the average of the tops of both heaps.
   - If max heap has one more element, the median is its top.

### Time Complexity
- Adding a number: O(log n) due to the insertion operation in heaps.
- Finding median: O(1).

### Space Complexity
- O(n) to store the numbers in the heaps.

### Solution in Java
```java
import java.util.PriorityQueue;

class MedianFinder {
    // Max heap for the lower half
    private PriorityQueue<Integer> maxHeap;
    // Min heap for the upper half
    private PriorityQueue<Integer> minHeap;

    /** initialize your data structure here. */
    public MedianFinder() {
        // Max heap for values lower than the median
        maxHeap = new PriorityQueue<>((a, b) -> b - a);
        // Min heap for values greater than or equal to the median
        minHeap = new PriorityQueue<>();
    }
    
    public void addNum(int num) {
        // Add to max heap
        if (maxHeap.isEmpty() || num <= maxHeap.peek()) {
            maxHeap.add(num);
        } else {
            // Add to min heap if greater than the maximum of max heap
            minHeap.add(num);
        }
        
        // Balance heaps: max heap can have one more element than min heap
        if (maxHeap.size() > minHeap.size() + 1) {
            minHeap.add(maxHeap.poll());
        } else if (minHeap.size() > maxHeap.size()) {
            maxHeap.add(minHeap.poll());
        }
    }
    
    public double findMedian() {
        // If max heap has one extra element, it's the median
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.peek();
        } 
        // Otherwise, the median is the average of the tops of maxHeap and minHeap
        return (maxHeap.peek() + minHeap.peek()) / 2.0;
    }
}
```

This optimized solution efficiently maintains and retrieves the median as required, performing well even for large streams of data.

