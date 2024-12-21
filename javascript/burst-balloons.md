# [Leetcode 312: Burst Balloons](https://leetcode.com/problems/burst-balloons/)

## Solutions: 
- [Dynamic Programming Approach](#dynamic-programming-approach)

### Dynamic Programming Approach

**Intuition:**  
The Burst Balloons problem is a classic dynamic programming problem. The task is to burst all balloons between two ends with the maximum coins collected when doing so, considering each balloon burst brings coins equivalent to the product of the balloon itself and its neighboring balloons at the time of bursting.

The core idea is to consider using a dynamic programming (DP) table to store the results of subproblems. We will solve this problem using bottom-up dynamic programming, where we fill up a DP table where DP[i][j] represents the maximum coins we can collect by bursting all the balloons between index `i` and `j` (inclusive of `i` and `j`), assuming the balloons at index (i-1) and (j+1) are untouched.

**Approach:**

1. First, to effectively deal with edge cases and simplify boundary conditions, pad the `nums` array with 1 at both ends. This accounts for the virtual balloons that surround our actual balloons in nums.

2. Define a `dp` array where `dp[i][j]` will store the maximum coins obtainable from bursting all balloons strictly between index `i` and `j`.

3. Iterate over lengths of subarrays (`L`), ranging from 1 to n (length of the nums after padding).

4. For each pair `(i, j)` that has `L-1` balloons between them, compute the result by choosing a balloon `k` to burst last in this range `(i, j)` and add the results of solving the subproblems `(i, k)` and `(k, j)`.

5. The actual coins collected from bursting balloon `k` is given by the product `nums[i]*nums[k]*nums[j]`. Update the `dp[i][j]` accordingly.

6. After considering all subproblems, `dp[0][n+1]` gives the max coins collectable from the complete input.

**Time Complexity:** O(n^3) because of the three nested loops.  
**Space Complexity:** O(n^2) because of the DP table.

```javascript
function maxCoins(nums) {
    // Add 1 before and after nums, this results in a new list with padding
    nums = [1, ...nums, 1];
    const n = nums.length;
    // Create the dp array initialized with zeros
    const dp = Array.from({ length: n }, () => Array(n).fill(0));

    // Iterate over subarray lengths
    for (let length = 1; length < n - 1; length++) {
        // Iterate over subarray starting points
        for (let left = 0; left < n - length - 1; left++) {
            const right = left + length + 1;
            // Try to burst each balloon in the subarray last
            for (let i = left + 1; i < right; i++) { 
                // Calculate the maximum coins by bursting balloon i last in this range
                const coins = nums[left] * nums[i] * nums[right];
                dp[left][right] = Math.max(dp[left][right], dp[left][i] + coins + dp[i][right]);
                // Break down of computation:
                // - dp[left][i]: max coins from bursting all balloons between left and i
                // - coins: coins obtained from bursting balloon i, considering its neighbors left and right
                // - dp[i][right]: max coins from bursting all balloons between i and right
            }
        }
    }

    // Max coins from bursting all balloons between the two added 1's
    return dp[0][n - 1];
}
```


