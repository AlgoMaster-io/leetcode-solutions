# 23. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Approach 1: Priority Queue (Min-Heap)

### Solution
c++
// Time Complexity: O(n * log(k)), where n is the total number of elements and k is the number of lists
// Space Complexity: O(k), for the priority queue
```cpp
#include <queue>
#include <vector>

using namespace std;

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
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        // Min-heap to store the current nodes of each list
        auto compare = [](ListNode* a, ListNode* b) { return a->val > b->val; };
        priority_queue<ListNode*, vector<ListNode*>, decltype(compare)> minHeap(compare);
        
        // Add the head of each non-empty list to the heap
        for (ListNode* list : lists) {
            if (list != nullptr) {
                minHeap.push(list);
            }
        }

        ListNode dummy(-1); // Dummy node for result
        ListNode* current = &dummy;

        while (!minHeap.empty()) {
            // Get the smallest node from the heap
            ListNode* smallest = minHeap.top();
            minHeap.pop();
            current->next = smallest; // Append it to the result
            current = current->next;

            // If the next node exists, add it to the heap
            if (smallest->next != nullptr) {
                minHeap.push(smallest->next);
            }
        }

        return dummy.next; // Return the merged list
    }
};
```

## Approach 2: Divide and Conquer

### Solution
c++
// Time Complexity: O(n * log(k)), where n is the total number of elements and k is the number of lists
// Space Complexity: O(log(k)), due to recursive calls
```cpp
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
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.empty()) {
            return nullptr;
        }
        return mergeLists(lists, 0, lists.size() - 1);
    }

private:
    ListNode* mergeLists(vector<ListNode*>& lists, int left, int right) {
        if (left == right) {
            return lists[left];
        }

        int mid = left + (right - left) / 2;
        ListNode* l1 = mergeLists(lists, left, mid);
        ListNode* l2 = mergeLists(lists, mid + 1, right);
        return mergeTwoLists(l1, l2);
    }

    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode dummy(-1);
        ListNode* current = &dummy;

        while (l1 != nullptr && l2 != nullptr) {
            if (l1->val < l2->val) {
                current->next = l1;
                l1 = l1->next;
            } else {
                current->next = l2;
                l2 = l2->next;
            }
            current = current->next;
        }

        if (l1 != nullptr) {
            current->next = l1;
        } else if (l2 != nullptr) {
            current->next = l2;
        }

        return dummy.next;
    }
};
```

