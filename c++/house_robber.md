# 198. [House Robber](https://leetcode.com/problems/house-robber/)

## Approach 1: Recursive Solution

### Solution
```cpp
// Time Complexity: O(2^n)
// Space Complexity: O(n)
class Solution {
public:
    int rob(vector<int>& nums) {
        return robFrom(0, nums);
    }

private:
    int robFrom(int index, const vector<int>& nums) {
        // Base case: If index is out of bounds, return 0
        if (index >= nums.size()) {
            return 0;
        }
        
        // Either rob this house and skip one, or skip it
        int robCurrent = nums[index] + robFrom(index + 2, nums);
        int skipCurrent = robFrom(index + 1, nums);

        // Return the maximum of both choices
        return max(robCurrent, skipCurrent);
    }
};
```

## Approach 2: Recursion with Memoization

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_map>

class Solution {
public:
    int rob(vector<int>& nums) {
        unordered_map<int, int> memo;
        return robFrom(0, nums, memo);
    }

private:
    int robFrom(int index, const vector<int>& nums, unordered_map<int, int>& memo) {
        // Base case: If index is out of bounds, return 0
        if (index >= nums.size()) {
            return 0;
        }

        // Check memoization table
        if (memo.find(index) != memo.end()) {
            return memo[index];
        }

        // Either rob this house and skip one, or skip it
        int robCurrent = nums[index] + robFrom(index + 2, nums, memo);
        int skipCurrent = robFrom(index + 1, nums, memo);

        // Memoize the result
        int result = max(robCurrent, skipCurrent);
        memo[index] = result;

        return result;
    }
};
```

## Approach 3: Dynamic Programming (Bottom Up)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.empty()) return 0;

        vector<int> dp(nums.size() + 1);
        dp[0] = 0;
        dp[1] = nums[0];

        // Build the dp array
        for (int i = 2; i <= nums.size(); i++) {
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i - 1]);
        }

        return dp[nums.size()];
    }
};
```

## Approach 4: Optimized Dynamic Programming (Constant Space)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.empty()) return 0;

        int prev1 = 0; // max money can rob up to the previous house
        int prev2 = 0; // max money can rob up to two houses before

        // Iterate through each house
        for (int num : nums) {
            int temp = prev1;
            prev1 = max(prev2 + num, prev1);
            prev2 = temp;
        }

        return prev1;
    }
};
```

