# [Leetcode 1235: Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)

## Approaches
- [Approach 1: Dynamic Programming with Recursion and Memoization](#approach-1-dynamic-programming-with-recursion-and-memoization)
- [Approach 2: Iterative Dynamic Programming](#approach-2-iterative-dynamic-programming)
- [Approach 3: Binary Search with Dynamic Programming](#approach-3-binary-search-with-dynamic-programming)

---

## Approach 1: Dynamic Programming with Recursion and Memoization

### Intuition:
The problem is a variation of the Interval Scheduling Maximum Weight problem. The naive approach is to recursively explore each job: either take it or skip it. By using memoization, we can store the results of subproblems and avoid redundant calculations. When taking a job, recursively find the maximum profit including the next non-overlapping job.

### Steps:
1. Represent the jobs with start, end, and profit, and sort them by the end time to ease finding non-overlapping jobs.
2. Use a recursive function with memoization to calculate the maximum profit for any given job, at either including it or skipping it.
3. To include a job, use binary search (or loop) to find the next job that starts after the current job ends.

### C++ Code:
```cpp
#include <vector>
#include <algorithm>
#include <map>

using namespace std;

class Solution {
public:
    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        int n = startTime.size();
        vector<vector<int>> jobs;
        
        // Combine all job information into a single vector and sort by end time
        for (int i = 0; i < n; i++) {
            jobs.push_back({endTime[i], startTime[i], profit[i]});
        }
        sort(jobs.begin(), jobs.end());
        
        // Memoization for top-down DP
        map<int, int> memo;
        return dp(jobs, 0, memo);
    }
    
private:
    int dp(const vector<vector<int>>& jobs, int i, map<int, int>& memo) {
        if (i == jobs.size()) return 0;
        if (memo.count(i)) return memo[i];
        
        // Skip the current job
        int maxProfit = dp(jobs, i + 1, memo);
        
        // Take the current job and find the next non-overlapping job
        int nextJobIdx = findNextJob(jobs, jobs[i][0], i + 1);
        maxProfit = max(maxProfit, jobs[i][2] + dp(jobs, nextJobIdx, memo));
        
        return memo[i] = maxProfit;
    }
    
    int findNextJob(const vector<vector<int>>& jobs, int currentEndTime, int startIdx) {
        for (int i = startIdx; i < jobs.size(); i++) {
            if (jobs[i][1] >= currentEndTime) {
                return i;
            }
        }
        return jobs.size();
    }
};
```

### Complexity:
- **Time Complexity**: \(O(n^2)\), when finding the next job, each call may look at all remaining jobs.
- **Space Complexity**: \(O(n)\), space for the memoization map.

---

## Approach 2: Iterative Dynamic Programming

### Intuition:
By leveraging dynamic programming (DP), we can build solutions for smaller subproblems iteratively. Here, each job can be considered in sequence, choosing the maximum profit between taking or skipping the job. By precomputing each maximum job that can be taken iteratively, we can avoid recursion entirely.

### Steps:
1. Sort jobs based on end times.
2. Use a DP array where dp[i] represents the maximum profit we can achieve by considering the first i jobs.
3. For each job, find the previous job that can be scheduled without conflict using a reverse search.
4. Update the DP array either by taking the job's profit and adding the maximum profit from compatible jobs or by carrying forward the previous maximum.

### C++ Code:
```cpp
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        int n = startTime.size();
        vector<vector<int>> jobs;
        
        // Create a list of jobs and sort based on end times
        for (int i = 0; i < n; i++) {
            jobs.push_back({endTime[i], startTime[i], profit[i]});
        }
        sort(jobs.begin(), jobs.end());
        
        // dp[i] to store the maximum profit when considering the first i jobs
        vector<int> dp(n + 1);
        
        for (int i = 1; i <= n; i++) {
            // Max profit excluding the current job
            dp[i] = dp[i - 1];
            
            // Include the current job
            int inclProfit = jobs[i - 1][2];
            int l = searchLastNonConflict(jobs, i - 1);
            if (l != -1) {
                inclProfit += dp[l + 1];
            }
            dp[i] = max(dp[i], inclProfit);
        }
        return dp[n];
    }

private:
    int searchLastNonConflict(const vector<vector<int>>& jobs, int index) {
        for (int j = index - 1; j >= 0; j--) {
            if (jobs[j][0] <= jobs[index][1]) {
                return j;
            }
        }
        return -1;
    }
};
```

### Complexity:
- **Time Complexity**: \(O(n^2)\) due to each backward search for non-conflicting jobs.
- **Space Complexity**: \(O(n)\) for the DP array.

---

## Approach 3: Binary Search with Dynamic Programming

### Intuition:
To optimize the DP solution, we use binary search within our DP approach to identify the last non-conflicting job more efficiently. This reduces the search space from linear to logarithmic, allowing us to speed up the process of finding compatible jobs.

### Steps:
1. Sort jobs by end time.
2. Implement DP with a sorted list of end times for binary searching the latest non-conflicting job.
3. Ensure each entry in the DP represents the maximum profit using sorted job end times for binary search lookups.

### C++ Code:
```cpp
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        int n = startTime.size();
        vector<vector<int>> jobs;

        // Create a list of jobs and sort based on end times
        for (int i = 0; i < n; i++) {
            jobs.push_back({endTime[i], startTime[i], profit[i]});
        }
        sort(jobs.begin(), jobs.end());

        // DP vector to store maximum profit by the end of i-th job
        vector<int> dp(n);
        dp[0] = jobs[0][2];

        for (int i = 1; i < n; i++) {
            int inclProfit = jobs[i][2];
            int l = binarySearch(jobs, i); // Search for last non-conflicting job index
            if (l != -1) {
                inclProfit += dp[l];
            }
            dp[i] = max(dp[i - 1], inclProfit); // Max profit including or excluding the current job
        }

        return dp[n - 1];
    }

private:
    int binarySearch(const vector<vector<int>>& jobs, int index) {
        int low = 0, high = index - 1;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (jobs[mid][0] <= jobs[index][1]) {
                if (jobs[mid + 1][0] <= jobs[index][1]) {
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
};
```

### Complexity:
- **Time Complexity**: \(O(n \log n)\), achieving logarithmic efficiency through binary search for each job.
- **Space Complexity**: \(O(n)\) for the DP array.

---

Each approach offers insights into different strategies for solving interval scheduling problems, with binary search and dynamic programming culminating in the most efficient approach.

