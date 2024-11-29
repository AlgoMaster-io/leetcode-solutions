# [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

## Approach 1: Brute Force (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(n) per query
// Space Complexity: O(1)
class NumArray {
    private nums: number[];

    constructor(nums: number[]) {
        this.nums = nums;
    }

    sumRange(left: number, right: number): number {
        let sum = 0;

        // Calculate the sum for the range [left, right]
        for (let i = left; i <= right; i++) {
            sum += this.nums[i];
        }

        return sum;
    }
}
```

## Approach 2: Prefix Sum (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(1) per query, O(n) for initialization
// Space Complexity: O(n)
class NumArray {
    private prefixSums: number[];

    constructor(nums: number[]) {
        const n = nums.length;
        this.prefixSums = new Array(n + 1).fill(0); // Prefix sum array

        // Compute prefix sums
        for (let i = 0; i < n; i++) {
            this.prefixSums[i + 1] = this.prefixSums[i] + nums[i];
        }
    }

    sumRange(left: number, right: number): number {
        // Use the prefix sum array to calculate the range sum
        return this.prefixSums[right + 1] - this.prefixSums[left];
    }
}
```

