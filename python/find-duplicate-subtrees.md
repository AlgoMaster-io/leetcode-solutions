# [LeetCode 652: Find Duplicate Subtrees](https://leetcode.com/problems/find-duplicate-subtrees/)

### Approaches
1. [Basic DFS Traversal with a Dictionary](#basic-dfs-traversal-with-a-dictionary)
2. [Optimized Serialization with Memoization](#optimized-serialization-with-memoization)

---

## Basic DFS Traversal with a Dictionary

### Intuition
The problem essentially requires us to identify and return the roots of subtrees that appear more than once in a given binary tree. The key observation here is that each subtree can be uniquely identified by its structure and node values. A naive approach involves traversing every node and serializing the subtree rooted at that node. By using a dictionary to keep track of serialized subtrees and their occurrence count, we can theoretically identify duplicates.

### Approach
1. Traverse the tree using a depth-first search (DFS).
2. For each node, serialize the subtree rooted at that node.
3. Use a dictionary to keep track of each serialized subtree and the number of times it has been encountered.
4. If the occurrence of a serialized subtree exceeds one, record the root of that subtree.

### Code
```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

def findDuplicateSubtrees(root):
    from collections import defaultdict
    def serialize(node):
        if not node:
            return "#"
        serial = f"{node.val},{serialize(node.left)},{serialize(node.right)}"
        subtree_map[serial].append(node)
        return serial

    subtree_map = defaultdict(list)
    serialize(root)
    # Only return the root of those subtrees that appear more than once
    return [nodes[0] for nodes in subtree_map.values() if len(nodes) > 1]

# Complexity: 
# Time: O(n^2) - As serializing each subtree could potentially take O(n) time and we do this n times.
# Space: O(n^2) - In the worst case, n distinct serializations each taking O(n) space.
```

---

## Optimized Serialization with Memoization

### Intuition
The above method can be inefficient due to excessive recursion and re-calculations. We can optimize by using memoization techniques, reducing the overhead of serializing the same subtree multiple times. Instead of serializing the tree as a string, we can assign a unique ID to each unique serialization detected, further speeding up comparisons by using integers.

### Approach
1. Use a post-order traversal to ensure we fully construct a subtree before dealing with its parent.
2. Utilize a dictionary for memoization, storing unique subtree serializations and mapping them to an ID.
3. Use a second dictionary to track the IDs of the subtrees and count their occurrences.
4. When a duplicate subtree serialization is encountered, add its root node to the results. 

### Code
```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

def findDuplicateSubtrees(root):
    def serialize(node):
        if not node:
            return 0
        # Construct a serialization using a tuple.
        serial = (node.val, serialize(node.left), serialize(node.right))
        if serial not in memo:
            memo[serial] = len(memo) + 1
        serial_id = memo[serial]
        
        count[serial_id] += 1
        if count[serial_id] == 2:
            result.append(node)
        
        return serial_id

    memo = {}
    count = defaultdict(int)
    result = []
    serialize(root)
    return result

# Complexity:
# Time: O(n) - Each node is visited once resulting in linear time complexity.
# Space: O(n) - The space for memoization storage which depends on the number of unique subtrees.
```

This solution efficiently handles storing and looking up subtrees through memoization, minimizing redundant work and improving both time and space complexity relative to the basic approach.

