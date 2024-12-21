## [Leetcode 312: Burst Balloons](https://leetcode.com/problems/burst-balloons/)

### Navigation
- [Approach 1: Recursive with Memoization](#approach-1-recursive-with-memoization)
- [Approach 2: Dynamic Programming (Bottom-Up)](#approach-2-dynamic-programming-bottom-up)

---

### Approach 1: Recursive with Memoization

Intuition:
The problem can be viewed as a classic recursive problem with overlapping subproblems. Consider bursting all the balloons except the boundary ones (initially virtual balloons with value 1 on each side of the array). Each choice of a balloon to burst first maximizes the coins obtainable from that particular subproblem, reducing the problem into a left and right subproblem that can also be solved similarly.

Steps:
- Use a recursive function `maxCoins` with two key parameters: `left` and `right` which represent the current subarray boundaries.
- Extend the original array by padding with 1s on both sides to simplify edge cases.
- Use memoization to store results of subproblems, avoiding redundant calculations.

```csharp
public class Solution {
    public int MaxCoins(int[] nums) {
        int n = nums.Length;
        int[] points = new int[n + 2];
        points[0] = points[n + 1] = 1;
        for (int i = 1; i <= n; i++) {
            points[i] = nums[i - 1];
        }
        
        int[,] memo = new int[n + 2, n + 2];
        return Burst(memo, points, 0, n + 1);
    }
    
    private int Burst(int[,] memo, int[] points, int left, int right) {
        // If there's no more balloon between left and right
        if (left + 1 == right) return 0;
        
        if (memo[left, right] > 0) return memo[left, right];
        int result = 0;
        
        // Choose each balloon in the middle as the last one to burst
        for (int i = left + 1; i < right; i++) {
            int coins = points[left] * points[i] * points[right];
            coins += Burst(memo, points, left, i) + Burst(memo, points, i, right);
            result = Math.Max(result, coins);
        }
        
        memo[left, right] = result;
        return result;
    }
}
```

**Time Complexity**: O(N^3), where N is the number of balloons in the array.
**Space Complexity**: O(N^2), due to the memoization table.

### Approach 2: Dynamic Programming (Bottom-Up)

Intuition:
Instead of solving the problem top-down with recursion, you can think of it as a dynamic programming problem. The idea is to calculate the maximum number of coins for every possible balloon interval and gradually build up to the original problem. 

Steps:
1. Define a DP array `dp[left][right]` to represent the maximum coins obtained from bursting all the balloons between `left` and `right` (exclusive).
2. For each possible length of interval (from 2 to n+1) and each possible start of the interval, compute the maximum coins obtainable.
3. Each subproblem tries every possible `k` as the last balloon to burst in `[left, right]` for maximum gain.

```csharp
public class Solution {
    public int MaxCoins(int[] nums) {
        int n = nums.Length;
        int[] points = new int[n + 2];
        points[0] = points[n + 1] = 1;
        for (int i = 1; i <= n; i++) {
            points[i] = nums[i - 1];
        }
        
        int[,] dp = new int[n + 2, n + 2];
        
        for (int length = 2; length <= n + 1; length++) {
            for (int left = 0; left <= n + 1 - length; left++) {
                int right = left + length;
                for (int i = left + 1; i < right; i++) {
                    dp[left, right] = Math.Max(dp[left, right], points[left] * points[i] * points[right] 
                                               + dp[left, i] + dp[i, right]);
                }
            }
        }
        
        return dp[0, n + 1];
    }
}
```

**Time Complexity**: O(N^3), because of the triple nested loops iterating over possible lengths, left indices, and balloon indices.
**Space Complexity**: O(N^2), required for the DP table.

---

Each approach offers a way to tackle this problem with nuanced differences in how they build up the solution. The recursive solution is intuitive, leveraging memoization to refine its time complexity. On the other hand, the dynamic programming approach provides a clear bottom-up methodology, representing a fully iterative solution.

