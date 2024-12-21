# [Leetcode 502: IPO](https://leetcode.com/problems/ipo/)

## Approaches
- [Approach 1: Greedy with Sorting](#approach-1-greedy-with-sorting)
- [Approach 2: Max-Heap Optimization](#approach-2-max-heap-optimization)

---

## Approach 1: Greedy with Sorting

### Intuition
The problem requires us to maximize our capital after `k` investments. One intuitive strategy is to always choose the most profitable project available given the current capital. We can achieve this by sorting projects based on their required capital and processing them in order, adding the highest profit among available projects to our capital.

### Steps
1. Pair each project with its profit and capital requirements.
2. Sort the projects based on their capital requirement.
3. Iterate through projects, keeping track of available projects based on current capital.
4. Always select the project with the maximum profit that can be completed with the current capital.

### Code
```csharp
public class Solution {
    public int FindMaximizedCapital(int k, int W, int[] Profits, int[] Capital) {
        var projects = new List<(int capital, int profit)>();
        for (int i = 0; i < Profits.Length; i++) {
            projects.Add((Capital[i], Profits[i]));
        }
        
        // Sort projects by the capital required
        projects.Sort((a, b) => a.capital.CompareTo(b.capital));
        
        int index = 0, n = projects.Count;
        var maxProfitProjects = new SortedSet<(int profit, int capital)>(Comparer<(int profit, int capital)>.Create((a, b) => a.profit != b.profit ? b.profit - a.profit : a.capital - b.capital));

        while (k-- > 0) {
            // Add all available projects to the set of max profit projects
            while (index < n && projects[index].capital <= W) {
                maxProfitProjects.Add((projects[index].profit, projects[index].capital));
                index++;
            }
            
            // Select the project with the maximum profit
            if (maxProfitProjects.Count == 0) {
                break;
            }
            
            W += maxProfitProjects.First().profit;
            maxProfitProjects.Remove(maxProfitProjects.First());
        }
        
        return W;
    }
}
```

### Complexity
- **Time Complexity:** O(n log n + k log n), where n is the number of projects. We sort the projects, then use a sorted set to manage max profits.
- **Space Complexity:** O(n), to store the sorted list of projects and the set of selected projects.

---

## Approach 2: Max-Heap Optimization

### Intuition
We can optimize the above method using a max-heap (priority queue) to manage project profits dynamically. This lets us efficiently select the maximum profit project that we can afford with the current capital.

### Steps
1. Pair and sort the projects by the capital required as before.
2. Use a max-heap to track the profits of projects that can be undertaken with the current capital.
3. For each project iteration, insert the project profit into the max-heap if the capital requirement is met.
4. Extract the maximum profit project from the heap and add its profit to the capital.

### Code
```csharp
public class Solution {
    public int FindMaximizedCapital(int k, int W, int[] Profits, int[] Capital) {
        var projects = new List<(int capital, int profit)>();
        for (int i = 0; i < Profits.Length; i++) {
            projects.Add((Capital[i], Profits[i]));
        }
        
        // Sort projects by the capital required
        projects.Sort((a, b) => a.capital.CompareTo(b.capital));
        
        var maxProfitQueue = new PriorityQueue<int, int>();
        int index = 0, n = projects.Count;
        
        while (k-- > 0) {
            // Add all available projects to the max-profit heap
            while (index < n && projects[index].capital <= W) {
                maxProfitQueue.Enqueue(projects[index].profit, -projects[index].profit);
                index++;
            }
            
            // Select the project with the maximum profit
            if (maxProfitQueue.Count == 0) {
                break;
            }
            
            W += maxProfitQueue.Dequeue();
        }
        
        return W;
    }
}
```

### Complexity
- **Time Complexity:** O(n log n + k log n), where n is the number of projects. Sorting takes O(n log n), and managing the heap takes O(k log n).
- **Space Complexity:** O(n), mainly for sorting and heap storage.

These solutions progressively build a strategy to efficiently select projects based on projected return on investment (profit) and available capital, ensuring we maximize the accumulated capital over the allowed number of investments.

