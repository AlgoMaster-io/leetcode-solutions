# [Leetcode 234: Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

## Approaches
1. [Brute Force Approach Using Extra Space](#brute-force-approach)
2. [Efficient Approach Using Fast and Slow Pointers](#efficient-approach)

### Approach 1: Brute Force Approach Using Extra Space

**Intuition:**

We can determine if a linked list is a palindrome by storing its elements in an array. A linked list is a palindrome if the array reads the same from front to back as it does from back to front.

**Steps:**
1. Traverse the linked list and copy its elements into an array.
2. Use two pointers to simultaneously check from the start and end of the array whether the elements match.
3. If all elements match, then the linked list is a palindrome.

```javascript
var isPalindrome = function(head) {
    let nums = [];
    let currentNode = head;

    // Traverse the list and copy the elements to the array
    while (currentNode !== null) {
        nums.push(currentNode.val);
        currentNode = currentNode.next;
    }

    // Use two pointers to check palindrome property
    let left = 0;
    let right = nums.length - 1;
    while (left < right) {
        if (nums[left] !== nums[right]) {
            return false; // Mismatch found, hence not a palindrome
        }
        left++;
        right--;
    }
    
    return true; // All elements matched
};
```

- **Time Complexity:** O(n), where n is the number of elements in the linked list (due to traversal).
- **Space Complexity:** O(n), for storing elements in an array.

### Approach 2: Efficient Approach Using Fast and Slow Pointers

**Intuition:**

The goal with a more efficient approach is to do this in O(n) time and O(1) additional space. We can achieve this by:
- Using the fast and slow pointer technique to find the middle of the linked list.
- Reversing the second half of the linked list.
- Comparing the first half with the reversed second half.

**Steps:**
1. Use two pointers, `fast` and `slow`. Move `fast` at twice the speed of `slow` to find the midpoint of the list.
2. Reverse the second half of the linked list.
3. Compare the nodes in the first half of the list with the nodes in the reversed second half.
4. Restore the list to its original state (optional) by reversing the second half again.

```javascript
var isPalindrome = function(head) {
    if (head === null || head.next === null) return true;

    // Function to reverse a linked list
    const reverseList = (head) => {
        let prev = null;
        let current = head;
        while (current !== null) {
            let nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        return prev;
    };

    // Find the end of the first half and reverse the second half
    const findMiddle = (head) => {
        let fast = head;
        let slow = head;
        
        while (fast !== null && fast.next !== null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    };

    let firstEnd = findMiddle(head);
    let secondHalfStart = reverseList(firstEnd);

    // Check whether or not there is a palindrome
    let p1 = head;
    let p2 = secondHalfStart;
    let result = true;
    while (p2 !== null) {
        if (p1.val !== p2.val) {
            result = false;
            break;
        }
        p1 = p1.next;
        p2 = p2.next;
    }

    // Restore the original list structure
    reverseList(secondHalfStart);

    return result;
};
```

- **Time Complexity:** O(n), since each node is processed at most twice.
- **Space Complexity:** O(1), as we are not using any additional space related to the input size (only a few pointers).

This efficient approach allows us to verify the palindrome nature of the linked list while maintaining a space complexity of O(1), which is ideal for working with linked lists.

