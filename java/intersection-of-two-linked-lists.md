# [Leetcode 160: Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Using HashSet](#using-hashset)
3. [Two Pointer Technique](#two-pointer-technique)

### Brute Force Approach

#### Intuition
The simplest method to determine if the linked lists intersect is to compare each node in one list with every node in the other list. If there is a node that matches, then that node is the intersection node.

#### Code
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode a = headA;
        // Traverse each node in list A
        while (a != null) {
            ListNode b = headB;
            // Compare the current node in list A with all nodes in list B
            while (b != null) {
                // Check if they are the same node
                if (a == b) {
                    return a; // Found intersection
                }
                b = b.next;
            }
            a = a.next;
        }
        return null; // No intersection found
    }
}
```

#### Time Complexity
- O(m * n) where m and n are the lengths of the two linked lists.

#### Space Complexity
- O(1) as we are not using any extra space except two pointers.

### Using HashSet

#### Intuition
We can use a hash set to store all the nodes of one of the linked lists, then traverse the second linked list to see if any node is already in the set. If a node is found in the set, that node is the intersection node.

#### Code
```java
import java.util.HashSet;

public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        HashSet<ListNode> nodesInB = new HashSet<>();
        
        // Traverse through the second list and add its nodes to the hashset
        ListNode b = headB;
        while (b != null) {
            nodesInB.add(b);
            b = b.next;
        }
        
        // Traverse through the first list and check if any node is in the hashset
        ListNode a = headA;
        while (a != null) {
            if (nodesInB.contains(a)) {
                return a; // Found intersection
            }
            a = a.next;
        }
        
        return null; // No intersection found
    }
}
```

#### Time Complexity
- O(m + n) where m and n are the lengths of the two linked lists.

#### Space Complexity
- O(n) where n is the length of the longer linked list since all its nodes are stored in a set.

### Two Pointer Technique

#### Intuition
The most optimal approach uses two pointers. Initially, set two pointers to the heads of the two linked lists. Traverse through the linked lists, and when a pointer reaches the end of a linked list, redirect it to the head of the other linked list. If the lists intersect, the two pointers will eventually converge at the intersection node after (m + n) - c steps, where c is the length of the shared tail. If they don't intersect, both pointers will eventually become null, and the loop will end.

#### Code
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        // If either head is null, they cannot intersect
        if (headA == null || headB == null) {
            return null;
        }
        
        ListNode a = headA;
        ListNode b = headB;
        
        // Loop until the pointers meet or both reach to end
        while (a != b) {
            a = (a == null) ? headB : a.next;
            b = (b == null) ? headA : b.next;
        }
        
        // Either they met at intersection node or both are null
        return a;
    }
}
```

#### Time Complexity
- O(m + n) where m and n are the lengths of the two linked lists.

#### Space Complexity
- O(1) as no extra space is used, just pointers are moved.

