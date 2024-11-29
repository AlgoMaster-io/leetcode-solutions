# [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

## Approach 1: Two Passes (Count Nodes)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public ListNode MiddleNode(ListNode head) {
        int count = 0;
        ListNode current = head;

        // Count the total number of nodes
        while (current != null) {
            count++;
            current = current.next;
        }

        // Find the middle index
        int middleIndex = count / 2;
        current = head;

        // Move to the middle node
        for (int i = 0; i < middleIndex; i++) {
            current = current.next;
        }

        return current;
    }
}
```

## Approach 2: Fast and Slow Pointers (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public ListNode MiddleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        // Move fast pointer twice as fast as the slow pointer
        while (fast != null && fast.next != null) {
            slow = slow.next; // Move slow pointer one step
            fast = fast.next.next; // Move fast pointer two steps
        }

        return slow; // Slow pointer will be at the middle
    }
}
```

Note: The `ListNode` class structure should be predefined, typically like this in C#:

```csharp
public class ListNode {
    public int val;
    public ListNode next;
    public ListNode(int x) { val = x; }
}
```

