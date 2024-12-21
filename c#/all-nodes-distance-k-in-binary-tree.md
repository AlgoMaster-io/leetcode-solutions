# [Leetcode 863: All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)

## Approaches
- [Approach 1: BFS with Parent Pointer](#approach-1-bfs-with-parent-pointer)
- [Approach 2: DFS with BFS for Distance Calculation](#approach-2-dfs-with-bfs-for-distance-calculation)

## Approach 1: BFS with Parent Pointer

### Intuition
The core idea is to treat the binary tree like an undirected graph by marking parent references. After that, we can use a Breadth-First Search (BFS) to find nodes at distance K. This method involves two main steps:
1. Traverse the tree and store parent pointers for each node.
2. Perform a BFS starting from the target node, looking for nodes at distance K while avoiding revisiting nodes using a set.

### Implementation

```csharp
using System.Collections.Generic;

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public IList<int> DistanceK(TreeNode root, TreeNode target, int K) {
        // Dictionary to store parent pointers for each node.
        Dictionary<TreeNode, TreeNode> parentMap = new Dictionary<TreeNode, TreeNode>();
        // Build parent pointers.
        BuildParentMap(root, null, parentMap);
        
        // Perform BFS from the target node.
        Queue<TreeNode> queue = new Queue<TreeNode>();
        HashSet<TreeNode> visited = new HashSet<TreeNode>();
        queue.Enqueue(target);
        visited.Add(target);
        int currentDistance = 0;
        
        // List to hold nodes at distance K.
        List<int> result = new List<int>();
        
        while (queue.Count > 0) {
            int size = queue.Count;
            if (currentDistance == K) {
                foreach (var node in queue) {
                    result.Add(node.val);
                }
                break;
            }
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.Dequeue();
                // Explore neighbors: left child, right child, and parent.
                if (node.left != null && !visited.Contains(node.left)) {
                    queue.Enqueue(node.left);
                    visited.Add(node.left);
                }
                if (node.right != null && !visited.Contains(node.right)) {
                    queue.Enqueue(node.right);
                    visited.Add(node.right);
                }
                if (parentMap.ContainsKey(node) && !visited.Contains(parentMap[node])) {
                    queue.Enqueue(parentMap[node]);
                    visited.Add(parentMap[node]);
                }
            }
            currentDistance++;
        }
        
        return result;
    }
    
    private void BuildParentMap(TreeNode node, TreeNode parent, Dictionary<TreeNode, TreeNode> parentMap) {
        if (node != null) {
            parentMap[node] = parent;
            BuildParentMap(node.left, node, parentMap);
            BuildParentMap(node.right, node, parentMap);
        }
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(N) where N is the number of nodes in the tree. Building the parent map and traversing using BFS both take linear time.
- **Space Complexity**: O(N) to store the parent pointers and the visited set.

## Approach 2: DFS with BFS for Distance Calculation

### Intuition
This approach uses a Depth-First Search (DFS) to first find the target node and calculate distances in one pass. We calculate the distance from the target to all other nodes, and whenever we reach a distance of K, we add that node to our result list.

### Implementation

```csharp
using System.Collections.Generic;

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    private List<int> result = new List<int>();
    
    public IList<int> DistanceK(TreeNode root, TreeNode target, int K) {
        FindDistance(root, target, K);
        return result;
    }
    
    private int FindDistance(TreeNode node, TreeNode target, int K) {
        if (node == null) return -1;

        if (node == target) {
            CollectNodesAtDistance(node, K);
            return 0;
        }
        
        int left = FindDistance(node.left, target, K);
        if (left != -1) {
            if (left + 1 == K) result.Add(node.val);
            CollectNodesAtDistance(node.right, K - left - 2);
            return left + 1;
        }
        
        int right = FindDistance(node.right, target, K);
        if (right != -1) {
            if (right + 1 == K) result.Add(node.val);
            CollectNodesAtDistance(node.left, K - right - 2);
            return right + 1;
        }
        
        return -1;
    }
    
    private void CollectNodesAtDistance(TreeNode node, int K) {
        if (node == null || K < 0) return;
        if (K == 0) {
            result.Add(node.val);
            return;
        }
        CollectNodesAtDistance(node.left, K - 1);
        CollectNodesAtDistance(node.right, K - 1);
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(N) where N is the number of nodes in the tree. Each node is visited once during the DFS.
- **Space Complexity**: O(H) where H is the height of the tree, needed for recursion stack.

