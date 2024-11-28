# [1499. Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approach 1: Using a Priority Queue (Heap-Based)

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import java.util.PriorityQueue;

public class Solution {
    public int findMaxValueOfEquation(int[][] points, int k) {
        PriorityQueue<int[]> maxHeap = new PriorityQueue<>((a, b) -> b[0] - a[0]); // Max-heap: {y - x, x}
        int maxValue = Integer.MIN_VALUE;

        for (int[] point : points) {
            int x = point[0], y = point[1];

            // Remove points outside the range of k
            while (!maxHeap.isEmpty() && x - maxHeap.peek()[1] > k) {
                maxHeap.poll();
            }

            // If the heap is not empty, calculate the current value of the equation
            if (!maxHeap.isEmpty()) {
                maxValue = Math.max(maxValue, maxHeap.peek()[0] + y + x);
            }

            // Add the current point to the heap
            maxHeap.offer(new int[]{y - x, x});
        }

        return maxValue;
    }
}
```

## Approach 2: Using a Deque (Optimal Solution)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.Deque;
import java.util.LinkedList;

public class Solution {
    public int findMaxValueOfEquation(int[][] points, int k) {
        Deque<int[]> deque = new LinkedList<>(); // Stores pairs {y - x, x}
        int maxValue = Integer.MIN_VALUE;

        for (int[] point : points) {
            int x = point[0], y = point[1];

            // Remove points outside the range of k
            while (!deque.isEmpty() && x - deque.peekFirst()[1] > k) {
                deque.pollFirst();
            }

            // If the deque is not empty, calculate the current value of the equation
            if (!deque.isEmpty()) {
                maxValue = Math.max(maxValue, deque.peekFirst()[0] + y + x);
            }

            // Remove points from the back of the deque that have a smaller y - x value
            while (!deque.isEmpty() && deque.peekLast()[0] <= y - x) {
                deque.pollLast();
            }

            // Add the current point to the deque
            deque.offerLast(new int[]{y - x, x});
        }

        return maxValue;
    }
}
```