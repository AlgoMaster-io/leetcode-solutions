## [LeetCode Problem 148: Sort List](https://leetcode.com/problems/sort-list/)

### Approaches
1. [Iterative Merge Sort](#iterative-merge-sort)
2. [Recursive Merge Sort](#recursive-merge-sort)

---

### Iterative Merge Sort

**Intuition:**

The intuitive way to approach this problem with an optimal solution is using the merge sort algorithm, which has a time complexity of \(O(n \log n)\). The merge sort can be implemented iteratively. 

The idea is to repeatedly merge pairs of sublists of increasing lengths, doubling the length each time, until the entire list is sorted. An iterative version avoids the space overhead of recursion.

**Code:**

```javascript
function sortList(head) {
    if (!head || !head.next) return head;
    
    // Initial step size for merging is 1
    let step = 1;
    let tail;
    let dummy = new ListNode(0);
    
    dummy.next = head;
    
    while (true) {
        let current = dummy.next;
        tail = dummy;
        let flag = false;
        
        while (current) {
            let left = current;
            let right = split(left, step);
            current = split(right, step);
            tail = merge(left, right, tail);
            flag = true;
        }
        
        // If no further steps can be completed, break
        if (!flag) break;
        
        // Double the step size
        step *= 2;
    }
    
    return dummy.next;
    
    // Helper function to split the list into two halves and return the start of the second half
    function split(head, n) {
        for (let i = 1; head && head.next && i < n; i++) {
            head = head.next;
        }
        if (!head) return null;
        let second = head.next;
        head.next = null; // Split the list
        return second;
    }
    
    // Helper function to merge two sorted lists and attach it to the provided tail
    function merge(l1, l2, tail) {
        let current = tail;
        
        while (l1 && l2) {
            if (l1.val < l2.val) {
                current.next = l1;
                l1 = l1.next;
            } else {
                current.next = l2;
                l2 = l2.next;
            }
            current = current.next;
        }
        
        // Attach remaining elements
        current.next = l1 ? l1 : l2;
        
        // Move the current to the end of the merged list
        while (current.next) {
            current = current.next;
        }
        
        return current;
    }
}
```

**Complexity:**

- **Time Complexity:** \(O(n \log n)\), where \(n\) is the number of nodes in the linked list.
- **Space Complexity:** \(O(1)\), apart from the input list.


---

### Recursive Merge Sort

**Intuition:**

A more straightforward approach might be the recursive version of merge sort. The linked list will be split into two halves recursively until each sublist has one element, and then those sublists will be merged in a sorted manner.

This approach is less space-efficient due to the depth of the recursion stack but is easier to write and understand.

**Code:**

```javascript
function sortList(head) {
    if (!head || !head.next) return head;

    let mid = getMid(head);
    let left = sortList(head);
    let right = sortList(mid);

    return mergeTwoLists(left, right);
}

// Helper to split the list and return the start of the second half
function getMid(head) {
    let midPrev = null;
    while (head && head.next) {
        midPrev = midPrev === null ? head : midPrev.next;
        head = head.next.next;
    }
    let mid = midPrev.next;
    midPrev.next = null;
    return mid;
}

// Helper to merge two sorted lists
function mergeTwoLists(l1, l2) {
    let dummy = new ListNode(0);
    let current = dummy;
    
    while (l1 && l2) {
        if (l1.val < l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }
    current.next = l1 || l2;
    
    return dummy.next;
}
```

**Complexity:**

- **Time Complexity:** \(O(n \log n)\).
- **Space Complexity:** \(O(\log n)\) due to recursion stack space.

