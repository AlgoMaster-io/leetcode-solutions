# 494. [Target Sum](https://leetcode.com/problems/target-sum/)

## Approach 1: Recursive Brute Force

### Solution
typescript
```typescript
// Time Complexity: O(2^n)
// Space Complexity: O(n)
class Solution {
    findTargetSumWays(nums: number[], target: number): number {
        return this.calculate(nums, 0, 0, target);
    }

    private calculate(nums: number[], i: number, sum: number, target: number): number {
        // Base case: when we've considered all numbers
        if (i === nums.length) {
            // Return 1 if we've reached the target sum, 0 otherwise
            return sum === target ? 1 : 0;
        }

        // Explore both adding and subtracting the current number
        return this.calculate(nums, i + 1, sum + nums[i], target)
             + this.calculate(nums, i + 1, sum - nums[i], target);
    }
}
```

## Approach 2: Dynamic Programming with Memoization

### Solution
typescript
```typescript
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
class Solution {
    findTargetSumWays(nums: number[], target: number): number {
        // Use Map to store calculated results for memoization
        return this.calculate(nums, 0, 0, target, new Map<string, number>());
    }

    private calculate(nums: number[], i: number, sum: number, target: number, memo: Map<string, number>): number {
        const key = `${i},${sum}`;

        // If result is already calculated for this state, return it
        if (memo.has(key)) {
            return memo.get(key)!;
        }

        // Base case: when we've considered all numbers
        if (i === nums.length) {
            return sum === target ? 1 : 0;
        }

        // Explore both adding and subtracting the current number
        const add = this.calculate(nums, i + 1, sum + nums[i], target, memo);
        const subtract = this.calculate(nums, i + 1, sum - nums[i], target, memo);

        // Save result in memo
        memo.set(key, add + subtract);
        return add + subtract;
    }
}
```

## Approach 3: Dynamic Programming with Bottom-Up

### Solution
typescript
```typescript
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
class Solution {
    findTargetSumWays(nums: number[], target: number): number {
        let sum = 0;

        // Calculate total sum of the array
        for (const num of nums) {
            sum += num;
        }

        // If sum is smaller than target or (sum + target) is odd, return 0
        if (Math.abs(target) > sum || (sum + target) % 2 !== 0) {
            return 0;
        }

        const s = (sum + target) / 2;

        // dp[i] represents the number of ways to get the sum i
        const dp: number[] = new Array(s + 1).fill(0);
        dp[0] = 1;

        // Update dp array
        for (const num of nums) {
            for (let j = s; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }

        return dp[s];
    }
}
```

