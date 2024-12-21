# [LeetCode 146: LRU Cache](https://leetcode.com/problems/lru-cache/)

## Solutions

- [Approach 1: Using a List to Store Keys and Dictionary for Cache Operations](#approach-1-using-a-list-to-store-keys-and-dictionary-for-cache-operations)
- [Approach 2: Using OrderedDict](#approach-2-using-ordereddict)
- [Approach 3: Using Doubly Linked List and Dictionary (Optimal)](#approach-3-using-doubly-linked-list-and-dictionary-optimal)

---

## Approach 1: Using a List to Store Keys and Dictionary for Cache Operations

### Intuition

The most straightforward solution is to maintain two structures:

1. A list to track the order of access.
2. A dictionary to store the key-value pairs.

When accessing a key, we update its position in the list to mark it as recently used. When adding a new key-value pair, if we exceed the capacity, we remove the least recently used item from the list and the dictionary. 

### Steps

1. Use a list (`order`) to track the keys in the order of their usage.
2. Use a dictionary (`cache`) to store the key-value pairs.
3. On `get(key)`, move the key to the end of the list if it exists, signifying recent use.
4. On `put(key, value)`, add/update the dictionary entry and move key to the end of the list.
5. Remove the least recently used item if the size exceeds the capacity.

### Code

```python
class LRUCache:

    def __init__(self, capacity: int):
        # Initialize the cache with the given capacity
        self.capacity = capacity
        self.cache = {}
        self.order = []

    def get(self, key: int) -> int:
        if key in self.cache:
            # Move the key to the end to mark it as recently used
            self.order.remove(key)
            self.order.append(key)
            return self.cache[key]
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            # Update the existing key and move it to the end
            self.cache[key] = value
            self.order.remove(key)
            self.order.append(key)
        else:
            # Check if the cache is at capacity
            if len(self.cache) >= self.capacity:
                # Remove the least recently used item
                lru = self.order.pop(0)
                del self.cache[lru]
            # Insert the new key-value pair
            self.cache[key] = value
            self.order.append(key)
```

### Complexity

- **Time Complexity**: O(n) for `get` and `put` due to list operations.
- **Space Complexity**: O(capacity), since the list and dictionary store up to `capacity` elements.

---

## Approach 2: Using OrderedDict

### Intuition

Python's `OrderedDict` maintains the order of keys as they are inserted. By leveraging its internal structure, the LRU Cache can efficiently remove the least recently used key in constant time.

### Steps

1. Use `OrderedDict` to both maintain order and store key-value pairs.
2. On `get` or `put` operations, reorder the key to the end to signify it's recently used.
3. Overflow is managed by simply popping the first item in the `OrderedDict`.

### Code

```python
from collections import OrderedDict

class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = OrderedDict()

    def get(self, key: int) -> int:
        if key in self.cache:
            # Move the accessed key to end to show it's been used recently
            self.cache.move_to_end(key)
            return self.cache[key]
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            # Update the value and move to the end
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            # Pop oldest item
            self.cache.popitem(last=False)
```

### Complexity

- **Time Complexity**: O(1) for `get` and `put` since they leverage the `OrderedDict`.
- **Space Complexity**: O(capacity) for the `OrderedDict`.

---

## Approach 3: Using Doubly Linked List and Dictionary (Optimal)

### Intuition

To achieve constant time operations, we use a combination of:

1. A doubly linked list to maintain the order of recently used keys.
2. A dictionary that maps keys to nodes in the linked list.

This approach avoids the linear time ordering issue of the list operation in Approach 1. 

### Steps

1. Maintain head and tail pointers for the linked list.
2. A dictionary maps keys to their corresponding nodes in the linked list.
3. Move nodes in the linked list to mark access and update/insert efficiently.
4. Handle overflow by removing the tail node of the list.

### Code

```python
class Node:
    def __init__(self, key=None, value=None):
        self.key = key
        self.value = value
        self.prev = self.next = None

class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {}
        
        # Create sentinel nodes for head and tail
        self.head = Node()
        self.tail = Node()
        self.head.next = self.tail
        self.tail.prev = self.head

    def _remove(self, node):
        # Removes node from the linked list
        prev, next = node.prev, node.next
        prev.next, next.prev = next, prev

    def _add(self, node):
        # Adds node right before the tail
        prev, next = self.tail.prev, self.tail
        prev.next = next.prev = node
        node.prev, node.next = prev, next

    def get(self, key: int) -> int:
        if key in self.cache:
            # Move to the end as it's recently used
            node = self.cache[key]
            self._remove(node)
            self._add(node)
            return node.value
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            # Update the value and move to the end
            self._remove(self.cache[key])
            self._add(self.cache[key])
            self.cache[key].value = value
        else:
            if len(self.cache) >= self.capacity:
                # Remove from the linked list and dictionary the LRU item
                lru = self.head.next
                self._remove(lru)
                del self.cache[lru.key]
            # Create a new node and add it to the linked list and dictionary
            new_node = Node(key, value)
            self.cache[key] = new_node
            self._add(new_node)
```

### Complexity

- **Time Complexity**: O(1) for both `get` and `put` due to efficient node manipulation and dictionary operations.
- **Space Complexity**: O(capacity) to store the linked list nodes and dictionary entries.

Each approach has its trade-offs, with approach 3 providing the optimal efficiency in terms of time by leveraging data structures in Python.

