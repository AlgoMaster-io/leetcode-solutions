## [Leetcode Problem 23: Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

### Approaches:
- [Approach 1: Brute Force Approach](#approach-1-brute-force-approach)
- [Approach 2: Min Heap / Priority Queue](#approach-2-min-heap-priority-queue)
- [Approach 3: Divide and Conquer](#approach-3-divide-and-conquer)

---

### Approach 1: Brute Force Approach

#### Intuition:
The simplest way to merge k sorted linked lists is to collect all the nodes from each list and then sort them based on their values. After sorting, create a new linked list by connecting all these sorted nodes.

#### Steps:
1. Traverse each linked list and collect all the nodes in a list.
2. Sort the list of nodes based on their values.
3. Create a new linked list using the sorted list of nodes.

#### Time Complexity:
- Sorting the nodes will take O(n log n), where n is the total number of nodes.
- Collecting nodes will take O(n).
- Overall complexity: **O(n log n)**

#### Space Complexity:
- We are using additional space to store the nodes, so the space complexity is **O(n)**.

```typescript
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.next = (next === undefined ? null : next);
    }
}

function mergeKLists(lists: Array<ListNode | null>): ListNode | null {
    const nodes: ListNode[] = [];

    for (const list of lists) {
        let node = list;
        // Collect all nodes from each list
        while (node != null) {
            nodes.push(node);
            node = node.next;
        }
    }

    // Sort the nodes based on their value
    nodes.sort((a, b) => a.val - b.val);

    // Connect the sorted nodes
    const dummy = new ListNode(0);
    let current = dummy;
    for (const node of nodes) {
        current.next = node;
        current = current.next;
    }

    // Ensure the last node points to null
    current.next = null;

    return dummy.next;
}
```

---

### Approach 2: Min Heap / Priority Queue

#### Intuition:
Utilizing a priority queue allows us to efficiently track and merge the k sorted linked lists by always choosing the smallest available value.

#### Steps:
1. Initialize a min heap (priority queue) to keep track of the smallest node among k lists.
2. Insert the first node of each list into the heap.
3. Repeatedly extract the smallest node from the heap, attach it to the merged list, and insert the next node from the extracted node's list into the heap.
4. Continue this process until the heap is empty.

#### Time Complexity:
- Inserting each node into the heap and extracting will take **O(log k)**.
- We process all n nodes, overall complexity: **O(n log k)**

#### Space Complexity:
- The heap stores at most k nodes at any time, so the space complexity is **O(k)**.

```typescript
import { MinPriorityQueue } from '@datastructures-js/priority-queue';

function mergeKLists(lists: Array<ListNode | null>): ListNode | null {
    // Min-heap (priority queue) to store nodes
    const minHeap = new MinPriorityQueue<ListNode>({ priority: node => node.val });

    for (const list of lists) {
        if (list !== null) {
            minHeap.enqueue(list);
        }
    }

    const dummy = new ListNode(0);
    let current = dummy;

    while (!minHeap.isEmpty()) {
        // Extract the smallest node
        const node = minHeap.dequeue().element;
        current.next = node;
        current = current.next;

        // Add the next node from the list to the heap
        if (node.next !== null) {
            minHeap.enqueue(node.next);
        }
    }

    return dummy.next;
}
```

---

### Approach 3: Divide and Conquer

#### Intuition:
By merging pairs of linked lists via a divide and conquer strategy, we can efficiently merge all k lists. This method is similar to the merge step in merge sort, providing a balanced approach to the problem.

#### Steps:
1. Pair and merge the linked lists recursively until a single list remains:
   - Recursively merge the first half of the list.
   - Recursively merge the second half of the list.
   - Merge the two halves.

#### Time Complexity:
- Each merge operation takes O(n) where n is the total length of lists being merged.
- Overall Complexity: **O(n log k)**

#### Space Complexity:
- Recursion uses stack space proportional to log k, thus the space complexity is **O(log k)**.

```typescript
function mergeKLists(lists: Array<ListNode | null>): ListNode | null {
    if (lists.length === 0) return null;

    return mergeLists(lists, 0, lists.length - 1);

    function mergeLists(lists: Array<ListNode | null>, start: number, end: number): ListNode | null {
        if (start === end) {
            return lists[start];
        }

        const mid = Math.floor((start + end) / 2);
        const left = mergeLists(lists, start, mid);
        const right = mergeLists(lists, mid + 1, end);

        return mergeTwoLists(left, right);
    }

    function mergeTwoLists(l1: ListNode | null, l2: ListNode | null): ListNode | null {
        const dummy = new ListNode(0);
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
        }
        if (l2 !== null) {
            current.next = l2;
        }

        return dummy.next;
    }
}
```

