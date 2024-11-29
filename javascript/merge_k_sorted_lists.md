# 23. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Approach 1: Priority Queue (Min-Heap)

### Solution
```javascript
// Time Complexity: O(n * log(k)), where n is the total number of elements and k is the number of lists
// Space Complexity: O(k), for the priority queue

class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

var mergeKLists = function(lists) {
    const minHeap = new MinPriorityQueue({ priority: node => node.val });
    
    for (let list of lists) {
        if (list !== null) {
            minHeap.enqueue(list);
        }
    }
    
    let dummy = new ListNode(-1);
    let current = dummy;
    
    while (!minHeap.isEmpty()) {
        let smallest = minHeap.dequeue().element;
        current.next = smallest;
        current = current.next;
        
        if (smallest.next !== null) {
            minHeap.enqueue(smallest.next);
        }
    }
    
    return dummy.next;
};

// Assuming a basic MinHeap or using a library such as 'datastructures-js/min-priority-queue'
```

## Approach 2: Divide and Conquer

### Solution
```javascript
// Time Complexity: O(n * log(k)), where n is the total number of elements and k is the number of lists
// Space Complexity: O(log(k)), due to recursive calls

class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

var mergeKLists = function(lists) {
    if (lists === null || lists.length === 0) {
        return null;
    }
    return mergeLists(lists, 0, lists.length - 1);
};

var mergeLists = function(lists, left, right) {
    if (left === right) {
        return lists[left];
    }
    
    let mid = left + Math.floor((right - left) / 2);
    let l1 = mergeLists(lists, left, mid);
    let l2 = mergeLists(lists, mid + 1, right);
    return mergeTwoLists(l1, l2);
};

var mergeTwoLists = function(l1, l2) {
    let dummy = new ListNode(-1);
    let current = dummy;
    
    while (l1 !== null && l2 !== null) {
        if (l1.val < l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }
    
    if (l1 !== null) {
        current.next = l1;
    } else if (l2 !== null) {
        current.next = l2;
    }
    
    return dummy.next;
};
```


