# [Leetcode 23: Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Priority Queue (Min-Heap) Approach](#priority-queue-min-heap-approach)
3. [Divide and Conquer Approach](#divide-and-conquer-approach)

## Brute Force Approach

### Intuition
The most straightforward approach is to merge all given linked lists into a single list, then extract all the elements and sort them. After sorting, recreate the linked list in the sorted order. This approach is simple to implement but is not the most efficient in terms of time complexity.

### Steps
1. Traverse each linked list and add all elements to an array.
2. Sort the array.
3. Create a new linked list from sorted elements.

### Code
```javascript
function mergeKLists(lists) {
    // Create an array to hold all the nodes' values
    let nodes = [];
    
    // Traverse through each list
    for (let i = 0; i < lists.length; i++) {
        let current = lists[i];
        // Collect every node's value into nodes array
        while (current !== null) {
            nodes.push(current.val);
            current = current.next;
        }
    }
    
    // Sort the array of node values
    nodes.sort((a, b) => a - b);
    
    // Create a dummy head for the resulting linked list
    let dummy = new ListNode();
    let current = dummy;
    
    // Construct the new sorted linked list
    for (let i = 0; i < nodes.length; i++) {
        current.next = new ListNode(nodes[i]);
        current = current.next;
    }
    
    // Return the head of the merged linked list
    return dummy.next;
}
```

### Time Complexity
- Time: \(O(N \log N)\), where \(N\) is the total number of nodes across all lists (due to sorting).
- Space: \(O(N)\) for creating the array storing all node values.

## Priority Queue (Min-Heap) Approach

### Intuition
Using a min-heap allows us to easily keep track of the smallest node among the heads of the k lists. We can incrementally build the merged linked list by always choosing the smallest node from the heap and adding the next element from that list back into the heap.

### Steps
1. Push the head of each list into a min-heap.
2. Extract the smallest element from the heap, add it to the merged list, and push the next element from the extracted node's list to the heap.
3. Continue until the heap is empty.

### Code
```javascript
function mergeKLists(lists) {
    // Min-Heap comparison function
    const compare = (a, b) => a.val - b.val;

    // Create a min-heap
    const heap = new MinPriorityQueue({ compare });

    // Push the head of each list to the heap
    for (let i = 0; i < lists.length; i++) {
        if (lists[i] !== null) {
            heap.enqueue(lists[i]);
        }
    }

    let dummy = new ListNode();
    let current = dummy;

    // While the heap is not empty
    while (!heap.isEmpty()) {
        let node = heap.dequeue();
        current.next = node;
        current = current.next;

        // Add the next node from the list to the heap
        if (node.next !== null) {
            heap.enqueue(node.next);
        }
    }

    return dummy.next;
}
```

### Time Complexity
- Time: \(O(N \log k)\), where \(N\) is the total number of nodes and \(k\) is the number of lists.
- Space: \(O(k)\) for the heap.

## Divide and Conquer Approach

### Intuition
We can optimize further by using the divide and conquer paradigm, similar to the merge sort algorithm. Recursively split the set of lists into two halves, merge each half, and then merge the results.

### Steps
1. Divide the list of linked lists into two halves.
2. Recursively merge the two halves.
3. Merge two sorted lists into one, repeatedly.

### Code
```javascript
function mergeKLists(lists) {
    if (lists.length === 0) return null;

    // Helper function to merge two sorted lists
    const mergeTwoLists = (l1, l2) => {
        let dummy = new ListNode();
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
        if (l1 !== null) current.next = l1;
        if (l2 !== null) current.next = l2;

        return dummy.next;
    };

    const merge = (lists, start, end) => {
        if (start === end) return lists[start];
        let mid = Math.floor((start + end) / 2);
        let left = merge(lists, start, mid);
        let right = merge(lists, mid + 1, end);
        return mergeTwoLists(left, right);
    };

    return merge(lists, 0, lists.length - 1);
}
```

### Time Complexity
- Time: \(O(N \log k)\), where \(N\) is the total number of nodes and \(k\) is the number of lists.
- Space: \(O(\log k)\) due to recursive stack space.

