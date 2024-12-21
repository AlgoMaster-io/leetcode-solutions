# [Minimum Cost to Hire K Workers - LeetCode](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Heap Based Approach](#heap-based-approach)

## Brute Force Approach

### Intuition

The problem of hiring K workers at the minimum cost can be broken down into finding the optimal group of K workers such that the ratio (quality to wage ratio) aligns across all selected workers. This can be achieved by evaluating all possible combinations of K workers, which although infeasible for large inputs, provides a necessary understanding of the problem structure.

### Approach

1. First, calculate the ratio of wage to quality for every worker and sort these ratios.
2. Evaluate all potential combinations of workers, selecting and ensuring that an optimal ratio is met for any chosen group of K workers. The aim is to minimize the total wage while ensuring no worker in the group is underpaid based on their specified ratio.
3. This method is computationally expensive because it examines a large number of combinations, but serves as a direct application of understanding the constraint problem.

### Java Code

```java
import java.util.Arrays;

public class Solution {
    public double mincostToHireWorkers(int[] quality, int[] wage, int K) {
        int n = quality.length;
        Pair[] workers = new Pair[n];
        for (int i = 0; i < n; i++) {
            workers[i] = new Pair((double) wage[i] / quality[i], quality[i]);
        }

        // Sort workers by their wage/quality ratio
        Arrays.sort(workers, (a, b) -> Double.compare(a.ratio, b.ratio));

        double minCost = Double.MAX_VALUE;

        // Brute force K workers based on fixed ratio selection
        for (int i = 0; i <= n - K; i++) {
            double sumQuality = 0;
            int count = 0;
            for (int j = i; j < n; j++) {
                sumQuality += workers[j].quality;
                count++;

                if (count == K) {
                    // Calculate cost for this set of K workers
                    double cost = sumQuality * workers[i].ratio;
                    minCost = Math.min(minCost, cost);
                    break;
                }
            }
        }
        
        return minCost;
    }

    private static class Pair {
        double ratio;
        int quality;

        Pair(double ratio, int quality) {
            this.ratio = ratio;
            this.quality = quality;
        }
    }
}
```

### Complexity Analysis

- **Time Complexity**: \( O(n^2 \log n) \) due to sorting and brute force combinations.
- **Space Complexity**: \( O(n) \) for storing the ratio-quality pairs.

## Heap Based Approach

### Intuition

To hire K workers while minimizing the cost, consider the smallest ratio of wage-to-quality and always select the lowest quality workers to achieve optimal cost. By applying a max-heap on quality, we can evaluate the minimal ratios while keeping control over the maximum quality sum to pay the minimal cost.

### Approach

1. Calculate and sort workers based on their wage-to-quality ratio. 
2. Use a max-heap to efficiently manage the top K lowest quality workers.
3. For each worker, if the heap size exceeds K, remove the largest quality element, then calculate the minimal possible cost using fixed ratio across these K workers.

### Java Code

```java
import java.util.Arrays;
import java.util.PriorityQueue;

public class Solution {
    public double mincostToHireWorkers(int[] quality, int[] wage, int K) {
        int n = quality.length;
        Pair[] workers = new Pair[n];
        for (int i = 0; i < n; i++) {
            workers[i] = new Pair((double) wage[i] / quality[i], quality[i]);
        }

        // Sort workers by wage/quality ratio
        Arrays.sort(workers, (a, b) -> Double.compare(a.ratio, b.ratio));

        double minCost = Double.MAX_VALUE;
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
        double sumQuality = 0;

        for (Pair worker : workers) {
            maxHeap.add(worker.quality);
            sumQuality += worker.quality;

            // Maintain heap size of K
            if (maxHeap.size() > K) {
                sumQuality -= maxHeap.poll();
            }
            
            if (maxHeap.size() == K) {
                minCost = Math.min(minCost, sumQuality * worker.ratio);
            }
        }

        return minCost;
    }

    private static class Pair {
        double ratio;
        int quality;

        Pair(double ratio, int quality) {
            this.ratio = ratio;
            this.quality = quality;
        }
    }
}
```

### Complexity Analysis

- **Time Complexity**: \( O(n \log n) \) due to sort and heap operations.
- **Space Complexity**: \( O(K) \) for managing K elements in the max-heap.

