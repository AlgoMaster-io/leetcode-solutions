# [Leetcode 863: All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)
  
Approaches:
- [Approach 1: Convert Tree to Undirected Graph](#approach-1)
- [Approach 2: DFS to Find Target Node and Explore Neighbors Using BFS](#approach-2)

## Approach 1: Convert Tree to Undirected Graph

### Intuition
The problem can be simplified by viewing the binary tree as an undirected graph. Each node can be connected to its children and, for our purposes, we can connect each child back to its parent, forming a graph-like adjacency list. Once this graph is prepared, we can perform a Breadth-First Search (BFS) to find all nodes that are exactly K distance away from the target node.

### Steps:
1. **Build Graph**: Start by performing a DFS traversal on the tree to convert it into an undirected graph, represented by an adjacency list.
2. **BFS Traversal**: Perform a BFS starting from the target node, keeping track of visited nodes to avoid cycles. Record nodes that are exactly K edges away.

### Java Code
```java
import java.util.*;

// Definition for a binary tree node.
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

public class Solution {
    public List<Integer> distanceK(TreeNode root, TreeNode target, int K) {
        Map<TreeNode, List<TreeNode>> graph = new HashMap<>();
        buildGraph(root, null, graph);
        
        // BFS initialization
        Queue<TreeNode> queue = new LinkedList<>();
        Set<TreeNode> visited = new HashSet<>();
        queue.add(target);
        visited.add(target);

        // Starting BFS
        int distance = 0;
        while (!queue.isEmpty()) {
            if (distance == K) {
                List<Integer> result = new ArrayList<>();
                for (TreeNode node : queue) {
                    result.add(node.val);
                }
                return result;
            }

            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                for (TreeNode neighbor : graph.get(node)) {
                    if (!visited.contains(neighbor)) {
                        visited.add(neighbor);
                        queue.add(neighbor);
                    }
                }
            }
            distance++;
        }

        return new ArrayList<>();
    }

    private void buildGraph(TreeNode node, TreeNode parent, Map<TreeNode, List<TreeNode>> graph) {
        if (node == null) return;
        graph.putIfAbsent(node, new ArrayList<>());
        if (parent != null) {
            graph.get(node).add(parent);
            graph.get(parent).add(node);
        }
        buildGraph(node.left, node, graph);
        buildGraph(node.right, node, graph);
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(N) where N is the number of nodes. Constructing the graph takes O(N) and performing BFS also takes O(N).
- **Space Complexity**: O(N) for storing the graph and visited nodes.

## Approach 2: DFS to Find Target Node and Explore Neighbors Using BFS

### Intuition
Instead of explicitly building a graph, we can use a combination of DFS and BFS directly on the tree structure. First, use DFS to find the path to the target node. Then, treat this path as the base and perform BFS from the target while marking distances in the BFS.

### Steps:
1. **DFS to Target**: Find the target node and the path leading to it by using DFS. Mark each node's distance from the root (or considered starting point).
2. **BFS Exploration**: Use BFS from the target to find nodes at distance K. Treat any node along the path to the target as an "intermediary root" to account for upward paths.

### Code
```java
import java.util.*;

public class Solution {
    public List<Integer> distanceK(TreeNode root, TreeNode target, int K) {
        List<Integer> result = new ArrayList<>();
        findDistanceAndCollect(root, target, K, result);
        return result;
    }
    
    private int findDistanceAndCollect(TreeNode node, TreeNode target, int K, List<Integer> result) {
        if (node == null) return -1;   // Target not found in this subtree
        
        if (node == target) {
            collectNodesAtDistance(node, K, result);
            return 0;   // Distance from target to itself is 0
        }
        
        int leftDistance = findDistanceAndCollect(node.left, target, K, result);
        if (leftDistance != -1) {
            if (leftDistance + 1 == K) result.add(node.val);  // Node is K distance from target
            else collectNodesAtDistance(node.right, K - leftDistance - 2, result);  // Check the opposite subtree
            return leftDistance + 1;
        }
        
        int rightDistance = findDistanceAndCollect(node.right, target, K, result);
        if (rightDistance != -1) {
            if (rightDistance + 1 == K) result.add(node.val);  // Node is K distance from target
            else collectNodesAtDistance(node.left, K - rightDistance - 2, result);  // Check the opposite subtree
            return rightDistance + 1;
        }
        
        return -1;   // Target was not found in either subtree
    }
    
    private void collectNodesAtDistance(TreeNode node, int distance, List<Integer> result) {
        if (node == null || distance < 0) return;
        
        if (distance == 0) {
            result.add(node.val);
            return;
        }
        
        // Traverse both children to find nodes at required distance
        collectNodesAtDistance(node.left, distance - 1, result);
        collectNodesAtDistance(node.right, distance - 1, result);
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the tree, since we potentially traverse the entire tree.
- **Space Complexity**: O(N) due to recursive stack usage in DFS calls.

