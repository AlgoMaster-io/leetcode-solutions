# [Leetcode 502: IPO](https://leetcode.com/problems/ipo/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Greedy Approach using Max Heap](#greedy-approach-using-max-heap)

---

### Brute Force Approach

**Intuition:**

The brute force method involves examining all possible combinations of projects and selecting those which maximize the profit within the given constraint of initial capital. This approach is straightforward but computationally expensive, especially when the number of projects is large or the constraints are tight.

**Detailed Steps:**

1. Iterate through all the projects.
2. For each project, check if it can be started with the available capital.
3. Choose the project giving the maximum profit that can be accommodated by your current capital.
4. Update the capital with the profit obtained and repeat the process for `k` selections.

**Code:**

```java
class Solution {
    public int findMaximizedCapital(int k, int W, int[] Profits, int[] Capital) {
        int n = Profits.length;
        for (int i = 0; i < k; i++) {
            int index = -1;
            // Find the project with the largest profit within the capital constraints
            for (int j = 0; j < n; j++) {
                if (Capital[j] <= W) { // Check if we can start this project
                    if (index == -1 || Profits[j] > Profits[index]) {
                        index = j; // Update index to the project with max profit
                    }
                }
            }
            if (index == -1) {
                break; // No project can be initiated, break the loop
            }
            W += Profits[index]; // Update available capital after choosing project
            Capital[index] = Integer.MAX_VALUE; // Mark this project as done
        }
        return W;
    }
}
```

**Complexity Analysis:**

- Time Complexity: \(O(kn)\), where \(n\) is the number of projects and \(k\) is the number of possible selections.
- Space Complexity: \(O(1)\). Only a few extra variables are used.

---

### Greedy Approach using Max Heap

**Intuition:**

To optimize the profit selection, we use a max heap to always extract the project that offers the maximum profit which can be started given the current capital. We keep track of projects whose capital requirement is within our current capital using a min-heap.

**Detailed Steps:**

1. Pair up profits and capital into a list of jobs.
2. Sort this list based on the capital required.
3. Use a max-heap to manage the projects that can be started with the available capital.
4. For `k` selections:
   - Push all projects into the max-heap that can be started with the current capital.
   - If the max-heap is empty, break out as no further projects can be started.
   - Extract project with the maximum profit, and update the capital with the profit obtained.
   
**Code:**

```java
import java.util.*;

class Solution {
    public int findMaximizedCapital(int k, int W, int[] Profits, int[] Capital) {
        int n = Profits.length;
        Project[] projects = new Project[n];
        
        // Pair each project with its capital and profit
        for (int i = 0; i < n; i++) {
            projects[i] = new Project(Capital[i], Profits[i]);
        }

        // Sort by capital required
        Arrays.sort(projects, Comparator.comparingInt(a -> a.capital));

        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a); // Max-Heap for profits
        int index = 0;

        for (int i = 0; i < k; i++) {
            // Push all projects into maxHeap that can be started with available capital
            while (index < n && projects[index].capital <= W) {
                maxHeap.offer(projects[index].profit);
                index++;
            }
            
            if (maxHeap.isEmpty()) {
                break; // If no projects can be started
            }
            
            W += maxHeap.poll(); // Start the project with maximum profit
        }
        
        return W;
    }
    
    // Helper class to store project data
    static class Project {
        int capital;
        int profit;
        
        Project(int capital, int profit) {
            this.capital = capital;
            this.profit = profit;
        }
    }
}
```

**Complexity Analysis:**

- Time Complexity: \(O(n \log n + k \log n)\). Sorting the projects takes \(O(n \log n)\), and managing the max heap for up to \(k\) operations takes \(O(k \log n)\).
- Space Complexity: \(O(n)\) due to storing projects and the space used by the heap.

