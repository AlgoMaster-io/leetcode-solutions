# [Problem: Leetcode 753 - Cracking the Safe](https://leetcode.com/problems/cracking-the-safe/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: DFS with Backtracking](#approach-2-dfs-with-backtracking)
- [Approach 3: Hierholzer's Algorithm](#approach-3-hierholzers-algorithm)

---

## Approach 1: Brute Force

### Intuition
The problem essentially asks us to find the shortest string that contains all possible combinations of a password from `k` characters and of length `n`. A brute force approach would try generating all possible combinations and checking if they satisfy the condition.

### Implementation

```csharp
// This solution is not shown here because it's impractical and inefficient,
// especially for larger values of n and k.
```

### Complexity
- **Time Complexity:** O(k^n * n), due to the generation of all possible combinations.
- **Space Complexity:** O(k^n), to store the combinations.

---

## Approach 2: DFS with Backtracking

### Intuition
This approach utilizes the depth-first search to explore sequences, backtracking when necessary. The trick is to generate each combination exactly once and form a valid sequence without repeated processing.

### Implementation

```csharp
public class Solution {
    public string CrackSafe(int n, int k) {
        var start = new string('0', n);
        var visited = new HashSet<string>();
        var result = new StringBuilder();
        
        // Start DFS from the initial password
        Dfs(start, visited, result, n, k);
        
        // Append the initial part to complete the circle
        result.Append(start);
        
        return result.ToString();
    }

    private void Dfs(string node, HashSet<string> visited, StringBuilder result, int n, int k) {
        for (int i = 0; i < k; i++) {
            // Try appending each digit and forming a new node
            var newNode = node.Substring(1) + i;
            
            // If not visited, continue exploring
            if (!visited.Contains(newNode)) {
                visited.Add(newNode);
                Dfs(newNode, visited, result, n, k);
                // Append this digit to result (creating Eulerian path step)
                result.Append(i);
            }
        }
    }
}
```

### Complexity
- **Time Complexity:** O(k^n), to visit each possible combination (Eulerian path).
- **Space Complexity:** O(k^n), for storing visited nodes.

---

## Approach 3: Hierholzer's Algorithm

### Intuition
Hierholzer’s algorithm is used to find an Eulerian path (or circuit) efficiently. An Eulerian circuit is a path that visits every edge of a graph exactly once. By constructing this path for our problem, we create a sequence that includes all combinations implicitly.

### Implementation

```csharp
public class Solution {
    public string CrackSafe(int n, int k) {
        int totalCombinations = (int)Math.Pow(k, n);
        HashSet<string> visited = new HashSet<string>();
        StringBuilder result = new StringBuilder();
        
        // Start with an initial node (all zeros)
        string startNode = new string('0', n);
        visited.Add(startNode);

        // Use Hierholzer's algorithm to find Eulerian path
        EulerianPath(startNode, visited, result, n, k);

        // Complete the result string with the initial node to cover all combinations
        result.Append(startNode.Substring(0, n - 1));

        return result.ToString();
    }
    
    private void EulerianPath(string node, HashSet<string> visited, StringBuilder result, int n, int k) {
        for (int i = 0; i < k; i++) {
            // Create the next node by shifting and adding new digit
            string nextNode = node.Substring(1) + i;

            // Visit the edge only if it hasn't been visited yet
            if (!visited.Contains(nextNode)) {
                visited.Add(nextNode);
                EulerianPath(nextNode, visited, result, n, k);
                // Append only after visiting the node's neighbors (post-order)
                result.Append(i);
            }
        }
    }
}
```

### Complexity
- **Time Complexity:** O(k^n), efficiently visits all edge combinations once.
- **Space Complexity:** O(k^n), maintaining visited nodes and result string.

---

