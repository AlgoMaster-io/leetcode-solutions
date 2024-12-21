# [Leetcode 138: Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

## Solutions:
- [Approach 1: Using HashMap](#approach-1-using-hashmap)
- [Approach 2: Interleaved List](#approach-2-interleaved-list)


### Approach 1: Using HashMap

**Intuition:**

The main idea is to traverse the original linked list and copy each node. During this traversal, a `HashMap` is used to keep track of the original nodes and their corresponding copied nodes. By using this map, we can access the copied node of a particular original node efficiently.

The algorithm can be summarized in two main passes over the linked list:
1. **Copy Nodes and Fill HashMap:**
   - Create a new list node for each node in the original list,
   - Save the mapping between the original node and copied node in a `HashMap`.

2. **Assign Next and Random Pointers:**
   - Iterate over the original list again. For each node, set the `next` and `random` pointers for the copied nodes using the HashMap.


```java
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}

public class Solution {

    public Node copyRandomList(Node head) {
        // If the original list is empty, return null
        if (head == null) return null;
        
        // Mapping from original nodes to their corresponding copied nodes
        Map<Node, Node> nodeMap = new HashMap<>();

        // Step 1: Copy all the nodes and store the mapping in hashmap
        Node current = head;
        while (current != null) {
            nodeMap.put(current, new Node(current.val));
            current = current.next;
        }

        // Step 2: Assign the next and random pointers for the copied nodes
        current = head;
        while (current != null) {
            nodeMap.get(current).next = nodeMap.get(current.next);
            nodeMap.get(current).random = nodeMap.get(current.random);
            current = current.next;
        }
        
        // Return the deep copied head node
        return nodeMap.get(head);
    }
}
```

- **Time Complexity:** O(N), where N is the number of nodes in the linked list. We iterate over the list twice.
- **Space Complexity:** O(N), as we store N node mappings in the HashMap.

### Approach 2: Interleaved List

**Intuition:**

We can optimize the space used for storing node mappings by integrating the copied nodes directly into the original list. The procedure is as follows:

1. **First pass:** For each node in the original list, create a new node and interleaved it between the current node and the next node.

2. **Second pass:** Assign `random` pointers for the newly created nodes by utilizing the existing `random` pointers.

3. **Third pass:** Separate the original and copied list.

This approach removes the need for a HashMap to store node mappings, thereby reducing the space complexity.


```java
public class Solution {

    public Node copyRandomList(Node head) {
        if (head == null) return null;
        
        // Step 1: Create new nodes and interleave them with the original nodes
        Node current = head;
        while (current != null) {
            Node newNode = new Node(current.val);
            newNode.next = current.next;
            current.next = newNode;
            current = newNode.next;
        }
        
        // Step 2: Assign random pointers for the copied nodes
        current = head;
        while (current != null) {
            if (current.random != null) {
                current.next.random = current.random.next;
            }
            current = current.next.next;
        }
        
        // Step 3: Separate the copied list from the original list
        current = head;
        Node copiedHead = head.next;
        while (current != null) {
            Node copiedNode = current.next;
            current.next = copiedNode.next;
            current = current.next;
            if (current != null) {
                copiedNode.next = current.next;
            }
        }
        
        return copiedHead;
    }
}
```

- **Time Complexity:** O(N), where N is the number of nodes in the linked list. We iterate through the list multiple times.
- **Space Complexity:** O(1), as we don't use any additional space that scales with input. The space allocated for new nodes doesn't count as auxiliary space.

