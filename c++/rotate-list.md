# [Leetcode 61: Rotate List](https://leetcode.com/problems/rotate-list/)

## Approaches
- [Approach 1: Brute Force Solution](#approach-1-brute-force-solution)
- [Approach 2: Two-Pass Solution with Linked List Closure](#approach-2-two-pass-solution-with-linked-list-closure)

## Approach 1: Brute Force Solution

### Intuition
The brute force method involves rotating the linked list by one position k times. Each rotation involves sequentially making the last element the head of the list. This method doesn't optimize the number of iterations or the bounds of k which can be kept within the list length.

### Solution

```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head || !head->next || k == 0) return head;

        int length = 0;
        ListNode* temp = head;
        while (temp) {
            length++;
            temp = temp->next;
        }

        k = k % length;
        if (k == 0) return head;

        for (int i = 0; i < k; i++) {
            ListNode* temp = head;
            // Move to the second last node
            while (temp->next->next) {
                temp = temp->next;
            }
            // Rotate the list
            ListNode* lastNode = temp->next;
            temp->next = nullptr;
            lastNode->next = head;
            head = lastNode;
        }
        return head;
    }
};
```

### Time Complexity
- Worst case O(n * k), where n is the number of nodes in the list and k is the number of rotations.

### Space Complexity
- O(1) as no extra space is used.

## Approach 2: Two-Pass Solution with Linked List Closure

### Intuition
A more efficient way leverages the insight that rotating the list k times is equivalent to breaking the list after the (length - k % length)th node and rejoining the list in a cyclic manner. 

### Solution

```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head || !head->next || k == 0) return head;

        // First pass to find the length of list and connect last node to the first node
        int length = 1;
        ListNode* lastNode = head;
        while (lastNode->next) {
            lastNode = lastNode->next;
            length++;
        }
        
        // Connect the tail to head to make the list circular
        lastNode->next = head;

        // Find the new tail, which will be at length - k % length
        int stepsToNewHead = length - k % length;
        ListNode* newTail = head;
        
        // Find the new tail node
        for (int i = 1; i < stepsToNewHead; i++) {
            newTail = newTail->next;
        }

        // Set the new head and break the loop
        ListNode* newHead = newTail->next;
        newTail->next = nullptr;

        return newHead;
    }
};
```

### Time Complexity
- O(n), where n is the number of nodes in the list. We pass twice through the list to find the length and then to locate the new head.

### Space Complexity
- O(1) as no additional storage is needed.

