# [Jump Game VI - LeetCode](https://leetcode.com/problems/jump-game-vi/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming with Deque](#dynamic-programming-with-deque)
3. [Dynamic Programming with Priority Queue](#dynamic-programming-with-priority-queue)

---

## Brute Force Approach

### Intuition

In the brute force approach, the idea is to explore every possible path by jumping up to `k` steps forward and choose the path that yields the maximum score. This involves recursively jumping from the current position all the way to the `n-1` index and calculating the score for each path.

### Code

```java
public class JumpGameVI {
    public int maxResult(int[] nums, int k) {
        return dfs(nums, 0, k);
    }

    private int dfs(int[] nums, int index, int k) {
        // If we reach the last index, we return its value
        if (index == nums.length - 1) return nums[index];
        
        int maxScore = Integer.MIN_VALUE;
        
        // Try each possible jump from current index, limited by k
        for (int jump = 1; jump <= k && index + jump < nums.length; jump++) {
            int score = nums[index] + dfs(nums, index + jump, k);
            maxScore = Math.max(maxScore, score);
        }
        
        return maxScore;
    }
}
```

### Time and Space Complexity

- **Time Complexity**: O(k^n), as each step can branch into up to `k` further recursive calls.
- **Space Complexity**: O(n), due to the recursion stack that can go as deep as `n`.

---

## Dynamic Programming with Deque

### Intuition

To achieve better performance, we utilize a deque to maintain a sliding window of size `k` over the array and store indices. The front of the deque will always represent the maximum possible score we can get at that point. This allows calculating the maximum score at each step in constant time.

### Code

```java
import java.util.Deque;
import java.util.LinkedList;

public class JumpGameVI {
    public int maxResult(int[] nums, int k) {
        // Deque to keep indices of potential max scores in window
        Deque<Integer> deque = new LinkedList<>();
        deque.offer(0); // Initialize with the first element
        
        // DP array to store maximum score to reach each index
        int[] dp = new int[nums.length];
        dp[0] = nums[0];

        for (int i = 1; i < nums.length; i++) {
            // Remove indices that are out of the sliding window
            while (!deque.isEmpty() && deque.peek() < i - k) {
                deque.poll();
            }
            
            // Calculate the maximum score for current index
            dp[i] = nums[i] + dp[deque.peek()];
            
            // Maintain the deque for potential max scores, removing less useful indices
            while (!deque.isEmpty() && dp[i] >= dp[deque.peekLast()]) {
                deque.pollLast();
            }

            // Add current index to deque
            deque.offer(i);
        }

        return dp[nums.length - 1];
    }
}
```

### Time and Space Complexity

- **Time Complexity**: O(n), each element is processed at most twice.
- **Space Complexity**: O(n), due to the storage of the dp array and deque.

---

## Dynamic Programming with Priority Queue

### Intuition

This approach uses a priority queue to maintain the maximum scores at each index similar to the deque strategy. The priority queue will always allow us to peek the current maximum score in logarithmic time.

### Code

```java
import java.util.PriorityQueue;

public class JumpGameVI {
    public int maxResult(int[] nums, int k) {
        // Priority Queue to keep pairs of (score, index)
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[0] - a[0]);
        pq.offer(new int[]{nums[0], 0});
        int maxScore = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            // Remove elements that are out of bounds of the current window size `k`
            while (!pq.isEmpty() && pq.peek()[1] < i - k) {
                pq.poll();
            }

            maxScore = nums[i] + pq.peek()[0];
            
            // Add the current max score and index
            pq.offer(new int[]{maxScore, i});
        }
        
        return maxScore;
    }
}
```

### Time and Space Complexity

- **Time Complexity**: O(n log k), due to the insert and remove operations on the priority queue.
- **Space Complexity**: O(n), as we store scores and their respective indices.

---

These solutions demonstrate different approaches to solving Jump Game VI, each optimizing time and space usage efficiently using classic data structures such as deques and priority queues.

