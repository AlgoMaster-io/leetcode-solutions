# [Leetcode 198: House Robber](https://leetcode.com/problems/house-robber/)

## Approaches
- [Approach 1: Recursion](#approach-1-recursion)
- [Approach 2: Recursion with Memoization](#approach-2-recursion-with-memoization)
- [Approach 3: Dynamic Programming (Bottom-Up)](#approach-3-dynamic-programming-bottom-up)
- [Approach 4: Dynamic Programming with Space Optimization](#approach-4-dynamic-programming-with-space-optimization)

### Approach 1: Recursion

**Intuition**

The problem is essentially about choosing non-adjacent elements to maximize the sum. A simple way to approach this is to use recursion: for each house, you decide to either rob it or not, and recursively calculate the maximum robbery value for the rest of the houses. Here, the base case is when there are no houses left to rob.

**Code**

```cpp
class Solution {
public:
    int robRecursively(vector<int>& nums, int n) {
        // Base case: if no houses left, return 0
        if (n < 0) {
            return 0;
        }
        // Recursive case: return the maximum of two choices
        return max(robRecursively(nums, n - 2) + nums[n], robRecursively(nums, n - 1));
    }
    
    int rob(vector<int>& nums) {
        return robRecursively(nums, nums.size() - 1);
    }
};
```

**Time Complexity:** O(2^n) due to the large number of repeated sub-problems.  
**Space Complexity:** O(n) due to the recursion stack.

### Approach 2: Recursion with Memoization

**Intuition**

The above recursive solution recalculates the same sub-problems multiple times. Using a memoization table allows us to store the results of sub-problems and avoids repeated calculations, effectively converting the recursion into dynamic programming.

**Code**

```cpp
class Solution {
public:
    int robMemo(vector<int>& nums, vector<int>& memo, int n) {
        // Base case: if no houses left, return 0
        if (n < 0) {
            return 0;
        }
        // If we've already computed this subproblem, return its result
        if (memo[n] != -1) {
            return memo[n];
        }
        // Otherwise compute it, store it in the memo, and return the result
        memo[n] = max(robMemo(nums, memo, n - 2) + nums[n], robMemo(nums, memo, n - 1));
        return memo[n];
    }
    
    int rob(vector<int>& nums) {
        vector<int> memo(nums.size(), -1);
        return robMemo(nums, memo, nums.size() - 1);
    }
};
```

**Time Complexity:** O(n) because each sub-problem is calculated only once.  
**Space Complexity:** O(n) for the memoization table.

### Approach 3: Dynamic Programming (Bottom-Up)

**Intuition**

In the bottom-up approach, we iteratively solve smaller subproblems to build up the solution to the original problem. We use a dynamic programming table where dp[i] represents the maximum amount of money that can be robbed from the first i houses.

**Code**

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.empty()) return 0;
        if (nums.size() == 1) return nums[0];

        vector<int> dp(nums.size(), 0);
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);

        for (int i = 2; i < nums.size(); ++i) {
            // Decide whether to rob the current house or not
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);
        }
        return dp.back();
    }
};
```

**Time Complexity:** O(n) because we iterate over the houses once.  
**Space Complexity:** O(n) due to the dp array.

### Approach 4: Dynamic Programming with Space Optimization

**Intuition**

The final step improves space complexity. The state at each step only depends on the results of the previous two states, meaning we can reduce our space usage to two variables instead of an entire array.

**Code**

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.empty()) return 0;
        if (nums.size() == 1) return nums[0];

        int prev1 = 0; // Initialize variable for dp[i-2]
        int prev2 = 0; // Initialize variable for dp[i-1]

        for (int num : nums) {
            int temp = max(prev2, prev1 + num);
            prev1 = prev2;
            prev2 = temp;
        }
        return prev2;
    }
};
```

**Time Complexity:** O(n) for the single pass through the array.  
**Space Complexity:** O(1) due to the use of a constant amount of space.

