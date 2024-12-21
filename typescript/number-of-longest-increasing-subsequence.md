## [LeetCode 673: Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

### Table of Contents
- [Approach 1: Dynamic Programming (Basic Version)](#approach-1-dynamic-programming-basic-version)
- [Approach 2: Dynamic Programming with Optimized Computation](#approach-2-dynamic-programming-with-optimized-computation)

### Approach 1: Dynamic Programming (Basic Version)

#### Intuition
The problem asks us to find the number of longest increasing subsequences. This is quite similar to the classic problem of finding the length of the longest increasing subsequence (LIS). Here, we also need to keep track of the number of such subsequences.

The basic idea is to use dynamic programming where:
- `length[i]` represents the length of the longest increasing subsequence ending at index `i`.
- `count[i]` keeps track of the number of longest increasing subsequences that end with `nums[i]`.

We initialize each `length[i]` to 1 and `count[i]` to 1 because a single element can be considered as an increasing subsequence of length 1 with 1 way to achieve it.

We iterate through each pair of indices `(i, j)` where `0 <= j < i`, to update `length[i]` and `count[i]` based on conditions from `nums[j]` to `nums[i]`.

#### Code
```typescript
function findNumberOfLIS(nums: number[]): number {
    const n = nums.length;
    if (n <= 1) return n;

    const length = new Array(n).fill(1);
    const count = new Array(n).fill(1);

    for (let i = 0; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                if (length[j] + 1 > length[i]) {
                    // Update the longest length ending at i
                    length[i] = length[j] + 1;
                    count[i] = count[j]; // Inherit the count
                } else if (length[j] + 1 === length[i]) {
                    // Another sequence of same length found
                    count[i] += count[j];
                }
            }
        }
    }

    const longest = Math.max(...length);
    let totalCount = 0;
    for (let i = 0; i < n; i++) {
        if (length[i] === longest) {
            totalCount += count[i];
        }
    }
    
    return totalCount;
}
```

#### Complexity
- **Time Complexity**: O(n²), because there are two nested loops each over the size of the array `n`.
- **Space Complexity**: O(n), for storing the `length` and `count` arrays.

### Approach 2: Dynamic Programming with Optimized Computation

#### Intuition
To optimize further, one potential improvement is the usage of more sophisticated data structures, such as segment trees or binary indexed trees, to efficiently maintain and query the number of subsequences. Given the problem requirements, a full remodel involving these is more complex and not strictly necessary without further constraints (e.g., very large `n`).

Thus, Approach 1 is often the practically optimal solution. For completeness, I outline potential further investigations.

This remains theoretical here, as algorithm complexity remains high, but implementing segment trees can turn range update/query complexity to better than O(n), yet generally with larger hidden constants.

### Future Steps
- Explore data structures like segment trees or Fenwick trees if working towards optimizing multiple sequence computations with constraints.
- Gain mastery on techniques that might leverage this problem structure for more real-world constraints with heavier performance demands.

Each solution's focus ultimately weighs on balancing complexity, predictiveness, and problem constraints towards the optimal direction for dynamic scenarios.

