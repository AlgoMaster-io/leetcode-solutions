# [Leetcode Problem 86: Partition List](https://leetcode.com/problems/partition-list/)

## Approaches:
- [Approach 1: Two Pointers and Dummy Nodes](#approach-1-two-pointers-and-dummy-nodes)

---

## Approach 1: Two Pointers and Dummy Nodes

### Intuition
The problem requires rearranging the linked list such that all nodes less than `x` appear before nodes greater than or equal to `x`. This can be efficiently done by using the two-pointer technique and dummy nodes. The idea is to maintain two separate lists: one for nodes less than `x` and another for nodes greater or equal. Finally, we concatenate these two lists.

### Steps:
1. Create two dummy nodes, `before` and `after`, that will be used for lists of nodes with values less than `x`, and greater than or equal to `x` respectively.
2. Initialize two pointers, `before_head` and `after_head`, pointing to these dummy nodes.
3. Traverse the original list. For each node:
   - If its value is less than `x`, append it to the `before` list.
   - Otherwise, append it to the `after` list.
4. After completing the traversal, connect the `before` list to the `after` list.
5. Return the `next` of the `before_head` which points to the new head of the rearranged list.

### Time complexity:
- **O(n)**: We traverse the list only once where `n` is the number of nodes in the list.

### Space complexity:
- **O(1)**: We use a constant amount of extra space for dummy nodes and pointers.

### C# Code Implementation

```csharp
public class ListNode {
    public int val;
    public ListNode next;
    public ListNode(int val=0, ListNode next=null) {
        this.val = val;
        this.next = next;
    }
}

public ListNode Partition(ListNode head, int x) {
    // Dummy heads for before and after lists
    ListNode before_head = new ListNode(0);
    ListNode after_head = new ListNode(0);

    // Pointers to the current end of before and after lists
    ListNode before = before_head;
    ListNode after = after_head;

    // Traverse the original list
    while (head != null) {
        if (head.val < x) {
            // Append to before list
            before.next = head;
            before = before.next;
        } else {
            // Append to after list
            after.next = head;
            after = after.next;
        }
        // Move to the next node
        head = head.next;
    }

    // Prevent circular linkage in the final linked list
    after.next = null;

    // Connect the before list to the after list
    before.next = after_head.next;

    // Return the start of the rearranged linked list
    return before_head.next;
}
```


