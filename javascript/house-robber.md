# [Leetcode 198: House Robber](https://leetcode.com/problems/house-robber/)

## Approaches
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Recursive with Memoization](#approach-2-recursive-with-memoization)
- [Approach 3: Dynamic Programming with Tabulation](#approach-3-dynamic-programming-with-tabulation)
- [Approach 4: Optimized Dynamic Programming](#approach-4-optimized-dynamic-programming)

### Approach 1: Recursive Solution

The problem is about finding the maximum sum of non-adjacent numbers. A very basic approach is to use recursion to explore all possibilities.

#### Intuition
- At each house, you have two options:
  1. Rob the current house and then rob from the house that is two steps ahead.
  2. Skip the current house and rob from the next house.
- Use recursion to explore these two options for each house and return the maximum value.

```javascript
var rob = function(nums) {
    function robFrom(index) {
        // Base case: no houses left to rob
        if (index >= nums.length) {
            return 0;
        }
        // Decision: Rob this house
        let robCurrent = nums[index] + robFrom(index + 2);
        // Decision: Skip this house
        let skipCurrent = robFrom(index + 1);
        
        // Return the maximum of both decisions
        return Math.max(robCurrent, skipCurrent);
    }
    
    // Start from the first house
    return robFrom(0);
};
```

**Complexity Analysis**

- Time Complexity: O(2^n) - Exponential due to two recursive calls at each step.
- Space Complexity: O(n) - Due to the recursion call stack.

### Approach 2: Recursive with Memoization

To improve the basic recursive approach, memoization can be utilized to avoid redundant calculations.

#### Intuition
- Store the results of subproblems into a memo array so you do not compute them multiple times.

```javascript
var rob = function(nums) {
    const memo = [];
    
    function robFrom(index) {
        // Base case: no houses left to rob
        if (index >= nums.length) {
            return 0;
        }
        // If already computed, return the result
        if (memo[index] !== undefined) {
            return memo[index];
        }
        // Decision: Rob this house
        let robCurrent = nums[index] + robFrom(index + 2);
        // Decision: Skip this house
        let skipCurrent = robFrom(index + 1);
        
        // Store the result in the memo and return
        memo[index] = Math.max(robCurrent, skipCurrent);
        return memo[index];
    }
    
    // Start from the first house
    return robFrom(0);
};
```

**Complexity Analysis**

- Time Complexity: O(n) - Each subproblem is solved only once.
- Space Complexity: O(n) - Due to recursion call stack and memo array.

### Approach 3: Dynamic Programming with Tabulation

By using an iterative approach, we can further optimize the recursive approach by removing the recursion stack.

#### Intuition
- Use a table to store the maximum money that can be robbed up to each house.
- The result for each house depends on robbing it plus the result two houses back, or skipping it and taking the result from the previous house.

```javascript
var rob = function(nums) {
    if (nums.length === 0) return 0; // Edge case: no houses
    if (nums.length === 1) return nums[0]; // Edge case: one house

    const dp = Array(nums.length).fill(0);

    // Base cases: first house must be considered separately
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);

    for (let i = 2; i < nums.length; i++) {
        // Either rob this house and add the value two houses back or
        // skip this house and take the result from the previous house
        dp[i] = Math.max(dp[i-1], nums[i] + dp[i-2]);
    }

    // The last element contains the result
    return dp[nums.length - 1];
};
```

**Complexity Analysis**

- Time Complexity: O(n) - We solve the problem iteratively in a single pass.
- Space Complexity: O(n) - Due to the dp array.

### Approach 4: Optimized Dynamic Programming

Reduce space complexity by keeping track of only the last two states.

#### Intuition
- Instead of maintaining a large dp array, we only need two variables to track the maximum money robbed up to the last two houses.

```javascript
var rob = function(nums) {
    if (nums.length === 0) return 0; // Edge case: no houses
    if (nums.length === 1) return nums[0]; // Edge case: one house

    let prev1 = 0; // Money robbed from previous house (n-1)
    let prev2 = 0; // Money robbed from the house before the previous (n-2)

    for (let num of nums) {
        let temp = prev1; // Save the previous house result
        // Maximum of robbing the current house plus two back, or skipping current
        prev1 = Math.max(prev1, prev2 + num);
        prev2 = temp; // Move 'prev2' to previous house
    }

    return prev1; // The final state contains the result
};
```

**Complexity Analysis**

- Time Complexity: O(n) - We solve the problem iteratively in a single pass.
- Space Complexity: O(1) - Uses constant space for the variables `prev1` and `prev2`.

