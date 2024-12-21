## [Leetcode 1376: Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

### Navigation
- [Approach 1: BFS](#approach-1-bfs)
- [Approach 2: DFS](#approach-2-dfs)
- [Approach 3: DFS with Memoization](#approach-3-dfs-with-memoization)

---

### Approach 1: BFS

**Intuition:**

Convert the organizational structure into a tree where each node represents an employee. The CEO's id is `headID`. Using a Breadth First Search (BFS), calculate the total time needed for all direct and indirect employees to receive the information from the CEO.
  
In a BFS approach, we use a queue to traverse each level of the organization, propagating the information and accumulating the time needed.

**Steps:**

1. Create a list of lists where `subordinates[i]` holds all employees directly reporting to employee `i`.
2. Initialize a queue and start from the head of the organization (CEO).
3. At each employee, traverse their subordinates, adding the time taken to fetch the information and update the maximum time needed.

**Time Complexity:** O(N), where N is the number of employees since each employee is visited once.

**Space Complexity:** O(N) for storing the subordinates list and the queue.

```java
import java.util.*;

class Solution {
    public int numOfMinutes(int n, int headID, int[] manager, int[] informTime) {
        // Build the tree structure of employees
        List<List<Integer>> subordinates = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            subordinates.add(new ArrayList<>());
        }
        for (int i = 0; i < n; i++) {
            if (manager[i] != -1) {
                subordinates.get(manager[i]).add(i);
            }
        }
        
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[] {headID, 0});
        int maxTime = 0;
        
        // Begin BFS
        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            int currentId = current[0];
            int currentTime = current[1];
            maxTime = Math.max(maxTime, currentTime);
            
            // Traverse subordinates
            for (int sub : subordinates.get(currentId)) {
                queue.offer(new int[] {sub, currentTime + informTime[currentId]});
            }
        }
        return maxTime;
    }
}
```

---

### Approach 2: DFS

**Intuition:**

Similar to BFS, DFS also helps us traverse the structure of employees, but instead of using a queue, we use recursion. For each employee, we traverse deeper into their subordinates before moving on to the next sibling.

**Steps:**

1. Build a tree representation using adjacency lists for subordinates.
2. Use DFS to recursively traverse from the head of the organization.
3. At each recursive call, add the inform time particular to an employee and update the maximum time.

**Time Complexity:** O(N), since each employee is visited once.

**Space Complexity:** O(N), which is the recursion stack depth in the worst case.

```java
import java.util.*;

class Solution {
    int maxTime = 0;
    
    public int numOfMinutes(int n, int headID, int[] manager, int[] informTime) {
        // Build the tree structure of employees
        List<List<Integer>> subordinates = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            subordinates.add(new ArrayList<>());
        }
        for (int i = 0; i < n; i++) {
            if (manager[i] != -1) {
                subordinates.get(manager[i]).add(i);
            }
        }

        // Call DFS from the head of the organization
        dfs(headID, 0, subordinates, informTime);
        return maxTime;
    }
    
    private void dfs(int currentId, int currentTime, List<List<Integer>> subordinates, int[] informTime) {
        // Update maxTime
        maxTime = Math.max(maxTime, currentTime);
        
        // Depth-first search into subordinates
        for (int sub : subordinates.get(currentId)) {
            dfs(sub, currentTime + informTime[currentId], subordinates, informTime);
        }
    }
}
```

---

### Approach 3: DFS with Memoization

**Intuition:**

This approach recognizes that a problem needs to calculate the time involved recursively but instead of recalculating the time each time, it stores (memoizes) the computed time for each employee to save computational steps.

**Steps:**

1. Same as previous DFS approach, but maintain a memoization array to store the time required to inform each subtree.
2. Benefits arise when subtrees are visited multiple times through different paths.

**Time Complexity:** O(N), as we compute each subtree's time once.

**Space Complexity:** O(N), owing to recursion space and additional space for the memo array.

```java
import java.util.*;

class Solution {
    public int numOfMinutes(int n, int headID, int[] manager, int[] informTime) {
        List<List<Integer>> subordinates = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            subordinates.add(new ArrayList<>());
        }
        for (int i = 0; i < n; i++) {
            if (manager[i] != -1) {
                subordinates.get(manager[i]).add(i);
            }
        }

        int[] memo = new int[n];
        Arrays.fill(memo, -1);

        return dfs(headID, subordinates, informTime, memo);
    }
    
    private int dfs(int currentId, List<List<Integer>> subordinates, int[] informTime, int[] memo) {
        if (memo[currentId] != -1) {
            return memo[currentId];
        }

        int max = 0;
        for (int sub : subordinates.get(currentId)) {
            max = Math.max(max, dfs(sub, subordinates, informTime, memo));
        }
        
        memo[currentId] = informTime[currentId] + max;
        return memo[currentId];
    }
}
```

---

Each approach offers unique benefits and insights into solving the problem using traversal algorithms, giving a well-rounded perspective of how to tackle recursive-accumulative problems.

