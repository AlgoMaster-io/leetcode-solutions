# 23. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Approach 1: Priority Queue (Min-Heap)

### Solution
csharp
```csharp
// Time Complexity: O(n * log(k)), where n is the total number of elements and k is the number of lists
// Space Complexity: O(k), for the priority queue
using System.Collections.Generic;

public class ListNode {
    public int val;
    public ListNode next;
    public ListNode(int val = 0, ListNode next = null) {
        this.val = val;
        this.next = next;
    }
}

public class Solution {
    public ListNode MergeKLists(ListNode[] lists) {
        // Min-heap to store the current nodes of each list
        PriorityQueue<ListNode, int> minHeap = new PriorityQueue<ListNode, int>();
        
        // Add the head of each non-empty list to the heap
        foreach (var list in lists) {
            if (list != null) {
                minHeap.Enqueue(list, list.val);
            }
        }

        ListNode dummy = new ListNode(-1); // Dummy node for result
        ListNode current = dummy;

        while (minHeap.Count > 0) {
            // Get the smallest node from the heap
            ListNode smallest = minHeap.Dequeue();
            current.next = smallest; // Append it to the result
            current = current.next;

            // If the next node exists, add it to the heap
            if (smallest.next != null) {
                minHeap.Enqueue(smallest.next, smallest.next.val);
            }
        }

        return dummy.next; // Return the merged list
    }
}
```

## Approach 2: Divide and Conquer

### Solution
csharp
```csharp
// Time Complexity: O(n * log(k)), where n is the total number of elements and k is the number of lists
// Space Complexity: O(log(k)), due to recursive calls
public class ListNode {
    public int val;
    public ListNode next;
    public ListNode(int val = 0, ListNode next = null) {
        this.val = val;
        this.next = next;
    }
}

public class Solution {
    public ListNode MergeKLists(ListNode[] lists) {
        if (lists == null || lists.Length == 0) {
            return null;
        }
        return MergeLists(lists, 0, lists.Length - 1);
    }

    private ListNode MergeLists(ListNode[] lists, int left, int right) {
        if (left == right) {
            return lists[left];
        }

        int mid = left + (right - left) / 2;
        ListNode l1 = MergeLists(lists, left, mid);
        ListNode l2 = MergeLists(lists, mid + 1, right);
        return MergeTwoLists(l1, l2);
    }

    private ListNode MergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode current = dummy;

        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                current.next = l1;
                l1 = l1.next;
            } else {
                current.next = l2;
                l2 = l2.next;
            }
            current = current.next;
        }

        if (l1 != null) {
            current.next = l1;
        } else if (l2 != null) {
            current.next = l2;
        }

        return dummy.next;
    }
}
```

