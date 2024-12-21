# [Leetcode 1049: Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

## Approaches
- [Approach 1: Recursion](#approach-1-recursion)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)

## Approach 1: Recursion

### Intuition
The problem can be reduced to a classic "Partition problem" where we need to split the stones into two groups such that their absolute difference in sum is minimized. A recursive approach involves dividing the set of stones into subsets and calculating their possible weights. The main recursive function attempts to either include or exclude a stone in a possible "bucket" or subset and recursively attempts to minimize the difference between two subsets.

### Algorithm
1. Calculate half of the total weight of all stones (`sum/2`).
2. Define a recursive function that tries to reach a sum as close to `half` as possible from available stones.
3. Base case: If no more stones are left, return the absolute difference between `2*currentSubsetSum` and `totalSum`.
4. For each stone, make an include/exclude decision and recursively compute the answer.

### Code
```java
public class Solution {
    public int lastStoneWeightII(int[] stones) {
        int totalSum = 0;
        for (int stone : stones) {
            totalSum += stone;
        }
        
        return helper(stones, stones.length - 1, totalSum, 0);
    }
    
    private int helper(int[] stones, int index, int totalSum, int currentSubsetSum) {
        if (index < 0) {
            return Math.abs(totalSum - 2 * currentSubsetSum);
        }
        
        // Recursive case: consider placing the current stone in the subset
        int include = helper(stones, index - 1, totalSum, currentSubsetSum + stones[index]);
        int exclude = helper(stones, index - 1, totalSum, currentSubsetSum);
        
        return Math.min(include, exclude);
    }
}
```
### Time and Space Complexity
- **Time Complexity:** \(O(2^n)\) where `n` is the number of stones. The recursive function potentially explores all subsets of stones.
- **Space Complexity:** \(O(n)\) due to the recursion stack space.

## Approach 2: Dynamic Programming

### Intuition
To improve upon our recursive method, we can utilize a dynamic programming approach similar to solving the subset sum problem. Our task is to find a subset of stones whose sum is as close as possible to `sum/2`. Using a DP table, we can keep track of the maximum possible sum of stone weights that can be achieved.

### Algorithm
1. Calculate `half = totalSum / 2`: our goal is to find a sum as close as possible to `half`.
2. Use a DP array `dp` where `dp[j]` will maintain whether a sum `j` can be achieved with available stones.
3. Initialize `dp[0] = true` since a sum of zero can always be achieved without including any stone.
4. Iterate over each stone, and for each stone, iterate over all possible sums from `half` to the stone weight in reverse, updating the DP array.
5. Look for the largest `j` such that `dp[j]` is `true`. The result will be `totalSum - 2*j`.

### Code
```java
public class Solution {
    public int lastStoneWeightII(int[] stones) {
        int totalSum = 0;
        for (int stone : stones) {
            totalSum += stone;
        }
        
        int half = totalSum / 2;
        boolean[] dp = new boolean[half + 1];
        dp[0] = true;

        for (int stone : stones) {
            for (int j = half; j >= stone; j--) {
                dp[j] = dp[j] || dp[j - stone];
            }
        }
        
        for (int j = half; j >= 0; j--) {
            if (dp[j]) {
                return totalSum - 2 * j;
            }
        }
        
        return 0;
    }
}
```
### Time and Space Complexity
- **Time Complexity:** \(O(n \times \text{sum / 2})\), where `n` is the number of stones and `sum` is the total sum of stones.
- **Space Complexity:** \(O(\text{sum / 2})\) due to the DP array.

