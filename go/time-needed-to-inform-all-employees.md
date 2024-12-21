# [Leetcode 1376: Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

## Approaches
1. [DFS Approach](#dfs-approach)
2. [BFS Approach](#bfs-approach)
3. [Optimized DFS Approach with Memoization](#optimized-dfs-approach-with-memoization)

### DFS Approach

**Intuition**

The problem involves calculating the maximum time required to inform all employees in a tree-like hierarchy, where each node represents an employee. This can be solved using Depth First Search (DFS) to explore each path in the employee hierarchy starting from the root node, which is the `headID`, until all nodes have been visited.

**Approach**
- Represent the hierarchy as an adjacency list.
- Perform DFS starting from the `headID`.
- At each node (employee), recursively calculate the time to inform all subordinates.
- The total time for any employee is the time to inform itself plus the maximum time taken by its subordinates.
 
```go
func numOfMinutes(n int, headID int, manager []int, informTime []int) int {
    hierarchy := make(map[int][]int)
    // Build the adjacency list from the manager array
    for emp, mng := range manager {
        if mng != -1 {
            hierarchy[mng] = append(hierarchy[mng], emp)
        }
    }
    
    // DFS function to calculate the total time to inform all employees under employee `emp`
    var dfs func(emp int) int
    dfs = func(emp int) int {
        // If the employee has no subordinates, return 0
        if len(hierarchy[emp]) == 0 {
            return 0
        }
        
        maxTime := 0
        // Explore all subordinates of the current employee
        for _, sub := range hierarchy[emp] {
            // Recursively find the time for each subordinate and take the maximum
            maxTime = max(maxTime, dfs(sub))
        }
        
        return informTime[emp] + maxTime
    }
    
    return dfs(headID)
}

func max(x, y int) int {
    if x > y {
        return x
    }
    return y
}
```

**Time Complexity**: O(n) - We visit each employee node exactly once.  
**Space Complexity**: O(n) - Due to the storage space for the adjacency list and the recursive call stack.

---

### BFS Approach

**Intuition**

A Breadth First Search (BFS) approach can also solve this problem using a queue to iterate through each level of the hierarchy. BFS gives another way to explore each employee level by level, rather than depth.

**Approach**
- Use a queue to perform a level order traversal starting from the `headID`.
- For each employee, enqueue their subordinates with cumulative time taken to inform them.
- Track the maximum time taken for any subordinate.

```go
func numOfMinutesBFS(n int, headID int, manager []int, informTime []int) int {
    hierarchy := make(map[int][]int)
    // Build the adjacency list from the manager array
    for emp, mng := range manager {
        if mng != -1 {
            hierarchy[mng] = append(hierarchy[mng], emp)
        }
    }
    
    type pair struct {
        emp, time int
    }
    
    queue := []pair{{headID, 0}}
    maxTime := 0
    
    // BFS traversal using queue
    for len(queue) > 0 {
        current := queue[0]
        queue = queue[1:]
        
        currentTime := current.time + informTime[current.emp]
        maxTime = max(maxTime, currentTime)
        
        // Enqueue all subordinates of current.employee
        for _, sub := range hierarchy[current.emp] {
            queue = append(queue, pair{sub, currentTime})
        }
    }
    
    return maxTime
}
```

**Time Complexity**: O(n) - Each employee is processed once.  
**Space Complexity**: O(n) - Due to the adjacency list and queue for BFS.

---

### Optimized DFS Approach with Memoization

**Intuition**

To further optimize the DFS approach, introduce memoization to store and reuse the results of redundant computation for subtrees in the hierarchy. This avoids recomputation and saves time on traversing previously computed paths.

**Approach**
- Utilize memoization to store the inform time required for each subtree as the DFS progresses.
- Use this stored time in future computations for nodes that have already been processed.

```go
func numOfMinutesMemo(n int, headID int, manager []int, informTime []int) int {
    hierarchy := make(map[int][]int)
    memo := make(map[int]int)  // Memoization map
    for emp, mng := range manager {
        if mng != -1 {
            hierarchy[mng] = append(hierarchy[mng], emp)
        }
    }
    
    var dfs func(emp int) int
    dfs = func(emp int) int {
        if val, exists := memo[emp]; exists {
            return val
        }
        
        if len(hierarchy[emp]) == 0 {
            return 0
        }
        
        maxTime := 0
        for _, sub := range hierarchy[emp] {
            maxTime = max(maxTime, dfs(sub))
        }
        
        totalTime := informTime[emp] + maxTime
        memo[emp] = totalTime
        return totalTime
    }
    
    return dfs(headID)
}
```

**Time Complexity**: O(n) - Each employee node is computed once and stored for future reference.  
**Space Complexity**: O(n) - Additional space is used for memoization apart from adjacency list and recursive stack.

By understanding and applying these approaches, you can efficiently solve the problem of finding the maximum time required to inform all employees in the hierarchy.

