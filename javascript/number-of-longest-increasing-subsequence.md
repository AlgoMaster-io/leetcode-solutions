# [Leetcode 673: Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## Table of Contents
- [Solution 1: Dynamic Programming](#solution-1-dynamic-programming)
- [Solution 2: Dynamic Programming with Bit Manipulation](#solution-2-dynamic-programming-with-bit-manipulation)

### Solution 1: Dynamic Programming

**Intuition**

In this approach, we use dynamic programming to keep track of two main aspects at every index of the array:
1. The length of the longest increasing subsequence (LIS) ending at that index.
2. The count of such longest subsequences.

The idea is to iterate through the array and for each element, look back at all the previous elements to see if they can form an increasing subsequence. For each element at index `i`, if `nums[j] < nums[i]`, then `nums[i]` can potentially extend the increasing subsequences ending at `nums[j]`.

- If the length of subsequence ending at `j` plus one exceeds the length ending at `i`, update the length and the count.
- If they are equal, simply add the count of subsequences ending at `j` to the count at `i`.

By the end of the iteration, you will have the lengths of the longest subsequences and their counts. 

**Algorithm**:
1. Initialize two arrays `lengths` and `counts` of the same size as `nums` with 1s. 
   - `lengths[i]` will store the length of longest subsequence ending at index `i`.
   - `counts[i]` will store the number of longest subsequences ending at index `i`.

2. Iterate over the array with two pointers `i` and `j`. For each `i`, iterate from start till `i` with `j`.
   - If `nums[j] < nums[i]`, check the lengths:
     - If `lengths[j] + 1 > lengths[i]`, update `lengths[i]` to `lengths[j] + 1` and set `counts[i]` to `counts[j]`.
     - If `lengths[j] + 1 === lengths[i]`, increment `counts[i]` by `counts[j]`.

3. Find the maximum value in `lengths` which will be the length of the longest increasing subsequence.
4. Sum the values in `counts` for all indices where the length matches the maximum value.

**Time Complexity:** O(n^2)

**Space Complexity:** O(n) for storing the `lengths` and `counts` arrays.

```javascript
function findNumberOfLIS(nums) {
    const n = nums.length;
    if (n <= 1) return n;

    const lengths = Array(n).fill(1); // lengths[i] = length of longest ending in nums[i]
    const counts = Array(n).fill(1);  // count[i] = number of longest ending in nums[i]

    for (let i = 0; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[j] < nums[i]) { // ensures increasing order
                if (lengths[j] + 1 > lengths[i]) {
                    lengths[i] = lengths[j] + 1;
                    counts[i] = counts[j];
                } else if (lengths[j] + 1 === lengths[i]) {
                    counts[i] += counts[j];
                }
            }
        }
    }

    // Find the max length
    const maxLen = Math.max(...lengths);
    // Sum up the counts of all subsequences with the max length
    let result = 0;
    for (let i = 0; i < n; i++) {
        if (lengths[i] === maxLen) {
            result += counts[i];
        }
    }
    return result;
}
```

### Solution 2: Dynamic Programming with Bit Manipulation

This problem doesn't intuitively translate to bit manipulation, and a bit manipulation solution might not be practical or straightforward. The nature of maintaining both the lengths and counts of subsequences doesn't lend itself well to a direct bit manipulation approach, primarily due to the varying subsequence lengths and required summation operations. Therefore, the primary approach remains DP.

Given the structure of the problem, the dynamic programming approach described remains the most optimal solution.

