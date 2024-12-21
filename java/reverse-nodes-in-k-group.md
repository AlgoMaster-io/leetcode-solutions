# [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Recursive Approach](#recursive-approach)
- [Iterative Approach](#iterative-approach)

---

### Brute Force Approach

**Intuition:**  
The most straightforward approach to reverse nodes in k-group involves using an additional data structure, such as a list or stack, to store the k nodes temporarily. Once we have stored k nodes, we reverse them and link them back to the list.

1. Traverse the list, extracting groups of k nodes and reverse each group.
2. Use a stack or list to reverse the nodes in each group.
3. Reconnect the reversed nodes with the rest of the list.
4. Continue this process for each k-sized group in the list until you have traversed the entire list.

**Time Complexity:** O(n), where n is the number of nodes in the list.  
**Space Complexity:** O(k), for storing each group of nodes temporarily.

```java
// Definition for singly-linked list.
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}

public class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        // Use a dummy node to handle edge cases easily
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        // Initialize pointers
        ListNode current = dummy, next = dummy, prev = dummy;

        // Count the number of nodes in the list
        int count = 0;
        while (current.next != null) {
            current = current.next;
            count++;
        }

        // Reverse in groups
        while (count >= k) {
            current = prev.next; // Current points to the start of the group
            next = current.next; // Next node to be processed
            for (int i = 1; i < k; i++) {
                // Reversal process
                current.next = next.next;
                next.next = prev.next;
                prev.next = next;
                next = current.next;
            }
            prev = current; // Move prev to the end of the reversed group
            count -= k; // Decrement count by k as k nodes have been reversed
        }
        return dummy.next;
    }
}
```

---

### Recursive Approach

**Intuition:**  
The recursive approach is an elegant way to solve the problem. In each recursive call, reverse a k-sized group and link the rest using recursion.

1. Base case: If the number of nodes is less than k, directly return the head.
2. Recursively reverse the k-sized group, and then link it with the recursively processed next k-sized group.
3. Continue doing this for all the nodes in the list.

**Time Complexity:** O(n), as each node is traversed once.  
**Space Complexity:** O(n/k) due to the recursion stack.

```java
public class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode current = head;
        int count = 0;

        // Find the k+1 node
        while (current != null && count != k) {
            current = current.next;
            count++;
        }

        // If we reached k nodes, we reverse the current k nodes
        if (count == k) {
            current = reverseKGroup(current, k); // Recursively reverse remaining nodes
            while (count-- > 0) { // Reverse the k nodes
                ListNode temp = head.next;
                head.next = current;
                current = head;
                head = temp;
            }
            head = current; // Current becomes new head of reversed list
        }
        return head;
    }
}
```

---

### Iterative Approach

**Intuition:**  
An optimal solution with an iterative approach avoids using extra space apart from the input list itself.

1. Use pointers to manage and reverse the k-sized groups in a single pass without additional space for node storage.
2. Reverse the nodes within each segment of k-sized nodes by adjusting the pointers directly in the list.
3. Utilize a dummy node and handle edge cases to ensure the list is correctly reconstructed after reversal.

**Time Complexity:** O(n), where n is the length of the linked list since each node is processed at most twice.  
**Space Complexity:** O(1), because the operation is done in place.

```java
public class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || k == 1) return head;

        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode curr = dummy, next = dummy, prev = dummy;
        int count = 0;

        // Count the number of nodes
        while (curr.next != null) {
            curr = curr.next;
            count++;
        }

        // Reverse every k-group of nodes
        while (count >= k) {
            curr = prev.next; // Current starts at the beginning of the group
            next = curr.next; // Next is the node that follows current
            for (int i = 1; i < k; i++) {
                curr.next = next.next; // Current's next skips to the next of next
                next.next = prev.next; // Next is moved to the front of the group
                prev.next = next;      // Previous points to the newly fronted node
                next = curr.next;      // Move next to the next node in the sequence
            }
            prev = curr; // Move prev to the end of the reversed group
            count -= k; // Decrement count by k
        }
        return dummy.next; // Return new head
    }
}
```

