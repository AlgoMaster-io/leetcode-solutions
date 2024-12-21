# [Leetcode 1376: Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

## Approaches:
- [Approach 1: DFS (Depth-First Search) Brute Force](#approach-1)
- [Approach 2: BFS (Breadth-First Search) Optimal](#approach-2)

---

### Approach 1: DFS (Depth-First Search) Brute Force

**Intuition:**
The problem can be visualized as a tree where each employee is a node, and the manager-employee relationship is represented as edges. The crux of this problem is to travel from the head of the organization (the root of the tree) to each employee (leaf nodes) and calculate the maximum time taken in any path to inform all employees. By utilizing a DFS approach, we explore every path from the head to the leaves, tracking the time at each step.

**Steps:**
1. Construct a tree using adjacency list where we map managers to their direct reports.
2. Perform a DFS traversal starting from the head.
3. At each node, calculate the time taken to inform the entire subtree rooted at that node.
4. Keep track of the maximum time across all leaf nodes.

```javascript
var numOfMinutes = function(n, headID, manager, informTime) {
    // Create adjacency list for ease of traversal
    const employees = new Array(n).fill(null).map(() => []);
    
    // Populate the adjacency list with manager -> subordinates mapping
    for (let i = 0; i < manager.length; i++) {
        if (manager[i] !== -1) {
            employees[manager[i]].push(i);
        }
    }
    
    // Recursive DFS helper function
    const dfs = (node) => {
        // Base case: If no subordinates, no additional time needed
        if (employees[node].length === 0) {
            return 0;
        }
        
        let maxTime = 0;
        
        // Traverse each subordinate
        for (let subordinate of employees[node]) {
            // Calculate the time taken for each subtree and update the maxTime
            maxTime = Math.max(maxTime, dfs(subordinate) + informTime[node]);
        }
        
        return maxTime;
    };
    
    // Start DFS from headID
    return dfs(headID);
};
```

**Time Complexity:** O(N), where N is the number of employees. Each employee is visited once.

**Space Complexity:** O(N), for storing the adjacency list and the recursive stack.

---

### Approach 2: BFS (Breadth-First Search) Optimal

**Intuition:**
While DFS explores paths deeply, BFS can provide a more intuitive level-order traversal approach to track the optimal path to inform all employees in parallel. Instead of exploring to leaves first, a queue can be used to process each level of employees, updating the total time dynamically at each level.

**Steps:**
1. Build the graph structure using an adjacency list from the manager-to-employees relationship.
2. Initialize a queue with the head of the organization and its inform time.
3. Perform BFS, for each manager, enqueue their subordinates with the updated time.
4. Keep a record of the total time using a variable and update it each time a new level is processed.

```javascript
var numOfMinutes = function(n, headID, manager, informTime) {
    const employees = new Array(n).fill(null).map(() => []);
    for (let i = 0; i < manager.length; i++) {
        if (manager[i] !== -1) {
            employees[manager[i]].push(i);
        }
    }
    
    let queue = [[headID, 0]];
    let maxTime = 0;
    
    // BFS traversal
    while (queue.length > 0) {
        const [currentManager, currentTime] = queue.shift();
        
        // Update max time using the current time after informing this level
        maxTime = Math.max(maxTime, currentTime);
        
        for (let subordinate of employees[currentManager]) {
            queue.push([subordinate, currentTime + informTime[currentManager]]);
        }
    }
    
    return maxTime;
};
```

**Time Complexity:** O(N), since each employee is processed exactly once.

**Space Complexity:** O(N), primarily for the storage of the hierarchy in the adjacency list and BFS queue.

--- 

In both approaches, the goal is to ensure that every employee is informed, and at the end, you capture the time it takes for the last employee to receive the information.

