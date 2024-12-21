# [Leetcode Problem 146: LRU Cache](https://leetcode.com/problems/lru-cache/)

## Approaches

- [Brute Force using ArrayList](#brute-force-using-arraylist)
- [Using Doubly Linked List and HashMap](#using-doubly-linked-list-and-hashmap)

---

### Brute Force using ArrayList

#### Intuition

The Least Recently Used (LRU) Cache requires the least recently used items to be discarded first. A naive approach to implement this is through an `ArrayList` where we can keep track of order of elements. On every get or put operation, we need to move the accessed item to the end of the list to denote it's the most recently used.

#### Steps:
1. Use an array or list to store the order in which keys are used.
2. On a get operation, find the key in the list using a linear search, move it to the end, and return the value if exists.
3. On a put operation, first check if the key exists. If it does, update the value and move it to the end. If it does not exist, add them to your list and dictionary.
4. If the cache exceeds capacity during a put, remove the first element of the list (the least recently used item).

#### Code

```go
type LRUCache struct {
    capacity int
    cache    map[int]int
    order    []int
}

func Constructor(capacity int) LRUCache {
    return LRUCache{
        capacity: capacity,
        cache:    make(map[int]int),
        order:    []int{},
    }
}

func (this *LRUCache) Get(key int) int {
    // If the key exists, update its usage order and return its value
    if val, found := this.cache[key]; found {
        this.updateUsage(key)
        return val
    }
    return -1 // Key not found
}

func (this *LRUCache) Put(key int, value int) {
    // Update value if key exists
    if _, found := this.cache[key]; found {
        this.cache[key] = value
        this.updateUsage(key)
        return
    }

    // If at capacity, remove the LRU key
    if len(this.cache) == this.capacity {
        lru := this.order[0]
        this.order = this.order[1:]
        delete(this.cache, lru)
    }

    // Insert the new key-value pair
    this.cache[key] = value
    this.order = append(this.order, key)
}

func (this *LRUCache) updateUsage(key int) {
    // Remove the existing occurrence of the key
    for i := 0; i < len(this.order); i++ {
        if this.order[i] == key {
            this.order = append(this.order[:i], this.order[i+1:]...)
            break
        }
    }
    // Add the key to the end to signify recent usage
    this.order = append(this.order, key)
}
```

#### Complexity
- Time Complexity: O(n) for both get and put operations, where n is the number of keys.
- Space Complexity: O(n) for storing the cache and order.

---

### Using Doubly Linked List and HashMap

#### Intuition

For an optimal solution, we can use a combination of a doubly-linked list and a hashmap. The hashmap allows for O(1) access and modification of cache entries, and the doubly linked list allows O(1) insertion and deletion from any position given a pointer to the node.

#### Steps:
1. Create a doubly linked list where each node stores a key-value pair, along with previous and next pointers.
2. Maintain a hashmap that maps keys to their respective nodes in the doubly linked list.
3. `Get` checks the hashmap. If present, it accesses the node, moves it to the head of the linked list, and returns the value.
4. `Put` checks the capacity. If full, remove the tail node from the linked list (least recently used item) and delete its entry in the hashmap. Then add the new node to the head of the list and update the hashmap.

#### Code

```go
type DLinkedNode struct {
    key, value int
    prev, next *DLinkedNode
}

type LRUCache struct {
    capacity   int
    cache      map[int]*DLinkedNode
    head, tail *DLinkedNode
}

func initDLinkedNode(key, value int) *DLinkedNode {
    return &DLinkedNode{key: key, value: value}
}

func Constructor(capacity int) LRUCache {
    l := LRUCache{
        capacity: capacity,
        cache:    make(map[int]*DLinkedNode),
        head:     initDLinkedNode(0, 0),
        tail:     initDLinkedNode(0, 0),
    }
    l.head.next = l.tail
    l.tail.prev = l.head
    return l
}

func (this *LRUCache) Get(key int) int {
    if node, found := this.cache[key]; found {
        this.moveToHead(node)
        return node.value
    }
    return -1
}

func (this *LRUCache) Put(key int, value int) {
    if node, found := this.cache[key]; found {
        node.value = value
        this.moveToHead(node)
    } else {
        node := initDLinkedNode(key, value)
        this.cache[key] = node
        this.addToHead(node)
        if len(this.cache) > this.capacity {
            removed := this.removeTail()
            delete(this.cache, removed.key)
        }
    }
}

func (this *LRUCache) addToHead(node *DLinkedNode) {
    node.prev = this.head
    node.next = this.head.next
    this.head.next.prev = node
    this.head.next = node
}

func (this *LRUCache) removeNode(node *DLinkedNode) {
    node.prev.next = node.next
    node.next.prev = node.prev
}

func (this *LRUCache) moveToHead(node *DLinkedNode) {
    this.removeNode(node)
    this.addToHead(node)
}

func (this *LRUCache) removeTail() *DLinkedNode {
    node := this.tail.prev
    this.removeNode(node)
    return node
}
```

#### Complexity
- Time Complexity: O(1) for both get and put operations.
- Space Complexity: O(n) where n is the number of unique keys.

