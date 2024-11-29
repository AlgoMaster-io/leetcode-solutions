# 300. [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

## Approach 1: Dynamic Programming

### Solution
javascript
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
function lengthOfLIS(nums) {
    if (!nums || nums.length === 0) {
        return 0;
    }

    const dp = new Array(nums.length).fill(1); // dp array to store the LIS ending at each index
    let maxLength = 1; // Minimum LIS length is 1 (a single element)

    // Build the dp array
    for (let i = 1; i < nums.length; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[i] > nums[j]) { // Check if nums[i] can extend the subsequence at nums[j]
                dp[i] = Math.max(dp[i], dp[j] + 1); // Update dp[i] with the maximum length found
            }
        }
        maxLength = Math.max(maxLength, dp[i]); // Update the overall LIS length
    }

    return maxLength;
}
```

## Approach 2: Binary Search with Patience Sorting

### Solution
javascript
```javascript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
function lengthOfLIS(nums) {
    if (!nums || nums.length === 0) {
        return 0;
    }

    const tails = new Array(nums.length); // Array to store the smallest tail for all increasing subsequences
    let size = 0; // Size of the effective array

    for (let num of nums) {
        let i = 0, j = size; // Binary search for the insertion point of num in tails

        while (i < j) {
            let mid = Math.floor((i + j) / 2);
            if (tails[mid] < num) {
                i = mid + 1;
            } else {
                j = mid;
            }
        }

        tails[i] = num; // Replace or extend the increasing subsequence
        if (i === size) {
            size++; // Increment size if a new subsequence tail is created
        }
    }

    return size;
}
```

