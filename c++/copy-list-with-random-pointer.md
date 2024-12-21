# [Leetcode 138: Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

## Approaches
- [Approach 1: Brute Force with Hash Map](#approach-1-brute-force-with-hash-map)
- [Approach 2: Optimized In-Place Copy](#approach-2-optimized-in-place-copy)

## Approach 1: Brute Force with Hash Map

### Intuition
In the brute force approach, we use a hash map to maintain a mapping between the original nodes and their respective copies. This allows us to easily assign the `next` and `random` pointers for each copied node, with the help of `_hash_map` lookups.

### Steps
1. **First Pass:** Traverse the original list and create a copy of each node. Store these copies in a hash map, with the original node as the key and the copied node as the value.
2. **Second Pass:** Traverse the list again and use the hash map to assign the `next` and `random` pointers for each node in the copied list.

### Code
```cpp
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};

class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (!head) return nullptr;

        // Step 1: Create a hash map to store the mapping from original nodes to cloned nodes
        std::unordered_map<Node*, Node*> nodeMap;
        
        // Step 2: Clone nodes and store them in the map
        Node* current = head;
        while (current) {
            nodeMap[current] = new Node(current->val);
            current = current->next;
        }
        
        // Step 3: Assign next and random pointers for the cloned list using the map
        current = head;
        while (current) {
            nodeMap[current]->next = nodeMap[current->next];
            nodeMap[current]->random = nodeMap[current->random];
            current = current->next;
        }
        
        return nodeMap[head];
    }
};
```

### Complexity
- **Time Complexity:** O(N), where N is the number of nodes in the linked list.
- **Space Complexity:** O(N), due to the space required for the hash map.

## Approach 2: Optimized In-Place Copy

### Intuition
The optimized approach reduces the space complexity by performing the copy in-place. We interleave the copied nodes with the original nodes in the linked list during the first phase and then set up the `random` pointers in the second phase before finally splitting the interleaved list to separate the copied nodes.

### Steps
1. **In-place Copy:** For each original node, insert the copied node immediately next to it.
2. **Assign Random Pointers:** Iterate again to set the `random` pointers for the copied nodes.
3. **Restore the Original List:** Split the interleaved list to separate out the copied nodes into a new linked list.

### Code
```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (!head) return nullptr;

        // Step 1: Interleave copied nodes with original nodes
        Node* current = head;
        while (current) {
            Node* copy = new Node(current->val);
            copy->next = current->next;
            current->next = copy;
            current = copy->next;
        }
        
        // Step 2: Assign random pointers for the copied nodes
        current = head;
        while (current) {
            current->next->random = current->random ? current->random->next : nullptr;
            current = current->next->next;
        }
        
        // Step 3: Restore the original list and separate the copied list
        Node* copyHead = head->next;
        Node* copyCurrent = copyHead;
        current = head;
        while (current) {
            current->next = current->next->next;
            copyCurrent->next = copyCurrent->next ? copyCurrent->next->next : nullptr;
            current = current->next;
            copyCurrent = copyCurrent->next;
        }
        
        return copyHead;
    }
};
```

### Complexity
- **Time Complexity:** O(N), where N is the number of nodes in the linked list.
- **Space Complexity:** O(1), as this approach performs in-place operations while only using constant extra space for variables.

