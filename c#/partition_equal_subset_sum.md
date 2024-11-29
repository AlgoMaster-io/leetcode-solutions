# 416. [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

## Approach 1: Recursive Approach

### Solution
csharp
```csharp
// Time Complexity: O(2^n)
// Space Complexity: O(n)
public class Solution {
    public bool CanPartition(int[] nums) {
        int sum = 0;
        foreach (int num in nums) {
            sum += num;
        }
        // If the sum is odd, partitioning into equal subsets is not possible
        if (sum % 2 != 0) return false;
        
        // Target is half of the total sum
        return CanPartition(nums, sum / 2, 0);
    }
    
    private bool CanPartition(int[] nums, int target, int index) {
        // Base cases
        if (target == 0) return true;
        if (target < 0 || index >= nums.Length) return false;
        
        // Recursive call to include or exclude the current element
        return CanPartition(nums, target - nums[index], index + 1) || 
               CanPartition(nums, target, index + 1);
    }
}
```

## Approach 2: Dynamic Programming (DP) with 2D Array

### Solution
csharp
```csharp
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
public class Solution {
    public bool CanPartition(int[] nums) {
        int sum = 0;
        foreach (int num in nums) {
            sum += num;
        }
        // Sum is odd so return false
        if (sum % 2 != 0) return false;
        int target = sum / 2;
        
        // Create a DP table to store previously computed results
        bool[,] dp = new bool[nums.Length + 1, target + 1];
        
        // Initialize first column, sum of 0 can be achieved without any elements
        for (int i = 0; i <= nums.Length; i++) {
            dp[i, 0] = true;
        }
        
        for (int i = 1; i <= nums.Length; i++) {
            for (int j = 1; j <= target; j++) {
                if (j < nums[i - 1]) {
                    dp[i, j] = dp[i - 1, j];
                } else {
                    dp[i, j] = dp[i - 1, j] || dp[i - 1, j - nums[i - 1]];
                }
            }
        }
        
        return dp[nums.Length, target];
    }
}
```

## Approach 3: Dynamic Programming (DP) with 1D Array

### Solution
csharp
```csharp
// Time Complexity: O(n * target)
// Space Complexity: O(target)
public class Solution {
    public bool CanPartition(int[] nums) {
        int sum = 0;
        foreach (int num in nums) {
            sum += num;
        }
        // If the sum is odd, partitioning into equal subsets is not possible
        if (sum % 2 != 0) return false;
        int target = sum / 2;
        
        // Create a DP array to hold boolean values
        bool[] dp = new bool[target + 1];
        dp[0] = true; // Initial state; zero sum can always be achieved
        
        foreach (int num in nums) {
            // Traverse from target backward to avoid overwriting
            for (int j = target; j >= num; j--) {
                dp[j] = dp[j] || dp[j - num];
            }
        }
        
        return dp[target];
    }
}
```

