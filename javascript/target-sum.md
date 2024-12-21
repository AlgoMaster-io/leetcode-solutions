## [Leetcode 494: Target Sum](https://leetcode.com/problems/target-sum/)

### Table of Contents
1. [Approach 1: Brute Force Using Recursion](#approach-1-brute-force-using-recursion)
2. [Approach 2: Recursion with Memoization](#approach-2-recursion-with-memoization)
3. [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)

---

### Approach 1: Brute Force Using Recursion

#### Intuition
The simplest way to solve this problem is by trying out all possible ways to assign '+' or '-' to each element. This can be done by using a recursive function that takes the current index and the current sum as arguments. 

For each element in the array, we have two choices:
1. Add it to the current sum.
2. Subtract it from the current sum.

We recursively continue this process until we've considered each number, then check if the current sum equals the target.

#### Solution

```javascript
var findTargetSumWays = function(nums, target) {
    function calculate(index, currentSum) {
        // Base case: if we've processed all elements, check if the current sum is equal to target
        if (index === nums.length) {
            return currentSum === target ? 1 : 0;
        }
        
        // Recursive case: consider both adding and subtracting the current number
        return calculate(index + 1, currentSum + nums[index]) + calculate(index + 1, currentSum - nums[index]);
    }
    
    return calculate(0, 0);
};
```

#### Complexity Analysis
- **Time Complexity:** O(2^n) - Each element has two possibilities ('+' or '-').
- **Space Complexity:** O(n) - The space used on the call stack during recursion.

---

### Approach 2: Recursion with Memoization

#### Intuition
The recursive approach generates a lot of repeated calculations. To optimize, we can use memoization to store results of subproblems (i.e., results of `calculate(index, currentSum)`) so that we never solve the same subproblem more than once.

#### Solution

```javascript
var findTargetSumWays = function(nums, target) {
    const memo = {};
    
    function calculate(index, currentSum) {
        if (index === nums.length) {
            return currentSum === target ? 1 : 0;
        }
        
        // Use a string key for memoization
        const key = `${index}-${currentSum}`;
        if (memo[key] !== undefined) {
            return memo[key];
        }
        
        memo[key] = calculate(index + 1, currentSum + nums[index]) + calculate(index + 1, currentSum - nums[index]);
        return memo[key];
    }
    
    return calculate(0, 0);
};
```

#### Complexity Analysis
- **Time Complexity:** O(n * sum) - `n` is the length of the array, and `sum` is the possible range of sums.
- **Space Complexity:** O(n * sum) - Using extra space for memoization.

---

### Approach 3: Dynamic Programming

#### Intuition
This problem can be transformed into a subset sum problem. The idea is to find the number of ways to assign "+" and "-" to the elements such that the sum is equal to the target. This can be reduced to a subset problem: if we divide the array into two subsets S1 and S2 such that S1 - S2 = target, we have:
  
  S1 + S2 = sum(nums)
  S1 - S2 = target

This leads to:
  
  2 * S1 = sum(nums) + target, so S1 = (sum(nums) + target) / 2

Thus, it becomes a problem of finding the number of subsets with sum equal to S1.

#### Solution

```javascript
var findTargetSumWays = function(nums, target) {
    const totalSum = nums.reduce((a, b) => a + b, 0);
    if ((totalSum + target) % 2 !== 0) return 0;  // If S1 is not an integer

    const subsetSum = (totalSum + target) / 2;
    const dp = new Array(subsetSum + 1).fill(0);
    dp[0] = 1; // There's one way to sum to 0: using no elements

    nums.forEach(num => {
        for (let sum = subsetSum; sum >= num; sum--) {
            dp[sum] += dp[sum - num];
        }
    });

    return dp[subsetSum];
};
```

#### Complexity Analysis
- **Time Complexity:** O(n * sum) - `n` is the number of elements and `sum` is the target sum adjusted.
- **Space Complexity:** O(sum) - We maintain an array of size `sum + 1`.

