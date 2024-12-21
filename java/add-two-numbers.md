# [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

In this problem, you are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. You need to add the two numbers and return the sum as a linked list. You cannot modify the input lists, and you should return a new linked list.

## Table of Contents
1. [Naive Approach](#naive-approach)
2. [Optimal Approach](#optimal-approach)

---

## Naive Approach

The naive approach involves iterating through both linked lists simultaneously, adding the digits, and managing the carry.

### Intuition

- The core idea is to traverse both linked lists while simultaneously keeping track of the carry, resulting from the sum of respective digits.
- When one of the linked lists is exhausted, continue the addition with the remaining linked list considering the carry.
- If we are left with a carry after completely processing both lists, add a new node to represent that carry.

### Algorithm
1. Initialize a `dummyHead` with value `0`. This will help to easily return the head of the new linked list.
2. Initialize `current` pointer to point at `dummyHead`.
3. Initialize a variable `carry` to `0`.
4. Loop over the linked lists as long as there are digits to process from either one or there is a carry to consider.
   - Compute the sum of the current digits (considering `null` to represent value `0`) and the carry.
   - Update the carry based on the sum (it'll be either `0` or `1`).
   - Create a new node with the digit value of `sum % 10` and attach it to `current`.
   - Advance to the next nodes in the linked lists as well as move `current`.
5. Return `dummyHead.next`.

### Java Code

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */

public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0); // Dummy node to form the result list
    ListNode p = l1, q = l2, current = dummyHead; // Pointers for list traversal
    int carry = 0; // Initialize carry to zero
    
    while (p != null || q != null) {
        // Get the current values from the list nodes or use 0 if node is null
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        
        int sum = carry + x + y; // Sum current digits and carry
        carry = sum / 10; // Update carry for next iteration
        
        current.next = new ListNode(sum % 10); // Create node for the digit sum
        current = current.next; // Move to the next node in result
        
        // Advance p and q pointers
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    
    // If there's a carry left, create a new node for it
    if (carry > 0) {
        current.next = new ListNode(carry);
    }
    
    return dummyHead.next; // Skip dummy node and return result list
}
```

### Time Complexity

- **Time Complexity**: O(max(m, n)), where `m` and `n` are the lengths of the two lists. We iterate through both lists once.
- **Space Complexity**: O(max(m, n)). The result can be at most max(m, n) + 1 in length to accommodate any carry.

---

## Optimal Approach

The naive approach already solves this problem optimally with O(max(m, n)) time complexity and utilizes a single pass through the lists. Thus, any variation of this approach, in this case, can be considered optimal assuming that it is sufficiently intuitive and clear.

This approach maintains optimal performance using basic operations while efficiently simulating the addition of two numbers digit by digit stored in reverse order. Each iteration processes corresponding nodes of both linked lists while considering carry forward.

