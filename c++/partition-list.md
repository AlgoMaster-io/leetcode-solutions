# [Leetcode 86: Partition List](https://leetcode.com/problems/partition-list/)

## Approaches
1. [Two Pointers with Two Separate Lists](#approach-1)
2. [Two-Pointer with Single Scan and Rearrange](#approach-2)

---

### Approach 1: Two Pointers with Two Separate Lists

#### Intuition:
The problem asks us to partition a linked list into two lists: nodes with values less than `x`, and nodes with values greater than or equal to `x`, while preserving the relative order of the nodes in each of these lists. An intuitive approach is to maintain two separate lists, and then connect them at the end.

#### Steps:
1. Initialize two new linked lists, `leftList` and `rightList`, which will store the nodes less than `x` and nodes greater than or equal to `x` respectively.
2. Use two pointers, `left` and `right`, to keep track of the current end of each list.
3. Traverse the original list:
   - If a node’s value is less than `x`, append it to `left`.
   - Otherwise, append it to `right`.
4. Connect the end of the `left` list to the beginning of the `right` list.
5. Return the merged list starting from the head of `leftList`.

#### Implementation:
```cpp
ListNode* partition(ListNode* head, int x) {
    ListNode leftHead(0), rightHead(0); // Dummy nodes for left and right partitions
    ListNode* left = &leftHead;
    ListNode* right = &rightHead;

    while (head != nullptr) {
        if (head->val < x) {
            left->next = head; // Append node to left partition
            left = left->next; // Move left pointer
        } else {
            right->next = head; // Append node to right partition
            right = right->next; // Move right pointer
        }
        head = head->next; // Move to the next node in the list
    }

    right->next = nullptr; // Terminate the right partition
    left->next = rightHead.next; // Append the right partition after left partition

    return leftHead.next; // Return the head of the modified list
}
```

**Time Complexity:** O(n) - We iterate over the list exactly once.

**Space Complexity:** O(1) - We use constant extra space outside of the input and output lists.

---

### Approach 2: Two-Pointer with Single Scan and Rearrange

#### Intuition:
We use two pointers strategy in one single scan of the list but rearrange the nodes in place by splitting them into two parts and rearranging them with effective node connections, thereby making this approach a bit more efficient in terms of contextual understanding of elements' arrangement.

#### Steps:
1. Create two pointers, `beforeHead` and `afterHead`, to keep track of two partitions.
2. Traverse each node using a `current` pointer.
   - If the value is less than `x`, append it to the `before` partition.
   - Otherwise, append it to the `after` partition.
3. Connect the last node of the `before` partition to the start of the `after` partition to form the final rearranged list.

#### Implementation:
```cpp
ListNode* partition(ListNode* head, int x) {
    ListNode* beforeHead = new ListNode(0);
    ListNode* before = beforeHead;
    ListNode* afterHead = new ListNode(0);
    ListNode* after = afterHead;

    while (head != nullptr) {
        if (head->val < x) {
            before->next = head;
            before = before->next;
        } else {
            after->next = head;
            after = after->next;
        }
        head = head->next;
    }

    after->next = nullptr; // To avoid cycles in linked list
    before->next = afterHead->next; // Connect partitions

    ListNode* newHead = beforeHead->next; // The head of the modified list
    delete beforeHead; // Release the dummy node memory
    delete afterHead;

    return newHead;
}
```

**Time Complexity:** O(n) - Linear traversal of the list.

**Space Complexity:** O(1) - Uses only a fixed number of additional pointers.

