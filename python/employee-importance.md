# [690. Employee Importance](https://leetcode.com/problems/employee-importance/)

## Approaches:
1. [Graph Traversal Using BFS](#approach-1-graph-traversal-using-bfs)
2. [Graph Traversal Using DFS](#approach-2-graph-traversal-using-dfs)

---

## Approach 1: Graph Traversal Using BFS

### Intuition:
The problem can be visualized as a directed graph where each employee is a node, and each employee's list of subordinates represents directed edges to other nodes (employees). Our task is to determine the total "importance" by summing the importance values of the employee and their subordinates. We can achieve this using a Breadth-First Search (BFS), which is suitable for exploring level-wise structures like this one.

### Implementation:
```python
# Definition for Employee.
class Employee:
    def __init__(self, id: int, importance: int, subordinates: List[int]):
        self.id = id
        self.importance = importance
        self.subordinates = subordinates

class Solution:
    def getImportance(self, employees: List[Employee], id: int) -> int:
        # Create a mapping of employee ID to the Employee object
        employee_map = {employee.id: employee for employee in employees}
        
        total_importance = 0
        queue = [id]  # Start BFS with the employee whose importance we need
        
        while queue:
            current_id = queue.pop(0)  # Dequeue the first element
            employee = employee_map[current_id]
            
            # Accumulate the importance
            total_importance += employee.importance
            
            # Enqueue all the subordinates
            for sub_id in employee.subordinates:
                queue.append(sub_id)
        
        return total_importance

```
### Complexities:
- **Time Complexity**: \(O(N)\) where \(N\) is the number of employees, since each employee is processed once.
- **Space Complexity**: \(O(N)\) for the employee mapping and the queue.

---

## Approach 2: Graph Traversal Using DFS

### Intuition:
As an alternative to BFS, we can use Depth-First Search (DFS) to calculate the total importance. In DFS, we traverse as deep as possible along each branch before backtracking, which can often be implemented more efficiently using recursion in problems like this.

### Implementation:
```python
# Definition for Employee.
class Employee:
    def __init__(self, id: int, importance: int, subordinates: List[int]):
        self.id = id
        self.importance = importance
        self.subordinates = subordinates

class Solution:
    def getImportance(self, employees: List[Employee], id: int) -> int:
        # Create a dictionary to map each employee's id to their Employee object
        employee_map = {employee.id: employee for employee in employees}
        
        # Define the DFS function
        def dfs(employee_id):
            employee = employee_map[employee_id]
            # Start with current employee's importance
            total = employee.importance
            # Recursively add the importance of subordinates
            for sub_id in employee.subordinates:
                total += dfs(sub_id)
            return total
        
        # Start DFS from the given employee id
        return dfs(id)
```
### Complexities:
- **Time Complexity**: \(O(N)\), where \(N\) is the number of employees, since each employee and their subordinates are considered once.
- **Space Complexity**: \(O(N)\) for the employee mapping and the recursion stack in the worst case scenario.

These two traversal methods effectively allow us to calculate the total importance recursively or iteratively depending on one's preference or additional constraints such as maximum call stack size in practical scenarios.

