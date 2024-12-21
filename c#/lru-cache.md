# [LeetCode 146: LRU Cache](https://leetcode.com/problems/lru-cache/)

## Solutions
- [Approach 1: Simple Doubly Linked List with HashMap](#approach-1-simple-doubly-linked-list-with-hashmap)
- [Approach 2: Optimized Doubly Linked List with HashMap](#approach-2-optimized-doubly-linked-list-with-hashmap)

### Approach 1: Simple Doubly Linked List with HashMap

**Intuition:**

An LRU (Least Recently Used) Cache can be efficiently implemented using a Doubly Linked List (DLL) to maintain the order of use and a HashMap for quick access to nodes. The idea is to always have the most recently used item at the head of the DLL and the least recently used item at the tail. 

1. **Doubly Linked List**: Allows us to remove nodes and insert them at the head or tail in O(1) time.
2. **HashMap**: Provides O(1) operations for accessing and updating the node associated with a given key.

**Steps:**
- Initialize a DLL with a head and tail sentinel node that guide the placement of nodes.
- Maintain a HashMap for quick access to DLL nodes.
- On a `get(key)` operation, move the accessed node to the head of the list.
- On a `put(key, value)` operation, insert or update the node, and potentially evict the tail node if capacity is exceeded.

**Code:**

```csharp
public class LRUCache {

    private class Node {
        public int key;
        public int value;
        public Node prev;
        public Node next;
        
        public Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private readonly int capacity;
    private readonly Dictionary<int, Node> map;
    private readonly Node head;
    private readonly Node tail;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new Dictionary<int, Node>();
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }
    
    public int Get(int key) {
        if (map.ContainsKey(key)) {
            Node node = map[key];
            Remove(node);
            AddToHead(node);
            return node.value;
        }
        return -1;
    }
    
    public void Put(int key, int value) {
        if (map.ContainsKey(key)) {
            Remove(map[key]);
        }
        Node newNode = new Node(key, value);
        map[key] = newNode;
        AddToHead(newNode);
        if (map.Count > capacity) {
            Node tailNode = tail.prev;
            Remove(tailNode);
            map.Remove(tailNode.key);
        }
    }
    
    private void AddToHead(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }
    
    private void Remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
}
```

**Complexity:**
- **Time complexity**: O(1) for both `get` and `put` operations. This is because all operations - adding, removing from the list, and hashmap operations are O(1).
- **Space complexity**: O(capacity) as we store up to `capacity` items in the map and list.

### Approach 2: Optimized Doubly Linked List with HashMap

The structure and operations are the same as the first approach, but you could optimize certain operations such as the removal or addition by reducing redundant connection updates, although this initial approach is already quite optimal for an LRU Cache.

Overall, the most significant optimization in practice is ensuring operations stay O(1), which is achieved by the simultaneous use of the DLL and HashMap.

This approach illustrates great synergy of a linked list for order maintenance and a hashmap for direct access — an effective pattern for similar cache problems.

