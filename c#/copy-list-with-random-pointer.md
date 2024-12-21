# [Leetcode 138: Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

## Solutions

- [Approach 1: Brute Force using a HashMap](#approach-1-brute-force-using-a-hashmap)
- [Approach 2: Optimized Approach using Node Interweaving](#approach-2-optimized-approach-using-node-interweaving)

### Approach 1: Brute Force using a HashMap

**Intuition:**

The problem involves creating a deep copy of a linked list where each node has an additional random pointer. The brute-force approach involves using a hash map (dictionary) to maintain mappings between the original list nodes and the new list nodes. 

1. Traverse the original list and for each node, create a new node and store the mapping from original to new in the hash map.
2. Traverse the original list again, setting the next and random pointers of each new node using the hash map.

This approach ensures that we can access the corresponding copied node of any original node directly via the hash map in constant time.

```csharp
// Definition for a Node.
public class Node {
    public int val;
    public Node next;
    public Node random;
    
    public Node(int _val) {
        val = _val;
        next = null;
        random = null;
    }
}

public class Solution {
    public Node CopyRandomList(Node head) {
        if (head == null) return null;

        // Create a hash map to store the mapping of the original node to the copied node
        Dictionary<Node, Node> map = new Dictionary<Node, Node>();

        Node current = head;
        // First pass: create a copy of each node and store in the map
        while (current != null) {
            map[current] = new Node(current.val);
            current = current.next;
        }

        current = head;
        // Second pass: assign next and random pointers using the map
        while (current != null) {
            map[current].next = current.next != null ? map[current.next] : null;
            map[current].random = current.random != null ? map[current.random] : null;
            current = current.next;
        }

        // Return the deep copied list's head
        return map[head];
    }
}
```

**Time Complexity:** O(n), where n is the number of nodes in the list since we traverse the list twice.

**Space Complexity:** O(n) for the hash map to store mappings for each node.

### Approach 2: Optimized Approach using Node Interweaving

**Intuition:**

To optimize the space complexity, we can avoid using an extra hash map by interweaving the original list with the copied nodes and then setting the next and random pointers appropriately.

1. **Interweave the list:** For each node in the original list, create a copy and place it immediately next to the original node. This effectively intersperses the copied nodes with the original nodes.
2. **Set random pointers:** Traverse the modified list and set the random pointer for each copied node.
3. **Extract the copied list:** Separate the copied nodes from the interwoven list to restore the original list and create the copied list.

This method cleverly uses the original list structure to handle the copying in-place without additional space for a mapping.

```csharp
public class Solution {
    public Node CopyRandomList(Node head) {
        if (head == null) return null;

        Node current = head;

        // Step 1: Interweave the original and copied nodes
        while (current != null) {
            Node next = current.next;
            Node copy = new Node(current.val);
            current.next = copy;
            copy.next = next;
            current = next;
        }

        current = head;
        // Step 2: Set random pointers for copied nodes
        while (current != null) {
            if (current.random != null) {
                current.next.random = current.random.next;
            }
            current = current.next.next;
        }

        // Step 3: Separate the copied nodes from the interwoven list
        Node pseudoHead = new Node(0);
        Node copyCurrent = pseudoHead;
        current = head;

        while (current != null) {
            Node next = current.next.next;

            // Extract the copied node
            Node copy = current.next;
            copyCurrent.next = copy;
            copyCurrent = copy;
            
            // Restore the original list
            current.next = next;
            current = next;
        }

        return pseudoHead.next;
    }
}
```

**Time Complexity:** O(n), where n is the number of nodes in the list.

**Space Complexity:** O(1), since we are working in-place with the list itself.

This optimized solution leverages the existing list to minimize extra space usage and efficiently creates a deep copy of the list with random pointers.

