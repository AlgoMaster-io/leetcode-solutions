# [Leetcode 148: Sort List](https://leetcode.com/problems/sort-list/)

## Approaches:
1. [Basic Approach: Merge Sort](#basic-approach-merge-sort)
2. [Optimal Approach: Bottom-Up Merge Sort](#optimal-approach-bottom-up-merge-sort)

## Basic Approach: Merge Sort

### Intuition
The problem can be approached using the merge sort algorithm which is a classic divide and conquer algorithm. Given its time complexity of O(n log n) which aligns with the ideal requirement for sorting algorithms, it becomes a suitable choice for sorting a linked list.

The merge sort works as follows:
1. **Divide** the list into two halves by finding the middle node.
2. **Conquer** each sublist recursively until the base case (a single node or null) is reached.
3. **Combine** the sorted sublists to produce the sorted list.

### Steps
- Find the middle of the linked list using the slow and fast pointer approach.
- Recursively sort the two halves of the linked list.
- Merge the sorted halves.

### Detailed Code

```csharp
public class ListNode {
    public int val;
    public ListNode next;
    public ListNode(int val=0, ListNode next=null) {
        this.val = val;
        this.next = next;
    }
}

public class Solution {
    public ListNode SortList(ListNode head) {
        // Base case: if head is null or there is only one element
        if (head == null || head.next == null) {
            return head;
        }

        // Step 1: Split the linked list into two halves using slow and fast.
        ListNode mid = GetMiddle(head);
        ListNode left = head;
        ListNode right = mid.next;
        mid.next = null;

        // Step 2: Recursively sort each half
        left = SortList(left);
        right = SortList(right);

        // Step 3: Merge the two sorted halves
        return Merge(left, right);
    }

    // Utility function to find the middle of the linked list
    private ListNode GetMiddle(ListNode head) {
        if (head == null) return head;

        ListNode slow = head;
        ListNode fast = head.next;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        return slow;
    }

    // Utility function to merge two sorted linked lists
    private ListNode Merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                current.next = l1;
                l1 = l1.next;
            } else {
                current.next = l2;
                l2 = l2.next;
            }
            current = current.next;
        }
        
        if (l1 != null) {
            current.next = l1;
        }
        
        if (l2 != null) {
            current.next = l2;
        }
        
        return dummy.next;
    }
}
```

### Time and Space Complexity
- **Time Complexity:** O(n log n), where n is the number of nodes in the linked list.
- **Space Complexity:** O(log n), due to the recursion stack.

## Optimal Approach: Bottom-Up Merge Sort

### Intuition
While the recursive merge sort is efficient, it consumes extra space due to recursion stack which could go up to O(log n). We can optimize this using an iterative approach with a bottom-up strategy to avoid the recursive calls.

### Steps
- Iteratively merge sublists of increasing sizes from 1 up to the list's length.
- Use a dummy node to build the final sorted list incrementally.

### Detailed Code

```csharp
public class Solution {
    public ListNode SortList(ListNode head) {
        if (head == null || head.next == null) return head;

        int length = GetLength(head);
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        for (int size = 1; size < length; size *= 2) {
            ListNode current = dummy.next;
            ListNode tail = dummy;

            while (current != null) {
                ListNode left = current;
                ListNode right = Split(left, size);
                current = Split(right, size);
                tail = Merge(left, right, tail);
            }
        }

        return dummy.next;
    }

    private int GetLength(ListNode head) {
        int length = 0;
        while (head != null) {
            length++;
            head = head.next;
        }
        return length;
    }

    private ListNode Split(ListNode head, int size) {
        for (int i = 1; head != null && i < size; i++) {
            head = head.next;
        }
        if (head == null) return null;
        ListNode second = head.next;
        head.next = null;
        return second;
    }

    private ListNode Merge(ListNode l1, ListNode l2, ListNode tail) {
        ListNode current = tail;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                current.next = l1;
                l1 = l1.next;
            } else {
                current.next = l2;
                l2 = l2.next;
            }
            current = current.next;
        }

        current.next = l1 != null ? l1 : l2;
        while (current.next != null) {
            current = current.next;
        }

        return current;
    }
}
```

### Time and Space Complexity
- **Time Complexity:** O(n log n), as we are still performing merge sort.
- **Space Complexity:** O(1), since it is an iterative implementation using the existing linked list nodes.

