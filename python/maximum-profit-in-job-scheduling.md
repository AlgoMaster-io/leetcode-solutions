## [Leetcode 1235: Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)

### Approaches
- [Dynamic Programming using Events](#dynamic-programming-using-events)
- [Dynamic Programming with Binary Search](#dynamic-programming-with-binary-search)

### Dynamic Programming using Events

#### Intuition
The problem involves selecting non-overlapping jobs to maximize the profit. A naïve approach is to consider all possible subsets of jobs, but this would be computationally infeasible due to the large number of combinations. Instead, we can leverage dynamic programming to efficiently solve this problem by considering each job's influence on a cumulative profit timeline.

#### Solution
1. Sort all events (start and end of a job) based on the time. This helps in processing events in a chronological order.
2. Create a dictionary `dp` to store the maximum profit achievable up to a certain time.
3. Traverse each event:
   - If it is a start of a job, consider it as a potential start and hence push into a max heap to track the highest profit jobs ending at that time.
   - If it's an end of a job, update the `dp` dictionary to accumulate the maximum profit possible up to that point by adding the job profit if it ends there.
   
This approach ensures every job is either taken or bypassed considering if relevant prior jobs can boost the profit up to its start.

```python
def jobScheduling(startTime, endTime, profit):
    # Create a combined list of all job events: (time, start=1/end=0, profit)
    events = []
    for st, et, p in zip(startTime, endTime, profit):
        events.append((st, 1, p))
        events.append((et, 0, p))
        
    # Sort by time, with tie breaking on start then profit
    events.sort()

    # dp keeps track of max profit till current time
    dp = {0: 0}
    max_profit = 0

    for time, is_start, p in events:
        if is_start:
            # Potential new start point, find most profit without clashing
            max_profit = max(dp[time] if time in dp else 0, max_profit)
        else:
            # Job ends, calculate new potential end profit 
            dp[time] = max(dp[time] if time in dp else 0, max_profit + p)

    return max(dp.values())

# Time Complexity: O(N log N), primarily from sorting the events
# Space Complexity: O(N), storage for the events list and the dp dictionary
```

### Dynamic Programming with Binary Search

#### Intuition
We can use a sorted list of jobs and binary search to efficiently determine the last non-conflicting job. This reduces the problem into a simpler one-dimensional dynamic programming problem where each job decision considers only the inclusion of an optimal previous job.

#### Solution
1. Create a list of jobs combining start, end, and profit.
2. Sort the jobs based on their end times to ensure chronological order for binary searching non-overlapping jobs.
3. Use dynamic programming:
   - For each job, consider the profit if we ignore it, and the profit if we include it (added with maximum profit of the last non-conflicting job).
   - Update the DP array with the maximum of these two options.
4. Utilize binary search to efficiently find the last job that doesn't overlap with the current one to optimize the job inclusion profit calculation.

```python
def jobScheduling(startTime, endTime, profit):
    jobs = sorted(zip(startTime, endTime, profit), key=lambda x: x[1])
    n = len(jobs)
    dp = [0] * (n + 1)

    # Binary search to find the last non-conflicting job
    def binary_search(job_index):
        lo, hi = 0, job_index - 1
        while lo <= hi:
            mid = (lo + hi) // 2
            if jobs[mid][1] <= jobs[job_index][0]:
                if jobs[mid + 1][1] <= jobs[job_index][0]:
                    lo = mid + 1
                else:
                    return mid
            else:
                hi = mid - 1
        return -1

    for i in range(1, n + 1):
        include_profit = jobs[i - 1][2]
        last_non_conflict = binary_search(i - 1)
        if last_non_conflict != -1:
            include_profit += dp[last_non_conflict + 1]
        dp[i] = max(dp[i - 1], include_profit)

    return dp[n]

# Time Complexity: O(N log N), for sorting and binary search
# Space Complexity: O(N), for the dp array and job data storage
```

Both approaches utilize dynamic programming principles with distinct methods of handling job scheduling logic - events versus direct job processing with binary search.

