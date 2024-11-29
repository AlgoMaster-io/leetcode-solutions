# 23. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Approach 1: Priority Queue (Min-Heap)

### Solution
typescript
```typescript
// Time Complexity: O(n * log(k)), where n is the total number of elements and k is the number of lists
// Space Complexity: O(k), for the priority queue

class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = val === undefined ? 0 : val;
        this.next = next === undefined ? null : next;
    }
}

function mergeKLists(lists: Array<ListNode | null>): ListNode | null {
    const minHeap: ListNode[] = [];

    // Function to add a node into the heap, maintaining the heap property
    const addNodeToHeap = (node: ListNode) => {
        minHeap.push(node);
        let i = minHeap.length - 1;
        while (i > 0) {
            const parentIndex = Math.floor((i - 1) / 2);
            if (minHeap[parentIndex].val <= minHeap[i].val) break;
            [minHeap[parentIndex], minHeap[i]] = [minHeap[i], minHeap[parentIndex]];
            i = parentIndex;
        }
    };

    // Function to remove and return the smallest node from the heap
    const pollNodeFromHeap = (): ListNode | null => {
        if (minHeap.length === 0) return null;
        const result = minHeap[0];
        const lastNode = minHeap.pop();
        if (minHeap.length > 0 && lastNode) {
            minHeap[0] = lastNode;
            let i = 0;
            while (i * 2 + 1 < minHeap.length) {
                let minChildIndex = i * 2 + 1;
                if (minChildIndex + 1 < minHeap.length && minHeap[minChildIndex + 1].val < minHeap[minChildIndex].val) {
                    minChildIndex++;
                }
                if (minHeap[i].val <= minHeap[minChildIndex].val) break;
                [minHeap[i], minHeap[minChildIndex]] = [minHeap[minChildIndex], minHeap[i]];
                i = minChildIndex;
            }
        }
        return result;
    };

    for (const list of lists) {
        if (list) {
            addNodeToHeap(list);
        }
    }

    const dummy = new ListNode(-1);
    let current = dummy;

    while (minHeap.length > 0) {
        const smallest = pollNodeFromHeap();
        if (smallest) {
            current.next = smallest;
            current = current.next;
            if (smallest.next) {
                addNodeToHeap(smallest.next);
            }
        }
    }

    return dummy.next;
}
```

## Approach 2: Divide and Conquer

### Solution
typescript
```typescript
// Time Complexity: O(n * log(k)), where n is the total number of elements and k is the number of lists
// Space Complexity: O(log(k)), due to recursive calls

class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = val === undefined ? 0 : val;
        this.next = next === undefined ? null : next;
    }
}

function mergeKLists(lists: Array<ListNode | null>): ListNode | null {
    if (!lists || lists.length === 0) {
        return null;
    }
    return mergeLists(lists, 0, lists.length - 1);
}

function mergeLists(lists: Array<ListNode | null>, left: number, right: number): ListNode | null {
    if (left === right) {
        return lists[left];
    }

    const mid = Math.floor(left + (right - left) / 2);
    const l1 = mergeLists(lists, left, mid);
    const l2 = mergeLists(lists, mid + 1, right);

    return mergeTwoLists(l1, l2);
}

function mergeTwoLists(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    const dummy = new ListNode(-1);
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
}
```

