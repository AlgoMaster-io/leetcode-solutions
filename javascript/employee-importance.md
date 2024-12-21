# [Leetcode 690: Employee Importance](https://leetcode.com/problems/employee-importance/)

## Approaches
- [Approach 1: Depth-First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Breadth-First Search (BFS)](#approach-2-breadth-first-search-bfs)
- [Approach 3: Dictionary (HashMap) with DFS](#approach-3-dictionary-hashmap-with-dfs)

### Approach 1: Depth-First Search (DFS)

**Intuition**

We want to calculate the total importance value of a given employee and all their subordinates. A recursive approach, such as Depth-First Search (DFS), can help traverse through each employee and their subordinates to sum up their importance values. 

**Steps**

1. Identify the employee by id using an object lookup for quick access.
2. Create a recursive DFS function:
   - Base case: If the employee has no subordinates, return their importance value.
   - Recursive case: Sum the employee's importance with the importance values of their subordinates by recursively applying the DFS function.

**Code**

```javascript
/**
 * // Definition for Employee.
 * function Employee(id, importance, subordinates) {
 *     this.id = id;
 *     this.importance = importance;
 *     this.subordinates = subordinates;
 * }
 */

/**
 * @param {Employee[]} employees
 * @param {number} id
 * @return {number}
 */
var GetImportance = function(employees, id) {
    // Create a map for quick lookup by employee id
    const map = new Map();
    for (let employee of employees) {
        map.set(employee.id, employee);
    }
    
    // Define the DFS function
    const dfs = (id) => {
        const employee = map.get(id);
        let totalImportance = employee.importance;
        for (let subId of employee.subordinates) {
            totalImportance += dfs(subId);
        }
        return totalImportance;
    };
    
    return dfs(id);
};
```

**Time Complexity:** O(N), where N is the number of employees. In the worst case, we might have to visit all employees.
**Space Complexity:** O(N), because of the recursion stack and map storage.

### Approach 2: Breadth-First Search (BFS)

**Intuition**

An iterative approach to solve this problem can be done using Breadth-First Search (BFS) with a queue. Similar to the DFS approach, we will explore each employee's subordinates level by level, keeping track of the total importance value.

**Steps**

1. Create a map for quick employee look-up by id.
2. Use a queue to iterate through employees: starting with the employee whose id is given.
3. Dequeue an employee, add their importance to a running total, and enqueue all their subordinates.

**Code**

```javascript
/**
 * @param {Employee[]} employees
 * @param {number} id
 * @return {number}
 */
var GetImportance = function(employees, id) {
    const map = new Map();
    for (let employee of employees) {
        map.set(employee.id, employee);
    }
    
    let totalImportance = 0;
    const queue = [id];
    
    while (queue.length > 0) {
        const currentId = queue.shift();
        const employee = map.get(currentId);
        totalImportance += employee.importance;
        for (let subId of employee.subordinates) {
            queue.push(subId);
        }
    }
    
    return totalImportance;
};
```

**Time Complexity:** O(N), where N is the number of employees.
**Space Complexity:** O(N) for the queue and map storage.

### Approach 3: Dictionary (HashMap) with DFS

**Intuition**

This is a slight optimization over standard DFS. By leveraging a HashMap (Dictionary) for direct access to employees, we streamline the lookup process. This synergizes well with the recursive nature of DFS.

**Steps**

1. Build a HashMap from given employees for O(1) access time.
2. Implement a recursive DFS function to explore and calculate the importance.
3. Sum up the importance of the employee and his subordinates recursively.

**Code**

```javascript
/**
 * @param {Employee[]} employees
 * @param {number} id
 * @return {number}
 */
var GetImportance = function(employees, id) {
    // Initialize a hash map to store employees by ID
    const employeeMap = {};
    for (let employee of employees) {
        employeeMap[employee.id] = employee;
    }
    
    // Recursive function to calculate importance via DFS
    const calculateImportance = (id) => {
        const employee = employeeMap[id];
        let total = employee.importance;
        for (let subordinate of employee.subordinates) {
            total += calculateImportance(subordinate);
        }
        return total;
    };
    
    return calculateImportance(id);
};
```

**Time Complexity:** O(N), since every employee and their subordinates need to be visited.
**Space Complexity:** O(N), due to the recursion stack and map storage.

