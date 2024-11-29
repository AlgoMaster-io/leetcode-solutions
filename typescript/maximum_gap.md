# [164. Maximum Gap](https://leetcode.com/problems/maximum-gap/)

## Approach 1: Sorting

### Solution
typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(1) (ignoring the space required for sorting)

function maximumGap(nums: number[]): number {
    if (nums == null || nums.length < 2) {
        return 0;
    }

    // Sort the array
    nums.sort((a, b) => a - b);

    let maxGap = 0;

    // Calculate the maximum gap between consecutive elements
    for (let i = 1; i < nums.length; i++) {
        maxGap = Math.max(maxGap, nums[i] - nums[i - 1]);
    }

    return maxGap;
}
```

## Approach 2: Bucket Sort (Using Pigeonhole Principle)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)

function maximumGap(nums: number[]): number {
    if (nums == null || nums.length < 2) {
        return 0;
    }

    let min = Number.MAX_VALUE, max = Number.MIN_VALUE;
    for (const num of nums) {
        min = Math.min(min, num);
        max = Math.max(max, num);
    }

    // Calculate the gap size and number of buckets
    const n = nums.length;
    const gap = Math.ceil((max - min) / (n - 1));
    const bucketMin = new Array(n - 1).fill(Number.MAX_VALUE);
    const bucketMax = new Array(n - 1).fill(Number.MIN_VALUE);

    // Place each number into a bucket
    for (const num of nums) {
        if (num === min || num === max) {
            continue;
        }
        const index = Math.floor((num - min) / gap);
        bucketMin[index] = Math.min(bucketMin[index], num);
        bucketMax[index] = Math.max(bucketMax[index], num);
    }

    // Calculate the maximum gap
    let maxGap = 0, previous = min;
    for (let i = 0; i < n - 1; i++) {
        if (bucketMin[i] === Number.MAX_VALUE) {
            continue; // Skip empty buckets
        }
        maxGap = Math.max(maxGap, bucketMin[i] - previous);
        previous = bucketMax[i];
    }
    maxGap = Math.max(maxGap, max - previous); // Final gap with the max element

    return maxGap;
}
```

