# [LeetCode 23: Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Approaches
1. [Brute Force - Collect and Sort](#approach-1-brute-force---collect-and-sort)
2. [Min-Heap Approach](#approach-2-min-heap-approach)
3. [Divide and Conquer](#approach-3-divide-and-conquer)

---

## Approach 1: Brute Force - Collect and Sort

### Intuition
The simplest solution is to iterate through all the nodes of each list, collect them into a single list, and sort the resultant list. Once we have the sorted list of nodes, we can create a new linked list from it. This approach, however, is not efficient.

### Steps
1. Traverse each linked list and append all nodes to a vector.
2. Sort the vector.
3. Create a new linked list from the sorted values in the vector.

```cpp
#include <vector>
#include <algorithm>

// Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

class Solution {
public:
    ListNode* mergeKLists(std::vector<ListNode*>& lists) {
        std::vector<int> nodes;
        
        // Collect all the nodes' values
        for (auto list : lists) {
            while (list) {
                nodes.push_back(list->val);
                list = list->next;
            }
        }
        
        // Sort the collected values
        std::sort(nodes.begin(), nodes.end());
        
        // Create a new sorted linked list
        ListNode* dummy = new ListNode();
        ListNode* current = dummy;
        for (int val : nodes) {
            current->next = new ListNode(val);
            current = current->next;
        }
        
        return dummy->next;
    }
};
```

### Time Complexity
- O(N log N): Collecting nodes and then sorting them, where N is the total number of nodes.
- O(N): Constructing the linked list from the sorted values.

### Space Complexity
- O(N): Space used to collect node values.

---

## Approach 2: Min-Heap Approach

### Intuition
Using a min-heap (priority queue), we can efficiently keep track of the smallest current node among all the head nodes of the k lists. By using a heap, we can repeatedly extract the smallest node and maintain the order.

### Steps
1. Push the head of each list into the min-heap.
2. While the heap is not empty, extract the smallest node and move its pointer to the next node.
3. Push the next node of extracted node into the min-heap.
4. Append the extracted node to the dummy list.

```cpp
#include <queue>

class Solution {
public:
    ListNode* mergeKLists(std::vector<ListNode*>& lists) {
        auto compare = [](ListNode* a, ListNode* b) { return a->val > b->val; };
        std::priority_queue<ListNode*, std::vector<ListNode*>, decltype(compare)> minHeap(compare);
        
        for (auto list : lists) {
            if (list) {
                minHeap.push(list);
            }
        }
        
        ListNode* dummy = new ListNode();
        ListNode* current = dummy;
        
        while (!minHeap.empty()) {
            ListNode* smallest = minHeap.top();
            minHeap.pop();
            current->next = smallest;
            current = current->next;
            if (smallest->next) {
                minHeap.push(smallest->next);
            }
        }
        
        return dummy->next;
    }
};
```

### Time Complexity
- O(N log k): Each insertion and extraction operation takes log k time, because the heap size is at most k.

### Space Complexity
- O(k): Maximum space used by the heap is equal to k.

---

## Approach 3: Divide and Conquer

### Intuition
The divide and conquer strategy repeatedly merges pairs of lists, similar to how the merge step works in a merge sort algorithm. This greatly improves efficiency compared to the brute force approach.

### Steps
1. Pair and merge lists like a tournament until only one list is left.
2. Use a helper function to merge two sorted lists.

```cpp
class Solution {
public:
    ListNode* mergeKLists(std::vector<ListNode*>& lists) {
        if (lists.empty()) return nullptr;
        int interval = 1;
        int n = lists.size();
        while (interval < n) {
            for (int i = 0; i + interval < n; i += 2 * interval) {
                lists[i] = mergeTwoLists(lists[i], lists[i + interval]);
            }
            interval *= 2;
        }
        return lists[0];
    }

    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode dummy;
        ListNode* tail = &dummy;
        while (l1 && l2) {
            if (l1->val < l2->val) {
                tail->next = l1;
                l1 = l1->next;
            } else {
                tail->next = l2;
                l2 = l2->next;
            }
            tail = tail->next;
        }
        tail->next = l1 ? l1 : l2;
        return dummy.next;
    }
};
```

### Time Complexity
- O(N log k): Each list is merged log k times.

### Space Complexity
- O(1): The merge function uses constant space. Each recursion call uses stack space which is O(log k) due to divide and conquer.

