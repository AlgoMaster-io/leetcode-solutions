# [Leetcode 1235: Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)

### Approaches:
- [Approach 1: Recursion with Memoization](#approach-1-recursion-with-memoization)
- [Approach 2: Dynamic Programming with Binary Search](#approach-2-dynamic-programming-with-binary-search)

## Approach 1: Recursion with Memoization

### Intuition
The core idea is to make decisions for each job: either include the job in our schedule or skip it. If we include it, we need to find the next possible job that does not overlap with the current job's end time. This can be efficiently solved using recursion, but a naive recursive approach will have overlapping subproblems, hence we use memoization to store results of subproblems that we have already computed.

### Code
```javascript
function jobScheduling(startTime, endTime, profit) {
    const n = startTime.length;
    const jobs = [];
    
    // Combine start, end, and profit into a single array of jobs
    for (let i = 0; i < n; i++) {
        jobs.push([startTime[i], endTime[i], profit[i]]);
    }
    
    // Sort jobs based on end time
    jobs.sort((a, b) => a[1] - b[1]);
    
    const memo = new Array(n).fill(-1);
    
    // Recursive function to find max profit
    function findMaxProfit(index) {
        if (index >= n) {
            return 0;
        }
        
        if (memo[index] !== -1) {
            return memo[index];
        }
        
        // Find the next job that doesn't overlap
        let nextJob = index + 1;
        while (nextJob < n && jobs[nextJob][0] < jobs[index][1]) {
            nextJob++;
        }
        
        // Choose the maximum between including the current job and skipping it
        const includeProfit = jobs[index][2] + findMaxProfit(nextJob);
        const excludeProfit = findMaxProfit(index + 1);
        
        memo[index] = Math.max(includeProfit, excludeProfit);
        return memo[index];
    }
    
    return findMaxProfit(0);
}
```

### Time Complexity
- **O(n^2)** due to the recursive evaluation and the loop to find the next non-overlapping job.
- **Space Complexity**: **O(n)** due to the memoization array.

## Approach 2: Dynamic Programming with Binary Search

### Intuition
This approach improves upon the recursion by avoiding duplicated calculations and efficiently finding the next non-overlapping job using binary search. We sort the jobs by their end time and use a dp array, where `dp[i]` represents the maximum profit by scheduling jobs from `0` to `i`.

For each job, we either include it and add its profit to the maximum profit we can gain by scheduling non-conflicting jobs before it, or we exclude the job. To find the non-conflicting job efficiently, we use binary search.

### Code
```javascript
function jobScheduling(startTime, endTime, profit) {
    const n = startTime.length;
    const jobs = [];
    
    // Combine start, end, and profit into a single array of jobs
    for (let i = 0; i < n; i++) {
        jobs.push([startTime[i], endTime[i], profit[i]]);
    }
    
    // Sort jobs based on end time
    jobs.sort((a, b) => a[1] - b[1]);
    
    const dp = new Array(n).fill(0);
    dp[0] = jobs[0][2];
    
    for (let i = 1; i < n; i++) {
        const currentJobProfit = jobs[i][2];
        
        // Binary search to find the last job not overlapping with current job
        let l = 0, r = i - 1, lastNonConflicting = -1;
        while (l <= r) {
            const m = Math.floor((l + r) / 2);
            if (jobs[m][1] <= jobs[i][0]) {
                lastNonConflicting = m;
                l = m + 1;
            } else {
                r = m - 1;
            }
        }
        
        // Include current job (if applicable) and the best from non-conflicting ones
        const includeProfit = currentJobProfit + (lastNonConflicting != -1 ? dp[lastNonConflicting] : 0);
        
        // Choose the maximum profit by including or excluding the current job
        dp[i] = Math.max(dp[i - 1], includeProfit);
    }
    
    return dp[n - 1];
}
```

### Time Complexity
- **O(n log n)** due to the sorting of jobs and binary search operations.
- **Space Complexity**: **O(n)** for the dp array.

This problem is a classical example where multiple dynamic programming techniques and optimizations can be applied based on overlapping subproblem identification and efficient job scheduling logic.

