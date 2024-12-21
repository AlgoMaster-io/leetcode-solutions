# [Leetcode 380: Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## Approaches
- [Approach 1: Using Hashmap and ArrayList](#approach-1-using-hashmap-and-arraylist)
- [Approach 2: Optimized Hashmap and ArrayList with Swapping](#approach-2-optimized-hashmap-and-arraylist-with-swapping)

### Approach 1: Using Hashmap and ArrayList

**Intuition**: The problem requires constant time operations for insertion, deletion, and getting a random element. A hashmap allows for O(1) average time complexity for these operations, but it does not support index-based operations which we're often required to perform for random access. An array allows for O(1) time complexity for retrieving elements by index. By combining these two data structures, we can achieve the desired time complexity.

1. **Insertion**: Insert the element at the end of the list and store its index in the hashmap.
2. **Deletion**: To delete an element, find its index from the hashmap, and swap it with the last element of the list. Update the hashmap for the element moved to the index and then remove the element from the hashmap and the list.
3. **GetRandom**: Simply return a random element from the list. As lists support index-based operations, this can be done in O(1) time.

```go
import (
    "math/rand"
    "time"
)

type RandomizedSet struct {
    nums    []int
    hashMap map[int]int
}

func Constructor() RandomizedSet {
    rand.Seed(time.Now().UnixNano())
    return RandomizedSet{
        nums:    make([]int, 0),
        hashMap: make(map[int]int),
    }
}

func (this *RandomizedSet) Insert(val int) bool {
    if _, exists := this.hashMap[val]; exists {
        return false // Element already exists, don't insert
    }

    this.hashMap[val] = len(this.nums) // Store index in hashmap
    this.nums = append(this.nums, val) // Append to list
    return true
}

func (this *RandomizedSet) Remove(val int) bool {
    index, exists := this.hashMap[val]
    if !exists {
        return false // Element doesn't exist to remove
    }

    // Swap the last element with the element to delete
    lastElement := this.nums[len(this.nums)-1]
    this.nums[index] = lastElement
    this.hashMap[lastElement] = index

    // Remove last element
    this.nums = this.nums[:len(this.nums)-1]
    delete(this.hashMap, val)
    return true
}

func (this *RandomizedSet) GetRandom() int {
    return this.nums[rand.Intn(len(this.nums))]
}
```

**Time Complexity**: 
- `Insert`: O(1) average case due to hashmap operations.
- `Remove`: O(1) due to a constant number of swaps and hashmap operations.
- `GetRandom`: O(1) as we retrieve an element at a random index.
  
**Space Complexity**: O(n) where n is the number of elements in the set. We need to maintain the list and the hashmap.

### Approach 2: Optimized Hashmap and ArrayList with Swapping

**Intuition**: This approach is essentially the same as the first but highlights that the act of swapping and managing indices accurately can optimize the deletion process, ensuring it remains in constant time by handling indices meticulously.

The intuition remains the same as in Approach 1, but often involves ensuring that the swapped elements in the array are consistently tracked in the hashmap, which enables a solid understanding of the linkage between indices and array elements.

For the most optimal solutions, attention must also be paid to edge cases, such as when the list is at the start or end or when duplicate removal is checked efficiently via hashmaps.

This approach is effectively an explanation enhancement over Approach 1 and utilizes detailed management of indices to ensure no overhead occurs due to list operations beyond necessary.

The implementation remains the same as in the first approach, reflecting a highly optimized handling of swaps and deletions using both list and hashmap effectively.

**Time Complexity**:
- `Insert`: O(1) average case
- `Remove`: O(1) with a definite handling of swap operations
- `GetRandom`: O(1)

**Space Complexity**: O(n) similar to Approach 1.

By ensuring such meticulous management of the swap process, this approach lays excellent groundwork for handling dynamic datasets efficiently.

