## [Leetcode 312: Burst Balloons](https://leetcode.com/problems/burst-balloons/)

### Approaches:

1. [Recursion with Memoization](#recursion-with-memoization)
2. [Dynamic Programming](#dynamic-programming)

---

### Approach 1: Recursion with Memoization

**Intuition:**

The problem can be broken down into smaller subproblems: determining the maximum coins collected from a continuous subarray of balloons and storing intermediate results to optimize performance. Since directly bursting balloons starting from either end doesn’t seem to yield an optimal solution, considering every balloon as a potential 'last' balloon to burst in a given valid range provides a way to evaluate its best position.

**Algorithm:**

1. Add two fictitious balloons with value 1 at the boundaries (left and right of the actual balloons array) to avoid handling edge cases separately.
2. Use a recursive helper function `maxCoins` with parameters `(left, right)` returning the maximum coins obtained from balloons within this range.
3. For each balloon `k` between `left` and `right`, consider it as the last to burst, calculate the coins collected from recursively solving `(left, k-1)` and `(k+1, right)` plus the coins from bursting `k` using its neighboring balloons.
4. Store results in a memoization table to avoid redundant calculations.

**Code:**

```typescript
function maxCoins(nums: number[]): number {
    const n = nums.length;
    const newNums = [1, ...nums, 1]; // Padding our list with 1 at both ends
    const memo: number[][] = Array.from({length: n+2}, () => Array(n+2).fill(0)); // Initializing memoization

    function getCoins(left: number, right: number): number {
        if (left + 1 === right) return 0; // Base: No balloon between left and right

        if (memo[left][right] > 0) return memo[left][right]; // Return if calculated

        let max = 0;
        for (let i = left + 1; i < right; i++) {
            // Calculate coins if i is the last balloon to burst between left and right
            const coins = newNums[left] * newNums[i] * newNums[right];
            const total = coins + getCoins(left, i) + getCoins(i, right);
            max = Math.max(max, total); // Keep track of max coins obtainable
        }

        memo[left][right] = max;
        return max;
    }

    return getCoins(0, n + 1); // Run the recursive function from full range
}
```

**Time Complexity:** O(n^3)  
**Space Complexity:** O(n^2) due to memoization storage

---

### Approach 2: Dynamic Programming

**Intuition:**

Transitioning from recursion with memoization, we can iteratively compute results with dynamic programming. The solution relies on systematically determining optimal sequences of balloon bursts over increasing lengths of `intervals`.

**Algorithm:**

1. Construct a new list including the sentinel balloons [1, ..., 1].
2. Create a `dp` table where `dp[i][j]` represents the largest number of coins obtainable from bursting all the balloons in the range`(i, j)`.
3. Iterate over `len` which indicates the length of sections of balloons being considered.
4. For each section length, iterate all possible subsections `(i, j)`.
5. For each possible subarray, calculate and update the maximum possible coins if each `k` in the subsection is the last to burst.
6. Eventually, "dp[0][n + 1]" holds the result.

**Code:**

```typescript
function maxCoins(nums: number[]): number {
    const n = nums.length;
    const newNums = [1, ...nums, 1]; // Start and end with 1 to manage edge cases
    const dp: number[][] = Array.from({ length: n + 2 }, () => Array(n + 2).fill(0));

    for (let len = 2; len < n + 2; len++) {
        for (let left = 0; left < n + 2 - len; left++) {
            const right = left + len;
            for (let i = left + 1; i < right; i++) {
                // Calculating maximum coins obtained by using 'i' as the last burst balloon between left and right
                dp[left][right] = Math.max(
                    dp[left][right],
                    newNums[left] * newNums[i] * newNums[right] + dp[left][i] + dp[i][right]
                );
            }
        }
    }

    return dp[0][n + 1]; // Full range result considering all balloons
}
```

**Time Complexity:** O(n^3)  
**Space Complexity:** O(n^2) for the dynamic programming table

--- 

Both approaches are inherently cubic due to considering every possible sequence of bursting balloons. By rendering intermediate subproblem solutions in a top-down or bottom-up manner, we resolve the complex nested dependencies efficiently.

