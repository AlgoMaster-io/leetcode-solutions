[Leetcode 230: Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst)

Approaches:
- [Approach 1: Inorder Traversal with Recursion](#approach-1-inorder-traversal-with-recursion)
- [Approach 2: Inorder Traversal with Iteration](#approach-2-inorder-traversal-with-iteration)
- [Approach 3: Optimized Inorder Traversal with Early Stopping](#approach-3-optimized-inorder-traversal-with-early-stopping)

### Approach 1: Inorder Traversal with Recursion

In a Binary Search Tree (BST), an inorder traversal visits elements in sorted order. Therefore, to find the k-th smallest element, one straightforward approach is to perform an inorder traversal and collect the elements until the k-th one is reached.

#### Intuition
- Perform an inorder traversal which inherently sorts the elements.
- Count the elements while traversing.
- When the count reaches k, return the current node's value.

#### Time Complexity
- **O(N)** where N is the number of nodes in the BST. In the worst case, we might traverse all nodes.

#### Space Complexity
- **O(N)** for the recursive call stack in the worst case.

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.left = (left===undefined ? null : left);
        this.right = (right===undefined ? null : right);
    }
}

function kthSmallest(root: TreeNode | null, k: number): number {
    function inorderTraversal(node: TreeNode | null, elements: number[]): void {
        if (node === null) return;
        inorderTraversal(node.left, elements);
        elements.push(node.val);
        inorderTraversal(node.right, elements);
    }

    const elements: number[] = [];
    inorderTraversal(root, elements);
    return elements[k - 1];
}
```

### Approach 2: Inorder Traversal with Iteration

Instead of using recursion, use an explicit stack to perform the inorder traversal iteratively. This avoids issues with recursion stack overflow in very large trees.

#### Intuition
- Use a stack to simulate the recursive call stack of the inorder traversal.
- Push all left nodes to the stack until a node has no left child.
- Pop from the stack and visit nodes, counting until the k-th pop.

#### Time Complexity
- **O(N)** where N is the number of nodes in the BST.

#### Space Complexity
- **O(H)** where H is the height of the tree. This corresponds to the stack size.

```typescript
function kthSmallest(root: TreeNode | null, k: number): number {
    const stack: TreeNode[] = [];
    let currentNode = root;
    
    while (currentNode !== null || stack.length > 0) {
        // Go to the leftmost node
        while (currentNode !== null) {
            stack.push(currentNode);
            currentNode = currentNode.left;
        }
        
        // Process the node
        currentNode = stack.pop();
        k--;
        if (k === 0) return currentNode!.val;
        
        // Explore right subtree
        currentNode = currentNode.right;
    }
    
    return -1; // This line should theoretically never be reached.
}
```

### Approach 3: Optimized Inorder Traversal with Early Stopping

Optimize further by stopping the traversal as soon as the k-th smallest element is found to avoid unnecessary traversal of parts of the tree.

#### Intuition
- Traverse the tree in inorder fashion.
- Maintain a counter that tracks the number of nodes visited.
- Stop the traversal as soon as the counter reaches k.

#### Time Complexity
- **O(H + k)** where H is the height of the tree and k is the position of the kth smallest element.

#### Space Complexity
- **O(H)** due to the recursion stack.

```typescript
class Counter {
    count: number = 0;
    result: number = -1;
}

function kthSmallest(root: TreeNode | null, k: number): number {
    const counter = new Counter();

    function inorderTraversal(node: TreeNode | null, counter: Counter): void {
        if (node === null || counter.count >= k) return;

        inorderTraversal(node.left, counter);
        counter.count++;
        if (counter.count === k) {
            counter.result = node.val;
            return;
        }
        inorderTraversal(node.right, counter);
    }

    inorderTraversal(root, counter);
    return counter.result;
}
```

Each approach progressively optimizes either the space or the traversal time by terminating early as soon as the requisite amount of information is obtained. Choose the best approach based on the constraints and expected tree size.

