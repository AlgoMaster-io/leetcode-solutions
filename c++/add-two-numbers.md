# [Leetcode Problem 2: Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

## Approaches
- [Approach 1: Iterative Addition of Nodes](#approach-1-iterative-addition-of-nodes)

---

## Approach 1: Iterative Addition of Nodes
The problem involves adding two numbers represented by linked lists, where each node contains a single digit. The digits are stored in reverse order, meaning the head of the list represents the least significant digit.

### Intuition
We iterate through both linked lists, adding the digits node by node. Given the least significant digit is at the head, straightforward addition with carry management simulates how addition is done manually (from right to left). To handle different lengths of the input lists, assume missing nodes contain the value 0.

### Detailed Steps
1. Initialize a dummy node which helps in simplifying the operations. It acts as a placeholder to easily return the resultant list.
2. Set up a pointer to iterate over the nodes, starting at the dummy node.
3. Initialize a `carry` to hold the carry-over during the summation process, initially set to 0.
4. Iterate through each corresponding pair of nodes in both linked lists while there are nodes to process or carry > 0:
   - Extract the values from the current nodes of both lists if they exist; otherwise, consider 0.
   - Compute the sum of the two values, along with any carry from the previous step.
   - Use modulus to determine the new carry and the digit to store in the current node.
   - Move to the next nodes in each list and the next position in the result list.
5. Once done, return the next pointer of the dummy node which points to the head of the resultant list.

### Code

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        // Step 1: Create a dummy node to simplify the process and hold the result list.
        ListNode* dummy = new ListNode(0);
        ListNode* current = dummy;
        
        // Step 2: Initialize carry to handle carry over during addition.
        int carry = 0;

        // Step 3: Iterate while there are nodes to process or we still have a carry.
        while (l1 != nullptr || l2 != nullptr || carry != 0) {
            int sum = carry;
            
            // Step 4: Add values from l1 and l2 if they exist.
            if (l1 != nullptr) {
                sum += l1->val;
                l1 = l1->next;
            }
            
            if (l2 != nullptr) {
                sum += l2->val;
                l2 = l2->next;
            }
            
            // Step 5: Update carry for next iteration.
            carry = sum / 10;

            // Step 6: Create a new node for the current digit of the result.
            current->next = new ListNode(sum % 10);
            current = current->next;
        }
        
        // Step 7: The dummy's next node is the head of the resultant list.
        return dummy->next;
    }
};
```

### Complexity Analysis
- **Time Complexity**: O(max(N, M)), where N is the number of nodes in `l1` and M is the number of nodes in `l2`. We iterate once over each list.
- **Space Complexity**: O(max(N, M)), for storing the output linked list. Each node contributes to the overall space usage.

