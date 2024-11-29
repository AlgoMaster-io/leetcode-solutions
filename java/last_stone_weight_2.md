# 1049. [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

## Approach 1: Dynamic Programming with a Set

### Solution
```java
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public int lastStoneWeightII(int[] stones) {
        Set<Integer> dp = new HashSet<>();
        dp.add(0);
        int sum = 0;

        // Iterate over each stone
        for (int stone : stones) {
            Set<Integer> newDp = new HashSet<>();
            // Update possible sums with and without the current stone
            for (int s : dp) {
                newDp.add(s + stone);
                newDp.add(s - stone);
            }
            // Move to the new set of possible sums
            dp = newDp;
            sum += stone;
        }

        // Find the minimum possible non-negative sum (half the total sum)
        int minAbs = Integer.MAX_VALUE;
        for (int s : dp) {
            if (s >= 0) {
                minAbs = Math.min(minAbs, s);
            }
        }
        
        return minAbs;
    }
}
```

## Approach 2: Dynamic Programming with Boolean Array

### Solution
```java
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
public class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for (int stone : stones) {
            sum += stone;
        }
        boolean[] dp = new boolean[sum / 2 + 1];
        dp[0] = true;

        // Evaluate possible subset sums
        for (int stone : stones) {
            for (int j = dp.length - 1; j >= stone; j--) {
                dp[j] = dp[j] || dp[j - stone];
            }
        }

        // Find the largest possible subset sum that is <= sum/2
        for (int j = dp.length - 1; j >= 0; j--) {
            if (dp[j]) {
                return sum - 2 * j;
            }
        }

        return 0;
    }
}
```

## Approach 3: Optimized DP with Single Array

### Solution
```java
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
public class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for (int stone : stones) {
            sum += stone;
        }
        int target = sum / 2;
        int[] dp = new int[target + 1];

        // Calculate optimized subset sums
        for (int stone : stones) {
            for (int j = target; j >= stone; j--) {
                dp[j] = Math.max(dp[j], dp[j - stone] + stone);
            }
        }

        // Result is the difference between total sum and twice the best subset sum
        return sum - 2 * dp[target];
    }
}
```


