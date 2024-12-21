# [Leetcode 25: Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

## Table of Contents
1. [Approach 1: Iterative with Extra Space](#approach-1)
2. [Approach 2: Recursive Solution](#approach-2)
3. [Approach 3: Iterative In-Place](#approach-3)

### Approach 1: Iterative with Extra Space

#### Intuition:
The idea is to use a list or vector to store nodes of every k-group found and reverse them using auxiliary space, and then reassemble the nodes back into the list.

#### Steps:
1. Traverse the linked list and store pointers in chunks of size k in a temporary list.
2. Reverse these chunks and append them together.
3. Any remaining nodes (less than k) are appended to the result as they are.

#### Time Complexity:
- O(N), where N is the number of nodes in the list.

#### Space Complexity:
- O(k) for storing nodes of the current k-group in extra space.

```cpp
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode dummy(0);
        ListNode *prev = &dummy;
        dummy.next = head;
        
        while (true) {
            ListNode *kStart = prev;
            ListNode *kEnd = prev;
            // Find the end of the current k-group
            for (int i = 0; i < k && kEnd != nullptr; i++) {
                kEnd = kEnd->next;
            }
            if (kEnd == nullptr) {
                break;
            }
            
            // Reverse group
            ListNode *curr = kStart->next;
            ListNode *next = nullptr;
            ListNode *prev1 = kEnd->next;
            while (curr != kEnd->next) {
                next = curr->next;
                curr->next = prev1;
                prev1 = curr;
                curr = next;
            }
            
            // Re-link the previous group's end to current group's new start
            next = kStart->next;
            kStart->next = prev1;
            prev = next;
        }
        return dummy.next;
    }
};
```

### Approach 2: Recursive Solution

#### Intuition:
This approach uses a recursive technique to reduce problems into smaller chunks. Reverse the first k nodes and call the function recursively for the rest of the list.

#### Steps:
1. Check if you have enough nodes (k) to reverse; if not, return head.
2. Reverse the first k nodes.
3. Recursively call the function for the rest and attach reversed part returned as a result.

#### Time Complexity:
- O(N), because each node is visited exactly once.

#### Space Complexity:
- O(N/k) due to recursive function calls on the call stack.

```cpp
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* curr = head;
        int count = 0;
        while (curr != nullptr && count < k) {
            curr = curr->next;
            count++;
        }
        
        if (count == k) {
            // Reverse first k nodes
            ListNode* reversedHead = reverseKGroup(curr, k);
            while (count-- > 0) {
                ListNode* temp = head->next;
                head->next = reversedHead;
                reversedHead = head;
                head = temp;
            }
            head = reversedHead;
        }
        return head;
    }
};
```

### Approach 3: Iterative In-Place

#### Intuition:
To improve on space, perform the reversal directly as you traverse the list, carefully reconnecting the nodes during the process.

#### Steps:
1. Use a dummy node to manage the new head connection.
2. Traverse and reverse nodes in groups of k in one pass.
3. Handle the connecting pointers carefully to maintain the integrity of the linked list.

#### Time Complexity:
- O(N), where N is the number of nodes in the list.

#### Space Complexity:
- O(1), as we are reversing in-place without using any significant extra space.

```cpp
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode dummy(0);
        dummy.next = head;
        ListNode *prev = &dummy, *curr = head, *next = nullptr;

        int length = 0;
        while (curr) {
            curr = curr->next;
            length++;
        }
        curr = head;

        while (length >= k) {
            next = curr->next;
            ListNode* last = prev->next;
            for (int i = 1; i < k; i++) {
                curr->next = next->next;
                next->next = prev->next;
                prev->next = next;
                next = curr->next;
            }
            prev = last;
            curr = prev->next;
            length -= k;
        }
        return dummy.next;
    }
};
```

Each approach provides a solution using different paradigms (iterative, recursive, in-place) to solve the problem of reversing nodes in k-groups, providing both qualitative insights into their use and computational efficiency metrics.

