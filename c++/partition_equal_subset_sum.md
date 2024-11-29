# 416. [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

## Approach 1: Recursive Approach

### Solution
```cpp
// Time Complexity: O(2^n)
// Space Complexity: O(n)
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        
        // If the sum is odd, partitioning into equal subsets is not possible
        if (sum % 2 != 0) return false;
        
        // Target is half of total sum
        return canPartition(nums, sum / 2, 0);
    }
    
private:
    bool canPartition(vector<int>& nums, int target, int index) {
        // Base cases
        if (target == 0) return true;
        if (target < 0 || index >= nums.size()) return false;
        
        // Recursive call to include or exclude the current element
        return canPartition(nums, target - nums[index], index + 1) || 
               canPartition(nums, target, index + 1);
    }
};
```

## Approach 2: Dynamic Programming (DP) with 2D Array

### Solution
```cpp
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        
        // Sum is odd so return false
        if (sum % 2 != 0) return false;
        int target = sum / 2;
        
        // Create a DP table to store previously computed results
        vector<vector<bool>> dp(nums.size() + 1, vector<bool>(target + 1, false));
        
        // Initialize first column, sum of 0 can be achieved without any elements
        for (int i = 0; i <= nums.size(); i++) {
            dp[i][0] = true;
        }
        
        for (int i = 1; i <= nums.size(); i++) {
            for (int j = 1; j <= target; j++) {
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

## Approach 3: Dynamic Programming (DP) with 1D Array

### Solution
```cpp
// Time Complexity: O(n * target)
// Space Complexity: O(target)
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        
        // If the sum is odd, partitioning into equal subsets is not possible
        if (sum % 2 != 0) return false;
        int target = sum / 2;
        
        // Create a DP array to hold boolean values
        vector<bool> dp(target + 1, false);
        dp[0] = true; // Initial state; zero sum can always be achieved
        
        for (int num : nums) {
            // Traverse from target backward to avoid overwriting
            for (int j = target; j >= num; j--) {
                dp[j] = dp[j] || dp[j - num];
            }
        }
        
        return dp[target];
    }
};
```

