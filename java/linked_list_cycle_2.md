# [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Approach 1: Using HashSet to Track Visited Nodes

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashSet;

public class Solution {
    public ListNode detectCycle(ListNode head) {
        HashSet<ListNode> visited = new HashSet<>();

        while (head != null) {
            // If the node has been visited before, it is the start of the cycle
            if (visited.contains(head)) {
                return head;
            }
            visited.add(head);
            head = head.next;
        }

        return null; // No cycle detected
    }
}
```

## Approach 2: Floyd's Cycle Detection Algorithm (Optimal)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null) {
            return null; // No cycle possible
        }

        ListNode slow = head;
        ListNode fast = head;

        // Detect if a cycle exists
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast) { // Cycle detected
                ListNode pointer = head;

                // Find the start of the cycle
                while (pointer != slow) {
                    pointer = pointer.next;
                    slow = slow.next;
                }

                return pointer; // Start of the cycle
            }
        }

        return null; // No cycle detected
    }
}
```