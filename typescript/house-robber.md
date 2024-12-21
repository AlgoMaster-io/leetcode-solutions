# [Leetcode 198: House Robber](https://leetcode.com/problems/house-robber/)

## Approaches
- [Approach 1: Recursive Backtracking](#approach-1-recursive-backtracking)
- [Approach 2: Dynamic Programming with Memoization (Top-Down)](#approach-2-dynamic-programming-with-memoization-top-down)
- [Approach 3: Dynamic Programming (Bottom-Up)](#approach-3-dynamic-programming-bottom-up)
- [Approach 4: Optimized Dynamic Programming with Space Reduction](#approach-4-optimized-dynamic-programming-with-space-reduction)

### Approach 1: Recursive Backtracking
The simplest way to solve the problem is using a recursive approach. The idea is to:
1. Consider each house and decide whether to rob it or not.
2. If a house is robbed, then the next house cannot be robbed. Hence, skip to the house after the next one.
3. If a house is not robbed, move to the next house.

```typescript
function robRecursive(nums: number[], index: number = 0): number {
    if (index >= nums.length) {
        // Base case: If we've gone past the last house, no more money can be robbed.
        return 0;
    }

    // Two options: Rob the current house, or skip to the next one.
    const robCurrent = nums[index] + robRecursive(nums, index + 2);
    const skipCurrent = robRecursive(nums, index + 1);

    // Return the maximum money that can be robbed between these options.
    return Math.max(robCurrent, skipCurrent);
}
```

**Time Complexity**: O(2^n), where n is the number of houses. This is because each house gives us two choices: to rob or not to rob.
**Space Complexity**: O(n) due to the recursion stack. 

### Approach 2: Dynamic Programming with Memoization (Top-Down)
In this approach, we use memoization to store the results of the subproblems. This avoids redundant calculations and improves efficiency.

```typescript
function robMemo(nums: number[]): number {
    const memo: number[] = new Array(nums.length).fill(-1);

    function robFrom(index: number): number {
        if (index >= nums.length) {
            return 0;
        }

        if (memo[index] !== -1) {
            return memo[index];
        }

        const robCurrent = nums[index] + robFrom(index + 2);
        const skipCurrent = robFrom(index + 1);

        const result = Math.max(robCurrent, skipCurrent);
        memo[index] = result; // Store result in memo

        return result;
    }

    return robFrom(0);
}
```

**Time Complexity**: O(n), as each house is only computed once.
**Space Complexity**: O(n), due to the memoization array.

### Approach 3: Dynamic Programming (Bottom-Up)
This approach builds the solution from the ground up. We use an array to store the maximum money that can be robbed up to each house.

```typescript
function robDP(nums: number[]): number {
    const n = nums.length;
    if (n === 0) return 0;
    if (n === 1) return nums[0];

    const dp: number[] = new Array(n);
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);

    for (let i = 2; i < n; ++i) {
        dp[i] = Math.max(dp[i - 1], nums[i] + dp[i - 2]);
    }

    return dp[n - 1]; // Maximum amount robable is at the last house.
}
```

**Time Complexity**: O(n).
**Space Complexity**: O(n), due to the dp array.

### Approach 4: Optimized Dynamic Programming with Space Reduction
Instead of maintaining a dp array, we can keep track of just the last two computations, thus reducing space complexity.

```typescript
function robOptimized(nums: number[]): number {
    const n = nums.length;
    if (n === 0) return 0;
    if (n === 1) return nums[0];

    let prev1 = 0; // Equivalent to dp[i-2]
    let prev2 = 0; // Equivalent to dp[i-1]

    for (let num of nums) {
        const temp = Math.max(num + prev1, prev2);
        prev1 = prev2;
        prev2 = temp;
    }

    return prev2; // prev2 stores the result after processing all houses.
}
```

**Time Complexity**: O(n).
**Space Complexity**: O(1), as we are using only constant space.

