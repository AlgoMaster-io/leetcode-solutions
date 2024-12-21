# [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

## Approaches
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Iterative Using a Stack](#approach-2-iterative-using-a-stack)

## Approach 1: Recursive Approach

### Intuition
The problem can be solved recursively by selecting the greatest value in an array to be the root, and then splitting the array into left and right subtrees that also need to be processed. The left subtree is the elements to the left of the maximum value, and the right subtree is the elements to the right.

### Algorithm
1. Find the maximum element in the current array to form the root node.
2. Recursively use the subarray left of the maximum element to construct the left subtree.
3. Recursively use the subarray right of the maximum element to construct the right subtree.
4. Return the constructed tree.

### Code
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

function constructMaximumBinaryTree(nums: number[]): TreeNode | null {
    if (nums.length === 0) return null;

    // find the root value and its index which is maximum in 'nums'
    let maxIndex = 0;
    for (let i = 1; i < nums.length; i++) {
        if (nums[i] > nums[maxIndex]) {
            maxIndex = i;
        }
    }
    
    const root = new TreeNode(nums[maxIndex]);
    
    // recursively construct the left and right subtrees
    root.left = constructMaximumBinaryTree(nums.slice(0, maxIndex));
    root.right = constructMaximumBinaryTree(nums.slice(maxIndex + 1));
    
    return root;
}
```

### Time Complexity
- Finding the maximum value takes O(n) and splitting the array can make this worst-case O(n^2) for a sorted (or inversely sorted) array.
- Space Complexity: O(n) due to the recursion stack.

## Approach 2: Iterative Using a Stack

### Intuition
Instead of constructing the binary tree recursively, use a monotonic stack structure that keeps track of the current state of the tree in construction. Insert each element and modify the tree as necessary to maintain the conditions of the maximum binary tree.

### Algorithm
1. Initialize an empty stack that will hold `TreeNodes`.
2. Traverse the array. For each number:
    - Create a new `TreeNode` with it.
    - Pop nodes from the stack while they are smaller than the current number. These become the left children of the current number.
    - The right child of the last popped node becomes the left child of the new node.
    - Push the new node onto the stack.
3. The root of the maximum binary tree is the bottom of the stack.

### Code
```typescript
function constructMaximumBinaryTree(nums: number[]): TreeNode | null {
    const stack: TreeNode[] = [];

    for (const num of nums) {
        // Create a new TreeNode for the current number
        const current = new TreeNode(num);
        
        // Ensure that the nodes in the stack are less than the current node
        while (stack.length > 0 && stack[stack.length - 1].val < num) {
            // Pop the node as it should be the left child of the current node
            current.left = stack.pop()!;
        }
        
        // If stack is non-empty, attach current as right child to top of stack
        if (stack.length > 0) {
            stack[stack.length - 1].right = current;
        }
        
        // Push the current TreeNode onto the stack
        stack.push(current);
    }

    // Stack base contains the largest root of the entire tree
    return stack[0];
}
```

### Time Complexity
- Each number is pushed and popped exactly once, making this O(n).
- Space Complexity: O(n) due to the stack holding nodes.

The stack-based approach is more efficient and avoids deep recursion, making it suitable for larger inputs without hitting limits for stack overflow.

