# [Leetcode 857: Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Table of Contents
1. [Approach 1: Brute Force](#approach-1-brute-force)
2. [Approach 2: Minimum Heap](#approach-2-minimum-heap)

---

## Approach 1: Brute Force

### Intuition:
The core idea is to find a permutation of K workers such that the cost of hiring these workers is minimized. We can generate all possible combinations of selecting K workers from N available workers, calculate the total cost for each combination and then select the minimum cost. This approach is not efficient because it involves generating all combinations and is computationally expensive.

### Detailed Comments in Code:
- Enumerate all possible combinations of K workers from the list.
- For each combination, calculate the cost using the maximum quality in this combination as the quality standard.
- Calculate the total cost for this specific combination.
- Track the minimum cost encountered during the combinations.

### Time and Space Complexity:
- **Time Complexity**: O(C(N, K) * K), where C(N, K) is the combination function which is N! / (K! * (N-K)!).
- **Space Complexity**: O(N), required for storing all workers permutations.

```csharp
// Brute force solution code goes here, but often impractical for large inputs due to time complexity constraints.
// Not recommended to implement due to excessive computational requirements in a practical scenario.
```

---

## Approach 2: Minimum Heap

### Intuition:
To hire K workers at the minimum cost, we need to ensure that the ratio between the wage expectation and quality for the selected workers remains consistent and minimizes cost. We can use a greedy strategy combined with a priority queue (min-heap) to achieve this. We'll iterate through workers sorted by their ratio of wage to quality, maintaining a heap of workers with the lowest total quality.

### Detailed Comments in Code:
1. **Ratio Sort**: Sort workers by their `wage/quality` ratio.
2. **Heap Maintenance**: Use a max heap to keep track of K workers with the lowest quality.
3. **Cost Calculation**:
   - As you process each worker, add their quality to the heap. If the heap exceeds size K, remove the worker with the largest quality since they contribute most to the cost.
   - Compute the cost using the current ratio and total quality of workers in the heap.
   - Update the minimum cost if the current combination yields a lower cost.

### Time and Space Complexity:
- **Time Complexity**: O(N log N), primarily due to sorting the workers and maintaining the heap operations.
- **Space Complexity**: O(K), where K is the size for the heap storing the workers' qualities.

```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public double MincostToHireWorkers(int[] quality, int[] wage, int K) {
        int n = quality.Length;
        double[][] workers = new double[n][];
        
        // Compute the ratio and store quality, wage ratios
        for (int i = 0; i < n; i++) {
            workers[i] = new double[] { (double)wage[i] / quality[i], (double)quality[i] };
        }
        
        // Sort workers by their ratio of wage/quality
        Array.Sort(workers, (a, b) => a[0].CompareTo(b[0]));
        
        double totalQuality = 0.0, minCost = double.MaxValue;
        PriorityQueue<int, int> maxHeap = new PriorityQueue<int, int>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
        
        // Iterate through each worker
        for (int i = 0; i < n; i++) {
            // Add current quality to total and to heap
            totalQuality += workers[i][1];
            maxHeap.Enqueue((int)workers[i][1], (int)workers[i][1]);
            
            // Ensure the heap size doesn't exceed K
            if (maxHeap.Count > K) {
                totalQuality -= maxHeap.Dequeue(); // Remove max quality from the sum
            }
            
            // Calculate minimum cost when the heap size is exactly K
            if (maxHeap.Count == K) {
                minCost = Math.Min(minCost, totalQuality * workers[i][0]);
            }
        }
        
        return minCost;
    }
}
```

Here, the C# code uses a priority queue to manage the largest qualities and calculates the minimum cost when exactly K workers are considered. The core idea is maintaining a consistent quality-to-wage ratio, sorting the workers, and dynamically assessing worker combinations for cost efficiency.

