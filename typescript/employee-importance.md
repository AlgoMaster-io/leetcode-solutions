## [Leetcode 690: Employee Importance](https://leetcode.com/problems/employee-importance/)

### Approaches:
1. [Depth-First Search (DFS) Approach](#dfs-approach)
2. [Breadth-First Search (BFS) Approach](#bfs-approach)

### DFS Approach

#### Intuition:
To find the total importance value of an employee and their subordinates, we can apply a recursive Depth-First Search (DFS). Each call of the recursion will process an employee, adding their importance to a cumulative total and recursively invoking for each of their direct reports. This way, we will traverse all subordinates and calculate the total importance.

#### Implementation:

```typescript
class Employee {
    id: number;
    importance: number;
    subordinates: number[];
    constructor(id: number, importance: number, subordinates: number[]) {
        this.id = id;
        this.importance = importance;
        this.subordinates = subordinates;
    }
}

function getImportance(employees: Employee[], id: number): number {
    // Create a map for quick access to employees by their id
    const employeeMap = new Map<number, Employee>();
    
    // Populate the map with employee id as key and Employee object as value
    for (const employee of employees) {
        employeeMap.set(employee.id, employee);
    }

    // Helper function to perform DFS
    function dfs(empId: number): number {
        const employee = employeeMap.get(empId);
        if (!employee) return 0;
        
        let totalImportance = employee.importance;
        
        // Recursively add the importance of all subordinates
        for (const subordinate of employee.subordinates) {
            totalImportance += dfs(subordinate);
        }
        
        return totalImportance;
    }

    return dfs(id);
}
```

#### Time Complexity:
- **O(N)** where N is the number of employees. We visit each employee and their subordinates once.

#### Space Complexity:
- **O(N)** due to the map storing the employees and the call stack of the recursive function.

---

### BFS Approach

#### Intuition:
Another iterative solution is possible using Breadth-First Search (BFS). Instead of recursion, maintain a queue to traverse the employee hierarchy level by level. Start with the given employee and continue to load and process each subordinate into the queue, accumulating their importance as you proceed.

#### Implementation:

```typescript
function getImportanceBFS(employees: Employee[], id: number): number {
    const employeeMap = new Map<number, Employee>();

    for (const employee of employees) {
        employeeMap.set(employee.id, employee);
    }

    let totalImportance = 0;
    const queue = [id];

    while (queue.length > 0) {
        const currentId = queue.shift()!;
        const employee = employeeMap.get(currentId);

        if (employee) {
            totalImportance += employee.importance;

            // Load all subordinates into the queue for processing
            for (const subordinate of employee.subordinates) {
                queue.push(subordinate);
            }
        }
    }

    return totalImportance;
}
```

#### Time Complexity:
- **O(N)** where N is the number of employees. Each employee and their subordinates are processed once.

#### Space Complexity:
- **O(N)** due to the map used for employee lookup and the queue for managing BFS traversal.

---

By applying these strategies, the problem of computing employee importance can be solved either recursively or iteratively, each having the same time and space complexity but different execution paradigms.

