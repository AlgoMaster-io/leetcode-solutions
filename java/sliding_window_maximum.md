# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## Approach 1: Using Deque (Optimal Solution)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(k)
import java.util.Deque;
import java.util.LinkedList;

public class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] result = new int[n - k + 1];
        Deque<Integer> deque = new LinkedList<>(); // Stores indices of elements

        for (int i = 0; i < n; i++) {
            // Remove indices out of the current window
            if (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
                deque.pollFirst();
            }

            // Remove elements smaller than the current element from the back
            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
                deque.pollLast();
            }

            deque.offerLast(i); // Add the current element's index to the deque

            // Add the maximum element of the current window to the result
            if (i >= k - 1) {
                result[i - k + 1] = nums[deque.peekFirst()];
            }
        }

        return result;
    }
}
```

## Approach 2: Using Priority Queue (Heap-Based)

### Solution
```java
// Time Complexity: O(n log k)
// Space Complexity: O(k)
import java.util.PriorityQueue;

public class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] result = new int[n - k + 1];
        PriorityQueue<int[]> maxHeap = new PriorityQueue<>((a, b) -> b[0] - a[0]); // Max-heap: {value, index}

        for (int i = 0; i < n; i++) {
            // Add the current element to the heap
            maxHeap.offer(new int[]{nums[i], i});

            // Remove elements that are out of the current window
            while (maxHeap.peek()[1] < i - k + 1) {
                maxHeap.poll();
            }

            // Add the maximum element of the current window to the result
            if (i >= k - 1) {
                result[i - k + 1] = maxHeap.peek()[0];
            }
        }

        return result;
    }
}
```