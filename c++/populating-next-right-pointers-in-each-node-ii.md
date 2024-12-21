# [Leetcode 117: Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

## Approaches:
- [Approach 1: Level Order Traversal using Queue](#approach-1-level-order-traversal-using-queue)
- [Approach 2: Constant Space Approach (O(1) Space Complexity)](#approach-2-constant-space-approach-o1-space-complexity)

## Approach 1: Level Order Traversal using Queue

### Intuition
The problem requires connecting each node to its adjacent node in the same depth. A straightforward approach is to perform a level-order traversal (BFS) of the tree, using a queue. At each level, connect all nodes iteratively from left to right.

### Solution
```cpp
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;
    Node() : val(0), left(NULL), right(NULL), next(NULL) {}
    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}
    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};

Node* connect(Node* root) {
    if (!root) return nullptr;
    
    // Initialize the queue for level order traversal
    std::queue<Node*> q;
    q.push(root);
    
    while (!q.empty()) {
        int size = q.size();
        Node* prev = nullptr;
        
        // Iterate through the current level
        for (int i = 0; i < size; ++i) {
            Node* currentNode = q.front();
            q.pop();
            
            // Connect the previous node on this level to the current node
            if (prev != nullptr) {
                prev->next = currentNode;
            }
            prev = currentNode;
            
            // Add child nodes of the current node to the queue
            if (currentNode->left) q.push(currentNode->left);
            if (currentNode->right) q.push(currentNode->right);
        }
    }
    
    return root;
}
```

### Time Complexity
- **Time**: O(n), where `n` is the number of nodes in the tree. Each node is inserted and removed from the queue once.
- **Space**: O(n), for storing nodes at each level in the queue. At worst, the space complexity could reach O(n/2) which simplifies to O(n).

## Approach 2: Constant Space Approach (O(1) Space Complexity)

### Intuition
Instead of using extra memory with a queue to store nodes at each level, we can use the `next` pointers to traverse the current level and set up `next` pointers for the next level. This takes advantage of the already connected nodes.

### Solution
```cpp
Node* connect(Node* root) {
    if (!root) return nullptr;
    
    Node* head = root; // Start with the head of the current level.
    
    while (head != nullptr) {
        Node* dummy = new Node(); // Dummy node to start linking the next level
        Node* tail = dummy; // Tail keeps track of the end of the new next level
        
        for (Node* current = head; current != nullptr; current = current->next) {
            if (current->left) {
                tail->next = current->left;
                tail = tail->next; // Move to newly added node
            }
            if (current->right) {
                tail->next = current->right;
                tail = tail->next; // Move to newly added node
            }
        }
        
        // Move to the next level
        head = dummy->next;
        delete dummy; // Free up the unused dummy node
    }
    
    return root;
}
```

### Time Complexity
- **Time**: O(n), where `n` is the number of nodes in the tree. We visit each node once.
- **Space**: O(1), since we do not use additional data structures that scale with input size. The auxiliary space is mainly used for stack during recursive calls, which can be optimized here by iterative approach.

