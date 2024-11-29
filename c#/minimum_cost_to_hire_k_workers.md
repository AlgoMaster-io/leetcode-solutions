# 857. [Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approach: Sorting and Priority Queue (Max-Heap)

### Solution
csharp
```csharp
// Time Complexity: O(n * log(n) + n * log(k)), where n is the number of workers and k is the number of workers to hire
// Space Complexity: O(k), for the max-heap
using System;
using System.Collections.Generic;

public class Solution
{
    public double MincostToHireWorkers(int[] quality, int[] wage, int k)
    {
        int n = quality.Length;
        double[][] workers = new double[n][];

        // Create an array of workers with their wage-to-quality ratio and quality
        for (int i = 0; i < n; i++)
        {
            workers[i] = new double[2];
            workers[i][0] = (double)wage[i] / quality[i]; // Wage-to-quality ratio
            workers[i][1] = quality[i];
        }

        // Sort workers by their wage-to-quality ratio
        Array.Sort(workers, (a, b) => a[0].CompareTo(b[0]));

        PriorityQueue<int, int> maxHeap = new PriorityQueue<int, int>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
        int totalQuality = 0; // Sum of qualities in the current group
        double minCost = double.MaxValue;

        foreach (var worker in workers)
        {
            int currentQuality = (int)worker[1];
            totalQuality += currentQuality;
            maxHeap.Enqueue(currentQuality, currentQuality);

            // If we exceed k workers, remove the one with the highest quality
            if (maxHeap.Count > k)
            {
                totalQuality -= maxHeap.Dequeue();
            }

            // If we have exactly k workers, calculate the cost
            if (maxHeap.Count == k)
            {
                minCost = Math.Min(minCost, totalQuality * worker[0]);
            }
        }

        return minCost;
    }
}
```

