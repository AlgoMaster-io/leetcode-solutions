# [LeetCode 21: Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

## Table of Contents
- [Approach 1: Iterative Approach](#approach-1-iterative-approach)
- [Approach 2: Recursive Approach](#approach-2-recursive-approach)

---

## Approach 1: Iterative Approach

### Intuition
The problem requires merging two sorted linked lists into one sorted list. An efficient way is to create a dummy node that helps in easily manipulating the list without worrying about edge cases like empty lists. We maintain a tail pointer that tracks the current end of the sorted list.

### Detailed Explanation
1. **Initialize a Dummy Node**: 
   - This node helps simplify the merging process by providing a starting point.

2. **Traverse Both Lists**:
   - Compare the current nodes of both lists.
   - Attach the smaller node to the tail of the merged list.
   - Move the pointer of that list to the next node.
   - Move the tail pointer to its next node.

3. **Attach Remaining Nodes**:
   - Once one of the lists is exhausted, simply attach the remaining part of the other list because it is already sorted.

4. **Return the Merged List**:
   - The merged list will be the `next` of the dummy node because the dummy node itself was just a placeholder.

### Code
```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    // Create a dummy node to simplify merging process
    ListNode dummy(0);
    ListNode* tail = &dummy;

    // Loop until one of the lists is exhausted
    while (l1 != nullptr && l2 != nullptr) {
        // Compare current nodes and link the smaller one
        if (l1->val < l2->val) {
            tail->next = l1;
            l1 = l1->next;
        } else {
            tail->next = l2;
            l2 = l2->next;
        }
        // Move the tail to the new end of the merged list
        tail = tail->next;
    }

    // Attach the remaining nodes
    if (l1 != nullptr) tail->next = l1;
    if (l2 != nullptr) tail->next = l2;

    // Return the merged list
    return dummy.next;
}
```

### Time and Space Complexity
- **Time Complexity**: O(n + m) - where n and m are the lengths of the two lists, as we iterate through both.
- **Space Complexity**: O(1) - as we use a constant amount of additional space.

---

## Approach 2: Recursive Approach

### Intuition
In recursion, when trying to sort or merge structures like lists, a common approach is to recursively handle smaller subproblems till base conditions are met. Here, we'll take advantage of the sorted nature of the two lists.

### Detailed Explanation
1. **Base Case**:
   - If either `l1` or `l2` is null, return the other list because no further merging is required.
   
2. **Recursive Case**:
   - Compare the heads of `l1` and `l2`.
   - The head of the merged list will be the smaller of the two heads.
   - Recursively merge the rest of the nodes.
   
3. **Build the List**:
   - Attach the result of the recursive call to the next of the selected node.

### Code
```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    // Base cases
    if (l1 == nullptr) return l2;
    if (l2 == nullptr) return l1;
    
    // Recursive case
    if (l1->val < l2->val) {
        l1->next = mergeTwoLists(l1->next, l2);
        return l1;
    } else {
        l2->next = mergeTwoLists(l1, l2->next);
        return l2;
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O(n + m) - where n and m are the lengths of the two lists.
- **Space Complexity**: O(n + m) - due to the recursion stack that stores values for each function call. This can also be directly observed from the number of recursive calls made.

By using these approaches, we can effectively merge two sorted lists in a new sorted list, utilizing iterative and recursive strategies to handle the problem with ease.

