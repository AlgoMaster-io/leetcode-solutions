# Binary Tree Paths
Given the root of a binary tree, return all root-to-leaf paths in string format. A leaf is a node with no children.

### Constraints:
- The number of nodes in the tree is in the range [1, 100].
- -100 <= Node.val <= 100

### Examples
```javascript
Input: root = [1,2,3,null,5]
Output: ["1->2->5", "1->3"]

Explanation: The root-to-leaf path 1->2->5 and 1->3 are returned.

Input: root = [1]
Output: ["1"]

Explanation: There is only one path from the root to the leaf, which is the root itself.
```

## Approaches to Solve the Problem
### Approach 1: Depth-First Search (DFS) - Recursive (Optimal Solution)
##### Intuition:
The problem can be solved using depth-first search (DFS) by traversing each path from the root to every leaf node. The core idea is to build a path string while traversing the tree. When a leaf node is reached (i.e., a node with no left and right children), the current path is added to the result list.

- We can recursively traverse the tree, keeping track of the path from the root to the current node.
- Whenever we reach a leaf node, the current path is considered complete, and we add it to the result.

Steps:
1. Define a helper function dfs(node, path) that takes the current node and the path built so far as input.
2. If the current node is a leaf, add the path to the result.
3. If the current node is not a leaf, recursively call the function for its left and right children, appending the node's value to the path.
4. Return the result list after traversing all paths.
##### Visualization:
For root = [1, 2, 3, null, 5], the DFS would traverse like this:


```rust
Start at 1:
- Path: "1"
  Go left to 2:
  - Path: "1->2"
    Go right to 5:
    - Path: "1->2->5" (leaf node, add to result)
  Back to 1, go right to 3:
  - Path: "1->3" (leaf node, add to result)

Final result: ["1->2->5", "1->3"]
```
##### Time Complexity:
O(n), where n is the number of nodes in the tree. We visit every node exactly once.
##### Space Complexity:
O(n), where n is the number of nodes, due to the recursion stack and the result list.
##### Python Code:
```python
# Definition for a binary tree node.
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def binaryTreePaths(root):
    def dfs(node, path):
        if not node:
            return
        
        # Append current node to the path
        path += str(node.val)
        
        # If it's a leaf node, add the path to the result
        if not node.left and not node.right:
            paths.append(path)
        else:
            # If not a leaf, extend the path with "->" and explore children
            path += "->"
            dfs(node.left, path)
            dfs(node.right, path)
    
    paths = []
    dfs(root, "")
    return paths
```
### Approach 2: Iterative DFS with Stack
##### Intuition:
We can also solve the problem using iterative depth-first search (DFS) with the help of a stack. The stack will hold pairs of the current node and the path built so far.

- Push the root node along with an empty path to the stack.
- Process the nodes in a depth-first manner by popping them from the stack.
- If the node is a leaf, add the current path to the result list.
- If the node is not a leaf, push its children to the stack along with the updated path.

Steps:
1. Initialize a stack with the root node and an empty path string.
2. While the stack is not empty, pop the top element (a node and its path).
3. If the node is a leaf, add the path to the result list.
4. If the node has children, push them onto the stack along with the updated path.
##### Visualization:
For root = [1, 2, 3, null, 5], the stack would operate as follows:

```rust
Stack: [(1, "1")]
- Pop 1, push (2, "1->2") and (3, "1->3")
- Pop 3, it's a leaf → Add "1->3" to result
- Pop 2, push (5, "1->2->5")
- Pop 5, it's a leaf → Add "1->2->5" to result
Final result: ["1->2->5", "1->3"]
```
##### Time Complexity:
O(n), where n is the number of nodes. We visit each node exactly once.
##### Space Complexity:
O(n), for the stack and the result list.
##### Python Code:
```python
def binaryTreePaths(root):
    if not root:
        return []
    
    stack = [(root, str(root.val))]
    paths = []
    
    while stack:
        node, path = stack.pop()
        
        # If it's a leaf node, add the path to result
        if not node.left and not node.right:
            paths.append(path)
        # If there are children, push them to the stack with updated path
        if node.right:
            stack.append((node.right, path + "->" + str(node.right.val)))
        if node.left:
            stack.append((node.left, path + "->" + str(node.left.val)))
    
    return paths
```
### Approach 3: Breadth-First Search (BFS) with Queue
##### Intuition:
Alternatively, we can solve this problem using breadth-first search (BFS) by using a queue. We start with the root node and explore all levels of the tree. For each node, we track the path from the root to that node.

- Use a queue to hold pairs of the current node and its path.
- For each node, if it is a leaf, append the path to the result.
- If the node has children, enqueue them with the updated path.

Steps:
1. Initialize a queue with the root node and an empty path string.
2. While the queue is not empty, dequeue the front element (a node and its path).
3. If the node is a leaf, add the path to the result list.
4. If the node has children, enqueue them with the updated path.
##### Time Complexity:
O(n), where n is the number of nodes. Each node is processed once.
##### Space Complexity:
O(n), for the queue and result list.
##### Python Code:
```python
from collections import deque

def binaryTreePaths(root):
    if not root:
        return []
    
    queue = deque([(root, str(root.val))])
    paths = []
    
    while queue:
        node, path = queue.popleft()
        
        # If it's a leaf node, add the path to result
        if not node.left and not node.right:
            paths.append(path)
        
        # Enqueue children with the updated path
        if node.left:
            queue.append((node.left, path + "->" + str(node.left.val)))
        if node.right:
            queue.append((node.right, path + "->" + str(node.right.val)))
    
    return paths
```
### Edge Cases
1. Single Node Tree: If the tree consists of only one node (the root), the result should be a single path that contains only the root's value.
2. Null Root: If the input tree is empty (root == None), the result should be an empty list ([]).
3. Deep Tree: If the tree is very deep (with many levels), the solution should handle the recursion/iteration efficiently.
### Summary
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Recursive DFS			      | O(n)      | O(n)             |
| Iterative DFS (Stack)			      | O(n)      | O(n)             |
| BFS (Queue)				      | O(n)      | O(n)             |

The Recursive DFS approach is intuitive and straightforward, while the Iterative DFS and BFS approaches are iterative solutions that avoid recursion, making them suitable for larger trees where recursion depth might be a concern.