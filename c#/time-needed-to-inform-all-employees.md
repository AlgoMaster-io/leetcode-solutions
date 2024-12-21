## [Leetcode 1376: Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

### Approaches:
- [DFS Approach](#dfs-approach)
- [BFS Approach](#bfs-approach)

### DFS Approach

#### Intuition:
We can solve the problem using a depth-first search (DFS) approach. The idea is to treat the tree of employees as a graph and recursively calculate the total time needed to inform all subordinates by traversing the hierarchy from the head of the company down to the leaves (employees with no subordinates).

In this approach, whenever we are at a particular employee, we'll make recursive calls to calculate the total time needed to inform all its subordinates and then add the time it takes for the current employee to inform their direct reports.

#### Implementation:
```csharp
public class Solution {
    public int NumOfMinutes(int n, int headID, int[] manager, int[] informTime) {
        // Create a list of lists to represent the hierarchal tree
        var tree = new List<int>[n];
        
        // Initialize each list in the array
        for (int i = 0; i < n; i++) {
            tree[i] = new List<int>();
        }
        
        // Populate the tree with manager-subordinate relationships
        for (int i = 0; i < n; i++) {
            if (manager[i] != -1) {
                tree[manager[i]].Add(i);
            }
        }
        
        // Recursive DFS function to calculate the time needed
        int DFS(int node) {
            int maxTime = 0;

            // Traverse each subordinate
            foreach (var subordinate in tree[node]) {
                // Calculate the time to inform each subordinate
                maxTime = Math.Max(maxTime, DFS(subordinate));
            }
            
            // Add the time it takes for this node to inform its subordinates
            return maxTime + informTime[node];
        }
        
        // Start DFS from the head ID
        return DFS(headID);
    }
}
```

#### Complexity:
- **Time Complexity:** O(n), where n is the number of employees. Each employee is processed once.
- **Space Complexity:** O(n), for the recursion stack in the worst case and the adjacency list representation of the employee hierarchy.

### BFS Approach

#### Intuition:
Another way to approach the problem is using a breadth-first search (BFS). This way, we'll process nodes level by level starting from the head of the organization. We'll use a queue to manage the processing order and keep track of the accumulated time at each level.

This method systematically calculates the time required as we inform employees in waves, starting from the head and moving downwards, adding the corresponding informTime for each manager as we traverse.

#### Implementation:
```csharp
using System.Collections.Generic;

public class Solution {
    public int NumOfMinutes(int n, int headID, int[] manager, int[] informTime) {
        // Create the tree representation
        var tree = new List<int>[n];
        for (int i = 0; i < n; i++) {
            tree[i] = new List<int>();
        }
        for (int i = 0; i < n; i++) {
            if (manager[i] != -1) {
                tree[manager[i]].Add(i);
            }
        }
        
        // Queue for BFS
        Queue<(int, int)> queue = new Queue<(int, int)>();
        queue.Enqueue((headID, 0));
        
        int maxTime = 0;
        
        while (queue.Count > 0) {
            var (current, currentTime) = queue.Dequeue();
            
            // Update the maxTime needed to inform all employees
            maxTime = Math.Max(maxTime, currentTime);
            
            // Traverse all subordinates
            foreach (var subordinate in tree[current]) {
                queue.Enqueue((subordinate, currentTime + informTime[current]));
            }
        }
        
        return maxTime;
    }
}
```

#### Complexity:
- **Time Complexity:** O(n), because we process each employee once.
- **Space Complexity:** O(n), due to the space used by the queue and the tree representation.

By exploring both DFS and BFS approaches, you can select one based on your preference and the specific constraints of the problem you are dealing with. Both methods should yield the correct answer for this problem.

