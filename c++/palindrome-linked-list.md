# [Leetcode 234: Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

## Approaches
1. [Naive Approach: Using Extra Space](#naive-approach-using-extra-space)
2. [Optimized Approach: Reverse Second Half In-Place](#optimized-approach-reverse-second-half-in-place)

---

## Naive Approach: Using Extra Space

The simplest way to solve the problem is by using an auxiliary data structure to store the values of the linked list nodes. This approach checks if the linked list is a palindrome by comparing the list of node values with its reverse.

### Intuition:
- Traverse the linked list to record its node values in a vector.
- Check if this vector reads the same forward and backward: the list is a palindrome if the vector equals its reverse.

### Code:

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        // Vector to store the values of the linked list
        std::vector<int> values;
        
        // Traverse the linked list and push the values to the vector
        ListNode* current = head;
        while (current != nullptr) {
            values.push_back(current->val);
            current = current->next;
        }
        
        // Check if the vector is a palindrome
        int left = 0;
        int right = values.size() - 1;
        while (left < right) {
            if (values[left] != values[right]) {
                return false;
            }
            left++;
            right--;
        }
        
        return true;
    }
};
```

### Time Complexity:
- O(n), where n is the number of nodes in the linked list, because we have to traverse all the nodes twice, once to fill the vector and once to check for palindrome.

### Space Complexity:
- O(n), where n is the number of nodes in the linked list, due to the additional space used by the vector.

---

## Optimized Approach: Reverse Second Half In-Place

By using the Fast and Slow pointer technique to find the midpoint of the linked list, we can reverse the second half of the list in place and then compare it with the first half to determine if the list is a palindrome.

### Intuition:
- Use a fast and slow pointer to find the midpoint of the linked list. Reverse the second half of the list starting from the midpoint.
- Compare the first half and the reversed second half. If every pair of nodes matches, the list is a palindrome.
- Optionally, restore the second half of the list to its original order.

### Code:

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if (!head || !head->next) return true;
        
        // Find the end of the first half and the start of the second half
        ListNode* firstHalfEnd = endOfFirstHalf(head);
        ListNode* secondHalfStart = reverseList(firstHalfEnd->next);
        
        // Compare the first half and the reversed second half
        ListNode* p1 = head;
        ListNode* p2 = secondHalfStart;
        bool result = true;
        while (result && p2) {
            if (p1->val != p2->val) result = false;
            p1 = p1->next;
            p2 = p2->next;
        }
        
        // Restore the list (optional)
        firstHalfEnd->next = reverseList(secondHalfStart);
        
        return result;
    }
    
private:
    // Reverse linked list starting at head
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        while (head) {
            ListNode* nextNode = head->next;
            head->next = prev;
            prev = head;
            head = nextNode;
        }
        return prev;
    }
    
    // Find the end of the first half - helper function
    ListNode* endOfFirstHalf(ListNode* head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (fast->next && fast->next->next) {
            fast = fast->next->next;
            slow = slow->next;
        }
        return slow;
    }
};
```

### Time Complexity:
- O(n), where n is the number of nodes in the linked list. We traverse the list potentially twice in full, once to find the midpoint and once to compare the halves.

### Space Complexity:
- O(1), since we are only using a fixed amount of extra space for pointers. The list is modified in place.

This approach is more space-efficient than the naive approach while maintaining linear time complexity.

