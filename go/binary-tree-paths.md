## [Leetcode 257: Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

### Approaches
1. [Recursive DFS with String Concatenation](#approach-1-recursive-dfs-with-string-concatenation)
2. [Recursive DFS with StringBuilder](#approach-2-recursive-dfs-with-stringbuilder)

---

### Approach 1: Recursive DFS with String Concatenation

**Intuition**:
- This approach uses a simple depth-first search (DFS) to explore each path from the root to the leaf nodes.
- For each node, we construct the path as a string, appending each node's value as we traverse.
- When we reach a leaf node, we add the completed path to our result list.

**Steps**:
1. Use a helper function to do a DFS traversal.
2. At each node, append the node's value to the current path string.
3. If it's a leaf node, add the path string to the result list.
4. Recursively visit the left and right children, passing the updated path string.
5. Return the list of paths once the entire tree is traversed.

**Time Complexity**: O(N^2) 
- In the worst case, every node is a leaf which means each node's path requires time proportional to its depth, resulting in a path copying that can be up to O(N).

**Space Complexity**: O(N)
- Due to recursive stack space of the depth of the tree and storage of paths.

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func binaryTreePaths(root *TreeNode) []string {
    var paths []string
    if root == nil {
        return paths
    }
    dfs(root, "", &paths)
    return paths
}

func dfs(node *TreeNode, path string, paths *[]string) {
    if node.Left == nil && node.Right == nil {
        *paths = append(*paths, path+strconv.Itoa(node.Val))
    }
    if node.Left != nil {
        dfs(node.Left, path+strconv.Itoa(node.Val)+"->", paths)
    }
    if node.Right != nil {
        dfs(node.Right, path+strconv.Itoa(node.Val)+"->", paths)
    }
}
```

---

### Approach 2: Recursive DFS with StringBuilder

**Intuition**:
- Similar to the first approach, but instead of using string concatenation (which is costly in terms of time complexity), use a string builder approach.
- This can reduce time complexity when handling larger strings by building them incrementally without repeatedly copying.

**Steps**:
1. Use a DFS strategy with more optimal string handling.
2. Start from the root, and explore each node, appending to a string builder-like structure.
3. On reaching a leaf, convert the builder's content to a string and append to the results.
4. Clone the current builder state before recursive calls to prep for backtracking.

**Time Complexity**: O(N)
- More efficient due to less repeated string copying.

**Space Complexity**: O(N)
- Still maintains recursion stack depth as O(N).

```go
func binaryTreePaths(root *TreeNode) []string {
    var paths []string
    if root == nil {
        return paths
    }
    var path []string
    dfsOptimized(root, path, &paths)
    return paths
}

func dfsOptimized(node *TreeNode, path []string, paths *[]string) {
    path = append(path, strconv.Itoa(node.Val))
    
    if node.Left == nil && node.Right == nil {
        *paths = append(*paths, strings.Join(path, "->"))
    }
    if node.Left != nil {
        dfsOptimized(node.Left, path, paths)
    }
    if node.Right != nil {
        dfsOptimized(node.Right, path, paths)
    }
    path = path[:len(path)-1]  // Backtrack
}
```

---

By using these approaches, you can effectively solve the "Binary Tree Paths" problem with different levels of optimization on string operations, suitable for interview scenarios or when dealing with performance concerns.

