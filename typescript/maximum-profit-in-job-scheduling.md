# [Leetcode 1235: Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)

## Approaches
- [Approach 1: Recursive with Memoization](#approach-1-recursive-with-memoization)
- [Approach 2: Dynamic Programming with Binary Search](#approach-2-dynamic-programming-with-binary-search)

### Approach 1: Recursive with Memoization

**Intuition:**
The problem can be visualized as a decision-making process where at each step, we decide whether to include a job in our schedule or not. A recursive solution that explores all possibilities will inevitably involve recalculating the same subproblems multiple times, making it inefficient. Memoization helps by caching the results of these subproblems, thereby reducing computation time.

In this solution, we sort the jobs by start time. For each job, we decide whether to include it and add its profit to the result of solving the subproblem starting at its end time, or skip it and proceed to the next job.

**Steps:**
1. Sort the jobs based on their start time.
2. Use a recursive function to evaluate the maximum profit starting from each job.
3. Use binary search to quickly find the next job that can be started after the current one ends (to avoid overlapping).
4. Memoize results for each starting index to avoid re-calculation.

**Code:**

```typescript
function jobScheduling(startTime: number[], endTime: number[], profit: number[]): number {
    const n = startTime.length;
    const jobs = startTime.map((s, i) => ({ start: s, end: endTime[i], profit: profit[i] }));

    // Sort jobs by start time
    jobs.sort((a, b) => a.start - b.start);

    // Cache for memoization
    const memo: number[] = new Array(n).fill(-1);

    function dfs(index: number): number {
        if (index === n) return 0;

        if (memo[index] !== -1) return memo[index];

        // Binary search for the next job that starts after the current job ends
        let nextIndex = index + 1;
        let low = nextIndex, high = n;
        while (low < high) {
            const mid = Math.floor((low + high) / 2);
            if (jobs[mid].start >= jobs[index].end) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }

        // Include current job
        const includeProfit = jobs[index].profit + (low < n ? dfs(low) : 0);

        // Exclude current job
        const excludeProfit = dfs(index + 1);

        memo[index] = Math.max(includeProfit, excludeProfit);
        return memo[index];
    }

    return dfs(0);
}
```

**Time Complexity:** O(n log n) due to sorting and binary searching.  
**Space Complexity:** O(n) for the memoization cache.

### Approach 2: Dynamic Programming with Binary Search

**Intuition:**
In this approach, we use an iterative dynamic programming strategy, which simplifies the recursion seen in Approach 1. The key insight here is to use a sorted list of jobs and build a DP table where each entry at index `i` represents the maximum profit obtainable using jobs `[i, n-1]`. By employing binary search, we quickly find the next non-overlapping job that can be scheduled after the current one.

**Steps:**
1. Sort jobs by their end time.
2. Build a DP array where dp[i] reflects the maximum profit from scheduling jobs `i` through `n-1`.
3. Iterate backward over the jobs, updating the DP table by deciding whether to take the current job or skip it.
4. Use binary search to find the earliest job that starts after the current job ends.

**Code:**

```typescript
function jobScheduling(startTime: number[], endTime: number[], profit: number[]): number {
    const n = startTime.length;
    const jobs = startTime.map((s, i) => ({ start: s, end: endTime[i], profit: profit[i] }));

    // Sort jobs by end time
    jobs.sort((a, b) => a.end - b.end);

    const dp: number[] = new Array(n + 1).fill(0);

    for (let i = n - 1; i >= 0; i--) {
        // Binary search for the next job that starts after the current job ends
        let low = i + 1, high = n;
        while (low < high) {
            const mid = Math.floor((low + high) / 2);
            if (jobs[mid].start >= jobs[i].end) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }

        // Choose the max between including or excluding the current job
        dp[i] = Math.max(jobs[i].profit + dp[low], dp[i + 1]);
    }

    return dp[0];
}
```

**Time Complexity:** O(n log n) due to sorting and binary searches within the loop.  
**Space Complexity:** O(n) for the DP table.

These solutions use a combination of sorting, dynamic programming, and binary search to effectively manage overlapping intervals and maximize scheduling profit.

