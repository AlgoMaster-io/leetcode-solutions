# [Leetcode 1235: Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)

## Navigation
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming with Binary Search](#dynamic-programming-with-binary-search)
3. [Dynamic Programming with Optimized Search](#dynamic-programming-with-optimized-search)

## Brute Force Approach

### Intuition
The brute force method is about trying every possible combination of jobs and selecting the one which yields the maximum profit. For each job, we decide whether to include it in our schedule or not, and through recursive calls, simulate every possible job scheduling scenario.

### Approach
The brute force approach essentially generates all possible subsets of jobs and calculates the total profit for each. The goal is to find a subset that does not have scheduling conflicts (i.e., job times overlap) and yields the maximum profit.

### Time Complexity
- **Time:** O(2^N) due to all possible subsets being explored.
- **Space:** O(N) for the maximum depth of the recursion stack.

```go
func jobSchedulingBF(startTime []int, endTime []int, profit []int) int {
    return maxProfit(0, startTime, endTime, profit, make(map[int]int))
}

func maxProfit(currentIndex int, startTime, endTime, profit []int, memo map[int]int) int {
    if currentIndex >= len(startTime) {
        return 0
    }
    if val, exists := memo[currentIndex]; exists {
        return val
    }
    // Exclude the current job
    maxProfitWithOutCurrent := maxProfit(currentIndex+1, startTime, endTime, profit, memo)
    
    // Find the next job which starts after the current job ends
    nextIndex := currentIndex + 1
    for nextIndex < len(startTime) && startTime[nextIndex] < endTime[currentIndex] {
        nextIndex++
    }
    // Include the current job
    maxProfitWithCurrent := profit[currentIndex] + maxProfit(nextIndex, startTime, endTime, profit, memo)
    
    // Take the maximum of both scenarios
    result := max(maxProfitWithOutCurrent, maxProfitWithCurrent)
    memo[currentIndex] = result
    return result
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Dynamic Programming with Binary Search

### Intuition
This approach uses dynamic programming alongside binary search to optimize the selection of non-conflicting jobs. By sorting jobs according to their end times, we can use a binary search to find the index of the next job that starts after the current job ends, thus avoiding overlapping jobs efficiently.

### Approach
1. Sort the jobs by their end times.
2. Use dynamic programming where `dp[i]` represents the maximum profit we can obtain by considering the first `i` jobs.
3. For each job, use binary search to find the next job that does not overlap, then choose between including the job or not.

### Time Complexity
- **Time:** O(N log N) due to sorting and binary searches.
- **Space:** O(N) for the dynamic programming table.

```go
import "sort"

func jobSchedulingDP(startTime []int, endTime []int, profit []int) int {
    n := len(startTime)
    jobs := make([][3]int, n)
    
    // Creating job tuples
    for i := 0; i < n; i++ {
        jobs[i] = [3]int{startTime[i], endTime[i], profit[i]}
    }
    
    // Sort jobs based on end time
    sort.Slice(jobs, func(i, j int) bool {
        return jobs[i][1] < jobs[j][1]
    })
    
    dp := make([]int, n+1)
    
    for i := 1; i <= n; i++ {
        // Skip current job
        nonProfitJob := dp[i-1]
        
        // Find the latest job that doesn't conflict with the current job
        idx := sort.Search(i, func(j int) bool {
            return jobs[j][1] > jobs[i-1][0]
        })
        
        // Include current job profit
        includeProfit := jobs[i-1][2]
        if idx > 0 {
            includeProfit += dp[idx]
        }
        
        // Maximum profit by including or excluding the current job
        dp[i] = max(nonProfitJob, includeProfit)
    }
    
    return dp[n]
}
```

## Dynamic Programming with Optimized Search

### Intuition
This approach optimizes the dynamic programming approach even further by using structures to store the start, end, and profit in an efficient manner to directly retrieve the maximum profit. This reduces the total time complexity slightly by clever use of iterating from the end and using a map to store maximums.

### Approach
1. The jobs are pre-sorted by end time.
2. Utilize a map to store end times and corresponding maximum profit up to that end time.
3. While iterating through jobs, the map acts like a direct access table for maximum profit up until now, avoiding full binary searches.

### Time Complexity
- **Time:** O(N log N) which is mainly due to sorting. The search step is optimized using a map.
- **Space:** O(N) for the map and auxiliary structures.

```go
func jobSchedulingOptimized(startTime []int, endTime []int, profit []int) int {
    n := len(startTime)
    jobs := make([][3]int, n)

    for i := 0; i < n; i++ {
        jobs[i] = [3]int{startTime[i], endTime[i], profit[i]}
    }

    // Sort jobs by their end time
    sort.Slice(jobs, func(i, j int) bool {
        return jobs[i][1] < jobs[j][1]
    })
    
    dp := make(map[int]int)
    dp[0] = 0
    maxProfit := 0

    for i := 0; i < n; i++ {
        // Find last non-conflicting job
        start := jobs[i][0]
        profit := jobs[i][2]
        idx := sort.Search(len(dp), func(j int) bool {
            return jobs[j][1] <= start
        })

        prevJobProfit := 0
        if idx > 0 {
            prevJobProfit = dp[idx]
        }

        currentProfit := prevJobProfit + profit
        maxProfit = max(maxProfit, currentProfit)
        dp[i+1] = maxProfit
    }

    return maxProfit
}
```

These solutions progressively optimize both time and space complexity while maintaining clarity and correctness. As with many dynamic programming problems, the key is in choosing an efficient state representation and transition to handle overlapping subproblems optimally.

