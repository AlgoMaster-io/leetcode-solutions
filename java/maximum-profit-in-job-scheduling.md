# [1235. Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)

## Approaches
- [Approach 1: Recursion with Memoization (Dynamic Programming)](#approach-1)
- [Approach 2: Iterative Dynamic Programming Using Sorting and Binary Search](#approach-2)

---

## Approach 1: Recursion with Memoization (Dynamic Programming)

### Intuition
The problem can be broken down into deciding which jobs to take to maximize the profit. For each job, you have a choice of either taking the job or skipping it. If you take the job, you cannot take any other jobs overlapping with it. Dynamic programming with memoization helps avoid recalculating results of overlapping subproblems.

### Steps
1. **Sort Jobs by End Time**: This will allow us to process jobs in a logical order (left to right in scheduling terms).
2. **Use Dynamic Programming with Recursion**:
   - Define a recursive function `maxProfit(index)` that returns the maximum profit starting from the job at `index`.
   - Check two cases: 
     1. Skip the current job and call `maxProfit(index + 1)`.
     2. Include the current job, find the next job that can start after the current job ends using binary search, and call `maxProfit(nextIndex)`.
   - Use memoization to store and reuse results for each `index`.

### Java Code
```java
import java.util.*;

class Job {
    int start, end, profit;
    
    Job(int start, int end, int profit) {
        this.start = start;
        this.end = end;
        this.profit = profit;
    }
}

public class Solution {
    public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {
        int n = startTime.length;
        Job[] jobs = new Job[n];
        
        // Create job objects for easy handling
        for (int i = 0; i < n; i++) {
            jobs[i] = new Job(startTime[i], endTime[i], profit[i]);
        }
        
        // Sort jobs based on their ending time for easy scheduling
        Arrays.sort(jobs, Comparator.comparingInt(job -> job.end));
        
        // Memoization array for storing maximum profit at each position
        int[] memo = new int[n];
        Arrays.fill(memo, -1);
        
        return maxProfit(0, jobs, memo);
    }
    
    private int maxProfit(int index, Job[] jobs, int[] memo) {
        if (index >= jobs.length) return 0;
        
        if (memo[index] != -1) return memo[index];
        
        // Find next non-conflicting job using binary search
        int nextIndex = findNextJob(index, jobs);
        
        // Calculate max profit if we take or skip current job
        int takeJob = jobs[index].profit + maxProfit(nextIndex, jobs, memo);
        int skipJob = maxProfit(index + 1, jobs, memo);
        
        // Memoize the result for current index
        return memo[index] = Math.max(takeJob, skipJob);
    }
    
    private int findNextJob(int index, Job[] jobs) {
        int low = index + 1, high = jobs.length;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (jobs[mid].start < jobs[index].end) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n log n) due to sorting and binary search for each job.
- **Space Complexity**: O(n) for the memoization array.

---

## Approach 2: Iterative Dynamic Programming Using Sorting and Binary Search

### Intuition
Instead of using recursion, this approach uses an iterative method to build up solutions for every job from left to right. The addition of binary search helps in quickly finding the next available job that doesn't overlap with the current job.

### Steps
1. **Sort Jobs by End Time**: Same as before, sort jobs to handle them in scheduling order.
2. **Dynamic Array `dp`**: `dp[i]` holds the maximum profit considering jobs up to `i`.
3. **Iterate Over Each Job**:
   - Use binary search to find the maximum profit of the non-overlapping job.
   - Update `dp[i]` by comparing the addition of the current job's profit and the maximum profit of the last non-overlapping job with `dp[i-1]`.

### Java Code
```java
import java.util.*;

class Solution {
    public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {
        int n = startTime.length;
        Job[] jobs = new Job[n];
        
        for (int i = 0; i < n; i++) {
            jobs[i] = new Job(startTime[i], endTime[i], profit[i]);
        }
        
        Arrays.sort(jobs, (a, b) -> a.end - b.end);
        
        int[] dp = new int[n];
        dp[0] = jobs[0].profit;
        
        for (int i = 1; i < n; i++) {
            int includeProfit = jobs[i].profit;
            int l = binarySearch(jobs, i);
            if (l != -1) {
                includeProfit += dp[l];
            }
            dp[i] = Math.max(includeProfit, dp[i - 1]);
        }
        
        return dp[n - 1];
    }
    
    private int binarySearch(Job[] jobs, int index) {
        int low = 0, high = index - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (jobs[mid].end <= jobs[index].start) {
                if (jobs[mid + 1].end <= jobs[index].start) {
                    low = mid + 1;
                } else {
                    return mid;
                }
            } else {
                high = mid - 1;
            }
        }
        return -1;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n log n) due to sorting and binary search for each job.
- **Space Complexity**: O(n) for maintaining the dynamic programming table.

Both approaches are efficient, but the iterative DP solution might be easier to understand and debug than the recursive approach with memoization.

