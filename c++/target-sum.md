# [494. Target Sum](https://leetcode.com/problems/target-sum/)

## Approaches:
- [Approach 1: Recursive Backtracking](#approach-1-recursive-backtracking)
- [Approach 2: Top-down Dynamic Programming with Memoization](#approach-2-top-down-dynamic-programming-with-memoization)
- [Approach 3: Bottom-up Dynamic Programming](#approach-3-bottom-up-dynamic-programming)

---

## Approach 1: Recursive Backtracking

**Intuition:**
The problem can be broken down using recursion to try all possible combinations of '+' and '-' signs for each element in the array. The recursive function will compute the result by adding and subtracting each number.

**Implementation:**

```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        return backtrack(nums, target, 0, 0);
    }
    
private:
    int backtrack(vector<int>& nums, int target, int index, int currentSum) {
        // Base case: If we reach the end of the array, check if the current sum equals target
        if (index == nums.size()) {
            return currentSum == target ? 1 : 0;
        }

        // Recurse by including '+' sign
        int add = backtrack(nums, target, index + 1, currentSum + nums[index]);

        // Recurse by including '-' sign
        int subtract = backtrack(nums, target, index + 1, currentSum - nums[index]);

        // Sum the ways from both subproblems
        return add + subtract;
    }
};
```

**Time Complexity:** \(O(2^n)\), where \(n\) is the length of the input array.

**Space Complexity:** \(O(n)\) for the recursion call stack.

---

## Approach 2: Top-down Dynamic Programming with Memoization

**Intuition:**
We can utilize memoization to store the results of subproblems to avoid redundant calculations. This reduces the time complexity by avoiding repeated calculations of the same state.

**Implementation:**

```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        unordered_map<string, int> memo;
        return dp(nums, target, 0, 0, memo);
    }
    
private:
    int dp(vector<int>& nums, int target, int index, int currentSum, unordered_map<string, int>& memo) {
        // Base case: If we reach the end of the array, check if the current sum equals target
        if (index == nums.size()) {
            return currentSum == target ? 1 : 0;
        }
        
        // Create a unique key for the memoization map
        string key = to_string(index) + ',' + to_string(currentSum);
        
        // Check if this state is already computed
        if (memo.find(key) != memo.end()) {
            return memo[key];
        }
        
        // Recurse by including '+' sign
        int add = dp(nums, target, index + 1, currentSum + nums[index], memo);
        
        // Recurse by including '-' sign
        int subtract = dp(nums, target, index + 1, currentSum - nums[index], memo);
        
        // Store the result in memo and return
        memo[key] = add + subtract;
        
        return memo[key];
    }
};
```

**Time Complexity:** \(O(n \times s)\), where \(n\) is the number of elements and \(s\) is the sum of all numbers.

**Space Complexity:** \(O(n \times s)\) for the memoization table.

---

## Approach 3: Bottom-up Dynamic Programming

**Intuition:**
Instead of recursion, use a DP table to iteratively compute the number of ways to reach different sums using a DP matrix. Convert the problem into a subset sum problem after handling the target transformation.

**Implementation:**

```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int totalSum = accumulate(nums.begin(), nums.end(), 0);
        
        // If (sum + target) is odd or target is out of bounds, not possible
        if ((totalSum + target) % 2 != 0 || abs(target) > totalSum) {
            return 0;
        }
        
        int subsetSum = (totalSum + target) / 2;
        
        vector<int> dp(subsetSum + 1, 0);
        dp[0] = 1; // Base case: there's one way to get to sum zero (using no elements)
        
        for (int num : nums) {
            for (int j = subsetSum; j >= num; --j) {
                dp[j] += dp[j - num];
            }
        }
        
        return dp[subsetSum];
    }
};
```

**Time Complexity:** \(O(n \times s)\), where \(n\) is the length of the input array and \(s\) is the computed subset sum.

**Space Complexity:** \(O(s)\) because we use a one-dimensional array for the DP table. 

---
Each approach improves over the last in terms of efficiency, reducing redundant calculations by introducing memoization and optimizing the space used through dynamic programming strategies.

