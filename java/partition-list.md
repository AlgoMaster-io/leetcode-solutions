# [Leetcode 86: Partition List](https://leetcode.com/problems/partition-list/)

## Approaches
1. [Two-Pass: Array-based Partition](#approach-1)
2. [Two-Pointer: Single-pass LinkedList](#approach-2)

---

### Approach 1: Two-Pass: Array-based Partition

#### Intuition:
The idea is to first convert the linked list into an array, partition the array into two parts, and then reconstruct the linked list. Although not the most efficient, it provides a clear separation of the partition logic.

#### Steps:
1. Traverse through the original list and populate the values into an array.
2. Partition the array into two separate lists - one containing elements less than `x` and the other containing elements greater than or equal to `x`.
3. Traverse through these two lists and create a new linked list with the required order.

#### Code:
```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}

public class Solution {
    public ListNode partition(ListNode head, int x) {
        List<Integer> less = new ArrayList<>();
        List<Integer> greaterEqual = new ArrayList<>();

        // Traverse and partition into two arrays
        while (head != null) {
            if (head.val < x) {
                less.add(head.val);
            } else {
                greaterEqual.add(head.val);
            }
            head = head.next;
        }

        // Create a new LinkedList with ordered elements
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        for (int num : less) {
            current.next = new ListNode(num);
            current = current.next;
        }
        for (int num : greaterEqual) {
            current.next = new ListNode(num);
            current = current.next;
        }

        return dummy.next;
    }
}
```

#### Complexity Analysis:
- **Time Complexity:** O(n), where n is the number of nodes in the linked list. Each node is visited exactly once.
- **Space Complexity:** O(n), because of the use of additional lists to store node values.

---

### Approach 2: Two-Pointer: Single-pass LinkedList

#### Intuition:
Using two pointers, we directly manipulate the linked list to partition without extra space. This is achieved by creating two dummy head nodes representing partitions for values less than `x` and for values greater than or equal to `x`.

#### Steps:
1. Initialize two dummy nodes `lessHead` and `greaterHead` to handle partitions.
2. Iterate through the list, linking nodes with values less than `x` to `less` and others to `greater`.
3. Connect the `less` list with the `greater` list.
4. The new head is the start of the `less` list.

#### Code:
```java
class Solution {
    public ListNode partition(ListNode head, int x) {
        ListNode lessHead = new ListNode(0);
        ListNode greaterHead = new ListNode(0);
        
        ListNode less = lessHead;
        ListNode greater = greaterHead;

        // Partition list into less and greater lists
        while (head != null) {
            if (head.val < x) {
                // Append to less list
                less.next = head;
                less = less.next;
            } else {
                // Append to greater list
                greater.next = head;
                greater = greater.next;
            }
            head = head.next;
        }

        // Important: break any existing pointers to ensure no cycles
        greater.next = null;
        less.next = greaterHead.next;

        return lessHead.next;
    }
}
```

#### Complexity Analysis:
- **Time Complexity:** O(n), where n is the number of nodes in the linked list. Each node is visited exactly once.
- **Space Complexity:** O(1), no extra space except for pointers.

