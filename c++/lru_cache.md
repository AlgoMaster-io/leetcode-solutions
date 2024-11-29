# 146. [LRU Cache](https://leetcode.com/problems/lru-cache/)

## Approach: HashMap with Doubly Linked List

### Solution
```cpp
// Time Complexity: O(1) for both get and put operations
// Space Complexity: O(capacity), where capacity is the maximum number of entries in the cache
#include <unordered_map>

class LRUCache {
private:
    struct Node {
        int key, value;
        Node* prev;
        Node* next;
        Node(int k, int v) : key(k), value(v), prev(nullptr), next(nullptr) {}
    };

    int capacity;
    std::unordered_map<int, Node*> cache;
    Node* head;
    Node* tail;

public:
    LRUCache(int capacity) : capacity(capacity) {
        head = new Node(-1, -1); // Dummy head node
        tail = new Node(-1, -1); // Dummy tail node
        head->next = tail;
        tail->prev = head;
    }

    int get(int key) {
        if (cache.find(key) == cache.end()) {
            return -1;
        }
        Node* node = cache[key];
        remove(node); // Move the accessed node to the front
        insertToFront(node);
        return node->value;
    }

    void put(int key, int value) {
        if (cache.find(key) != cache.end()) {
            Node* node = cache[key];
            remove(node); // Remove the old node
            delete node;
        } else if (cache.size() == capacity) {
            // Evict the least recently used node
            cache.erase(tail->prev->key);
            remove(tail->prev);
        }

        Node* newNode = new Node(key, value);
        cache[key] = newNode;
        insertToFront(newNode);
    }

private:
    // Helper method to remove a node from the linked list
    void remove(Node* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    // Helper method to insert a node at the front of the linked list
    void insertToFront(Node* node) {
        node->next = head->next;
        node->prev = head;
        head->next->prev = node;
        head->next = node;
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key, value);
 */
```

