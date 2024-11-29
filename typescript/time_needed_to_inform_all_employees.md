# 1376. [Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

## Approach: Depth-First Search (DFS)

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of employees
// Space Complexity: O(n), for the adjacency list and recursion stack

function numOfMinutes(n: number, headID: number, manager: number[], informTime: number[]): number {
    // Step 1: Build an adjacency list for the employee-manager hierarchy
    const adjList: number[][] = Array.from({ length: n }, () => []);

    for (let i = 0; i < n; i++) {
        if (manager[i] !== -1) {
            adjList[manager[i]].push(i);
        }
    }

    // Step 2: Perform DFS to calculate the total time to inform all employees
    return dfs(headID, adjList, informTime);
}

function dfs(current: number, adjList: number[][], informTime: number[]): number {
    let maxTime = 0;

    // Recur for all subordinates of the current employee
    for (const subordinate of adjList[current]) {
        maxTime = Math.max(maxTime, dfs(subordinate, adjList, informTime));
    }

    // Total time is the current employee's inform time + max time of subordinates
    return informTime[current] + maxTime;
}
```

