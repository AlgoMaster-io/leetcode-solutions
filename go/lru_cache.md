# 146. [LRU Cache](https://leetcode.com/problems/lru-cache/)

## Approach: HashMap with Doubly Linked List

### Solution
go
```go
// Time Complexity: O(1) for both get and put operations
// Space Complexity: O(capacity), where capacity is the maximum number of entries in the cache
package main

import "container/list"

type LRUCache struct {
    capacity int
    cache    map[int]*list.Element
    list     *list.List
}

type Node struct {
    key, value int
}

func Constructor(capacity int) LRUCache {
    return LRUCache{
        capacity: capacity,
        cache:    make(map[int]*list.Element),
        list:     list.New(),
    }
}

func (this *LRUCache) Get(key int) int {
    if element, found := this.cache[key]; found {
        this.list.MoveToFront(element)
        return element.Value.(*Node).value
    }
    return -1
}

func (this *LRUCache) Put(key int, value int) {
    if element, found := this.cache[key]; found {
        this.list.MoveToFront(element)
        element.Value.(*Node).value = value
    } else {
        if this.list.Len() == this.capacity {
            oldest := this.list.Back()
            this.list.Remove(oldest)
            delete(this.cache, oldest.Value.(*Node).key)
        }
        node := &Node{key, value}
        element := this.list.PushFront(node)
        this.cache[key] = element
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * obj := Constructor(capacity);
 * param_1 := obj.Get(key);
 * obj.Put(key, value);
 */
```

