# 198. [House Robber](https://leetcode.com/problems/house-robber/)

## Approach 1: Recursive Solution

### Solution
typescript
```typescript
// Time Complexity: O(2^n)
// Space Complexity: O(n)
class Solution {
    rob(nums: number[]): number {
        return this.robFrom(0, nums);
    }

    private robFrom(index: number, nums: number[]): number {
        // Base case: If index is out of bounds, return 0
        if (index >= nums.length) {
            return 0;
        }
        
        // Either rob this house and skip one, or skip it
        const robCurrent = nums[index] + this.robFrom(index + 2, nums);
        const skipCurrent = this.robFrom(index + 1, nums);

        // Return the maximum of both choices
        return Math.max(robCurrent, skipCurrent);
    }
}
```

## Approach 2: Recursion with Memoization

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    rob(nums: number[]): number {
        return this.robFrom(0, nums, new Map<number, number>());
    }

    private robFrom(index: number, nums: number[], memo: Map<number, number>): number {
        // Base case: If index is out of bounds, return 0
        if (index >= nums.length) {
            return 0;
        }

        // Check memoization table
        if (memo.has(index)) {
            return memo.get(index)!;
        }

        // Either rob this house and skip one, or skip it
        const robCurrent = nums[index] + this.robFrom(index + 2, nums, memo);
        const skipCurrent = this.robFrom(index + 1, nums, memo);

        // Memoize the result
        const result = Math.max(robCurrent, skipCurrent);
        memo.set(index, result);

        return result;
    }
}
```

## Approach 3: Dynamic Programming (Bottom Up)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    rob(nums: number[]): number {
        if (nums.length === 0) return 0;

        const dp = new Array(nums.length + 1).fill(0);
        dp[0] = 0;
        dp[1] = nums[0];

        // Build the dp array
        for (let i = 2; i <= nums.length; i++) {
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i - 1]);
        }

        return dp[nums.length];
    }
}
```

## Approach 4: Optimized Dynamic Programming (Constant Space)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    rob(nums: number[]): number {
        if (nums.length === 0) return 0;

        let prev1 = 0; // max money can rob up to the previous house
        let prev2 = 0; // max money can rob up to two houses before

        // Iterate through each house
        for (const num of nums) {
            const temp = prev1;
            prev1 = Math.max(prev2 + num, prev1);
            prev2 = temp;
        }

        return prev1;
    }
}
```

