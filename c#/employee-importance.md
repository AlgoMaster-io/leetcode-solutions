# [Leetcode 690: Employee Importance](https://leetcode.com/problems/employee-importance/)

## Approaches
1. [DFS - Recursive Approach](#dfs-recursive-approach)
2. [BFS - Iterative Approach](#bfs-iterative-approach)

### DFS - Recursive Approach

**Intuition:**

The problem can be visualized as a tree structure where each employee node has a value (importance), and links to its subordinates. We can naturally apply a recursive depth-first search approach to traverse this structure. 

The idea is to start from the given employee, add their value to the result, and then recursively visit all their subordinates to accumulate their values.

**Key Steps:**

1. Use a dictionary to store employee id as the key and the employee object as the value, for quick access to any employee.
2. Define a recursive helper function that calculates the importance value for the given employee id.
3. For the given employee id, add the employee's importance to the total importance.
4. Recursively add the importance of each subordinate employee.
5. Return the total importance value calculated.

**C# Code:**

```csharp
using System.Collections.Generic;

// Employee info
public class Employee
{
    public int id;
    public int importance;
    public IList<int> subordinates;
}

public class Solution
{
    public int GetImportance(IList<Employee> employees, int id)
    {
        // Step 1: Build a dictionary for quick access to employees by their id
        Dictionary<int, Employee> employeeDict = new Dictionary<int, Employee>();
        foreach (var emp in employees)
        {
            employeeDict[emp.id] = emp;
        }

        // Step 2: Recursive function to calculate importance
        int totalImportance = DFS(id, employeeDict);
        return totalImportance;
    }

    private int DFS(int id, Dictionary<int, Employee> employeeDict)
    {
        Employee employee = employeeDict[id];
        int totalImportance = employee.importance;

        // Step 3: Add importance of subordinates recursively
        foreach (var subId in employee.subordinates)
        {
            totalImportance += DFS(subId, employeeDict);
        }

        return totalImportance;
    }
}
```

**Complexity Analysis:**

- **Time Complexity:** O(N), where N is the number of employees, as each employee is processed once.
- **Space Complexity:** O(N), due to the recursion stack and the dictionary containing N elements.

---

### BFS - Iterative Approach

**Intuition:**

Instead of using a recursive approach, we can iteratively compute the importance using a queue. This way, we avoid the limitations of recursion (e.g., stack overflow). 

**Key Steps:**

1. Populate a dictionary for quick access to employee information.
2. Initialize a queue with the starting employee's id.
3. Traverse the queue: for each employee id dequeued, add their importance to the result and enqueue all subordinates.
4. Continue until the queue is empty.

**C# Code:**

```csharp
using System.Collections.Generic;

public class Solution
{
    public int GetImportance(IList<Employee> employees, int id)
    {
        // Step 1: Build a dictionary for quick access to employees by their id
        Dictionary<int, Employee> employeeDict = new Dictionary<int, Employee>();
        foreach (var emp in employees)
        {
            employeeDict[emp.id] = emp;
        }

        // Step 2: BFS using queue
        Queue<int> queue = new Queue<int>();
        queue.Enqueue(id);
        int totalImportance = 0;

        // Step 3: Process each employee in the queue
        while (queue.Count > 0)
        {
            int currentId = queue.Dequeue();
            Employee currentEmployee = employeeDict[currentId];
            totalImportance += currentEmployee.importance;

            // Step 4: Enqueue all subordinates to process their importance
            foreach (var subId in currentEmployee.subordinates)
            {
                queue.Enqueue(subId);
            }
        }

        return totalImportance;
    }
}
```

**Complexity Analysis:**

- **Time Complexity:** O(N), as each employee is visited once.
- **Space Complexity:** O(N), mainly due to the queue and the dictionary.

