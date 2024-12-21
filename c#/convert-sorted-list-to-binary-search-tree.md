# [Leetcode 109: Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approaches

1. [Recursive Approach with Slow and Fast Pointers](#recursive-approach-with-slow-and-fast-pointers)
2. [Optimized Inorder Simulation Approach](#optimized-inorder-simulation-approach)

---

### Recursive Approach with Slow and Fast Pointers

#### Intuition:
The problem requires us to transform a sorted linked list into a height-balanced binary search tree (BST). A height-balanced BST is defined as a binary tree in which the depth of the two subtrees of every node never differs by more than 1. The middle element of the list can serve as the root, and the left and right halves of the list can form the left and right subtrees, respectively. We can use two pointers, slow and fast (tortoise and hare approach), to find the middle of the linked list. The slow pointer moves one step at a time, while the fast pointer moves two steps, thus when the fast pointer reaches the end, the slow pointer will be in the middle.

#### Steps:
1. Use the slow and fast pointer technique to find the middle node of the linked list.
2. The middle node becomes the root of the BST.
3. Recursively repeat the process for the left half, assigning it to the left child, and for the right half for the right child.

##### Time Complexity:
- O(n log n), where n is the number of nodes in the linked list. Each level of the recursion splits the list by half and finding the middle node requires O(n) time.

##### Space Complexity:
- O(log n) due to the recursion stack.

```csharp
public class ListNode {
    public int val;
    public ListNode next;
    public ListNode(int x) { val = x; }
}

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {

    private ListNode findMiddle(ListNode head) {
        ListNode prevPtr = null;
        ListNode slowPtr = head;
        ListNode fastPtr = head;
        
        // Move fastPtr by two and slowPtr by one, eventually slowPtr will be at the middle
        while (fastPtr != null && fastPtr.next != null) {
            prevPtr = slowPtr;
            slowPtr = slowPtr.next;
            fastPtr = fastPtr.next.next;
        }
        
        // Disconnect the left half from the mid node.
        if (prevPtr != null) {
            prevPtr.next = null;
        }
        
        return slowPtr;
    }

    public TreeNode SortedListToBST(ListNode head) {
        // Base case
        if (head == null) {
            return null;
        }
        
        // Get the middle of the list and make it root
        ListNode mid = findMiddle(head);

        TreeNode node = new TreeNode(mid.val);

        // Base case when there is just one element in the linked list
        if (head == mid) {
            return node;
        }
        
        // Recursively construct the left subtree and make it left child of root
        node.left = SortedListToBST(head);
        
        // Recursively construct the right subtree and make it right child of root
        node.right = SortedListToBST(mid.next);

        return node;
    }
}
```

---

### Optimized Inorder Simulation Approach

#### Intuition:
The above approach handles each recursion level by finding the middle element, which takes O(n) time. We can optimize this by simulating an inorder traversal on the linked list — leveraging the property that an inorder traversal of a BST will output the elements in sorted order (which the linked list already is).

#### Steps:
1. Use the size of the linked list to recursively build the tree, taking nodes in the same sequence as they appear. 
2. Maintain a global reference of the list head to iterate nodes in the in-order pattern.

##### Time Complexity:
- O(n), every node is visited and linked exactly once.

##### Space Complexity:
- O(log n) for the recursion stack.

```csharp
public class Solution {

    private ListNode currentNode;

    private int findSize(ListNode head) {
        ListNode node = head;
        int size = 0;
        while (node != null) {
            size++;
            node = node.next;
        }
        return size;
    }

    private TreeNode convertListToBST(int left, int right) {
        // Base case
        if (left > right) {
            return null;
        }

        // Recursively form the left half
        int mid = (left + right) / 2;
        TreeNode leftChild = convertListToBST(left, mid - 1);

        // Node processing
        TreeNode node = new TreeNode(currentNode.val);
        node.left = leftChild;

        // Move to the next node in the list
        currentNode = currentNode.next;

        // Recursively form the right half and link it with root
        node.right = convertListToBST(mid + 1, right);

        return node;
    }

    public TreeNode SortedListToBST(ListNode head) {
        // Get the size of the linked list
        int size = findSize(head);

        // Initialize the reference to the head for in-order processing
        currentNode = head;

        return convertListToBST(0, size - 1);
    }
}
```

This implementation simulates the inorder traversal on the linked list, which helps us avoid needing to repeatedly traverse and split the linked list, providing a more efficient solution.

