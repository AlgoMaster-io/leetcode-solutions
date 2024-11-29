# 1049. [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

## Approach 1: Dynamic Programming with a Set

### Solution
csharp
```csharp
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
using System;
using System.Collections.Generic;

public class Solution {
    public int LastStoneWeightII(int[] stones) {
        HashSet<int> dp = new HashSet<int>();
        dp.Add(0);
        int sum = 0;

        // Iterate over each stone
        foreach (int stone in stones) {
            HashSet<int> newDp = new HashSet<int>();
            // Update possible sums with and without the current stone
            foreach (int s in dp) {
                newDp.Add(s + stone);
                newDp.Add(s - stone);
            }
            // Move to the new set of possible sums
            dp = newDp;
            sum += stone;
        }

        // Find the minimum possible non-negative sum (half the total sum)
        int minAbs = int.MaxValue;
        foreach (int s in dp) {
            if (s >= 0) {
                minAbs = Math.Min(minAbs, s);
            }
        }
        
        return minAbs;
    }
}
```

## Approach 2: Dynamic Programming with Boolean Array

### Solution
csharp
```csharp
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
public class Solution {
    public int LastStoneWeightII(int[] stones) {
        int sum = 0;
        foreach (int stone in stones) {
            sum += stone;
        }
        bool[] dp = new bool[sum / 2 + 1];
        dp[0] = true;

        // Evaluate possible subset sums
        foreach (int stone in stones) {
            for (int j = dp.Length - 1; j >= stone; j--) {
                dp[j] = dp[j] || dp[j - stone];
            }
        }

        // Find the largest possible subset sum that is <= sum/2
        for (int j = dp.Length - 1; j >= 0; j--) {
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
csharp
```csharp
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
public class Solution {
    public int LastStoneWeightII(int[] stones) {
        int sum = 0;
        foreach (int stone in stones) {
            sum += stone;
        }
        int target = sum / 2;
        int[] dp = new int[target + 1];

        // Calculate optimized subset sums
        foreach (int stone in stones) {
            for (int j = target; j >= stone; j--) {
                dp[j] = Math.Max(dp[j], dp[j - stone] + stone);
            }
        }

        // Result is the difference between total sum and twice the best subset sum
        return sum - 2 * dp[target];
    }
}
```

