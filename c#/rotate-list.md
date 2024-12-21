# [Leetcode 61: Rotate List](https://leetcode.com/problems/rotate-list/)

## Table of Contents
- [Approach 1: Brute Force - Rotate One Node at a Time](#approach-1-brute-force)
- [Approach 2: Optimized Approach Using Length Calculation](#approach-2-optimized-approach-using-length-calculation)
- [Approach 3: Most Optimal - Using Linked List Properties](#approach-3-most-optimal-linked-list-properties)

---

## Approach 1: Brute Force - Rotate One Node at a Time

### Intuition
The simplest way to rotate the list is to repeatedly move the last node to the front of the list. This approach effectively rotates the list one node at a time for a total of `k` rotations.

### Steps
1. Handle the edge cases where the list is empty or `k` is zero.
2. For each rotation, find the last node.
3. Move this last node to the front.
4. Repeat `k` times.

### Code
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
    public ListNode RotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) return head;
        
        // Find the last node and calculate the length
        ListNode last = head;
        int length = 1;
        while (last.next != null) {
            last = last.next;
            length++;
        }
        
        // Actual number of rotations needed
        k = k % length;
        if (k == 0) return head;
        
        // Rotate k times
        for (int i = 0; i < k; i++) {
            ListNode temp = head;
            while (temp.next.next != null) {
                temp = temp.next;
            }
            last.next = head;
            head = last;
            temp.next = null;
            last = temp;
        }
        
        return head;
    }
}
```

### Complexity
- **Time Complexity**: \(O(n \times k)\), where \(n\) is the number of nodes and \(k\) is the number of rotations.
- **Space Complexity**: \(O(1)\), no additional space is used.

---

## Approach 2: Optimized Approach Using Length Calculation

### Intuition
We can optimize the rotation by first calculating the length of the list and effectively rotating the list in one go. This eliminates the need for redundant passes through the list.

### Steps
1. Calculate the length of the list.
2. Find the actual rotation needed by `k % length`.
3. Identify the new head after `rotation` number of moves from the current head.
4. Set the new head, and reconnect the end of the list correctly.

### Code
```csharp
public class Solution {
    public ListNode RotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) return head;

        // Calculate the length
        ListNode last = head;
        int length = 1;
        while (last.next != null) {
            last = last.next;
            length++;
        }

        // Compute the actual number of rotations needed
        k = k % length;
        if (k == 0) return head;

        // Find the new tail: (length - k) - 1
        ListNode newTail = head;
        for (int i = 0; i < length - k - 1; i++) {
            newTail = newTail.next;
        }

        // Set the new head
        ListNode newHead = newTail.next;
        
        // Break the circle
        newTail.next = null;
        
        // Connect the old tail to the old head
        last.next = head;

        return newHead;
    }
}
```

### Complexity
- **Time Complexity**: \(O(n)\), where \(n\) is the number of nodes in the list.
- **Space Complexity**: \(O(1)\), no additional space is used.

---

## Approach 3: Most Optimal - Using Linked List Properties

### Intuition
Instead of rotating one by one or recalculating moduli, simply utilize the properties of a linked list to transform the last node linking to the first, and break at the desired new tail.

### Steps
1. Calculate the length of the list.
2. Determine the effective rotations `k` using `k % length`.
3. Convert the list into a circular one by connecting the last node to the head.
4. Identify the new head and break the loop appropriately.

### Code
```csharp
public class Solution {
    public ListNode RotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) return head;

        // Calculate the length and connect the last to head to form a loop
        ListNode last = head;
        int length = 1;
        while (last.next != null) {
            last = last.next;
            length++;
        }
        k %= length;
        if (k == 0) return head;
        
        // Form a circle
        last.next = head;

        // Find the new tail, which is (length - k) from the start
        ListNode newTail = head;
        for (int i = 0; i < length - k - 1; i++) {
            newTail = newTail.next;
        }

        // Set the new head and break the circle
        ListNode newHead = newTail.next;
        newTail.next = null;

        return newHead;
    }
}
```

### Complexity
- **Time Complexity**: \(O(n)\), where \(n\) is the number of nodes in the list.
- **Space Complexity**: \(O(1)\), no additional space is used.

