[Leetcode Problem 1235: Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming with Sorting](#dynamic-programming-with-sorting)
3. [Dynamic Programming with Binary Search](#dynamic-programming-with-binary-search)

---

### Brute Force Approach

#### Intuition
In this approach, we compute the maximum profit by considering each job individually and recursively deciding whether to include it in our job schedule or not. This can be seen as a typical knapsack problem where we need to decide whether to include an item or not.

1. **Sort Jobs**: Start by sorting the jobs by their starting times.
2. **Recursive Function**: For each job, decide whether to include it in the current job schedule and move to the next non-conflicting job.
3. **Maximum Profit**: Compute the maximum of including the job or skipping it.

#### Steps

1. Sort the jobs based on their ending times.
2. Use a recursive function to explore all job schedules.
3. For each job, choose to include it or skip it.
4. Return the maximum profit from including or excluding the current job.

#### Code

```csharp
public class Job
{
    public int StartTime { get; set; }
    public int EndTime { get; set; }
    public int Profit { get; set; }
}

public class Solution {
    public int JobScheduling(int[] startTime, int[] endTime, int[] profit) {
        List<Job> jobs = new List<Job>();

        // Create the list of jobs
        for (int i = 0; i < startTime.Length; i++) {
            jobs.Add(new Job { StartTime = startTime[i], EndTime = endTime[i], Profit = profit[i] });
        }

        // Sort jobs based on end time
        jobs.Sort((a, b) => a.EndTime.CompareTo(b.EndTime));

        // Start the recursive exploration
        return JobSchedulingRecur(jobs, 0);
    }
    
    private int JobSchedulingRecur(List<Job> jobs, int currentIndex) {
        if (currentIndex >= jobs.Count) {
            return 0;
        }

        // Find next job which does not conflict with the current job
        int nextIndex = currentIndex + 1;
        while (nextIndex < jobs.Count && jobs[nextIndex].StartTime < jobs[currentIndex].EndTime) {
            nextIndex++;
        }

        // Include current job
        int includeJob = jobs[currentIndex].Profit + JobSchedulingRecur(jobs, nextIndex);

        // Exclude current job
        int excludeJob = JobSchedulingRecur(jobs, currentIndex + 1);

        // Return the max of including or excluding the job
        return Math.Max(includeJob, excludeJob);
    }
}
```

#### Time and Space Complexity
- **Time Complexity**: \(O(2^n)\). Each job can be included or excluded, leading to an exponential number of combinations.
- **Space Complexity**: \(O(n)\). Stack space due to recursion call.

---

### Dynamic Programming with Sorting

#### Intuition
This approach uses dynamic programming to store the maximum profit obtainable up to each job without overlapping. We will iteratively build up the solution using previously computed solutions for subproblems.

1. **Sort Jobs**: First, we sort jobs based on their ending times, just like in the brute force approach.
2. **DP Array**: Use a dynamic programming array (`dp`) where `dp[i]` represents the maximum profit obtainable up to the ith job.
3. **Iterative Calculation**: For each job, find the last non-conflicting job and update the dp array with the calculated maximum profit.

#### Code

```csharp
public class Solution {
    public int JobScheduling(int[] startTime, int[] endTime, int[] profit) {
        int n = startTime.Length;
        List<Job> jobs = new List<Job>();

        // Create the list of jobs
        for (int i = 0; i < n; i++) {
            jobs.Add(new Job { StartTime = startTime[i], EndTime = endTime[i], Profit = profit[i] });
        }

        // Sort jobs based on end time
        jobs.Sort((a, b) => a.EndTime.CompareTo(b.EndTime));

        int[] dp = new int[n];
        dp[0] = jobs[0].Profit;

        for (int i = 1; i < n; i++) {
            int includeProfit = jobs[i].Profit;
            int lastIndex = -1;

            for (int j = i - 1; j >= 0; j--) {
                if (jobs[j].EndTime <= jobs[i].StartTime) {
                    lastIndex = j;
                    break;
                }
            }

            if (lastIndex != -1) {
                includeProfit += dp[lastIndex];
            }

            dp[i] = Math.Max(dp[i - 1], includeProfit);
        }

        return dp[n - 1];
    }
}
```

#### Time and Space Complexity
- **Time Complexity**: \(O(n^2)\). For each job, we search for the last non-conflicting job.
- **Space Complexity**: \(O(n)\). To store dp array.

---

### Dynamic Programming with Binary Search

#### Intuition
To further optimize the searching of the last non-conflicting job, we use binary search. This reduces the nested loop nature and provides more efficient searching capability.

1. **Job Sorting**: Start by sorting jobs based on their ending times.
2. **Binary Search for Last Non-Conflicting Job**: Use binary search to efficiently find the last job before the current job that doesn't overlap.
3. **DP Array**: Use the same dynamic programming approach but with the enhanced search via binary search to quickly calculate the maximum profit.

#### Code

```csharp
public class Solution {
    public int JobScheduling(int[] startTime, int[] endTime, int[] profit) {
        int n = startTime.Length;
        List<Job> jobs = new List<Job>();

        // Create the list of jobs
        for (int i = 0; i < n; i++) {
            jobs.Add(new Job { StartTime = startTime[i], EndTime = endTime[i], Profit = profit[i] });
        }

        // Sort jobs based on end time
        jobs.Sort((a, b) => a.EndTime.CompareTo(b.EndTime));

        int[] dp = new int[n];
        dp[0] = jobs[0].Profit;

        for (int i = 1; i < n; i++) {
            int includeProfit = jobs[i].Profit;
            int lastIndex = BinarySearch(jobs, i);

            if (lastIndex != -1) {
                includeProfit += dp[lastIndex];
            }

            dp[i] = Math.Max(dp[i - 1], includeProfit);
        }

        return dp[n - 1];
    }

    private int BinarySearch(List<Job> jobs, int index) {
        int start = 0, end = index - 1;
        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (jobs[mid].EndTime <= jobs[index].StartTime) {
                if (jobs[mid + 1].EndTime <= jobs[index].StartTime) {
                    start = mid + 1;
                } else {
                    return mid;
                }
            } else {
                end = mid - 1;
            }
        }
        return -1;
    }
}
```

#### Time and Space Complexity
- **Time Complexity**: \(O(n \log n)\). Sorting takes \(O(n \log n)\), and for each job, binary search runs in \(O(\log n)\).
- **Space Complexity**: \(O(n)\). To store dp array.

This systematic progression from a brute force to an optimized dynamic programming solution demonstrates how adding strategic techniques like sorting and binary search can significantly improve performance for scheduling problems like the maximum profit in job scheduling.

