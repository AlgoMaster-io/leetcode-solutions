# [Leetcode 109: Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approaches:

1. [Naive Approach: Convert List to Array](#naive-approach-convert-list-to-array)
2. [Optimized Approach: Inorder Simulation](#optimized-approach-inorder-simulation)

## Naive Approach: Convert List to Array

### Intuition:

The simplest way to address this problem is to transform the sorted linked list into an array. Once in array form, the middle element of any subarray serves as the root for the Binary Search Tree (BST). By recursively applying this process to the left and right subarrays, we build the complete tree. 

This method is straightforward as it leverages the property of arrays that allow O(1) element access. However, this comes with a storage cost since we need additional space for the array.

### Steps:

1. Convert the linked list into an array.
2. Recursively build BST from the array’s middle elements:
   - Choose the middle of the current array segment as the root.
   - Recursively create left and right subtrees using the subarrays to the left and right of the middle element.
   
### Code:

```java
public class Solution {
    
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) return null;
        
        // Step 1: Convert the Linked List to an Array
        List<Integer> values = new ArrayList<>();
        ListNode current = head;
        while (current != null) {
            values.add(current.val);
            current = current.next;
        }
        
        // Step 2: Build BST recursively from the array
        return buildBSTFromArray(values, 0, values.size() - 1);
    }
    
    private TreeNode buildBSTFromArray(List<Integer> values, int left, int right) {
        if (left > right) return null;
        
        // Middle element becomes the root
        int mid = left + (right - left) / 2;
        TreeNode node = new TreeNode(values.get(mid));
        
        // Recursively construct the left and right subtrees
        node.left = buildBSTFromArray(values, left, mid - 1);
        node.right = buildBSTFromArray(values, mid + 1, right);
        
        return node;
    }
}
```

### Complexity Analysis:

- **Time Complexity**: O(N), where N is the number of elements in the list. Converting the list to an array takes O(N), and creating the BST takes O(N) as each element is processed once.
- **Space Complexity**: O(N), due to the additional space required to store the array.

## Optimized Approach: Inorder Simulation

### Intuition:

Instead of first converting the linked list to an array, we can directly use the properties of the list and simulate an inorder traversal. By using the fast and slow pointer technique, we can find the middle element of the linked list and use it as the root. This method eliminates the need for extra space associated with array storage.

### Steps:

1. Use a helper function to recursively find and set the middle element of the list as the root.
2. Use two pointers (fast and slow) to find the middle:
   - Move `fast` two steps and `slow` one step to maintain the alignment needed to find the middle.
3. Recursively apply the same process to the left and right sublists.

### Code:

```java
public class Solution {
    
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) return null;
        
        return convertToBST(head, null);
    }
    
    private TreeNode convertToBST(ListNode head, ListNode tail) {
        if (head == tail) return null;
        
        ListNode slow = head, fast = head;
        
        // Find the midpoint of the Linked List
        while (fast != tail && fast.next != tail) {
            fast = fast.next.next;
            slow = slow.next;
        }
        
        TreeNode node = new TreeNode(slow.val);
        
        // Left and right sublist recursion
        node.left = convertToBST(head, slow);
        node.right = convertToBST(slow.next, tail);
        
        return node;
    }
}
```

### Complexity Analysis:

- **Time Complexity**: O(N log N), each node is processed in O(log N) time and each call to find the middle takes linear time across all levels of the recursive tree.
- **Space Complexity**: O(log N), due to the recursion stack used during the divide process.

