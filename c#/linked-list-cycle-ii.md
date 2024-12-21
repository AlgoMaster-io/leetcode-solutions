# [Leetcode 141: Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Solutions
- [Naive Solution: Use HashSet](#naive-solution-use-hashset)
- [Optimal Solution: Floyd's Tortoise and Hare](#optimal-solution-floyds-tortoise-and-hare)

### Naive Solution: Use HashSet

**Intuition**:  
The simplest way to detect a cycle in a linked list is to keep track of all the nodes we've visited using an additional data structure like a `HashSet`. If a node is revisited, it means we've found the cycle.

```csharp
public class ListNode {
    public int val;
    public ListNode next;
    public ListNode(int x) { val = x; next = null; }
}

public class Solution {
    public ListNode DetectCycle(ListNode head) {
        HashSet<ListNode> visitedNodes = new HashSet<ListNode>();
        ListNode currentNode = head;

        while (currentNode != null) {
            // If the node is already in the set, we've found the cycle
            if (visitedNodes.Contains(currentNode)) {
                return currentNode;
            }
            // Add the current node to the visited set
            visitedNodes.Add(currentNode);
            // Move to the next node
            currentNode = currentNode.next;
        }

        // No cycle found, return null
        return null;
    }
}
```

**Time Complexity**: O(n), where n is the number of nodes in the linked list.  
**Space Complexity**: O(n), because we store up to n nodes in the `HashSet`.  

### Optimal Solution: Floyd's Tortoise and Hare

**Intuition**:  
Floyd's Tortoise and Hare algorithm doesn't require extra space. It uses two pointers moving at different speeds. The idea is if there is a cycle, the fast pointer (hare) will eventually meet the slow pointer (tortoise) due to the cyclical nature. Once they meet, to find the entry point of the cycle, set one pointer to the head and keep the other where they met, then move both one step at a time; they will meet at the cycle's entry point.

```csharp
public class Solution {
    public ListNode DetectCycle(ListNode head) {
        if (head == null || head.next == null) {
            return null;
        }

        ListNode slow = head;
        ListNode fast = head;

        // Move slow by 1 step and fast by 2 steps
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;

            // If slow and fast meet, there is a cycle
            if (slow == fast) {
                // To find the entry of the cycle
                ListNode entry = head;
                
                // Move both, starting from head and meeting point, one step at a time
                while (entry != slow) {
                    entry = entry.next;
                    slow = slow.next;
                }
                // Entry point of the cycle
                return entry;
            }
        }

        // No cycle found
        return null;
    }
}
```

**Time Complexity**: O(n), as each pointer travels approximately the number of nodes in the list.  
**Space Complexity**: O(1), as no extra data structure is used beyond pointers.

