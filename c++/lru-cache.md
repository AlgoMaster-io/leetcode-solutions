# [Leetcode 146: LRU Cache](https://leetcode.com/problems/lru-cache/)

## Table of Contents
1. [Approach 1: Simple Hashmap with Order Maintenance (Less Optimal)](#approach-1-simple-hashmap-with-order-maintenance-less-optimal)
2. [Approach 2: Using Doubly Linked List and Hashmap (Optimal)](#approach-2-using-doubly-linked-list-and-hashmap-optimal)

### Approach 1: Simple Hashmap with Order Maintenance (Less Optimal)

#### Intuition
For an LRU Cache, the main operations are `get` which should retrieve an existing value and mark it as the most recently used, and `put` which should insert a new value, possibly evicting the least recently used item if capacity is exceeded. A simple way is to use a hashmap to store keys to values, and an ordered data structure to keep track of the usage order (such as a list).

However, maintaining order manually isn't efficient. Lists do not provide efficient random access or deletions.

#### Approach

1. Use a linked list to keep track of the order of usage.
2. Use a hashmap where the key is the cache key and the value is an iterator to the position in the list.
3. On `put`, add to the tail of the list (indicating most recent) and update the hashmap. If the cache exceeds the capacity, remove the head of the list (least recent) and its corresponding hashmap entry.
4. On `get`, bring the accessed key to the tail of the list and update its position in the hashmap.

#### Time Complexity
- `get`: \(O(n)\), due to linear search and rearranging the list.
- `put`: \(O(n)\), due to list traversal and rearrangement.
  
#### Space Complexity
- \(O(capacity)\), for storing the cache entries.

```cpp
// Inefficient approach due to linear search in the list
#include <list>
#include <unordered_map>

class LRUCache {
public:
    LRUCache(int capacity) : cap(capacity) {}
    
    int get(int key) {
        auto it = cache.find(key);
        if (it == cache.end()) return -1;
        
        // Move accessed key to the back of the list to mark it as most recently used
        order.splice(order.end(), order, it->second);
        
        return it->second->second;
    }
    
    void put(int key, int value) {
        auto it = cache.find(key);
        
        if (it != cache.end()) {
            // Update the value
            it->second->second = value;
            // Move it to the back of the list
            order.splice(order.end(), order, it->second);
        } else {
            if (cache.size() == cap) {
                // Evict the least recently used item
                cache.erase(order.front().first);
                order.pop_front();
            }
            // Insert the new entry
            order.emplace_back(key, value);
            cache[key] = --order.end();
        }
    }
    
private:
    int cap;
    std::list<std::pair<int, int>> order;
    std::unordered_map<int, std::list<std::pair<int, int>>::iterator> cache;
};
```

### Approach 2: Using Doubly Linked List and Hashmap (Optimal)

#### Intuition
The key difference here is maintaining the order of elements using a doubly linked list which allows fast deletion and insertion at both ends, combined with a hashmap for fast key-to-node reference. This provides \(O(1)\) complexity for both operations.

#### Approach

1. Use a doubly linked list to represent the usage order, where nodes can be added to the head and removed from the tail in constant time.
2. Use a hashmap to map keys to their respective nodes in the list for constant time access.
3. Upon a `get` operation, move the accessed node to the head.
4. Upon a `put`, add the new node to the head. If the cache is full, remove the node from the tail.

#### Time Complexity
- `get`: \(O(1)\)
- `put`: \(O(1)\)

#### Space Complexity
- \(O(capacity)\), since additional nodes and pointers are required.

```cpp
// Optimal approach using doubly linked list and hashmap
#include <unordered_map>

class LRUCache {
public:
    LRUCache(int capacity) : cap(capacity) {
        // Initialize virtual head and tail
        head = new Node();
        tail = new Node();
        head->next = tail;
        tail->prev = head;
    }
    
    int get(int key) {
        auto it = cache.find(key);
        if (it == cache.end()) return -1;
        
        // Move accessed node to the head
        moveToHead(it->second);
        return it->second->value;
    }
    
    void put(int key, int value) {
        auto it = cache.find(key);
        if (it != cache.end()) {
            // Update the value and move to head
            it->second->value = value;
            moveToHead(it->second);
        } else {
            if (cache.size() == cap) {
                // Remove the least recently used item
                Node* lru = tail->prev;
                removeNode(lru);
                cache.erase(lru->key);
                delete lru;
            }
            // Add new node to the head
            Node* node = new Node(key, value);
            cache[key] = node;
            addToHead(node);
        }
    }

private:
    struct Node {
        int key, value;
        Node* prev;
        Node* next;
        Node() : key(0), value(0), prev(nullptr), next(nullptr) {}
        Node(int k, int v) : key(k), value(v), prev(nullptr), next(nullptr) {}
    };
    
    void addToHead(Node* node) {
        node->next = head->next;
        head->next->prev = node;
        node->prev = head;
        head->next = node;
    }
    
    void removeNode(Node* node) {
        Node* prev = node->prev;
        Node* next = node->next;
        prev->next = next;
        next->prev = prev;
    }
    
    void moveToHead(Node* node) {
        removeNode(node);
        addToHead(node);
    }
    
    int cap;
    Node* head;
    Node* tail;
    std::unordered_map<int, Node*> cache;
};
```

This optimal solution allows efficient `get` and `put` operations on the cache using a combination of a doubly linked list and a hashmap, maintaining the desired LRU order and achieving constant-time operations for cache access and updates.

