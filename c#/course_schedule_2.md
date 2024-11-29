# 210. [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

## Approach 1: Topological Sorting (Kahn's Algorithm) with BFS

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.*;

public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<List<Integer>> graph = new ArrayList<>();
        int[] inDegree = new int[numCourses];
        
        for (int i = 0; i < numCourses; i++) {
            graph.add(new ArrayList<>());
        }
        
        for (int[] prerequisite : prerequisites) {
            graph.get(prerequisite[1]).add(prerequisite[0]);
            inDegree[prerequisite[0]]++;
        }
        
        Queue<Integer> queue = new LinkedList<>();
        
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        int[] order = new int[numCourses];
        int index = 0;
        
        while (!queue.isEmpty()) {
            int course = queue.poll();
            order[index++] = course;
            
            for (int nextCourse : graph.get(course)) {
                if (--inDegree[nextCourse] == 0) {
                    queue.offer(nextCourse);
                }
            }
        }
        
        return index == numCourses ? order : new int[0];
    }
}
```

csharp
```csharp
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
using System;
using System.Collections.Generic;

public class Solution {
    public int[] FindOrder(int numCourses, int[][] prerequisites) {
        var graph = new List<List<int>>();
        var inDegree = new int[numCourses];
        
        for (int i = 0; i < numCourses; i++) {
            graph.Add(new List<int>());
        }
        
        foreach (var prerequisite in prerequisites) {
            graph[prerequisite[1]].Add(prerequisite[0]);
            inDegree[prerequisite[0]]++;
        }
        
        var queue = new Queue<int>();
        
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.Enqueue(i);
            }
        }
        
        var order = new int[numCourses];
        int index = 0;
        
        while (queue.Count > 0) {
            int course = queue.Dequeue();
            order[index++] = course;
            
            foreach (var nextCourse in graph[course]) {
                if (--inDegree[nextCourse] == 0) {
                    queue.Enqueue(nextCourse);
                }
            }
        }
        
        return index == numCourses ? order : new int[0];
    }
}
```

## Approach 2: Topological Sorting with DFS

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.*;

public class Solution {
    private boolean dfs(int node, List<List<Integer>> graph, boolean[] visited, boolean[] recursionStack, Stack<Integer> stack) {
        visited[node] = true;
        recursionStack[node] = true;
        
        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                if (!dfs(neighbor, graph, visited, recursionStack, stack)) {
                    return false;
                }
            } else if (recursionStack[neighbor]) {
                return false;
            }
        }
        
        recursionStack[node] = false;
        stack.push(node);
        return true;
    }
    
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<List<Integer>> graph = new ArrayList<>();
        
        for (int i = 0; i < numCourses; i++) {
            graph.add(new ArrayList<>());
        }
        
        for (int[] prerequisite : prerequisites) {
            graph.get(prerequisite[1]).add(prerequisite[0]);
        }
        
        boolean[] visited = new boolean[numCourses];
        boolean[] recursionStack = new boolean[numCourses];
        Stack<Integer> stack = new Stack<>();
        
        for (int i = 0; i < numCourses; i++) {
            if (!visited[i]) {
                if (!dfs(i, graph, visited, recursionStack, stack)) {
                    return new int[0];
                }
            }
        }
        
        int[] order = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            order[i] = stack.pop();
        }
        
        return order;
    }
}
```

csharp
```csharp
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
using System;
using System.Collections.Generic;

public class Solution {
    private bool Dfs(int node, List<List<int>> graph, bool[] visited, bool[] recursionStack, Stack<int> stack) {
        visited[node] = true;
        recursionStack[node] = true;
        
        foreach (var neighbor in graph[node]) {
            if (!visited[neighbor]) {
                if (!Dfs(neighbor, graph, visited, recursionStack, stack)) {
                    return false;
                }
            } else if (recursionStack[neighbor]) {
                return false;
            }
        }
        
        recursionStack[node] = false;
        stack.Push(node);
        return true;
    }
    
    public int[] FindOrder(int numCourses, int[][] prerequisites) {
        var graph = new List<List<int>>();
        
        for (int i = 0; i < numCourses; i++) {
            graph.Add(new List<int>());
        }
        
        foreach (var prerequisite in prerequisites) {
            graph[prerequisite[1]].Add(prerequisite[0]);
        }
        
        var visited = new bool[numCourses];
        var recursionStack = new bool[numCourses];
        var stack = new Stack<int>();
        
        for (int i = 0; i < numCourses; i++) {
            if (!visited[i]) {
                if (!Dfs(i, graph, visited, recursionStack, stack)) {
                    return new int[0];
                }
            }
        }
        
        var order = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            order[i] = stack.Pop();
        }
        
        return order;
    }
}
```

