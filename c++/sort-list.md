# [LeetCode 148: Sort List](https://leetcode.com/problems/sort-list/)

## Table of Contents
- [Approach 1: Insertion Sort](#approach-1-insertion-sort)
- [Approach 2: Bottom-Up Merge Sort](#approach-2-bottom-up-merge-sort)
- [Approach 3: Top-Down Merge Sort](#approach-3-top-down-merge-sort)

## Approach 1: Insertion Sort

### Intuition:
Insertion Sort is a simple sorting algorithm that builds the final sorted list one item at a time. It is much less efficient on large lists than more advanced algorithms such as merge sort.

### Implementation:
```cpp
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        if (!head || !head->next) return head;
        
        ListNode* sorted = nullptr; // Initially, the sorted list is empty
        
        while (head) {
            ListNode* current = head; // Take the first element in the unsorted list
            head = head->next;       // Move head to the next element
            if (!sorted || sorted->val >= current->val) {
                current->next = sorted; // Insert at the start
                sorted = current;
            } else {
                ListNode* temp = sorted;
                // Traverse the sorted list to find where to insert
                while (temp->next && temp->next->val < current->val) {
                    temp = temp->next;
                }
                current->next = temp->next;
                temp->next = current;
            }
        }
        
        return sorted;
    }
};
```

### Complexity:
- **Time Complexity:** O(n^2), where n is the number of elements in the list. In the worst case, each insertion takes time proportional to the size of the entire already-sorted part of the list.
- **Space Complexity:** O(1), as we are sorting the list in place without extra space.

## Approach 2: Bottom-Up Merge Sort

### Intuition:
Bottom-up merge sort works by iteratively merging pairs of small sorted sublists, doubling the size of each sublist in each iteration until the whole list is sorted. This method avoids the overhead of recursive function calls.

### Implementation:
```cpp
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if (!head || !head->next) return head;

        // Calculate the total length of the list
        ListNode* curr = head;
        int list_size = 0;
        while (curr) {
            list_size++;
            curr = curr->next;
        }

        // Dummy node is used to facilitate the merge process
        ListNode* dummy = new ListNode(0);
        dummy->next = head;

        ListNode* tail;
        ListNode* left;
        ListNode* right;
        ListNode* nextSublist;

        for (int step = 1; step < list_size; step <<= 1) {
            curr = dummy->next;
            tail = dummy;

            while (curr) {
                left = curr;
                right = split(left, step);
                nextSublist = split(right, step);
                tail = merge(left, right, tail);
                curr = nextSublist;
            }
        }

        return dummy->next;
    }

    // Splits the list into two parts, first with size step or ends first
    ListNode* split(ListNode* head, int size) {
        ListNode* temp = head;
        for (int i = 1; temp && i < size; i++) {
            temp = temp->next;
        }
        if (!temp) return nullptr;

        ListNode* second = temp->next;
        temp->next = nullptr;
        return second;
    }

    // Merge two sorted lists and attach to tail
    ListNode* merge(ListNode* l1, ListNode* l2, ListNode* tail) {
        ListNode* curr = tail;
        while (l1 && l2) {
            if (l1->val < l2->val) {
                curr->next = l1;
                l1 = l1->next;
            } else {
                curr->next = l2;
                l2 = l2->next;
            }
            curr = curr->next;
        }
        curr->next = l1 ? l1 : l2;
        while (curr->next) {
            curr = curr->next;
        }
        return curr;
    }
};
```

### Complexity:
- **Time Complexity:** O(n log n), as merge sort divides the list into half at each step and performs merge operation in linear time.
- **Space Complexity:** O(1), since we are operating in place.

## Approach 3: Top-Down Merge Sort

### Intuition:
Top-Down Merge Sort is a classic divide-and-conquer algorithm. For each recursion, it splits the current list into two halves, recursively sorts them, and then merges the two sorted halves.

### Implementation:
```cpp
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if (!head || !head->next) return head;
        
        // Get the middle of the linked list
        ListNode* mid = getMid(head);
        ListNode* left = sortList(head);       // Sort the left half
        ListNode* right = sortList(mid);       // Sort the right half

        return merge(left, right);             // Merge sorted halves
    }
    
    // Function to split the list into two halves
    ListNode* getMid(ListNode* head) {
        if (!head) return head;

        ListNode* slow = head;
        ListNode* fast = head->next;

        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        ListNode* mid = slow->next;
        slow->next = nullptr;
        return mid;
    }

    // Merge two sorted lists into one sorted list
    ListNode* merge(ListNode* list1, ListNode* list2) {
        ListNode dummy(0);
        ListNode* tail = &dummy;

        while (list1 && list2) {
            if (list1->val < list2->val) {
                tail->next = list1;
                list1 = list1->next;
            } else {
                tail->next = list2;
                list2 = list2->next;
            }
            tail = tail->next;
        }
        tail->next = (list1) ? list1 : list2;
        return dummy.next;
    }
};
```

### Complexity:
- **Time Complexity:** O(n log n), typical for merge sort due to log(n) splits and n merges.
- **Space Complexity:** O(log n), used by the recursion stack.

