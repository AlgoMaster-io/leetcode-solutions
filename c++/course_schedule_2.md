# 210. [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

## Approach 1: Topological Sort using DFS

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adjList = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            adjList.add(new ArrayList<>());
        }
        for (int[] prereq : prerequisites) {
            adjList.get(prereq[1]).add(prereq[0]);
        }

        boolean[] visited = new boolean[numCourses];
        boolean[] onPath = new boolean[numCourses];
        Stack<Integer> stack = new Stack<>();
      
        for (int i = 0; i < numCourses; i++) {
            if (!dfs(adjList, visited, onPath, stack, i)) {
                return new int[0];
            }
        }
        
        int[] order = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            order[i] = stack.pop();
        }
        return order;
    }

    private boolean dfs(List<List<Integer>> adjList, boolean[] visited, boolean[] onPath, Stack<Integer> stack, int course) {
        if (onPath[course]) {
            return false;
        }
        if (visited[course]) {
            return true;
        }

        visited[course] = true;
        onPath[course] = true;
        for (int neighbor : adjList.get(course)) {
            if (!dfs(adjList, visited, onPath, stack, neighbor)) {
                return false;
            }
        }
        onPath[course] = false;
        stack.push(course);
        return true;
    }
}
```

c++
```cpp
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
#include <vector>
#include <stack>
using namespace std;

class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> adjList(numCourses);
        for (const auto& prereq : prerequisites) {
            adjList[prereq[1]].push_back(prereq[0]);
        }

        vector<bool> visited(numCourses, false);
        vector<bool> onPath(numCourses, false);
        stack<int> stack;

        for (int i = 0; i < numCourses; i++) {
            if (!dfs(adjList, visited, onPath, stack, i)) {
                return {};
            }
        }

        vector<int> order(numCourses);
        for (int i = 0; i < numCourses; i++) {
            order[i] = stack.top();
            stack.pop();
        }
        return order;
    }

private:
    bool dfs(vector<vector<int>>& adjList, vector<bool>& visited, vector<bool>& onPath, stack<int>& stack, int course) {
        if (onPath[course]) {
            return false;
        }
        if (visited[course]) {
            return true;
        }

        visited[course] = true;
        onPath[course] = true;
        for (int neighbor : adjList[course]) {
            if (!dfs(adjList, visited, onPath, stack, neighbor)) {
                return false;
            }
        }
        onPath[course] = false;
        stack.push(course);
        return true;
    }
};
```

## Approach 2: Topological Sort using Kahn's Algorithm (BFS)

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
        for (int i = 0; i < numCourses; i++) {
            adjList.add(new ArrayList<>());
        }
        int[] indegree = new int[numCourses];
        for (int[] prereq : prerequisites) {
            adjList.get(prereq[1]).add(prereq[0]);
            indegree[prereq[0]]++;
        }

        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                queue.offer(i);
            }
        }

        int[] order = new int[numCourses];
        int index = 0;
        while (!queue.isEmpty()) {
            int course = queue.poll();
            order[index++] = course;
            for (int neighbor : adjList.get(course)) {
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) {
                    queue.offer(neighbor);
                }
            }
        }

        return index == numCourses ? order : new int[0];
    }
}
```

c++
```cpp
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
#include <vector>
#include <queue>
using namespace std;

class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> adjList(numCourses);
        vector<int> indegree(numCourses, 0);
        for (const auto& prereq : prerequisites) {
            adjList[prereq[1]].push_back(prereq[0]);
            indegree[prereq[0]]++;
        }

        queue<int> queue;
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                queue.push(i);
            }
        }

        vector<int> order;
        while (!queue.empty()) {
            int course = queue.front();
            queue.pop();
            order.push_back(course);
            for (int neighbor : adjList[course]) {
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) {
                    queue.push(neighbor);
                }
            }
        }

        return order.size() == numCourses ? order : vector<int>();
    }
};
```

