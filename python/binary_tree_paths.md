# 257. [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

## Approach 1: Recursive Depth-First Search (DFS)

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for recursion stack

class Solution:
    def binaryTreePaths(self, root):
        def dfs(node, path, result):
            # Append the current node's value to the path
            path += str(node.val)
            
            # If the current node is a leaf, add the path to the result
            if not node.left and not node.right:
                result.append(path)
                return
            
            # If not a leaf, continue the path with "->" and recurse
            if node.left:
                dfs(node.left, path + "->", result)
            if node.right:
                dfs(node.right, path + "->", result)

        result = []
        if root:
            dfs(root, "", result)
        return result
```

## Approach 2: Iterative Depth-First Search (Using Stack)

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(n), for the stack and path storage

class Solution:
    def binaryTreePaths(self, root):
        if not root:
            return []  # Return empty result if the tree is empty

        result = []
        stack = [(root, str(root.val))]
        
        while stack:
            currentNode, currentPath = stack.pop()
            
            # If the current node is a leaf, add the path to the result
            if not currentNode.left and not currentNode.right:
                result.append(currentPath)
            
            # If not a leaf, push children to the stack with updated paths
            if currentNode.right:
                stack.append((currentNode.right, currentPath + "->" + str(currentNode.right.val)))
            if currentNode.left:
                stack.append((currentNode.left, currentPath + "->" + str(currentNode.left.val)))
        
        return result
```

## Approach 3: Backtracking

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for recursion stack

class Solution:
    def binaryTreePaths(self, root):
        def backtrack(node, path, result):
            length = len(path)
            path.append(str(node.val))

            # If the current node is a leaf, add the path to the result
            if not node.left and not node.right:
                result.append("->".join(path))
            else:
                # If not a leaf, continue the path
                path.append("->")
                if node.left:
                    backtrack(node.left, path, result)
                if node.right:
                    backtrack(node.right, path, result)
            
            # Backtrack to remove the current node's value and "->"
            path[length:] = []

        result = []
        if root:
            backtrack(root, [], result)
        return result
```

