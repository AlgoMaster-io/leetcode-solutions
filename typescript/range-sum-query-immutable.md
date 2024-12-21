# [Leetcode 303: Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Prefix Sum Array (Optimal)](#approach-2-prefix-sum-array)

### Approach 1: Brute Force

**Intuition:**

The simplest way to find the sum of elements between two indices `i` and `j` in an array is to iterate over all the elements between these indices and accumulate their sum. This approach requires a fresh calculation each time you query the sum, leading to a time complexity that depends on the span of the query.

**Code:**

```typescript
class NumArray {
    private nums: number[];

    constructor(nums: number[]) {
        this.nums = nums;
    }

    sumRange(i: number, j: number): number {
        let sum = 0;
        // Iterate from index i to j and accumulate the sum
        for (let k = i; k <= j; k++) {
            sum += this.nums[k];
        }
        return sum;
    }
}
```

**Time Complexity:** O(n) per query, where n is the number of elements between `i` and `j`.

**Space Complexity:** O(1), as no extra storage besides the input is needed.

---

### Approach 2: Prefix Sum Array (Optimal)

**Intuition:**

To optimize the repeated computation of sums, we can precompute the cumulative sum of elements from the start of the array up to each index. Once this prefix sum array is computed, any range sum query can be answered in constant time by leveraging the difference of prefix sums.

- Prefix sum up to index `k` is defined as the sum of all elements from the start to `k` (`prefix[k] = nums[0] + nums[1] + ... + nums[k]`).
- To get the sum from `i` to `j`, use the formula: `prefix[j] - prefix[i-1]`.

**Code:**

```typescript
class NumArray {
    private prefixSums: number[];

    constructor(nums: number[]) {
        this.prefixSums = new Array(nums.length + 1).fill(0);
        // Precompute the prefix sums
        for (let i = 1; i <= nums.length; i++) {
            this.prefixSums[i] = this.prefixSums[i - 1] + nums[i - 1];
        }
    }

    sumRange(i: number, j: number): number {
        // Utilize the prefix sum array to get the range sum in constant time
        return this.prefixSums[j + 1] - this.prefixSums[i];
    }
}
```

**Time Complexity:** O(1) per query after an initial O(n) preprocessing time to construct the prefix sum array.

**Space Complexity:** O(n) due to storing the prefix sums.

