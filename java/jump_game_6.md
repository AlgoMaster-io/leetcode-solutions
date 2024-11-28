# [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

## Approach 1: Dynamic Programming with Sliding Window (Deque)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.Deque;
import java.util.LinkedList;

public class Solution {
    public int maxResult(int[] nums, int k) {
        Deque<Integer> deque = new LinkedList<>(); // Stores indices of potential max scores
        deque.offer(0);
        int[] dp = new int[nums.length];
        dp[0] = nums[0];

        for (int i = 1; i < nums.length; i++) {
            // Remove indices that are out of the current window
            if (!deque.isEmpty() && deque.peekFirst() < i - k) {
                deque.pollFirst();
            }

            // Update dp[i] using the max value in the deque
            dp[i] = nums[i] + dp[deque.peekFirst()];

            // Maintain the deque to keep indices in decreasing order of their dp values
            while (!deque.isEmpty() && dp[deque.peekLast()] <= dp[i]) {
                deque.pollLast();
            }

            deque.offerLast(i);
        }

        return dp[nums.length - 1];
    }
}
```

## Approach 2: Priority Queue (Heap-Based)

### Solution
```java
// Time Complexity: O(n log k)
// Space Complexity: O(k)
import java.util.PriorityQueue;

public class Solution {
    public int maxResult(int[] nums, int k) {
        PriorityQueue<int[]> maxHeap = new PriorityQueue<>((a, b) -> b[0] - a[0]); // Max-heap
        maxHeap.offer(new int[]{nums[0], 0}); // Store {score, index}
        int score = nums[0];

        for (int i = 1; i < nums.length; i++) {
            // Remove elements outside the current window
            while (maxHeap.peek()[1] < i - k) {
                maxHeap.poll();
            }

            // The current score is the sum of the max score in the heap and nums[i]
            score = nums[i] + maxHeap.peek()[0];
            maxHeap.offer(new int[]{score, i});
        }

        return score;
    }
}
```