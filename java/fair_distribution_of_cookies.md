# 2305. [Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies/)

## Approach 1: Brute Force - Generate All Distributions

### Solution
```java
// Time Complexity: O(k^n)
// Space Complexity: O(k)
public class Solution {
    public int distributeCookies(int[] cookies, int k) {
        return helper(cookies, new int[k], 0);
    }

    private int helper(int[] cookies, int[] distribution, int index) {
        if (index == cookies.length) {
            int max = Integer.MIN_VALUE;
            for (int d : distribution) {
                max = Math.max(max, d);
            }
            return max;
        }

        int minUnfairness = Integer.MAX_VALUE;
        for (int i = 0; i < distribution.length; i++) {
            distribution[i] += cookies[index];
            minUnfairness = Math.min(minUnfairness, helper(cookies, distribution, index + 1));
            distribution[i] -= cookies[index];
        }
        return minUnfairness;
    }
}
```

## Approach 2: Backtracking with Pruning

### Solution
```java
// Time Complexity: O(k * 2^n)
// Space Complexity: O(k)
public class Solution {
    public int distributeCookies(int[] cookies, int k) {
        int[] distribution = new int[k];
        return helper(cookies, distribution, 0, 0, Integer.MAX_VALUE);
    }

    private int helper(int[] cookies, int[] distribution, int index, int currentMax, int minUnfairness) {
        if (index == cookies.length) {
            return Math.min(minUnfairness, currentMax);
        }

        for (int i = 0; i < distribution.length; i++) {
            distribution[i] += cookies[index];
            minUnfairness = helper(cookies, distribution, index + 1, Math.max(currentMax, distribution[i]), minUnfairness);
            distribution[i] -= cookies[index];

            if (distribution[i] == 0) { // Optimizing to prevent unnecessary states
                break;
            }
        }
        return minUnfairness;
    }
}
```

## Approach 3: Dynamic Programming with Bitmask

### Solution
```java
// Time Complexity: O(n*2^n)
// Space Complexity: O(2^n)
public class Solution {
    public int distributeCookies(int[] cookies, int k) {
        int n = cookies.length;
        int[] sum = new int[1 << n];
        
        // Precompute the sum of each subset
        for (int i = 0; i < (1 << n); i++) {
            for (int j = 0; j < n; j++) {
                if ((i & (1 << j)) != 0) {
                    sum[i] += cookies[j];
                }
            }
        }

        // dp[mask] is the number of k subsets with the j-th subset (sum of mask is taken) having the minimum maximum sum
        int[][] dp = new int[k + 1][1 << n];
        for (int i = 0; i < dp.length; i++) {
            for (int j = 0; j < dp[i].length; j++) {
                dp[i][j] = Integer.MAX_VALUE;
            }
        }
        
        dp[0][0] = 0; 

        for (int i = 1; i <= k; i++) {
            for (int mask = 0; mask < (1 << n); mask++) {
                for (int submask = mask; submask > 0; submask = (submask - 1) & mask) {
                    dp[i][mask] = Math.min(dp[i][mask], Math.max(dp[i - 1][mask ^ submask], sum[submask]));
                }
            }
        }
        
        return dp[k][(1 << n) - 1];
    }
}
```

