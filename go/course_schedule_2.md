# 210. [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

## Approach 1: Topological Sorting using Kahn's Algorithm

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.*;

public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        int[] inDegree = new int[numCourses];
        
        // Build graph and calculate in-degrees of each node
        for (int i = 0; i < numCourses; i++) {
            graph[i] = new ArrayList<>();
        }
        for (int[] prereq : prerequisites) {
            graph[prereq[1]].add(prereq[0]);
            inDegree[prereq[0]]++;
        }

        // Initialize queue with nodes of in-degree 0
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }

        // Perform topological sort
        int[] order = new int[numCourses];
        int index = 0;
        while (!queue.isEmpty()) {
            int course = queue.poll();
            order[index++] = course;
            for (int next : graph[course]) {
                if (--inDegree[next] == 0) {
                    queue.offer(next);
                }
            }
        }

        // Check if topological sorting is possible
        return index == numCourses ? order : new int[0];
    }
}
```

go
```go
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
package main

func findOrder(numCourses int, prerequisites [][]int) []int {
	graph := make([][]int, numCourses)
	inDegree := make([]int, numCourses)

	// Build graph and calculate in-degrees of each node
	for _, prereq := range prerequisites {
		first, second := prereq[1], prereq[0]
		graph[first] = append(graph[first], second)
		inDegree[second]++
	}

	// Initialize queue with nodes of in-degree 0
	queue := make([]int, 0)
	for i := 0; i < numCourses; i++ {
		if inDegree[i] == 0 {
			queue = append(queue, i)
		}
	}

	// Perform topological sort
	order := make([]int, 0, numCourses)
	for len(queue) > 0 {
		course := queue[0]
		queue = queue[1:]
		order = append(order, course)
		for _, next := range graph[course] {
			inDegree[next]--
			if inDegree[next] == 0 {
				queue = append(queue, next)
			}
		}
	}

	// Check if topological sorting is possible
	if len(order) == numCourses {
		return order
	}
	return nil
}
```

## Approach 2: DFS-based Topological Sorting

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.*;

public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        boolean[] visited = new boolean[numCourses];
        boolean[] onPath = new boolean[numCourses];
        List<Integer> order = new ArrayList<>();

        // Build graph
        for (int i = 0; i < numCourses; i++) {
            graph[i] = new ArrayList<>();
        }
        for (int[] prereq : prerequisites) {
            graph[prereq[1]].add(prereq[0]);
        }

        // Perform DFS visit for all courses
        for (int i = 0; i < numCourses; i++) {
            if (!visited[i] && hasCycle(graph, visited, onPath, order, i)) {
                return new int[0];
            }
        }

        Collections.reverse(order);
        return order.stream().mapToInt(i -> i).toArray();
    }

    private boolean hasCycle(List<Integer>[] graph, boolean[] visited, boolean[] onPath, List<Integer> order, int course) {
        if (onPath[course]) return true;
        if (visited[course]) return false;

        visited[course] = true;
        onPath[course] = true;
        for (int next : graph[course]) {
            if (hasCycle(graph, visited, onPath, order, next)) {
                return true;
            }
        }
        onPath[course] = false;
        order.add(course);
        return false;
    }
}
```

go
```go
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
package main

func findOrder(numCourses int, prerequisites [][]int) []int {
	graph := make([][]int, numCourses)
	visited := make([]bool, numCourses)
	onPath := make([]bool, numCourses)
	var order []int

	// Build graph
	for _, prereq := range prerequisites {
		graph[prereq[1]] = append(graph[prereq[1]], prereq[0])
	}

	// Perform DFS visit for all courses
	for i := 0; i < numCourses; i++ {
		if !visited[i] && hasCycle(graph, visited, onPath, &order, i) {
			return nil
		}
	}

	// Reverse order for topological sort
	reverse(order)
	return order
}

func hasCycle(graph [][]int, visited, onPath []bool, order *[]int, course int) bool {
	if onPath[course] {
		return true
	}
	if visited[course] {
		return false
	}

	visited[course] = true
	onPath[course] = true
	for _, next := range graph[course] {
		if hasCycle(graph, visited, onPath, order, next) {
			return true
		}
	}
	onPath[course] = false
	*order = append(*order, course)
	return false
}

func reverse(nums []int) {
	for i, j := 0, len(nums)-1; i < j; i, j = i+1, j-1 {
		nums[i], nums[j] = nums[j], nums[i]
	}
}
```

