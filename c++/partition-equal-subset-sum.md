[Leetcode 416: Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Top-Down Dynamic Programming (Memoization)](#top-down-dynamic-programming-memoization)
3. [Bottom-Up Dynamic Programming](#bottom-up-dynamic-programming)
4. [Optimized Bottom-Up Dynamic Programming](#optimized-bottom-up-dynamic-programming)

---

### Recursive Approach

#### Intuition:
The problem can be visualized as searching for a subset of the given array that sums to half of the total array sum. If the total sum is odd, partition is not possible. In this recursive approach, we try to include or exclude each element to reach the target sum of total/2.

#### Implementation:
```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int totalSum = accumulate(nums.begin(), nums.end(), 0);
        
        // If total sum is odd, we cannot partition it into two equal subsets
        if(totalSum % 2 != 0) return false;
        
        int target = totalSum / 2;
        return canPartitionRecursive(nums, target, nums.size() - 1);
    }

private:
    bool canPartitionRecursive(const vector<int>& nums, int target, int idx) {
        // Base cases
        if (target == 0) return true; // Found a valid subset
        if (idx < 0 || target < 0) return false; // Out of bounds or no solution
        
        // Recurrence relation: Either include the current element or exclude it
        return canPartitionRecursive(nums, target - nums[idx], idx - 1) || // Include current
               canPartitionRecursive(nums, target, idx - 1); // Exclude current
    }
};
```

#### Time Complexity:
- O(2^n) - As each element has two choices (include or exclude).
#### Space Complexity:
- O(n) - Recursion stack space.

---

### Top-Down Dynamic Programming (Memoization)

#### Intuition:
We use memoization to store results of previously computed states, reducing redundant calculations.

#### Implementation:
```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int totalSum = accumulate(nums.begin(), nums.end(), 0);
        if (totalSum % 2 != 0) return false;
        
        int target = totalSum / 2;
        vector<vector<int>> memo(nums.size(), vector<int>(target + 1, -1));
        
        return canPartitionMemo(nums, target, nums.size() - 1, memo);
    }

private:
    bool canPartitionMemo(const vector<int>& nums, int target, int idx, vector<vector<int>>& memo) {
        if (target == 0) return true;
        if (idx < 0 || target < 0) return false;
        
        if (memo[idx][target] != -1) return memo[idx][target];
        
        // Compute the value if not already computed
        bool result = canPartitionMemo(nums, target - nums[idx], idx - 1, memo) ||
                      canPartitionMemo(nums, target, idx - 1, memo);
        memo[idx][target] = result;
        return result;
    }
};
```

#### Time Complexity:
- O(n * target) - Each state is computed once.
#### Space Complexity:
- O(n * target) - For the memoization table.

---

### Bottom-Up Dynamic Programming

#### Intuition:
This approach builds up the solution using an iterative table, preventing recursive stack overflow issues and saving space compared to memoization.

#### Implementation:
```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int totalSum = accumulate(nums.begin(), nums.end(), 0);
        if (totalSum % 2 != 0) return false;
        
        int target = totalSum / 2;
        vector<vector<bool>> dp(nums.size() + 1, vector<bool>(target + 1, false));
        
        // Initialization: Zero target is always achievable with empty subset
        for (int i = 0; i <= nums.size(); ++i) {
            dp[i][0] = true;
        }
        
        // Fill the DP table
        for (int i = 1; i <= nums.size(); ++i) {
            for (int j = 1; j <= target; ++j) {
                if (j < nums[i - 1]) {
                    dp[i][j] = dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i - 1]];
                }
            }
        }
        
        return dp[nums.size()][target];
    }
};
```

#### Time Complexity:
- O(n * target) - Two nested loops over `n` and `target`.
#### Space Complexity:
- O(n * target) - Size of the DP table.

---

### Optimized Bottom-Up Dynamic Programming

#### Intuition:
We can optimize the space further by using a 1D DP array, iterating in reverse to prevent overwriting results used by previous calculations.

#### Implementation:
```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int totalSum = accumulate(nums.begin(), nums.end(), 0);
        if (totalSum % 2 != 0) return false;
        
        int target = totalSum / 2;
        vector<bool> dp(target + 1, false);
        dp[0] = true; // Initialization
        
        for (int num : nums) {
            for (int j = target; j >= num; --j) {
                dp[j] = dp[j] || dp[j - num];
            }
        }
        
        return dp[target];
    }
};
```

#### Time Complexity:
- O(n * target) - Two nested loops with respect to `n` and `target`.
#### Space Complexity:
- O(target) - Only a 1D DP array representing achievable sums up to target.

---

