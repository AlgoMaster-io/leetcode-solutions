# [109. Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approach 1: Recursion with Slow and Fast Pointers

### Solution
```python
# Time Complexity: O(n log n), where n is the length of the list
# Space Complexity: O(log n) (due to recursion stack)
class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        if not head:
            return None  # Base case: empty list
        if not head.next:
            return TreeNode(head.val)  # Single node

        # Find the middle of the list using slow and fast pointers
        prev, slow, fast = None, head, head
        while fast and fast.next:
            prev = slow
            slow = slow.next
            fast = fast.next.next

        # Disconnect the left half of the list
        if prev:
            prev.next = None

        # Create the root node
        root = TreeNode(slow.val)

        # Recursively build the left and right subtrees
        root.left = self.sortedListToBST(head)
        root.right = self.sortedListToBST(slow.next)

        return root
```

## Approach 2: In-Order Simulation with List Conversion

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        # Convert the linked list to an array for easier access
        values = []
        while head:
            values.append(head.val)
            head = head.next

        # Recursively construct the BST
        return self.buildBST(values, 0, len(values) - 1)

    def buildBST(self, values: List[int], left: int, right: int) -> TreeNode:
        if left > right:
            return None  # Base case

        mid = left + (right - left) // 2  # Find the middle element
        root = TreeNode(values[mid])  # Create the root node

        # Recursively build left and right subtrees
        root.left = self.buildBST(values, left, mid - 1)
        root.right = self.buildBST(values, mid + 1, right)

        return root
```

