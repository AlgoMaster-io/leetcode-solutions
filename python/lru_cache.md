# 146. [LRU Cache](https://leetcode.com/problems/lru-cache/)

## Approach: HashMap with Doubly Linked List

### Solution
```python
# Time Complexity: O(1) for both get and put operations
# Space Complexity: O(capacity), where capacity is the maximum number of entries in the cache

class LRUCache:
    class Node:
        def __init__(self, key, value):
            self.key = key
            self.value = value
            self.prev = None
            self.next = None

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {}
        self.head = self.Node(-1, -1)  # Dummy head node
        self.tail = self.Node(-1, -1)  # Dummy tail node
        self.head.next = self.tail
        self.tail.prev = self.head

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        node = self.cache[key]
        self.remove(node)  # Move the accessed node to the front
        self.insert_to_front(node)
        return node.value

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.remove(self.cache[key])  # Remove the old node
        elif len(self.cache) == self.capacity:
            # Evict the least recently used node
            lru_node = self.tail.prev
            self.remove(lru_node)
            del self.cache[lru_node.key]

        new_node = self.Node(key, value)
        self.cache[key] = new_node
        self.insert_to_front(new_node)

    # Helper method to remove a node from the linked list
    def remove(self, node: 'Node') -> None:
        node.prev.next = node.next
        node.next.prev = node.prev

    # Helper method to insert a node at the front of the linked list
    def insert_to_front(self, node: 'Node') -> None:
        node.next = self.head.next
        node.prev = self.head
        self.head.next.prev = node
        self.head.next = node

# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key, value)
```

