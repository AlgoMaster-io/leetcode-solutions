# [Leetcode 100: Same Tree](https://leetcode.com/problems/same-tree/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach using Queue](#iterative-approach-using-queue)
3. [Iterative Approach using Stack](#iterative-approach-using-stack)

### Recursive Approach

This problem can be efficiently solved using recursion. The primary intuition here is to simultaneously traverse both trees, `p` and `q`, and compare them node by node.

#### Intuition:
- If both nodes are `null`, they are equal (base case).
- If only one of the nodes is `null`, the trees are not the same.
- If values of both nodes are different, the trees are not the same.
- Recursively apply the same logic to both left and right subtrees.

#### Time Complexity:
- **O(n)** where `n` is the number of nodes in the tree, because we visit each node exactly once.

#### Space Complexity:
- **O(h)** where `h` is the height of the tree, representing the space occupied by the recursion stack.

```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        // Base case: if both nodes are nullptr, they are the same
        if (p == nullptr && q == nullptr) return true;
        // If one node is nullptr and the other isn't, they aren't the same
        if (p == nullptr || q == nullptr) return false;
        // Both nodes are not nullptr, so check their values and recurse on children
        // Check if current nodes have same value and recursively check for left and right subtrees
        return (p->val == q->val) && 
               isSameTree(p->left, q->left) && 
               isSameTree(p->right, q->right);
    }
};
```

### Iterative Approach using Queue

Another method to determine if two trees are the same is to use an iterative approach employing a queue for level-order traversal.

#### Intuition:
- Use a queue to hold pairs of nodes from both trees.
- Each step, dequeue a pair and compare them.
- Repeat this for both children: if not `null`, enqueue them as paired nodes.
- Continue while the queue is not empty.

#### Time Complexity:
- **O(n)** as we need to visit all `n` nodes of the tree.

#### Space Complexity:
- **O(n)** in the worst case when the tree is a complete binary tree.

```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        // Use a queue to store pairs of nodes from both trees
        queue<pair<TreeNode*, TreeNode*>> nodeQueue;
        nodeQueue.push({p, q});
        
        while (!nodeQueue.empty()) {
            auto [node1, node2] = nodeQueue.front();
            nodeQueue.pop();
            
            // If both nodes are null, continue to the next iteration
            if (!node1 && !node2) continue;
            // If one node is null or they have different values, trees are not the same
            if (!node1 || !node2 || node1->val != node2->val) return false;
            
            // Add child nodes to the queue as pairs
            nodeQueue.push({node1->left, node2->left});
            nodeQueue.push({node1->right, node2->right});
        }
        
        return true; // If we finish without finding differences, they're the same
    }
};
```

### Iterative Approach using Stack

This approach is similar to the queue-based method but uses a stack for depth-first traversal.

#### Intuition:
- Use a stack to store pairs of nodes from both trees for DFS traversal.
- Continuously pop pairs from the stack and compare them.
- Push their children nodes back on the stack when they match.

#### Time Complexity:
- **O(n)** as all nodes are eventually visited.

#### Space Complexity:
- **O(h)** where h is the max depth of the binary tree, due to the stack.

```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        // Stack for DFS containing pairs of nodes from both trees
        stack<pair<TreeNode*, TreeNode*>> nodeStack;
        nodeStack.push({p, q});
        
        while (!nodeStack.empty()) {
            auto [node1, node2] = nodeStack.top();
            nodeStack.pop();
            
            // If both nodes are null, continue
            if (!node1 && !node2) continue;
            // If one is null or values differ, they're not the same
            if (!node1 || !node2 || node1->val != node2->val) return false;
            
            // Push children nodes as pairs onto the stack
            nodeStack.push({node1->left, node2->left});
            nodeStack.push({node1->right, node2->right});
        }
        
        return true; // If everything matched, they are the same
    }
};
```

