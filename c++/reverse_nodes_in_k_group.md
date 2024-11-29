# [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

## Approach: Iterative Solution with Dummy Node

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        if (head == nullptr || k == 1) {
            return head;
        }

        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* prevGroup = dummy;

        while (true) {
            // Find the k-th node from prevGroup
            ListNode* kthNode = getKthNode(prevGroup, k);
            if (kthNode == nullptr) {
                break; // Not enough nodes to reverse
            }

            ListNode* nextGroup = kthNode->next; // Node after the k-th node
            ListNode* prev = nextGroup; // Start reversal with nextGroup as tail
            ListNode* current = prevGroup->next;

            // Reverse k nodes
            while (current != nextGroup) {
                ListNode* temp = current->next;
                current->next = prev;
                prev = current;
                current = temp;
            }

            // Update the connections
            ListNode* temp = prevGroup->next;
            prevGroup->next = kthNode;
            prevGroup = temp;
        }

        return dummy->next;
    }

private:
    ListNode* getKthNode(ListNode* start, int k) {
        while (start != nullptr && k > 0) {
            start = start->next;
            k--;
        }
        return start;
    }
};
```

