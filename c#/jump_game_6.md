# [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

## Approach 1: Dynamic Programming with Sliding Window (Deque)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int MaxResult(int[] nums, int k) {
        Deque<int> deque = new Deque<int>(); // Stores indices of potential max scores
        deque.AddLast(0);
        int[] dp = new int[nums.Length];
        dp[0] = nums[0];

        for (int i = 1; i < nums.Length; i++) {
            // Remove indices that are out of the current window
            if (deque.Count > 0 && deque.First.Value < i - k) {
                deque.RemoveFirst();
            }

            // Update dp[i] using the max value in the deque
            dp[i] = nums[i] + dp[deque.First.Value];

            // Maintain the deque to keep indices in decreasing order of their dp values
            while (deque.Count > 0 && dp[deque.Last.Value] <= dp[i]) {
                deque.RemoveLast();
            }

            deque.AddLast(i);
        }

        return dp[nums.Length - 1];
    }
}
```

## Approach 2: Priority Queue (Heap-Based)

### Solution
csharp
```csharp
// Time Complexity: O(n log k)
// Space Complexity: O(k)
using System.Collections.Generic;

public class Solution {
    public int MaxResult(int[] nums, int k) {
        var maxHeap = new PriorityQueue<(int score, int index)>(Comparer<(int, int)>.Create((a, b) => b.score.CompareTo(a.score))); // Max-heap
        maxHeap.Enqueue((nums[0], 0)); // Store {score, index}
        int score = nums[0];

        for (int i = 1; i < nums.Length; i++) {
            // Remove elements outside the current window
            while (maxHeap.Peek().index < i - k) {
                maxHeap.Dequeue();
            }

            // The current score is the sum of the max score in the heap and nums[i]
            score = nums[i] + maxHeap.Peek().score;
            maxHeap.Enqueue((score, i));
        }

        return score;
    }
}
```

Note: As of C# 9.0, there isn't a direct equivalent of Java's `PriorityQueue`, so I used `PriorityQueue<TElement, TPriority>` from .NET 6. You may need to adjust or implement a priority queue for your specific version of C#.

