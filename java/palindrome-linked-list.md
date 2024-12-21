# [Leetcode 234: Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

## Approaches:
- [Approach 1: Convert to Array and use Two-Pointer Technique](#approach-1-convert-to-array-and-use-two-pointer-technique)
- [Approach 2: Reverse Second Half in-place](#approach-2-reverse-second-half-in-place)

---

### Approach 1: Convert to Array and use Two-Pointer Technique

**Intuition:**

The easiest way to determine if a linked list is a palindrome is to convert the linked list into an array and then use the two-pointer technique to verify if the structure is a palindrome. The first pointer starts at the beginning of the array, and the second starts at the end. If all elements from both ends are equal, then the list is a palindrome.

**Steps:**
1. Traverse the linked list and store the values in an array.
2. Use two pointers, one starting at the beginning of the array and the other at the end.
3. Compare values at both pointers, move inward, and if all are equal, the list is a palindrome.

```java
public boolean isPalindrome(ListNode head) {
    List<Integer> values = new ArrayList<>();

    // Convert linked list into array list
    ListNode currentNode = head;
    while (currentNode != null) {
        values.add(currentNode.val);
        currentNode = currentNode.next;
    }

    // Use two-pointer technique
    int front = 0;
    int back = values.size() - 1;
    while (front < back) {
        // If elements are not the same, not a palindrome
        if (!values.get(front).equals(values.get(back))) {
            return false;
        }
        front++;
        back--;
    }

    // All elements match, it's a palindrome
    return true;
}
```

**Time Complexity:** O(n), where n is the number of nodes in the linked list.
**Space Complexity:** O(n), for storing the values in a list.

---

### Approach 2: Reverse Second Half in-place

**Intuition:**

A more optimized approach is to reverse the second half of the linked list and then compare it with the first half. If both halves are identical, then the linked list is a palindrome. This approach doesn't require extra space for storing values.

**Steps:**

1. Find the midpoint of the linked list using the fast and slow pointer technique.
2. Reverse the second half of the list.
3. Compare the first half with the reversed second half.
4. Restore the original list (optional).

```java
public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) {
        return true;
    }

    // Step 1: Find the end of the first half
    ListNode firstHalfEnd = endOfFirstHalf(head);
    // Step 2: Reverse the second half
    ListNode secondHalfStart = reverseList(firstHalfEnd.next);

    // Step 3: Check whether or not there's a palindrome
    ListNode p1 = head;
    ListNode p2 = secondHalfStart;
    boolean result = true;
    while (result && p2 != null) {
        if (p1.val != p2.val) {
            result = false;
        }
        p1 = p1.next;
        p2 = p2.next;
    }

    // (Optional) Step 4: Restore the list
    firstHalfEnd.next = reverseList(secondHalfStart);

    return result;
}

// Helper function to reverse the linked list from a given node
private ListNode reverseList(ListNode head) {
    ListNode prev = null;
    while (head != null) {
        ListNode nextNode = head.next;
        head.next = prev;
        prev = head;
        head = nextNode;
    }
    return prev;
}

// Helper function to find the end node of the first half
private ListNode endOfFirstHalf(ListNode head) {
    ListNode fast = head;
    ListNode slow = head;
    while (fast.next != null && fast.next.next != null) {
        fast = fast.next.next;
        slow = slow.next;
    }
    return slow;
}
```

**Time Complexity:** O(n), where n is the number of nodes in the linked list.
**Space Complexity:** O(1), as we're changing the pointers in-place.

