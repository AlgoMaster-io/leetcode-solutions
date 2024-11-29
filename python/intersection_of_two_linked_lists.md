# [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## Approach 1: Brute Force (Basic)

### Solution
python
```python
# Time Complexity: O(m * n)
# Space Complexity: O(1)
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        # Iterate through each node of list A
        while headA:
            temp = headB

            # Check if it matches any node in list B
            while temp:
                if headA == temp:
                    return headA  # Intersection found
                temp = temp.next

            headA = headA.next

        return None  # No intersection
```

## Approach 2: Using HashSet (Intermediate)

### Solution
python
```python
# Time Complexity: O(m + n)
# Space Complexity: O(m) or O(n)
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        visited_nodes = set()

        # Add all nodes of list A to the set
        while headA:
            visited_nodes.add(headA)
            headA = headA.next

        # Check for intersection in list B
        while headB:
            if headB in visited_nodes:
                return headB  # Intersection found
            headB = headB.next

        return None  # No intersection
```

## Approach 3: Two Pointers (Optimal)

### Solution
python
```python
# Time Complexity: O(m + n)
# Space Complexity: O(1)
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        if not headA or not headB:
            return None

        pointerA = headA
        pointerB = headB

        # Traverse both lists
        while pointerA != pointerB:
            # Switch to the other list when reaching the end
            pointerA = headB if pointerA is None else pointerA.next
            pointerB = headA if pointerB is None else pointerB.next

        return pointerA  # Either intersection node or None
```

