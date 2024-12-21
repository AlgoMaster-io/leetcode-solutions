# [Employee Importance](https://leetcode.com/problems/employee-importance/)

## Approaches
1. [Depth-First Search (DFS) Approach](#dfs-approach)
2. [Breadth-First Search (BFS) Approach](#bfs-approach)

---

## DFS Approach

### Intuition
To solve the problem with DFS, we recursively calculate the total importance of each employee by summing their own importance and the importance of all their subordinates. We can do this by first creating a map to easily access employees by their ID. Then, we recursively visit each employee and sum the importance.

### Steps
1. Create a map `employeeMap` to map each employee's ID to their `Employee` object.
2. Implement a recursive `dfs` function that:
   - Returns 0 if the employee ID is not found.
   - Accumulates the total importance by adding the current employee's importance and recursively calling itself on all subordinates.
3. Call the `dfs` function with the given `id` and return its value.

### Time Complexity
- **O(N)**: We must visit every employee and their subordinates exactly once.

### Space Complexity
- **O(N)**: Space required for the recursive call stack and the hash map.

```go
type Employee struct {
    Id int
    Importance int
    Subordinates []int
}

func GetImportance(employees []*Employee, id int) int {
    employeeMap := make(map[int]*Employee)
    
    for _, emp := range employees {
        employeeMap[emp.Id] = emp
    }

    var dfs func(id int) int
    dfs = func(id int) int {
        emp, exists := employeeMap[id]
        if !exists {
            return 0
        }
        
        totalImportance := emp.Importance
        // Add the importance of subordinates
        for _, subID := range emp.Subordinates {
            totalImportance += dfs(subID)
        }
        
        return totalImportance
    }

    return dfs(id)
}
```

---

## BFS Approach

### Intuition
The BFS approach uses a queue to process each employee while adding their importance to the total and enqueueing their subordinates. BFS ensures that we process the employees level by level.

### Steps
1. Create a map `employeeMap` similar to the DFS approach.
2. Initialize a queue and enqueue the employee with the given `id`.
3. While the queue is not empty, dequeue an employee, add their importance to a total, and enqueue all of their subordinates.
4. Return the total importance once the queue is empty.

### Time Complexity
- **O(N)**: Similar to DFS, where each employee and their subordinates are visited once.

### Space Complexity
- **O(N)**: Space used by the queue in the worst case if all nodes are at the same level.

```go
type Employee struct {
    Id int
    Importance int
    Subordinates []int
}

func GetImportance(employees []*Employee, id int) int {
    employeeMap := make(map[int]*Employee)
    
    for _, emp := range employees {
        employeeMap[emp.Id] = emp
    }
    
    totalImportance := 0
    queue := []int{id}
    
    for len(queue) > 0 {
        // Dequeue the first element
        currentID := queue[0]
        queue = queue[1:]
        
        emp, exists := employeeMap[currentID]
        if !exists {
            continue
        }
        
        // Add current employee importance to total
        totalImportance += emp.Importance
        
        // Enqueue all subordinates
        for _, subID := range emp.Subordinates {
            queue = append(queue, subID)
        }
    }
    
    return totalImportance
}
```


