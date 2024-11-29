# 416. [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

## Approach 1: Recursive Approach

### Solution
```javascript
// Time Complexity: O(2^n)
// Space Complexity: O(n)
class Solution {
    canPartition(nums) {
        const sum = nums.reduce((acc, num) => acc + num, 0);
        
        // If the sum is odd, partitioning into equal subsets is not possible
        if (sum % 2 !== 0) return false;
        
        // Target is half of total sum
        return this._canPartition(nums, sum / 2, 0);
    }
    
    _canPartition(nums, target, index) {
        // Base cases
        if (target === 0) return true;
        if (target < 0 || index >= nums.length) return false;
        
        // Recursive call to include or exclude the current element
        return this._canPartition(nums, target - nums[index], index + 1) || 
               this._canPartition(nums, target, index + 1);
    }
}
```

## Approach 2: Dynamic Programming (DP) with 2D Array

### Solution
```javascript
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
class Solution {
    canPartition(nums) {
        const sum = nums.reduce((acc, num) => acc + num, 0);
        
        // Sum is odd so return false
        if (sum % 2 !== 0) return false;
        const target = sum / 2;
        
        // Create a DP table to store previously computed results
        const dp = Array.from({length: nums.length + 1}, () => Array(target + 1).fill(false));
        
        // Initialize first column, sum of 0 can be achieved without any elements
        for (let i = 0; i <= nums.length; i++) {
            dp[i][0] = true;
        }
        
        for (let i = 1; i <= nums.length; i++) {
            for (let j = 1; j <= target; j++) {
                if (j < nums[i - 1]) {
                    dp[i][j] = dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i - 1]];
                }
            }
        }
        
        return dp[nums.length][target];
    }
}
```

## Approach 3: Dynamic Programming (DP) with 1D Array

### Solution
```javascript
// Time Complexity: O(n * target)
// Space Complexity: O(target)
class Solution {
    canPartition(nums) {
        const sum = nums.reduce((acc, num) => acc + num, 0);
        
        // If the sum is odd, partitioning into equal subsets is not possible
        if (sum % 2 !== 0) return false;
        const target = sum / 2;
        
        // Create a DP array to hold boolean values
        const dp = new Array(target + 1).fill(false);
        dp[0] = true; // Initial state; zero sum can always be achieved
        
        for (const num of nums) {
            // Traverse from target backward to avoid overwriting
            for (let j = target; j >= num; j--) {
                dp[j] = dp[j] || dp[j - num];
            }
        }
        
        return dp[target];
    }
}
```

