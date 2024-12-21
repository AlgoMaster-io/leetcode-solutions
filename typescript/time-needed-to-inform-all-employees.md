## [Leetcode 1376: Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

### Approaches:
1. [DFS (Depth-First Search) Approach](#dfs-approach)
2. [BFS (Breadth-First Search) Approach](#bfs-approach)

### DFS Approach

**Intuition:**

The problem can be viewed as a tree with the head of the company being the root. Each employee might have subordinates, forming a hierarchy tree. We aim to calculate the maximum time required to inform all employees, which translates to finding the "deepest" leaf if each edge has weight equal to `informTime` of the parent node.

We can use Depth-First Search (DFS) to traverse this tree from the root to all leaf nodes (employees who have no subordinates) and calculate the maximum time taken along any path.

1. Start DFS from the head node.
2. For each subordinate of the current node, calculate the informed time recursively.
3. Use the maximum informed time needed among all subordinates.
4. Return the maximum time plus the current node's `informTime`.

**Code:**

```typescript
function numOfMinutes(n: number, headID: number, manager: number[], informTime: number[]): number {
    // Step 1: Create adjacency list to represent the tree structure of employees
    const subordinates: Map<number, number[]> = new Map();

    for (let i = 0; i < n; i++) {
        if (manager[i] !== -1) {
            if (!subordinates.has(manager[i])) {
                subordinates.set(manager[i], []);
            }
            subordinates.get(manager[i])!.push(i);
        }
    }

    // Step 2: Define a recursive function for DFS
    function dfs(employeeID: number): number {
        // Base case: if the employee has no subordinates
        if (!subordinates.has(employeeID)) {
            return 0;
        }

        let maxTime = 0;
        for (const subID of subordinates.get(employeeID)!) {
            // Recursive call to get the time needed for each subordinate
            maxTime = Math.max(maxTime, dfs(subID));
        }

        // Return the maximum time from subordinates plus current employee's informTime
        return maxTime + informTime[employeeID];
    }

    // Step 3: Start DFS from headId
    return dfs(headID);
}
```

**Time Complexity:** O(n) - where n is the number of employees. Each employee is visited once.

**Space Complexity:** O(n) - Using a map to store subordinates and the space used in the recursive stack.

---

### BFS Approach

**Intuition:**

The BFS approach is suitable as it traverses the tree level-by-level. Using a queue, we can simulate diffusion of the information where each level of BFS equates to one "round" of informing the subordinates.

1. Initialize a queue starting with the head of the company.
2. For each node in the queue, push its subordinates to the queue.
3. Accumulate the time by using an additional property to track the time taken to reach each node.
4. Keep track of the maximum accumulated time needed among all BFS iterations.

**Code:**

```typescript
function numOfMinutes(n: number, headID: number, manager: number[], informTime: number[]): number {
    const subordinates: Map<number, number[]> = new Map();
    for (let i = 0; i < n; i++) {
        if (manager[i] !== -1) {
            if (!subordinates.has(manager[i])) {
                subordinates.set(manager[i], []);
            }
            subordinates.get(manager[i])!.push(i);
        }
    }

    const queue: [number, number][] = []; // [employeeID, accumulatedTime]
    queue.push([headID, 0]);

    let maxTime = 0;

    while (queue.length > 0) {
        const [currentID, currentTime] = queue.shift()!;
        maxTime = Math.max(maxTime, currentTime);
        
        if (subordinates.has(currentID)) {
            for (const subID of subordinates.get(currentID)!) {
                // Push subordinate with the cumulative inform time
                queue.push([subID, currentTime + informTime[currentID]]);
            }
        }
    }

    return maxTime;
}
```

**Time Complexity:** O(n) - Each employee is processed once.

**Space Complexity:** O(n) - Using a map for storing subordinates and a queue for BFS traversal.

