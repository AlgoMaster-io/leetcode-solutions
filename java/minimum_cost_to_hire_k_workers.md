# 857. [Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approach: Sorting and Priority Queue (Max-Heap)

### Solution
```java
// Time Complexity: O(n * log(n) + n * log(k)), where n is the number of workers and k is the number of workers to hire
// Space Complexity: O(k), for the max-heap
import java.util.Arrays;
import java.util.PriorityQueue;

public class Solution {
    public double mincostToHireWorkers(int[] quality, int[] wage, int k) {
        int n = quality.length;
        double[][] workers = new double[n][2];

        // Create an array of workers with their wage-to-quality ratio and quality
        for (int i = 0; i < n; i++) {
            workers[i][0] = (double) wage[i] / quality[i]; // Wage-to-quality ratio
            workers[i][1] = quality[i];
        }

        // Sort workers by their wage-to-quality ratio
        Arrays.sort(workers, (a, b) -> Double.compare(a[0], b[0]));

        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> Integer.compare(b, a)); // Max-heap for quality
        int totalQuality = 0; // Sum of qualities in the current group
        double minCost = Double.MAX_VALUE;

        for (double[] worker : workers) {
            int currentQuality = (int) worker[1];
            totalQuality += currentQuality;
            maxHeap.add(currentQuality);

            // If we exceed k workers, remove the one with the highest quality
            if (maxHeap.size() > k) {
                totalQuality -= maxHeap.poll();
            }

            // If we have exactly k workers, calculate the cost
            if (maxHeap.size() == k) {
                minCost = Math.min(minCost, totalQuality * worker[0]);
            }
        }

        return minCost;
    }
}
```