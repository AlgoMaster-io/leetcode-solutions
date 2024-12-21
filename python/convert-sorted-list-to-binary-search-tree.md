# [Convert Sorted List to Binary Search Tree - LeetCode](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approaches
1. [Brute Force: Convert List to Array](#approach-1-brute-force-convert-list-to-array)
2. [Optimal Approach: In-order Simulation and Conversion](#approach-2-optimal-approach-in-order-simulation-and-conversion)

---

## Approach 1: Brute Force: Convert List to Array

### Intuition
The brute force approach involves converting the sorted linked list into an array. Since the list is sorted in ascending order, we can directly apply the "middle element as root" strategy to construct a balanced BST. To maintain the properties of a BST, recursively choose the middle element of the subarray as the root of the subtree. 

### Implementation
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def sortedListToBST(head: ListNode) -> TreeNode:
    # Convert linked list to array
    nums = []
    while head:
        nums.append(head.val)
        head = head.next
    
    # Helper function to convert array to BST
    def sortedArrayToBST(left, right):
        if left > right:
            return None
        
        mid = (left + right) // 2
        node = TreeNode(nums[mid])
        
        if left == right:
            return node

        node.left = sortedArrayToBST(left, mid - 1)
        node.right = sortedArrayToBST(mid + 1, right)
        
        return node
    
    return sortedArrayToBST(0, len(nums) - 1)
```

### Time and Space Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the linked list. We traverse the linked list to convert it into an array, then traverse the array to construct the BST.
- **Space Complexity**: O(N), due to the space taken by the array.

---

## Approach 2: Optimal Approach: In-order Simulation and Conversion

### Intuition
To optimize the previous approach, we can simulate in-order traversal by manipulating pointers on the linked list. The idea is to build the left subtree, use the current node of the linked list as the root, and then move to the right subtree. This approach avoids the use of extra space for an array and constructs the BST in-place as we traverse the list.

### Implementation
```python
def sortedListToBST(head: ListNode) -> TreeNode:
    def findMiddle(left, right):
        slow = fast = left
        # Use the fast and slow pointer technique to find the middle of the linked list
        while fast != right and fast.next != right:
            slow = slow.next
            fast = fast.next.next
        return slow

    # Helper recursive function to build the BST
    def convertListToBST(left, right):
        if left == right:
            return None
        
        mid = findMiddle(left, right)
        node = TreeNode(mid.val)

        # Recursively build the left and right subtrees
        node.left = convertListToBST(left, mid)
        node.right = convertListToBST(mid.next, right)
        
        return node
    
    return convertListToBST(head, None)
```

### Time and Space Complexity Analysis
- **Time Complexity**: O(N log N) in the average case, because locating the middle node takes O(N) and building each level of the tree takes log N levels.
- **Space Complexity**: O(log N). The recursion stack carries the height of the tree which is log N for a balanced BST.

---

By carefully choosing the middle element at every stage and using a divide-and-conquer approach, we efficiently balance the BST construction. This solution is optimal in terms of space utilization as it avoids extraneous data structures.

