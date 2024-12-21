# Problem: [Leetcode 23: Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Approaches
1. [Basic Approach using Merge Two Lists](#basic-approach-using-merge-two-lists)
2. [Optimized Approach using Min-Heap](#optimized-approach-using-min-heap)
3. [Optimal Approach using Divide and Conquer](#optimal-approach-using-divide-and-conquer)

---

## Basic Approach using Merge Two Lists

### Intuition
The simplest approach to solve this problem is to merge the lists two at a time. We can take two lists at a time, merge them into a single sorted list, and repeat this process till all lists are merged. This approach can be inefficient due to repetitive scanning of the linked lists.

### Steps
1. Create a helper function to merge two sorted linked lists.
2. Use the helper function iteratively to merge all the linked lists one by one.

### Time Complexity
- O(k * n), where k is the number of lists and n is the total number of nodes when you merge all lists. This happens because each merging operation goes through the entirety of the current merged list.

### Space Complexity
- O(1), but the recursion stack can go up to O(n) if using a recursive merge function.

### Code
```csharp
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
        if (lists == null || lists.Length == 0) return null;
        
        ListNode mergedList = null;
        
        // Iteratively merge each list into the mergedList
        foreach (var list in lists) {
            mergedList = MergeTwoLists(mergedList, list);
        }
        
        return mergedList;
    }
    
    private ListNode MergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        
        // Go through both lists, adding the smallest element each time
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
        
        // Add remaining elements
        current.next = l1 ?? l2;
        
        return dummy.next;
    }
}
```

---

## Optimized Approach using Min-Heap

### Intuition
Using a min-heap (or priority queue) helps us keep track of the minimum elements effectively. By extracting the minimum element across the k lists efficiently, we can construct the final merged list with improved time complexity.

### Steps
1. Create a min-heap and insert the first node of each list into it.
2. Extract the smallest node from the heap and add it to the result list.
3. Insert the next node from the extracted node's list into the heap (if any).
4. Continue this until the heap is empty.

### Time Complexity
- O(n log k), where n is the total number of nodes across all lists, and k is the number of lists.
  This is because each element is added and removed from the heap, which has a logarithmic complexity concerning the number of lists.

### Space Complexity
- O(k), owing to the storage of the heap which will hold at most k nodes at any time.

### Code
```csharp
using System.Collections.Generic;

public class Solution {
    public ListNode MergeKLists(ListNode[] lists) {
        if (lists == null || lists.Length == 0) return null;
        
        // Min-Heap to keep track of the smallest elements
        var minHeap = new SortedDictionary<int, Queue<ListNode>>();
        
        // Add the head of each list to the min-heap
        foreach (var list in lists) {
            if (list != null) {
                if (!minHeap.ContainsKey(list.val)) {
                    minHeap[list.val] = new Queue<ListNode>();
                }
                minHeap[list.val].Enqueue(list);
            }
        }
        
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        
        while (minHeap.Count > 0) {
            var firstKey = minHeap.Keys.Min();
            var node = minHeap[firstKey].Dequeue();
            
            // Remove the list from dictionary if the queue is empty
            if (minHeap[firstKey].Count == 0) {
                minHeap.Remove(firstKey);
            }
            
            // Attach the node to the result
            current.next = node;
            current = current.next;
            
            // Add the next node from the list to the heap
            if (node.next != null) {
                if (!minHeap.ContainsKey(node.next.val)) {
                    minHeap[node.next.val] = new Queue<ListNode>();
                }
                minHeap[node.next.val].Enqueue(node.next);
            }
        }
        
        return dummy.next;
    }
}
```

---

## Optimal Approach using Divide and Conquer

### Intuition
To further optimize, we can employ a divide and conquer technique that effectively reduces the number of comparisons. The idea is similar to the merge sort algorithm.

### Steps
1. Pair up k lists and merge each pair.
2. After the first pairing, k/2 lists will remain. Continue the process of pairing and merging until only one list remains.
3. This significantly reduces the number of merge operations compared to the basic approach.

### Time Complexity
- O(n log k), similar to the min-heap approach where n is the total number of nodes and k is the number of lists.

### Space Complexity
- O(1) in terms of additional data structures, but recursive stack calls would still consume space.

### Code
```csharp
public class Solution {
    public ListNode MergeKLists(ListNode[] lists) {
        if (lists == null || lists.Length == 0) return null;
        
        return MergeKListsHelper(lists, 0, lists.Length - 1);
    }
    
    private ListNode MergeKListsHelper(ListNode[] lists, int left, int right) {
        if (left == right) return lists[left];
        
        int mid = left + (right - left) / 2;
        var l1 = MergeKListsHelper(lists, left, mid);
        var l2 = MergeKListsHelper(lists, mid + 1, right);
        
        return MergeTwoLists(l1, l2);
    }
    
    private ListNode MergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
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
        
        current.next = l1 ?? l2;
        
        return dummy.next;
    }
}
```

The divide and conquer method is highly efficient for this kind of problem, reducing the complexity and enhancing performance significantly over the naive approach.

