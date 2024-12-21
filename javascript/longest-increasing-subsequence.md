# [Leetcode 300: Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

## Approaches:
- [Dynamic Programming Solution](#dynamic-programming-solution)
- [Optimized Dynamic Programming with Binary Search](#optimized-dynamic-programming-with-binary-search)

---

## Dynamic Programming Solution

### Intuition

The idea here is to use a dynamic programming approach to solve the problem. We want to find the longest increasing subsequence, and for this, we can keep track of the length of the longest subsequence that ends at each element in the array. 

For each element `nums[i]`, we look at all the previous elements `nums[j]` (where `j < i`). If `nums[j]` is less than `nums[i]`, it means `nums[i]` can form an increasing sequence with `nums[j]`. We then update our `dp[i]` to be the maximum of its current value and `dp[j] + 1`. 

### Approach

1. Create a `dp` array of the same length as `nums`, where each index represents the longest increasing subsequence ending at that element.
2. Initialize each element of `dp` to 1, as every element is a subsequence of length 1 by itself.
3. Iterate over each element `i` in `nums`:
   - For each `i`, iterate over each element `j` which comes before `i`:
     - If `nums[j] < nums[i]`, update `dp[i]` as `dp[i] = Math.max(dp[i], dp[j] + 1)`.
4. The result is the maximum value in the `dp` array which represents the longest increasing subsequence.

### Code

```javascript
function lengthOfLIS(nums) {
    if (nums.length === 0) return 0;

    const dp = new Array(nums.length).fill(1);

    for (let i = 1; i < nums.length; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
    }

    return Math.max(...dp);
}
```

### Time and Space Complexity

- **Time Complexity**: O(n^2), where `n` is the length of the `nums` array. This is due to the nested loops over the `nums` array.
- **Space Complexity**: O(n), for the `dp` array.

---

## Optimized Dynamic Programming with Binary Search

### Intuition

We can optimize the Dynamic Programming solution using Binary Search. Instead of maintaining a `dp` array with the longest increasing subsequence length at each index, we maintain a `tails` array where each element represents the smallest ending element of an increasing subsequence of that length.

By using binary search, we can efficiently update the `tails` array to ensure that potential subsequences build correctly.

### Approach

1. Create an empty `tails` array.
2. Iterate over each element `x` in `nums`:
   - Use binary search to determine the index `i` at which to replace or append `x`.
   - If `x` is larger than the largest element in `tails`, append `x` to the end of `tails`.
   - Otherwise, replace the first element in `tails` that is greater than or equal to `x`.
3. The length of the `tails` array is the length of the longest increasing subsequence.

### Code

```javascript
function lengthOfLIS(nums) {
    const tails = [];

    for (let x of nums) {
        let left = 0, right = tails.length;
        
        // Binary search for the insertion position
        while (left < right) {
            const mid = Math.floor((left + right) / 2);
            
            if (tails[mid] < x) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }

        // Replace or append the current number
        if (left < tails.length) {
            tails[left] = x;
        } else {
            tails.push(x);
        }
    }

    return tails.length;
}
```

### Time and Space Complexity

- **Time Complexity**: O(n log n), where `n` is the length of the `nums` array. This is due to the binary search operation for each element.
- **Space Complexity**: O(n), in the worst case, we store all elements in the `tails` array.

