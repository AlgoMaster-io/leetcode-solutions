# [Leetcode 234: Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

## Approaches:
- [Approach 1: List Conversion and Two Pointers](#approach-1-list-conversion-and-two-pointers)
- [Approach 2: Recursive Check](#approach-2-recursive-check)
- [Approach 3: Fast and Slow Pointer with Reverse](#approach-3-fast-and-slow-pointer-with-reverse)

### Approach 1: List Conversion and Two Pointers

**Intuition:**

Convert the linked list to an array and then use a two-pointer technique to check for palindrome. This is straightforward as it reduces the problem to a basic palindrome check on an array.

**Steps:**

1. Traverse the linked list and store the values in a list.
2. Use two pointers, one starting at the beginning, and the other at the end of the list.
3. Compare the values at the two pointers. Move them closer to the center until they either meet or cross each other.

**Code:**

```csharp
public bool IsPalindrome(ListNode head) {
    // Store the values of the linked list nodes into a list for easy access
    List<int> values = new List<int>();
    ListNode current = head;
    while (current != null) {
        values.Add(current.val);
        current = current.next;
    }
    
    // Use two-pointer technique to check for palindrome
    int left = 0;
    int right = values.Count - 1;
    while (left < right) {
        if (values[left] != values[right]) {
            return false; // Not a palindrome
        }
        left++;
        right--;
    }
    
    return true; // Is a palindrome
}
```

**Complexity:**

- **Time:** O(n), where n is the number of nodes in the linked list.
- **Space:** O(n), to store the values in the list.

### Approach 2: Recursive Check

**Intuition:**

Use recursion to check if the linked list is a palindrome by comparing pairs of nodes from the start and end as the recursion unwinds.

**Steps:**

1. Use a helper function with recursion to go to the end of the list.
2. Compare the node values from the first node with the last node on the way back from recursion.
3. Use a reference to the head node to allow comparison from both ends.
4. Return false if a mismatch is found during the backtrace.

**Code:**

```csharp
public class Solution {
    private ListNode frontPointer;

    public bool IsPalindrome(ListNode head) {
        frontPointer = head;
        return recursivelyCheck(head);
    }
    
    private bool recursivelyCheck(ListNode currentNode) {
        if (currentNode != null) {
            if (!recursivelyCheck(currentNode.next)) return false;
            if (frontPointer.val != currentNode.val) return false;
            frontPointer = frontPointer.next;
        }
        return true;
    }
}
```

**Complexity:**

- **Time:** O(n), each node is visited once.
- **Space:** O(n), due to recursive stack calls.

### Approach 3: Fast and Slow Pointer with Reverse

**Intuition:**

By using a fast and slow pointer, we can find the middle of the linked list. Reverse the second half of the list and then check if both halves are palindromes.

**Steps:**

1. Use the fast and slow pointer method to find the midpoint of the list.
2. Reverse the second half of the list.
3. Compare the first half and the reversed second half of the list.
4. (Optional) Restore the original list structure by reversing the second half back.

**Code:**

```csharp
public bool IsPalindrome(ListNode head) {
    if (head == null) return true; // An empty list is a palindrome
    
    // Find the end of the first half and reverse second half
    ListNode firstHalfEnd = endOfFirstHalf(head);
    ListNode secondHalfStart = reverseList(firstHalfEnd.next);
    
    // Check whether or not there is a palindrome
    ListNode p1 = head;
    ListNode p2 = secondHalfStart;
    bool result = true;
    while (result && p2 != null) {
        if (p1.val != p2.val) result = false;
        p1 = p1.next;
        p2 = p2.next;
    }
    
    // Restore the list (optional)
    firstHalfEnd.next = reverseList(secondHalfStart);
    
    return result;
}

private ListNode endOfFirstHalf(ListNode head) {
    ListNode fast = head;
    ListNode slow = head;
    while (fast.next != null && fast.next.next != null) {
        fast = fast.next.next;
        slow = slow.next;
    }
    return slow;
}

private ListNode reverseList(ListNode head) {
    ListNode previous = null;
    ListNode current = head;
    while (current != null) {
        ListNode nextTemp = current.next;
        current.next = previous;
        previous = current;
        current = nextTemp;
    }
    return previous;
}
```

**Complexity:**

- **Time:** O(n), where n is the number of nodes in the list.
- **Space:** O(1), only a few additional pointers used.

