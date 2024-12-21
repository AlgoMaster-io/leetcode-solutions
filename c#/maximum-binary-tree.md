# [Leetcode 654: Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

## Table of Contents
- [Approach 1: Recursive Divide and Conquer](#approach-1-recursive-divide-and-conquer)
- [Approach 2: Iterative with Stack](#approach-2-iterative-with-stack)

---

## Approach 1: Recursive Divide and Conquer

### Intuition

The problem requires us to construct a binary tree by selecting the maximum element as the root and recursively applying the same logic to the left and right subarrays formed after splitting at that maximum element.

- **Step 1:** Find the maximum element in the array. This will be the root of the binary tree.
- **Step 2:** Recursively construct the left subtree using the subarray elements before the maximum element.
- **Step 3:** Recursively construct the right subtree using the subarray elements after the maximum element.

### Solution

```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public TreeNode ConstructMaximumBinaryTree(int[] nums) {
        return BuildTree(nums, 0, nums.Length - 1);
    }
    
    private TreeNode BuildTree(int[] nums, int left, int right) {
        // Base case: if no elements in the subarray, return null
        if (left > right) return null;

        // Find the index of the maximum element in the current subarray
        int maxIndex = left;
        for (int i = left; i <= right; i++) {
            if (nums[i] > nums[maxIndex]) {
                maxIndex = i;
            }
        }

        // Create the root node with the maximum element
        TreeNode root = new TreeNode(nums[maxIndex]);
        
        // Recursively construct the left and right subtrees
        root.left = BuildTree(nums, left, maxIndex - 1);
        root.right = BuildTree(nums, maxIndex + 1, right);

        return root;
    }
}
```

### Time Complexity

- **Time Complexity:** \(O(n^2)\) in the worst case where the array is sorted in descending order.
- **Space Complexity:** \(O(n)\) for the recursion stack, in the worst case when the tree is skewed.

---

## Approach 2: Iterative with Stack

### Intuition

We can optimize the recursive solution using a stack. The idea is to construct the tree such that each number in the array only has the previously seen greatest elements as its ancestors.

- **Step 1:** Use a stack to keep track of the TreeNodes.
- **Step 2:** For each element, create a TreeNode. If it is greater than the node at the top of the stack, pop nodes and assign them as left children of the new node.
- **Step 3:** The last node in the stack (after processing all elements) will be the root.

### Solution

```csharp
public class Solution {
    public TreeNode ConstructMaximumBinaryTree(int[] nums) {
        Stack<TreeNode> stack = new Stack<TreeNode>();
        
        foreach (int num in nums) {
            TreeNode currentNode = new TreeNode(num);
            while (stack.Count > 0 && stack.Peek().val < num) {
                // Pop smaller elements and link them as left children
                currentNode.left = stack.Pop();
            }
            if (stack.Count > 0) {
                // The element in the stack will be the parent of the current node
                stack.Peek().right = currentNode;
            }
            stack.Push(currentNode);
        }
        
        // The last element in the stack is the root of the tree
        return stack.First();
    }
}
```

### Time Complexity

- **Time Complexity:** \(O(n)\) as each element is pushed and popped from the stack at most once.
- **Space Complexity:** \(O(n)\) for the stack used to keep track of TreeNodes.

---

These approaches ensure a clear understanding of problem-solving by combining recursive and iterative paradigms. The recursive approach is straightforward and aligns closely with the problem's description, whereas the iterative approach leverages data structures to optimize performance.

