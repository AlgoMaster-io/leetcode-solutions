# [Leetcode 690: Employee Importance](https://leetcode.com/problems/employee-importance/)

## Approaches

1. [Breadth-First Search (BFS) Approach](#approach-1-bfs-approach)
2. [Depth-First Search (DFS) Approach](#approach-2-dfs-approach)


---

## Approach 1: BFS Approach

### Intuition
The problem involves calculating the importance of a specific employee, along with all their subordinates. Using a BFS approach neatly fits this problem as it allows us to start from a specific employee and explore all its related employees in a layer-by-layer fashion, akin to exploring levels in a tree or graph.

### Detailed Explanation
1. **Data Storage**: Use a hash map to quickly access employees by their unique ID.
2. **Queue Initialization**: Start by initializing a queue with the given employee ID.
3. **Iteration**:
   - As you dequeue each employee, add their importance value to the total.
   - Enqueue all their subordinates.
4. **Continue** this until there are no more employees in the queue.

This method ensures you check every associated employee only once, giving it a time complexity proportional to the number of employees and their connections.

### Code
```cpp
#include <unordered_map>
#include <queue>
#include <vector>

using namespace std;

// Definition for Employee.
class Employee {
public:
    int id;
    int importance;
    vector<int> subordinates;
};

class Solution {
public:
    int getImportance(vector<Employee*> employees, int id) {
        // Create a map for quick access to employees by their id
        unordered_map<int, Employee*> empMap;
        for (auto& emp : employees) {
            empMap[emp->id] = emp;
        }
        
        // BFS queue initialization
        queue<int> queue;
        queue.push(id);
        
        int total_importance = 0;
        
        // Process each employee in the queue
        while (!queue.empty()) {
            int currId = queue.front();
            queue.pop();
            
            Employee* emp = empMap[currId];
            total_importance += emp->importance;  // Add the current employee's importance
            
            // Add all subordinates of the current employee to the queue
            for (int subId : emp->subordinates) {
                queue.push(subId);
            }
        }
        
        return total_importance;
    }
};
```

### Time Complexity
- **O(N)**, where `N` is the number of employees. Each employee and their subordinates are processed once.

### Space Complexity
- **O(N)**, necessary for the queue and map which store all employees.

---

## Approach 2: DFS Approach

### Intuition
A DFS approach is also a natural fit because it allows for directly exploring each path starting at the given employee ID down to all possible subordinates, recursively accumulating their importance.

### Detailed Explanation
1. **Data Storage**: Similar to the BFS approach, create a hash map for easy access to employees.
2. **Recursive Function**:
   - For an employee, add their importance.
   - Recursively calculate the total importance for each of their subordinates.
3. **Base Case**: If the employee has no subordinates, return their importance immediately.

This recursive solution is elegant, with a direct representation of the problem structure.

### Code
```cpp
#include <unordered_map>
#include <vector>

using namespace std;

// Definition for Employee.
class Employee {
public:
    int id;
    int importance;
    vector<int> subordinates;
};

class Solution {
public:
    int getImportance(vector<Employee*> employees, int id) {
        // Create a map for quick access to employees by their id
        unordered_map<int, Employee*> empMap;
        for (auto& emp : employees) {
            empMap[emp->id] = emp;
        }
        
        // DFS function to calculate importance recursively
        return dfs(id, empMap);
    }
    
    int dfs(int id, unordered_map<int, Employee*>& empMap) {
        Employee* emp = empMap[id];
        int total_importance = emp->importance;
        
        for (int subId : emp->subordinates) {
            total_importance += dfs(subId, empMap);
        }
        
        return total_importance;
    }
};
```

### Time Complexity
- **O(N)**, as each employee and their subordinates are explored once.

### Space Complexity
- **O(N)**, primarily due to recursion stack and map storage.

---

