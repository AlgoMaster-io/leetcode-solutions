# [LeetCode 210: Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

## Approach:
- [Approach 1: DFS-Based Topological Sort](#approach-1-dfs-based-topological-sort)
- [Approach 2: Kahn's Algorithm (BFS-Based Topological Sort)](#approach-2-kahns-algorithm-bfs-based-topological-sort)

### Approach 1: DFS-Based Topological Sort

#### Intuition:
We can use a depth-first search (DFS) to perform a topological sort on the graph representing course dependencies. The idea is to traverse the graph and, upon finishing exploration of a node, add it to the result stack. A cycle presence check can be done using an additional path tracking array.

#### Steps:
1. Represent the graph using an adjacency list where each course points to its prerequisites.
2. Use three states to track nodes: `0` for unvisited, `1` for visiting, and `-1` for visited.
3. Traverse each node; if it's unvisited, perform a DFS.
4. If during DFS a node is revisited before it finishes, detect a cycle.
5. After successful DFS of all nodes, our stack will contain the courses in a topological order.

#### Code:
```python
def findOrder(numCourses, prerequisites):
    def dfs(course):
        if state[course] == -1:  # visited node
            return True
        if state[course] == 1:  # node is being visited, cycle detected
            return False

        # Mark the node as being visited
        state[course] = 1
        for prereq in adjacency_list[course]:
            if not dfs(prereq):
                return False

        # Mark node as visited
        state[course] = -1
        # Append the course in the order of finishing
        order.append(course)
        return True

    # Construct the graph
    adjacency_list = [[] for _ in range(numCourses)]
    for course, prereq in prerequisites:
        adjacency_list[course].append(prereq)

    state = [0] * numCourses
    order = []

    for course in range(numCourses):
        if state[course] == 0:  # Unvisited
            if not dfs(course):
                return []  # Return empty if a cycle is detected

    return order

# Example usage
print(findOrder(4, [[1,0],[2,0],[3,1],[3,2]]))  # Outputs: [0, 2, 1, 3] or any other valid order
```

#### Complexity:
- **Time Complexity:** \(O(V + E)\), where \(V\) is the number of courses and \(E\) is the number of dependencies.
- **Space Complexity:** \(O(V + E)\) for the graph representation and recursion stack.

### Approach 2: Kahn's Algorithm (BFS-Based Topological Sort)

#### Intuition:
Kahn’s Algorithm uses BFS to construct the topological order by maintaining a list/queue of nodes with no incoming edges. By systematically removing nodes with no prerequisites (zero in-degree), we can create a valid course order.

#### Steps:
1. Calculate the in-degree (number of incoming edges) for each node.
2. Initialize a queue with nodes of zero in-degree.
3. Process nodes from the queue, appending them to the course order.
4. After removing a node, reduce the in-degree of its neighbors.
5. If a neighbor’s in-degree becomes zero, add it to the queue.
6. If we can process all nodes, return order, else detect a cycle.

#### Code:
```python
from collections import deque

def findOrder(numCourses, prerequisites):
    # Initialize in-degree and adjacency list
    in_degree = [0] * numCourses
    adjacency_list = [[] for _ in range(numCourses)]
    for course, prereq in prerequisites:
        adjacency_list[prereq].append(course)
        in_degree[course] += 1

    # Initialize queue with zero in-degree nodes
    zero_in_degree_queue = deque([i for i in range(numCourses) if in_degree[i] == 0])
    order = []

    while zero_in_degree_queue:
        node = zero_in_degree_queue.popleft()
        order.append(node)
        # Reduce the in-degree of neighbors
        for neighbor in adjacency_list[node]:
            in_degree[neighbor] -= 1
            # If in-degree becomes zero, add neighbor to queue
            if in_degree[neighbor] == 0:
                zero_in_degree_queue.append(neighbor)
    
    # Check if topological sort includes all nodes
    return order if len(order) == numCourses else []

# Example usage
print(findOrder(4, [[1,0],[2,0],[3,1],[3,2]]))  # Outputs: [0, 1, 2, 3] or any other valid order
```

#### Complexity:
- **Time Complexity:** \(O(V + E)\), where \(V\) is the number of courses and \(E\) is the number of dependencies.
- **Space Complexity:** \(O(V + E)\) for the graph representation and deque.

