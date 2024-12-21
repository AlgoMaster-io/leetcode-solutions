[Leetcode Problem 210: Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

### Approaches

1. [Breadth-First Search (BFS) using Kahn's Algorithm](#approach-1-bfs-using-kahns-algorithm)
2. [Depth-First Search (DFS) Topological Sort](#approach-2-dfs-topological-sort)

---

## Approach 1: BFS using Kahn's Algorithm

Kahn’s algorithm is an efficient method to obtain a topological ordering of vertices in a Directed Acyclic Graph (DAG) using BFS. The idea is to identify vertices with no incoming edges to start with since they are independent. We iteratively remove and add vertices from our result while updating their neighbors’ dependencies.

### Intuition

- Construct a graph using adjacency lists.
- Maintain an indegree array to track the number of prerequisites for each course.
- Use a queue to process all courses with no prerequisites.
- Continuously add courses to the result which have no remaining dependencies and reduce the indegree of its neighbors.
- If all courses are added to the result as expected, a schedule was found. If not, the graph was likely cyclic, making it impossible.

### Code

```csharp
public int[] FindOrder(int numCourses, int[][] prerequisites) {
    // Initialize indegree array and adjacency list
    int[] indegree = new int[numCourses];
    List<int>[] adjacencyList = new List<int>[numCourses];
    
    for(int i = 0; i < numCourses; i++) {
        adjacencyList[i] = new List<int>();
    }
    
    // Build the graph and the indegree array
    foreach(var prereq in prerequisites) {
        indegree[prereq[0]]++;
        adjacencyList[prereq[1]].Add(prereq[0]);
    }
    
    // Queue for BFS
    Queue<int> queue = new Queue<int>();

    // Enqueue courses with no prerequisites
    for(int i = 0; i < numCourses; i++) {
        if(indegree[i] == 0) {
            queue.Enqueue(i);
        }
    }
    
    int[] order = new int[numCourses];
    int index = 0; // To track the position in order array
    
    // Process nodes in BFS manner
    while(queue.Count > 0) {
        int course = queue.Dequeue();
        order[index++] = course;

        foreach(int nextCourse in adjacencyList[course]) {
            indegree[nextCourse]--;
            // Enqueue if no more prerequisites
            if(indegree[nextCourse] == 0) {
                queue.Enqueue(nextCourse);
            }
        }
    }
    
    // If index equals numCourses, we have a valid ordering
    if(index == numCourses) {
        return order;
    } else {
        return new int[0];
    }
}
```

### Complexity Analysis

- **Time Complexity:** \(O(V + E)\) where \(V\) is the number of courses and \(E\) is the number of prerequisites.
- **Space Complexity:** \(O(V + E)\) to store the graph and the indegree array.

---

## Approach 2: DFS Topological Sort

DFS-based approach leverages recursion to visit nodes and build the course order list in reverse. Courses that appear later in the recursive call are more independent and are dependencies themselves, hence added earlier in the actual list.

### Intuition

- Use a stack for the reverse order list.
- Use a cycle detection mechanism with a state array to ensure no cycles (back edges).
- Deliver the order using the visited, state array to prevent revisiting nodes.

### Code

```csharp
public int[] FindOrder(int numCourses, int[][] prerequisites) {
    // Create the graph as adjacency list
    List<int>[] graph = new List<int>[numCourses];
    for (int i = 0; i < numCourses; i++) {
        graph[i] = new List<int>();
    }
    
    // Build the graph
    foreach (var pair in prerequisites) {
        graph[pair[1]].Add(pair[0]);
    }
    
    // 0 = unvisited, 1 = visiting, 2 = visited
    int[] state = new int[numCourses];
    Stack<int> stack = new Stack<int>();
    
    // Perform DFS for each course
    for (int i = 0; i < numCourses; i++) {
        if (!DFS(i, graph, state, stack)) {
            return new int[0]; // Return an empty array if a cycle is detected
        }
    }
    
    // Return stack content as array
    return stack.ToArray();
}

private bool DFS(int course, List<int>[] graph, int[] state, Stack<int> stack) {
    if (state[course] == 1) return false; // Detect cycle
    if (state[course] == 2) return true;

    state[course] = 1; // Mark as visiting

    // Visit all the dependencies of the current course
    foreach (var nextCourse in graph[course]) {
        if (!DFS(nextCourse, graph, state, stack)) {
            return false;
        }
    }

    state[course] = 2; // Mark as visited
    stack.Push(course); // Add to stack only when fully processed

    return true;
}
```

### Complexity Analysis

- **Time Complexity:** \(O(V + E)\) where \(V\) is the number of courses and \(E\) is the number of prerequisites.
- **Space Complexity:** \(O(V + E)\) due to stack space and the state array.

Both approaches efficiently resolve the problem with topological sorting techniques applicable to directed acyclic graphs, ensuring dependencies are respected.

