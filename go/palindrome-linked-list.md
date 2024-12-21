# [Leetcode 234: Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

## Solutions:

- [Approach 1: Convert to List and Use Two-Pointer Technique](#approach-1)
- [Approach 2: Reverse Second Half in-Place and Compare](#approach-2)
- [Approach 3: Recursive Check](#approach-3)

### Approach 1: Convert to List and Use Two-Pointer Technique

**Intuition:**

The simplest way is to convert the linked list into an array (or slice in Go) and then apply the two-pointer technique to check if the array is a palindrome. This approach benefits from leveraging array access for simplicity and speed.

**Code:**

```go
func isPalindrome(head *ListNode) bool {
    // Step 1: Convert the linked list into a slice
    values := []int{}
    for head != nil {
        values = append(values, head.Val)
        head = head.Next
    }
    
    // Step 2: Use two-pointer technique to check for palindrome
    left, right := 0, len(values)-1
    for left < right {
        if values[left] != values[right] {
            return false
        }
        left++
        right--
    }
    return true
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n), where n is the number of nodes in the linked list. We traverse the linked list once to convert it into a slice, then use the two-pointer technique.
- **Space Complexity:** O(n), due to the space required to store the list elements in the slice.

### Approach 2: Reverse Second Half in-Place and Compare

**Intuition:**

A more space-efficient solution involves finding the midpoint of the linked list, reversing the second half in place, and then comparing it with the first half. This avoids extra space usage for storing elements separately and works directly with the linked list.

**Detailed Steps:**

1. Use the slow and fast pointer technique to find the midpoint of the linked list.
2. Reverse the second half of the list starting from the midpoint.
3. Compare the two halves element by element.

**Code:**

```go
func isPalindrome(head *ListNode) bool {
    if head == nil || head.Next == nil {
        return true
    }
    
    // Step 1: Find the midpoint using slow and fast pointers
    slow, fast := head, head
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
    }
    
    // Step 2: Reverse the second half of the linked list
    secondHalf := reverseList(slow)
    firstHalf := head
    
    // Step 3: Compare the first and second halves
    result := true
    copySecondHalf := secondHalf // To restore later if needed
    for secondHalf != nil {
        if firstHalf.Val != secondHalf.Val {
            result = false
            break
        }
        firstHalf = firstHalf.Next
        secondHalf = secondHalf.Next
    }
    
    // (Optional) Restore the list by reversing the second half back
    reverseList(copySecondHalf)
    
    return result
}

// Helper function to reverse a linked list
func reverseList(head *ListNode) *ListNode {
    var prev *ListNode
    current := head
    for current != nil {
        next := current.Next
        current.Next = prev
        prev = current
        current = next
    }
    return prev
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n), the list is traversed twice: once to find the middle, and once to reverse and compare.
- **Space Complexity:** O(1), as the reversal is done in place.

### Approach 3: Recursive Check

**Intuition:**

Using recursion, we can reach the end of the list and then check in a backward manner if the list is a palindrome. The recursion uses the call stack to compare values from the back forward, which simulates the two-pointer technique.

**Code:**

```go
type recursiveHelper struct {
    left *ListNode
}

func isPalindrome(head *ListNode) bool {
    helper := &recursiveHelper{left: head}
    return checkRecursivePalindrome(helper, head)
}

func checkRecursivePalindrome(helper *recursiveHelper, right *ListNode) bool {
    if right == nil {
        return true
    }
    
    if !checkRecursivePalindrome(helper, right.Next) {
        return false
    }
    
    // Check values for the left and right pointers
    if helper.left.Val != right.Val {
        return false
    }
    
    // Move left pointer one step right
    helper.left = helper.left.Next
    return true
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n), since each node is visited once.
- **Space Complexity:** O(n), due to the recursive stack space.

Each approach is unique in handling the palindrome check with varied trade-offs between simplicity, space, and optimality.

