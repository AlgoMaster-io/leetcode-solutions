# [Leetcode 300: Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

## Approaches

1. [Dynamic Programming (Classic Approach)](#approach-1)
2. [Patience Sorting (Optimized Approach using Binary Search)](#approach-2)

---

## Approach 1: Dynamic Programming (Classic Approach)

### Intuition
The problem of finding the Longest Increasing Subsequence (LIS) can be solved using a dynamic programming approach. The main idea is to maintain a `dp` array where `dp[i]` represents the length of the longest increasing subsequence that ends with the element `nums[i]`. By iterating through the array and updating the `dp` array based on previous computations, we can efficiently build up the solution.

### Steps
1. Initialize a `dp` array of the same length as `nums`, filled with 1s, since the smallest LIS containing a single element has a length of 1.
2. Iterate through each element in the array with an outer loop `i`.
3. For each `i`, use an inner loop `j` to iterate through all elements before `i`.
   - If `nums[j] < nums[i]`, it means that we can append `nums[i]` to the increasing subsequence ending with `nums[j]`.
   - Update `dp[i] = max(dp[i], dp[j] + 1)`.
4. The final result would be the maximum value in the `dp` array.

### Code
```typescript
function lengthOfLIS(nums: number[]): number {
    if (nums.length === 0) return 0;
    
    const dp = new Array(nums.length).fill(1);

    for (let i = 0; i < nums.length; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
    }

    return Math.max(...dp);
}
```

### Complexity
- **Time Complexity**: O(n^2) due to the nested loops iterating over the elements.
- **Space Complexity**: O(n) for storing the `dp` array.

---

## Approach 2: Patience Sorting (Optimized Approach using Binary Search)

### Intuition
The Patience Sorting algorithm utilizes a clever strategy similar to the card game of patience (solitaire). The idea is to construct piles where each pile is the top of an increasing subsequence. By ensuring each pile is in sorted order, we can apply binary search to find the right pile for a number in `nums`, keeping the size of the piles as the answer.

### Steps
1. Initialize an empty array `piles` which will represent the tops of the piles.
2. Iterate over each `num` in `nums`.
   - Use binary search to determine the index at which `num` should be inserted into the `piles`.
   - If `num` is greater than all existing pile tops, it forms a new pile.
   - Otherwise, replace the existing pile top with `num`.
3. The length of `piles` will be the length of the Longest Increasing Subsequence.

### Code
```typescript
function lengthOfLIS(nums: number[]): number {
    const piles: number[] = [];
    
    for (const num of nums) {
        let left = 0;
        let right = piles.length;
        
        while (left < right) {
            const mid = Math.floor(left + (right - left) / 2);

            // Try to place 'num' in the correct position
            if (piles[mid] < num) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        // If 'left' is equal to piles.length, it means 'num' is greater than all elements
        if (left === piles.length) {
            piles.push(num);
        } else {
            piles[left] = num;
        }
    }
    
    return piles.length;
}
```

### Complexity
- **Time Complexity**: O(n log n) due to the binary search operations for each element.
- **Space Complexity**: O(n) for maintaining the `piles` array.

This approach is optimal for this problem, making it the preferred method for larger input sizes.

