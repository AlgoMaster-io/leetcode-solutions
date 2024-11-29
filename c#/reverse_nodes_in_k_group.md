# [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

## Approach: Iterative Solution with Dummy Node

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public ListNode ReverseKGroup(ListNode head, int k) {
        if (head == null || k == 1) {
            return head;
        }

        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prevGroup = dummy;

        while (true) {
            // Find the k-th node from prevGroup
            ListNode kthNode = GetKthNode(prevGroup, k);
            if (kthNode == null) {
                break; // Not enough nodes to reverse
            }

            ListNode nextGroup = kthNode.next; // Node after the k-th node
            ListNode prev = nextGroup; // Start reversal with nextGroup as tail
            ListNode current = prevGroup.next;

            // Reverse k nodes
            while (current != nextGroup) {
                ListNode temp = current.next;
                current.next = prev;
                prev = current;
                current = temp;
            }

            // Update the connections
            ListNode temp = prevGroup.next;
            prevGroup.next = kthNode;
            prevGroup = temp;
        }

        return dummy.next;
    }

    private ListNode GetKthNode(ListNode start, int k) {
        while (start != null && k > 0) {
            start = start.next;
            k--;
        }
        return start;
    }
}

public class ListNode {
    public int val;
    public ListNode next;
    public ListNode(int val = 0, ListNode next = null) {
        this.val = val;
        this.next = next;
    }
}
```

