[Intersection of Two Linked Lists - LeetCode](https://leetcode.com/problems/intersection-of-two-linked-lists/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Hash Set](#approach-2-hash-set)
- [Approach 3: Two Pointers](#approach-3-two-pointers)

---

### Approach 1: Brute Force

**Intuition:**  
The simplest brute force approach is to compare each node from the first list to every node in the second list to see if they are the same (intersecting). We look at every possible pair of nodes to see if they match.

**Algorithm:**  
1. Iterate through each node of the first linked list.
2. For each node in the first list, iterate through each node in the second list.
3. Check if both nodes are identical. If they are, return the node as the intersection.
4. If no intersection node is found after iterating through both lists, return `nullptr`.

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* currentA = headA;
        // Iterate through each node in List A
        while (currentA != nullptr) {
            ListNode* currentB = headB;
            // For each node in List A, iterate through List B
            while (currentB != nullptr) {
                // Compare the nodes, see if they're the same
                if (currentA == currentB) {
                    return currentA; // Return the intersection node
                }
                currentB = currentB->next;
            }
            currentA = currentA->next;
        }
        return nullptr; // No intersection
    }
};
```

- **Time Complexity:** O(N * M), where N and M are the lengths of the two linked lists.
- **Space Complexity:** O(1), since no additional space is used.

---

### Approach 2: Hash Set

**Intuition:**  
We can optimize the brute force approach by storing nodes of one list in a hash set and then checking for intersection with the nodes of the other list. This reduces the complexity by leveraging the properties of a hash set.

**Algorithm:**  
1. Traverse list A and store each node in a hash set.
2. Traverse list B; check if any node exists in the hash set.
3. If a node is found in the hash set, return it as the intersection node.
4. If the traversal finishes with no match, return `nullptr`.

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        std::unordered_set<ListNode*> nodes_set;
        // Store nodes from list A in a hash set
        while (headA != nullptr) {
            nodes_set.insert(headA);
            headA = headA->next;
        }
        // Check for intersection in list B
        while (headB != nullptr) {
            if (nodes_set.count(headB)) {
                return headB; // Intersection found
            }
            headB = headB->next;
        }
        return nullptr; // No intersection
    }
};
```

- **Time Complexity:** O(N + M), where N and M are the lengths of the two linked lists.
- **Space Complexity:** O(N) or O(M), whichever is smaller, for storing nodes in the hash set.

---

### Approach 3: Two Pointers

**Intuition:**  
The most optimal solution is to use two pointers with a mathematical approach. By iterating both pointers through the lists and swapping their starts after the first pass, they will eventually meet at the intersection node if there is one.

**Algorithm:**  
1. Initialize two pointers, `pointerA` and `pointerB`, at the head of `listA` and `listB`.
2. Traverse the lists with both pointers.
3. When a pointer reaches the end of a list, redirect it to the head of the other list.
4. If the lists intersect, pointers will meet at the intersection node after at most 2 passes.
5. If there's no intersection, both pointers will eventually become `nullptr` simultaneously after 2 passes.

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (headA == nullptr || headB == nullptr) return nullptr;

        ListNode* pointerA = headA;
        ListNode* pointerB = headB;

        // Traverse both lists
        while (pointerA != pointerB) {
            // Redirect pointer A if it reaches the end
            pointerA = (pointerA == nullptr) ? headB : pointerA->next;
            // Redirect pointer B if it reaches the end
            pointerB = (pointerB == nullptr) ? headA : pointerB->next;
        }

        return pointerA; // Either intersection node or null
    }
};
```

- **Time Complexity:** O(N + M), where N and M are the lengths of the two linked lists.
- **Space Complexity:** O(1), since only constant extra space is used.

