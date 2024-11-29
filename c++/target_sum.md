# 494. [Target Sum](https://leetcode.com/problems/target-sum/)

## Approach 1: Recursive Brute Force

### Solution
cpp
```cpp
// Time Complexity: O(2^n)
// Space Complexity: O(n)
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        return calculate(nums, 0, 0, target);
    }
    
private:
    int calculate(const vector<int>& nums, int i, int sum, int target) {
        // Base case: when we've considered all numbers
        if (i == nums.size()) {
            // Return 1 if we've reached the target sum, 0 otherwise
            return sum == target ? 1 : 0;
        }
    
        // Explore both adding and subtracting the current number
        return calculate(nums, i + 1, sum + nums[i], target) 
             + calculate(nums, i + 1, sum - nums[i], target);
    }
};
```

## Approach 2: Dynamic Programming with Memoization

### Solution
cpp
```cpp
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        // Use unordered_map to store calculated results for memoization
        unordered_map<string, int> memo;
        return calculate(nums, 0, 0, target, memo);
    }
    
private:
    int calculate(const vector<int>& nums, int i, int sum, int target, unordered_map<string, int>& memo) {
        string key = to_string(i) + "," + to_string(sum);
        
        // If result is already calculated for this state, return it
        if (memo.find(key) != memo.end()) {
            return memo[key];
        }
        
        // Base case: when we've considered all numbers
        if (i == nums.size()) {
            return sum == target ? 1 : 0;
        }
        
        // Explore both adding and subtracting the current number
        int add = calculate(nums, i + 1, sum + nums[i], target, memo);
        int subtract = calculate(nums, i + 1, sum - nums[i], target, memo);
        
        // Save result in memo
        memo[key] = add + subtract;
        return memo[key];
    }
};
```

## Approach 3: Dynamic Programming with Bottom-Up

### Solution
cpp
```cpp
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        
        // If sum is smaller than target or (sum + target) is odd, return 0
        if (abs(target) > sum || (sum + target) % 2 != 0) {
            return 0;
        }
        
        int s = (sum + target) / 2;
        
        // dp[i] represents the number of ways to get the sum i
        vector<int> dp(s + 1, 0);
        dp[0] = 1;
        
        // Update dp array
        for (int num : nums) {
            for (int j = s; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }
        
        return dp[s];
    }
};
```

