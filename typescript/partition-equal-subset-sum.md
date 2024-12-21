# [Leetcode 416: Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Dynamic Programming - Top Down (Memoization)](#dynamic-programming---top-down-memoization)
3. [Dynamic Programming - Bottom Up (Tabulation)](#dynamic-programming---bottom-up-tabulation)

---

### Recursive Approach

**Intuition:**

The problem can initially be understood as exploring subsets of the given array to find one whose sum equals half of the total sum of the array. If the total sum is odd, we can immediately return `false`. Otherwise, we need to decide for each element whether to include it in our subset or not and check recursively if we can form the required sum.

```typescript
function canPartition(nums: number[]): boolean {
    const sum = nums.reduce((acc, num) => acc + num, 0);
    if (sum % 2 !== 0) {
        return false; // If the total sum is odd, we can't partition into two equal subsets
    }

    const target = sum / 2;

    function canPartitionRecursive(n: number, remaining: number): boolean {
        if (remaining === 0) return true;
        if (n === 0 || remaining < 0) return false;

        if (nums[n-1] > remaining) {
            return canPartitionRecursive(n-1, remaining);
        }

        // Either include the current number or don't include it
        return canPartitionRecursive(n-1, remaining) || canPartitionRecursive(n-1, remaining - nums[n-1]);
    }

    return canPartitionRecursive(nums.length, target);
}
```

**Time Complexity:** O(2^N) - Because we try two possibilities (include/exclude) for each element.

**Space Complexity:** O(N) - Recursion stack space.

---

### Dynamic Programming - Top Down (Memoization)

**Intuition:**

The recursive solution has overlap in subproblems. For example, whether to include a number or not can be memoized to avoid recalculating this choice for the same state multiple times. We use a cache to store the outcome of subproblems and utilize it to optimize efficiency.

```typescript
function canPartition(nums: number[]): boolean {
    const sum = nums.reduce((acc, num) => acc + num, 0);
    if (sum % 2 !== 0) return false;

    const target = sum / 2;
    const memo: { [key: string]: boolean } = {};

    function canPartitionMemo(n: number, remaining: number): boolean {
        const key = `${n}-${remaining}`;
        if (key in memo) return memo[key];
        
        if (remaining === 0) return true;
        if (n === 0 || remaining < 0) return false;
        
        if (nums[n-1] > remaining) {
            return memo[key] = canPartitionMemo(n-1, remaining);
        }
        
        return memo[key] = canPartitionMemo(n-1, remaining) || canPartitionMemo(n-1, remaining - nums[n-1]);
    }

    return canPartitionMemo(nums.length, target);
}
```

**Time Complexity:** O(N*Sum) - The memo array may fill up for each number and target.

**Space Complexity:** O(N*Sum) - The cache and recursion stack.

---

### Dynamic Programming - Bottom Up (Tabulation)

**Intuition:**

We can transform our recursive and memoization solutions to use a table (array) that gradually builds up to find the solution by iterating through the numbers and possible subset sums.

```typescript
function canPartition(nums: number[]): boolean {
    const sum = nums.reduce((acc, num) => acc + num, 0);
    if (sum % 2 !== 0) return false;

    const target = sum / 2;
    const dp: boolean[] = new Array(target + 1).fill(false);
    dp[0] = true;
    
    // Traverse each number
    for (let num of nums) {
        // Traverse possible sums in reverse
        for (let j = target; j >= num; j--) {
            dp[j] = dp[j] || dp[j - num];
        }
    }

    return dp[target];
}
```

**Time Complexity:** O(N*Sum) - The two loops iterate through numbers and sums.

**Space Complexity:** O(Sum) - The dp array size is as large as the target sum.

---

Each method improves upon the previous by optimizing space and time via strategic decision making. In large inputs, understanding the nature of subproblems and how to store subproblem results is key in ensuring solutions within time constraints.

