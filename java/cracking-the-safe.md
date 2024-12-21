# [Leetcode 753: Cracking the Safe](https://leetcode.com/problems/cracking-the-safe/)

## Approaches:

- [Approach 1: Eulerian Circuit (Hierholzer’s Algorithm)](#approach-1-eulerian-circuit-hierholzers-algorithm)
- [Approach 2: Recursive Backtracking (Brute Force)](#approach-2-recursive-backtracking-brute-force)

## Approach 1: Eulerian Circuit (Hierholzer’s Algorithm)

### Intuition:

The problem can be seen as finding an Eulerian path in a de Bruijn graph. For a given `n` and `k`, the graph is constructed where each vertex is a string of length `n-1`, and edges are created by appending one more character from `0` to `k-1`. An Eulerian circuit exists if each vertex in a directed graph has equal in-degree and out-degree, which is the case here.

Here's the step-by-step intuition for this approach:

1. **Graph Formation**: 
   - Nodes: Every password of length `n-1`.
   - Edges: Each edge represents the possible extensions of these nodes by adding one of `k` characters (forming the next state).
   
2. **Hierholzer's Algorithm**: 
   - Choose an arbitrary starting point and follow a trail of unused edges to build the path.
   - Continue walking through unused edges until you return to your starting point, creating a cycle.
   - If there are still unused edges, start a new cycle from one of the vertices on your current path.
   - Connect all cycles to form the Eulerian circuit.

### Code:

```java
public class CrackingTheSafe {
    public String crackSafe(int n, int k) {
        StringBuilder result = new StringBuilder();
        Set<String> visited = new HashSet<>();
        
        String start = String.join("", Collections.nCopies(n - 1, "0"));
        dfs(result, visited, start, n, k);
        
        // Adding the starting node to complete the last n characters of the sequence
        result.append(start);
        return result.toString();
    }

    private void dfs(StringBuilder result, Set<String> visited, String node, int n, int k) {
        // Try all possible edges/extensions from the current node
        for (int i = 0; i < k; i++) {
            String next = node + i;
            if (!visited.contains(next)) {
                visited.add(next);
                // Explore this path
                dfs(result, visited, next.substring(1), n, k);
                // Append the edge (digit `i`) to the result
                result.append(i);
            }
        }
    }
}
```

### Complexities:

- **Time Complexity**: O(k^n). This is because each node can be extended by `k` possibilities and there are approximately `k^(n-1)` nodes to explore.
- **Space Complexity**: O(k^n). To store the visited nodes (each of which is a password).

## Approach 2: Recursive Backtracking (Brute Force)

### Intuition:

In this approach, we attempt every possible combination while ensuring that the solution is of the minimum length. The backtracking will involve generating each possible path and checking if it could complete the cracking of the safe.

1. **Path Exploration**:
   - Start with an initial state.
   - Explore all potential extensions from each state sequentially.
   - Backtrack once each potential path is entirely explored or deemed unviable.

2. **Cycle Formulation**:
   - Start forming cycles from smaller sequences while checking if they form a valid sequence.

### Code:

```java
public class CrackingTheSafeBacktrack {
    public String crackSafe(int n, int k) {
        StringBuilder result = new StringBuilder();
        Set<String> visited = new HashSet<>();
        
        String start = String.join("", Collections.nCopies(n - 1, "0"));
        visited.add(start);
        
        if (backtrack(result, visited, start, n, k, (int) Math.pow(k, n))) {
            return result.toString();
        }
        return "";
    }

    private boolean backtrack(StringBuilder result, Set<String> visited, String node, int n, int k, int total) {
        if (visited.size() == total) {
            return true;
        }
        
        String lastDigits = node.substring(node.length() - n + 1);
        for (int i = 0; i < k; i++) {
            String next = lastDigits + i;
            if (!visited.contains(next)) {
                visited.add(next);
                result.append(i);
                if (backtrack(result, visited, next, n, k, total)) {
                    return true;
                }
                // If this path doesn't lead to a solution, backtrack
                visited.remove(next);
                result.deleteCharAt(result.length() - 1);
            }
        }
        return false;
    }
}
```

### Complexities:

- **Time Complexity**: O(k^n + n*k^n), due to the high cost of exploring each path and verifying possible configurations.
- **Space Complexity**: O(n*k^n), for storing the paths and visited nodes.

