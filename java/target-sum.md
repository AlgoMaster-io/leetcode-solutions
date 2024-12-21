# [Leetcode 494: Target Sum](https://leetcode.com/problems/target-sum/)

## Approaches
1. [Recursive Backtracking](#recursive-backtracking)
2. [Memoization (Top-Down DP)](#memoization-top-down-dp)
3. [Dynamic Programming (Bottom-Up DP)](#dynamic-programming-bottom-up-dp)

---

### 1. Recursive Backtracking

The first approach uses recursion to explore all possible ways to add or subtract numbers from the array to achieve the target sum. 

#### Intuition:
For each number in the array, you have two choices:
- Add it to the running total.
- Subtract it from the running total.

By recursively exploring both these choices, you can determine if the target sum can be achieved. This will involve exploring all `2^n` combinations, where `n` is the length of the array.

#### Implementation:

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        return backtrack(nums, target, 0, 0);
    }

    private int backtrack(int[] nums, int target, int index, int currentSum) {
        // Base case: If we've considered all numbers
        if (index == nums.length) {
            // If the current sum is equal to the target, we found a valid way
            return currentSum == target ? 1 : 0;
        }
        // Recursive case: Consider both adding and subtracting the current number
        int add = backtrack(nums, target, index + 1, currentSum + nums[index]);
        int subtract = backtrack(nums, target, index + 1, currentSum - nums[index]);
        // The total number of ways to reach the target from this point is the sum of both ways
        return add + subtract;
    }
}
```

#### Time Complexity:
- `O(2^n)`, where `n` is the number of elements in the array. This is because we are exploring both add and subtract options for each number.

#### Space Complexity:
- `O(n)`, due to the recursive stack space.

---

### 2. Memoization (Top-Down DP)

The recursive approach has a lot of overlapping subproblems, hence we can use memoization to store and reuse the results of these subproblems.

#### Intuition:
By using a hashmap to save previously computed results, we can avoid redundant calculations. The key is to store the index and the current sum as a tuple (or concatenated string) to uniquely identify each state.

#### Implementation:

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        Map<String, Integer> memo = new HashMap<>();
        return dp(nums, target, 0, 0, memo);
    }

    private int dp(int[] nums, int target, int index, int currentSum, Map<String, Integer> memo) {
        String key = index + "," + currentSum;
        if (memo.containsKey(key)) {
            return memo.get(key);
        }
        
        if (index == nums.length) {
            return currentSum == target ? 1 : 0;
        }
        
        int add = dp(nums, target, index + 1, currentSum + nums[index], memo);
        int subtract = dp(nums, target, index + 1, currentSum - nums[index], memo);
        
        memo.put(key, add + subtract);
        return add + subtract;
    }
}
```

#### Time Complexity:
- `O(n * s)`, where `n` is the length of the array and `s` is the sum of the array. The memoization reduces the number of recursive calls.

#### Space Complexity:
- `O(n * s)`, for the memoization hashmap plus stack space.

---

### 3. Dynamic Programming (Bottom-Up DP)

We can further optimize by using a bottom-up approach to construct a dp array. This transforms the problem of target sum into a subset sum problem using transformation and leads to more efficient computation.

#### Intuition:
Consider the equation `sum(P) - sum(N) = target`, where `P` is the positive subset and `N` is the negative subset. The transformation gives `sum(P) = (target + sum(nums)) / 2`. Hence, this reduces to counting how many ways we can pick a subset of nums that adds up to this sum.

This requires `target + sum(nums)` to be even and non-negative.

#### Implementation:

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        
        // The transformed problem only makes sense if the sum can accommodate the target
        if (sum < target || (target + sum) % 2 != 0) {
            return 0;
        }
        
        int targetSum = (target + sum) / 2;
        return subsetSum(nums, targetSum);
    }
    
    private int subsetSum(int[] nums, int targetSum) {
        int[] dp = new int[targetSum + 1];
        dp[0] = 1; // There's one way to have a sum of 0, which is picking nothing
        
        for (int num : nums) {
            for (int j = targetSum; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }
        
        return dp[targetSum];
    }
}
```

#### Time Complexity:
- `O(n * s)`, where `n` is the length of nums and `s` is the target sum transformed as above.

#### Space Complexity:
- `O(s)`, where `s` is the target sum. We use an array proportional to the target sum.

By progressively optimizing from the naive recursive method to a dynamic programming solution, we become substantially more efficient, especially for larger inputs.

