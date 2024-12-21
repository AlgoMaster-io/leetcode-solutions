# [Leetcode Problem 61: Rotate List](https://leetcode.com/problems/rotate-list/)

## Solutions

1. [Basic Approach - Convert to Array](#approach-1---convert-to-array)
2. [Iterative In-Place Rotation](#approach-2---iterative-in-place-rotation)
3. [Optimal Approach - Identify and Connect Nodes](#approach-3---identify-and-connect-nodes)

---

## Approach 1 - Convert to Array

### Intuition
The basic idea in this approach is to first convert the linked list to an array. Once we have the array representation, rotating the list becomes straightforward. After performing the rotation on the array, we convert it back to a linked list.

### Steps
1. Convert the linked list to an array.
2. Rotate the array k times to the right.
3. Convert the array back to a linked list.

### Code
```java
public ListNode rotateRight(ListNode head, int k) {
    if (head == null || head.next == null || k == 0) return head;
    
    // Convert linked list to ArrayList
    List<ListNode> nodeList = new ArrayList<>();
    ListNode current = head;
    while (current != null) {
        nodeList.add(current);
        current = current.next;
    }
    
    // Determine the rotation
    int n = nodeList.size();
    k = k % n;
    if (k == 0) return head; // Rotation results in same list

    // Rearrange nodes in ArrayList
    Collections.rotate(nodeList, k);
    
    // Reconnect nodes to form a linked list
    for (int i = 0; i < n - 1; i++) {
        nodeList.get(i).next = nodeList.get(i + 1);
    }
    nodeList.get(n - 1).next = null;
    
    return nodeList.get(0);
}
```

### Complexity
- **Time Complexity:** O(N), where N is the number of nodes in the list. Conversion to and from the array takes linear time.
- **Space Complexity:** O(N), for storing the nodes in an ArrayList.

---

## Approach 2 - Iterative In-Place Rotation

### Intuition
Without converting to an array, we can perform the rotations directly on the linked list. This involves finding the new head and tail of the list after rotation.

### Steps
1. Count the length of the list.
2. Adjust k to handle cases where k is greater than the length.
3. Make the list circular by connecting the tail to the head.
4. Find the new tail and break the circle to form the new list.

### Code
```java
public ListNode rotateRight(ListNode head, int k) {
    if (head == null || head.next == null) return head;

    // Calculate the length of the list
    int length = 1;
    ListNode tail = head;
    while (tail.next != null) {
        tail = tail.next;
        length++;
    }
    
    // Make the list circular
    tail.next = head;

    // Find the new head and tail
    k = k % length;
    int stepsToNewHead = length - k;
    ListNode newTail = tail;
    while (stepsToNewHead-- > 0) {
        newTail = newTail.next;
    }
    ListNode newHead = newTail.next;
    newTail.next = null;
    
    return newHead;
}
```

### Complexity
- **Time Complexity:** O(N), where N is the number of nodes in the list.
- **Space Complexity:** O(1), no extra space except for pointers.

---

## Approach 3 - Identify and Connect Nodes

### Intuition
This approach optimizes the iterative in-place rotation by determining the exact nodes to connect or disconnect based on the computed new head position directly, without needing intermediate storage or iterations.

### Steps
1. Edge case handling for head being null or single element.
2. Compute the size of the list.
3. Utilize modulus operation for effective rotations.
4. Directly identify new connection points and manipulate pointers to achieve rotation.

### Code
```java
public ListNode rotateRight(ListNode head, int k) {
    if (head == null || head.next == null || k == 0) return head;

    // Find the length of the linked list
    ListNode tail = head;
    int length = 1;
    while (tail.next != null) {
        tail = tail.next;
        length++;
    }
    
    // Make it a circular list
    tail.next = head;
    
    // Calculate the break point
    k = k % length;
    int stepsToNewHead = length - k;
    ListNode newTail = tail;
    
    // Find the position to break the circle
    while (stepsToNewHead-- > 0) {
        newTail = newTail.next;
    }
    
    // Make new head and break the circle
    ListNode newHead = newTail.next;
    newTail.next = null;

    return newHead;
}
```

### Complexity
- **Time Complexity:** O(N), where N is the number of nodes in the list since we iterate through the list once to find its length and once to find the new tail.
- **Space Complexity:** O(1), as we modify the list in place and use only a few pointers.

