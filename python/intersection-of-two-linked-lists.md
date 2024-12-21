# [Leetcode 160: Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: HashSet](#approach-2-hashset)
- [Approach 3: Two Pointers](#approach-3-two-pointers)

### Approach 1: Brute Force

#### Intuition
The most straightforward approach is to take each node from the first linked list and compare it with every node of the second linked list to see if they match. If a match is found, that node is the start of the intersection.

#### Steps:
1. Iterate through each node of the first list.
2. For each node in the first list, iterate through every node in the second list.
3. If a node in the first list is the same as a node in the second list, return that node as the intersection.
4. If no intersection is found, return `None`.

#### Time Complexity:
- **O(m * n)** where m and n are the lengths of the two linked lists. This results from the nested loops.

#### Space Complexity:
- **O(1)** as no extra space is used except for pointer variables.

```python
def getIntersectionNode(headA, headB):
    # Go through each node in list A
    currentA = headA
    while currentA:
        # Go through each node in list B
        currentB = headB
        while currentB:
            # If they match, we found the intersection
            if currentA == currentB:
                return currentA
            currentB = currentB.next
        currentA = currentA.next
    # If we exit the loops without returning, there's no intersection
    return None
```

### Approach 2: HashSet

#### Intuition
By using a hash set, we can keep track of all the nodes in one of the lists. Then, as we traverse the other list, we simply check if any of its nodes are in the hash set, indicating an intersection.

#### Steps:
1. Traverse through all nodes of the first list and add each node to a set.
2. Then, traverse through the second list.
3. For each node in the second list, check if it is in the set of nodes from the first list.
4. Return the first common node found as the intersection.
5. If no intersection is found, return `None`.

#### Time Complexity:
- **O(m + n)** because each list is traversed separately.

#### Space Complexity:
- **O(m)** or **O(n)** depending on which list is stored in the set.

```python
def getIntersectionNode(headA, headB):
    nodes_in_A = set()
    
    # Store nodes of list A in a hash set
    currentA = headA
    while currentA:
        nodes_in_A.add(currentA)
        currentA = currentA.next
    
    # Traverse list B and check for intersection
    currentB = headB
    while currentB:
        if currentB in nodes_in_A:
            return currentB
        currentB = currentB.next
    
    return None
```

### Approach 3: Two Pointers

#### Intuition
Using two pointers, we can equalize the effective traversal length of both lists by redirecting them to start from the beginning of the other list once they hit the end of their respective lists. This means that if there is an intersection, the pointers will meet at the intersection node after at most two passes over each list.

#### Steps:
1. Set two pointers, one for each list.
2. Traverse each list at the same pace. Once a pointer hits the end of its list, redirect it to the head of the other list.
3. If at any point the two pointers meet, that node is the intersection.
4. Return the intersecting node when the pointers meet.
5. If both pointers reach the end (`None`) without meeting, there's no intersection.

#### Time Complexity:
- **O(m + n)** because each list might be traversed twice in the worst case.

#### Space Complexity:
- **O(1)** as this approach uses only two additional pointers.

```python
def getIntersectionNode(headA, headB):
    if not headA or not headB:
        return None
    
    # Initialize two pointers at the start of each list
    pointerA, pointerB = headA, headB
    
    # Traverse both lists
    while pointerA != pointerB:
        # Move to the next node in list A, or switch to head of list B
        pointerA = pointerA.next if pointerA else headB
        # Move to the next node in list B, or switch to head of list A
        pointerB = pointerB.next if pointerB else headA
    
    # When they meet, it's the intersection node or None if no intersection
    return pointerA  # Can be None if no intersection
```

This series of approaches provides a detailed path from the simplest, but least efficient, to the most optimal solution. The two-pointer approach is generally preferred due to its simplicity and optimal time complexity without additional space usage.

