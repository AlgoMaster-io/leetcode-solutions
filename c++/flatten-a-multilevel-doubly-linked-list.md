## [Leetcode Problem 430: Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/)

### Approaches
- [Approach 1: Recursive Depth First Search (DFS)](#approach-1-recursive-depth-first-search-dfs)
- [Approach 2: Iterative Depth First Search (DFS) Using Stack](#approach-2-iterative-depth-first-search-dfs-using-stack)

---

### Approach 1: Recursive Depth First Search (DFS)

**Intuition:**

In this approach, we use recursion to traverse through the list. The key idea is to process each node and its child list recursively. If a node has a child, we recursively flatten that sublist and append it to the main list. We continue this process until we reach the end of the list. By recursively flattening each child list and adjusting the pointers, we achieve a flattened structure.

**Steps:**

1. Define a helper function `flattenRecursive` which will flatten the list starting from the given node.
2. Traverse from the given node using a pointer.
3. For each node:
   - If it has a child, recursively flatten the child list.
   - Detach the child list by adjusting the `next` and `prev` pointers to integrate the child list into the existing list.
   - Move to the next node and repeat the process.
4. Return the head of the flattened list.

**Code:**
```cpp
class Node {
public:
    int val;
    Node* prev;
    Node* next;
    Node* child;
};

class Solution {
public:
    Node* flatten(Node* head) {
        flattenRecursive(head);
        return head;
    }

private:
    Node* flattenRecursive(Node* node) {
        Node* current = node;
        Node* tail = nullptr;

        while (current) {
            Node* next = current->next;
            if (current->child) {
                // Flatten the child list
                Node* childHead = flattenRecursive(current->child);
                // Save the tail of the child list
                Node* childTail = childHead;
                while (childTail->next) childTail = childTail->next;

                // Attach the child list
                current->next = childHead;
                childHead->prev = current;
                childTail->next = next;
                if (next) next->prev = childTail;

                // Remove the child pointer
                current->child = nullptr;
                // Move the current pointer to the childTail
                tail = childTail;
            } else {
                tail = current;
            }
            // Move to the next node
            current = next;
        }
        return node;
    }
};
```

**Complexity Analysis:**

- **Time Complexity:** O(n), where n is the number of nodes in the list. Each node is visited once.
- **Space Complexity:** O(n), due to the recursive stack in the worst case (if the list is a straight line of child nodes).

---

### Approach 2: Iterative Depth First Search (DFS) Using Stack

**Intuition:**

An iterative approach can also be used to flatten the list using a stack. The stack helps manage the nodes, enabling us to revisit nodes in-depth first order. We push nodes onto the stack, and by adjusting the `next` and `prev` relations, we progressively flatten the list.

**Steps:**

1. Use a stack to hold nodes.
2. Initialize the stack with the head node.
3. While the stack is not empty:
   - Pop a node from the stack.
   - If the node has a `next`, push it onto the stack.
   - If the node has a `child`, push the child onto the stack.
   - Adjust the `next` and `prev` pointers according to the current node and the stack's top.
4. Return the modified head of the list.

**Code:**
```cpp
class Solution {
public:
    Node* flatten(Node* head) {
        if (!head) return nullptr;

        std::stack<Node*> stack;
        stack.push(head);

        Node* prev = nullptr;

        while (!stack.empty()) {
            Node* current = stack.top();
            stack.pop();

            if (prev) {
                prev->next = current;
                current->prev = prev;
            }
            
            if (current->next) {
                stack.push(current->next);
            }
            
            if (current->child) {
                stack.push(current->child);
                current->child = nullptr;
            }

            prev = current;
        }

        return head;
    }
};
```

**Complexity Analysis:**

- **Time Complexity:** O(n), where n is the number of nodes in the list. Each node is processed once.
- **Space Complexity:** O(n), for the stack which may store all nodes in the worst case.

The iterative approach may be preferable in environments where recursion depth is a constraint, as it avoids potential stack overflow issues present in recursive solutions.

