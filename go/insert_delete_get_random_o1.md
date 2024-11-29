# 380. [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## Approach: HashMap with Slice

### Solution
```go
// Time Complexity: O(1) for insert, delete, and getRandom
// Space Complexity: O(n), where n is the number of elements in the data structure
import (
    "math/rand"
    "time"
)

type RandomizedSet struct {
    valueToIndex map[int]int
    values       []int
}

func Constructor() RandomizedSet {
    rand.Seed(time.Now().UnixNano())
    return RandomizedSet{
        valueToIndex: make(map[int]int),
        values:       []int{},
    }
}

func (this *RandomizedSet) Insert(val int) bool {
    if _, exists := this.valueToIndex[val]; exists {
        return false // Element already exists
    }
    // Add the value to the slice and its index to the map
    this.valueToIndex[val] = len(this.values)
    this.values = append(this.values, val)
    return true
}

func (this *RandomizedSet) Remove(val int) bool {
    if _, exists := this.valueToIndex[val]; !exists {
        return false // Element does not exist
    }

    // Swap the element to be removed with the last element in the slice
    index := this.valueToIndex[val]
    lastValue := this.values[len(this.values)-1]
    this.values[index] = lastValue
    this.valueToIndex[lastValue] = index

    // Remove the last element
    this.values = this.values[:len(this.values)-1]
    delete(this.valueToIndex, val)
    return true
}

func (this *RandomizedSet) GetRandom() int {
    // Return a random element from the slice
    return this.values[rand.Intn(len(this.values))]
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Insert(val);
 * param_2 := obj.Remove(val);
 * param_3 := obj.GetRandom();
 */
```

