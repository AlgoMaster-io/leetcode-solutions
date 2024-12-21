# [Leetcode 654: Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

## Approaches
- [Approach 1: Recursive Construction](#approach-1)
- [Approach 2: Iterative Approach Using Stack](#approach-2)

### Approach 1: Recursive Construction

#### Intuition
The problem involves constructing a binary tree based on the maximum value within subarrays of a given list. The direct approach is to leverage recursion:
1. Find the maximum element in the current array or subarray as the root.
2. Divide the array into left and right subarrays relative to this maximum element.
3. Recursively repeat the same process for both left and right subarrays to construct the complete binary tree.

This is a clear top-down approach using recursion.

#### Steps
1. Create a helper function that takes the list and indices representing the current subarray.
2. Within the helper, identify the maximum value and its index within the subarray.
3. Create a new tree node with the maximum value.
4. Recursively construct the left and right subtrees using the subarray to the left and right of the maximum element respectively.
5. Return the constructed tree node.

#### Code
```javascript
function TreeNode(val) {
    this.val = val;
    this.left = this.right = null;
}

var constructMaximumBinaryTree = function(nums) {
    // Recursive helper function to construct the tree
    function construct(nums, left, right) {
        if (left > right) return null;

        // Find the index of the maximum value
        let maxIdx = left;
        for (let i = left + 1; i <= right; i++) {
            if (nums[i] > nums[maxIdx]) {
                maxIdx = i;
            }
        }

        // Create a new node with the maximum value
        let node = new TreeNode(nums[maxIdx]);

        // Recursively build the left and right subtrees
        node.left = construct(nums, left, maxIdx - 1);
        node.right = construct(nums, maxIdx + 1, right);

        return node;
    }

    // Start construction with the full array
    return construct(nums, 0, nums.length - 1);
};
```

##### Complexity
- **Time Complexity**: O(n^2) in the worst case, where `n` is the number of elements in `nums`. This happens when the array is sorted in either ascending or descending order as we traverse nearly the entire array for each node.
- **Space Complexity**: O(n) due to the recursive stack depth in the worst case when the array is sorted.

### Approach 2: Iterative Approach Using Stack

#### Intuition
An alternative method involves an iterative approach using a stack to manage trees:
1. Traverse the list of numbers.
2. For each number, create a node.
3. Maintain a stack to hold nodes of the current potential tree.
4. If the current number is greater than elements in the stack, pop the stack to define the new relationships and insert this node.
5. The stack will aid in maintaining order and managing node relationships effectively.

This approach mimics the recursive nature but in an iterative manner using a stack.

#### Steps
1. Initialize an empty stack.
2. Traverse the array of numbers:
   - For each number, create a TreeNode.
   - Pop nodes from the stack until the top of the stack is greater than the current number.
   - The last popped node becomes the left child of the current node (reflecting the maximum).
   - The current node becomes the right child of the top node in the stack (if there's one).
3. Push the current node onto the stack.
4. The root of the maximum binary tree is the bottom-most element of the stack.

#### Code
```javascript
function TreeNode(val) {
    this.val = val;
    this.left = this.right = null;
}

var constructMaximumBinaryTree = function(nums) {
    const stack = [];
    
    for (let i = 0; i < nums.length; i++) {
        let current = new TreeNode(nums[i]);

        // Linking as the right child for all smaller nodes
        while (stack.length && stack[stack.length - 1].val < nums[i]) {
            current.left = stack.pop();
        }
        
        // Linking current to be the right child of the previous larger node
        if (stack.length) {
            stack[stack.length - 1].right = current;
        }
        
        // Push the current node to the stack
        stack.push(current);
    }
    
    // The bottom-most element will be the root of the maximum binary tree
    return stack[0];
};
```

##### Complexity
- **Time Complexity**: O(n), as each element is pushed and popped from the stack at most once.
- **Space Complexity**: O(n), to hold the stack of tree nodes.

By using the stack approach, the tree is constructed in an iterative manner, potentially being more efficient in terms of stack space and execution for sorted arrays.

