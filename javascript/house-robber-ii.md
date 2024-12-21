# [House Robber II - LeetCode](https://leetcode.com/problems/house-robber-ii/)

## Approaches

- [Approach 1: Basic Dynamic Programming (Without Circle Constraint)](#approach-1-basic-dynamic-programming-without-circle-constraint)
- [Approach 2: Dynamic Programming with Circle Constraint](#approach-2-dynamic-programming-with-circle-constraint)

### Approach 1: Basic Dynamic Programming (Without Circle Constraint)

**Intuition**:  
The problem is essentially asking to find a maximum sum of non-adjacent numbers, first without considering the circular constraint. We can utilize a dynamic programming approach similar to the original House Robber problem (LeetCode problem 198).

The idea is to maintain an array `dp` where `dp[i]` represents the maximum amount we can rob from the first `i` houses. For each house, we have two choices:
- Don't rob this house, in which case, the maximum amount from `i` houses is the same as from `i-1` houses.
- Rob this house, in which case we add its value to the maximum amount from `i-2` houses.

**Code**:

```javascript
function houseRob(nums) {
    let n = nums.length;
    if (n === 0) return 0;
    if (n === 1) return nums[0];

    let dp = Array(n).fill(0);
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);

    // Fill dp array
    for (let i = 2; i < n; i++) {
        dp[i] = Math.max(dp[i - 1], nums[i] + dp[i - 2]);
    }

    return dp[n - 1];
}
```

**Time Complexity**: O(n)  
**Space Complexity**: O(n)

### Approach 2: Dynamic Programming with Circle Constraint

**Intuition**:  
In the given problem, the houses form a circle, meaning the first and last houses are adjacent. Thus, we cannot simply apply the regular dynamic programming approach. However, the problem can be simplified by splitting it into two subproblems:
1. Rob houses excluding the last house.
2. Rob houses excluding the first house.

The correct solution will be the maximum of these two subproblems, as it ensures we'll never rob the first and last house together.

**Code**:

```javascript
function rob(nums) {
    let n = nums.length;
    if (n === 0) return 0;
    if (n === 1) return nums[0];

    // Helper function for linear house robbing
    function simpleRob(start, end) {
        let prev1 = 0, prev2 = 0;

        for (let i = start; i <= end; i++) {
            let temp = Math.max(prev1, prev2 + nums[i]); // option to rob or not to rob current house
            prev2 = prev1;  // move prev2 to prev1
            prev1 = temp;   // update prev1 to current max
        }

        return prev1;
    }

    // Compare two scenarios: Excluding the first or the last house
    return Math.max(simpleRob(0, n - 2), simpleRob(1, n - 1));
}
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

