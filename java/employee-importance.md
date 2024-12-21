# [Leetcode 690: Employee Importance](https://leetcode.com/problems/employee-importance/)

## Approaches
1. [Depth-First Search (DFS) Recursion](#depth-first-search-dfs-recursion)
2. [Breadth-First Search (BFS)](#breadth-first-search-bfs)

---

### Depth-First Search (DFS) Recursion

**Intuition:**

The main idea is to recursively calculate the total importance by summing up the importance of the given employee and recursively adding the importance of all subordinates. We will use a hashmap to quickly access an employee using their id.

**Approach:**

1. Use a hashmap to store the employee id and the related `Employee` object for quick retrieval.
2. Write a recursive helper function `dfs` that takes the id of the employee whose total importance is to be calculated.
3. The `dfs` function will retrieve the employee using the hashmap, start the sum with the employee's importance, and recursively add the importance of all subordinates by calling `dfs` for each subordinate id.
4. Call the `dfs` function starting with the given employee id.

**Code:**

```java
// Employee class definition
class Employee {
    public int id;
    public int importance;
    public List<Integer> subordinates;
}

public class Solution {
    public int getImportance(List<Employee> employees, int id) {
        // Create a map to store each employee by their id
        Map<Integer, Employee> map = new HashMap<>();
        
        // Fill the map with the employee data
        for (Employee emp : employees) {
            map.put(emp.id, emp);
        }
        
        // Call the DFS helper function and return the total importance
        return dfs(id, map);
    }

    // DFS function to compute the total importance starting from a particular employee id
    private int dfs(int id, Map<Integer, Employee> map) {
        // Get the current employee using the id
        Employee employee = map.get(id);
        
        // Start with the importance of the current employee
        int totalImportance = employee.importance;
        
        // Recursively get the importance of each subordinate
        for (int subid : employee.subordinates) {
            totalImportance += dfs(subid, map);
        }
        
        // Return the total computed importance
        return totalImportance;
    }
}
```

**Time Complexity:** `O(N)`, where `N` is the number of employees. We visit each employee once.

**Space Complexity:** `O(N)`, due to the recursion stack in the worst case and the space used by the hashmap.

---

### Breadth-First Search (BFS)

**Intuition:**

Instead of using recursion, we can implement an iterative version using a queue, which will help us traverse through the employees in a level-order approach (breadth-first search).

**Approach:**

1. Similar to DFS, use a hashmap to store employees by their id for quick access.
2. Use a queue to help in the BFS approach, starting by adding the main employee id to the queue.
3. While the queue is not empty, get the next id, add its importance, and add all its subordinates to the queue.
4. Continue summing up the importance until the queue is empty and return the total importance.

**Code:**

```java
public class Solution {
    public int getImportance(List<Employee> employees, int id) {
        // Create a map to store each employee by their id
        Map<Integer, Employee> map = new HashMap<>();
        
        // Fill the map with the employee data
        for (Employee emp : employees) {
            map.put(emp.id, emp);
        }
        
        // BFS implementation using a queue
        Queue<Integer> queue = new LinkedList<>();
        queue.add(id);
        
        // Track total importance
        int totalImportance = 0;
        
        // While there are ids in the queue to process
        while (!queue.isEmpty()) {
            // Extract the next employee's id from the queue
            int currentId = queue.poll();
            Employee employee = map.get(currentId);
            
            // Add the importance of the current employee
            totalImportance += employee.importance;
            
            // Add all subordinates to the queue
            for (int subid : employee.subordinates) {
                queue.add(subid);
            }
        }
        
        // Return the final calculated importance
        return totalImportance;
    }
}
```

**Time Complexity:** `O(N)`, where `N` is the number of employees. We visit each employee once.

**Space Complexity:** `O(N)`, due to the space used by the hashmap and the queue.

