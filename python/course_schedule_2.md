# 210. [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

## Approach 1: Topological Sort using Kahn's Algorithm

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adjList = new ArrayList<>();
        int[] indegree = new int[numCourses];
        int[] result = new int[numCourses];
        
        for (int i = 0; i < numCourses; i++) {
            adjList.add(new ArrayList<>());
        }
        
        for (int[] pre : prerequisites) {
            adjList.get(pre[1]).add(pre[0]);
            indegree[pre[0]]++;
        }
        
        Queue<Integer> queue = new LinkedList<>();
        
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        int index = 0;
        while (!queue.isEmpty()) {
            int course = queue.poll();
            result[index++] = course;
            
            for (int nextCourse : adjList.get(course)) {
                indegree[nextCourse]--;
                if (indegree[nextCourse] == 0) {
                    queue.offer(nextCourse);
                }
            }
        }
        
        return index == numCourses ? result : new int[0];
    }
}
```

python
```python
# Time Complexity: O(V + E)
# Space Complexity: O(V + E)
from collections import defaultdict, deque

class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        adj_list = defaultdict(list)
        indegree = [0] * numCourses
        result = []
        
        # Build the adjacency list and populate indegrees
        for dest, src in prerequisites:
            adj_list[src].append(dest)
            indegree[dest] += 1
        
        # Enqueue courses with no prerequisites
        queue = deque([i for i in range(numCourses) if indegree[i] == 0])
        
        while queue:
            course = queue.popleft()
            result.append(course)
            
            for next_course in adj_list[course]:
                indegree[next_course] -= 1
                # If indegree becomes zero, add to queue
                if indegree[next_course] == 0:
                    queue.append(next_course)
        
        return result if len(result) == numCourses else []
```

## Approach 2: Topological Sort using Depth-First Search (DFS)

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adjList = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            adjList.add(new ArrayList<>());
        }
        
        for (int[] pre : prerequisites) {
            adjList.get(pre[1]).add(pre[0]);
        }
        
        boolean[] visited = new boolean[numCourses];
        boolean[] onPath = new boolean[numCourses];
        List<Integer> result = new ArrayList<>();
        
        for (int i = 0; i < numCourses; i++) {
            if (!dfs(i, adjList, visited, onPath, result)) {
                return new int[0];
            }
        }
        
        int[] order = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            order[i] = result.get(numCourses - 1 - i);
        }
        
        return order;
    }
    
    private boolean dfs(int node, List<List<Integer>> adjList, boolean[] visited, boolean[] onPath, List<Integer> result) {
        if (onPath[node]) return false;
        if (visited[node]) return true;
        
        visited[node] = true;
        onPath[node] = true;
        
        for (int neighbor : adjList.get(node)) {
            if (!dfs(neighbor, adjList, visited, onPath, result)) {
                return false;
            }
        }
        
        onPath[node] = false;
        result.add(node);
        
        return true;
    }
}
```

python
```python
# Time Complexity: O(V + E)
# Space Complexity: O(V + E)
from collections import defaultdict

class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        adj_list = defaultdict(list)
        for dest, src in prerequisites:
            adj_list[src].append(dest)
        
        visited = [False] * numCourses
        on_path = [False] * numCourses
        result = []
        
        def dfs(node: int) -> bool:
            if on_path[node]:
                return False
            if visited[node]:
                return True
            
            visited[node] = True
            on_path[node] = True
            
            for neighbor in adj_list[node]:
                if not dfs(neighbor):
                    return False
            
            on_path[node] = False
            result.append(node)
            return True
        
        for i in range(numCourses):
            if not dfs(i):
                return []
        
        return result[::-1]
```

