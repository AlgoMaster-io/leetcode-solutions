# [LeetCode 312: Burst Balloons](https://leetcode.com/problems/burst-balloons/)

## Approaches:
- [Dynamic Programming (Memoization)](#dynamic-programming-memoization)
- [Dynamic Programming (Tabulation)](#dynamic-programming-tabulation)

### Dynamic Programming (Memoization)

#### Intuition
The key observation in this problem is that the order of bursting balloons does not matter globally, but it matters locally for a particular subproblem. We can think of each balloon as contributing coins when it is the last balloon of any subsequence to be burst. We can use dynamic programming to solve this by dividing the problem into subproblems, where we consider bursting a balloon last in a specific range, and compute the maximum coins obtainable.

Let's use a dynamic programming table `dp` where `dp[left][right]` represents the maximum coins obtainable by bursting all the balloons between index `left` and `right` (exclusive).

#### Detailed Steps
1. Use an auxiliary array `nums` with additional boundary balloons (values 1 at both ends), because the edge balloons behave as if they are surrounded by 1.
2. Use a helper function `maxCoins(left, right)` which uses memoization to store already computed results.
3. For each balloon `i` between `left` and `right`, calculate the coins gained by making it the last burst balloon, plus the resulting coins from the two partitions `(left, i)` and `(i, right)`.
4. Memorize and return the maximum result for subproblem `maxCoins(left, right)`.

```java
public class Solution {
    public int maxCoins(int[] iNums) {
        int[] nums = new int[iNums.length + 2];
        int n = 1;
        for (int x : iNums) nums[n++] = x;
        nums[0] = nums[n++] = 1;

        // Memoization table
        int[][] dp = new int[n][n];
        return burst(dp, nums, 0, n - 1);
    }

    private int burst(int[][] dp, int[] nums, int left, int right) {
        if (left + 1 == right) return 0; // Base case: no balloons
        if (dp[left][right] > 0) return dp[left][right]; // Already computed

        int maxCoins = 0;
        // i is the index for the last burst in a section nums[left]...nums[right]
        for (int i = left + 1; i < right; ++i) {
            maxCoins = Math.max(maxCoins, nums[left] * nums[i] * nums[right]
                + burst(dp, nums, left, i) // coins from the left section
                + burst(dp, nums, i, right)); // coins from the right section
        }
        dp[left][right] = maxCoins;
        return maxCoins;
    }
}
```

**Time Complexity**: \(O(n^3)\) - We have two loops and each subproblem solution involves a linear work due to memoization.

**Space Complexity**: \(O(n^2)\) - Due to the extra space for the memoization table.

### Dynamic Programming (Tabulation)

#### Intuition
This approach is similar to the memoization approach, but we fill up our table in a bottom-up manner instead. We compute the solution for smaller subproblems first and use them to build up the solution for larger subproblems.

#### Detailed Steps
1. We still prepare an auxiliary array `nums` with boundary balloons.
2. Initialize a DP table where `dp[i][j]` will represent the maximum coins obtainable by bursting all the balloons between index `i` and `j` (exclusive).
3. Use a nested loop structure to compute `dp[i][j]` for subproblems of increasing size.
4. For each `k` in current subproblem, compute maximum coins by bursting the `k-th` balloon last in its section.

```java
public class Solution {
    public int maxCoins(int[] iNums) {
        int[] nums = new int[iNums.length + 2];
        int n = 1;
        for (int x : iNums) nums[n++] = x;
        nums[0] = nums[n++] = 1;

        int[][] dp = new int[n][n];
        
        // Length is the current segment length from i to j
        for (int length = 2; length < n; ++length) {
            for (int left = 0; left < n - length; ++left) {
                int right = left + length;
                for (int i = left + 1; i < right; ++i) {
                    dp[left][right] = Math.max(dp[left][right],
                        nums[left] * nums[i] * nums[right] 
                        + dp[left][i] + dp[i][right]);
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

**Time Complexity**: \(O(n^3)\) - Due to three nested loops over the length and boundaries.

**Space Complexity**: \(O(n^2)\) - Due to the space needed for the DP table.

