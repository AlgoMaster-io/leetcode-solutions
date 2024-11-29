# 493. [Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

## Approach 1: Merge Sort (Divide and Conquer)

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(n)
package main

func reversePairs(nums []int) int {
    if nums == nil || len(nums) < 2 {
        return 0
    }
    return mergeSort(nums, 0, len(nums)-1)
}

func mergeSort(nums []int, left, right int) int {
    if left >= right {
        return 0
    }

    mid := left + (right-left)/2

    // Count reverse pairs in left and right halves, and across them
    count := mergeSort(nums, left, mid) + mergeSort(nums, mid+1, right)

    // Count cross reverse pairs
    j := mid + 1
    for i := left; i <= mid; i++ {
        for j <= right && int64(nums[i]) > 2*int64(nums[j]) {
            j++
        }
        count += (j - mid - 1)
    }

    // Merge the two halves
    merge(nums, left, mid, right)

    return count
}

func merge(nums []int, left, mid, right int) {
    temp := make([]int, right-left+1)
    i, j, k := left, mid+1, 0

    for i <= mid && j <= right {
        if nums[i] <= nums[j] {
            temp[k] = nums[i]
            i++
        } else {
            temp[k] = nums[j]
            j++
        }
        k++
    }

    for i <= mid {
        temp[k] = nums[i]
        i++
        k++
    }

    for j <= right {
        temp[k] = nums[j]
        j++
        k++
    }

    for i := 0; i < len(temp); i++ {
        nums[left+i] = temp[i]
    }
}
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(n)
package main

import "sort"

func reversePairs(nums []int) int {
    // Coordinate compression
    set := make(map[int64]struct{})
    for _, num := range nums {
        set[int64(num)] = struct{}{}
        set[int64(num)*2] = struct{}{}
    }

    keys := make([]int64, 0, len(set))
    for key := range set {
        keys = append(keys, key)
    }
    sort.Slice(keys, func(i, j int) bool { return keys[i] < keys[j] })

    mapIndex := make(map[int64]int)
    for i, v := range keys {
        mapIndex[v] = i + 1
    }

    // Binary Indexed Tree
    bit := make([]int, len(mapIndex)+1)
    count := 0

    for i := len(nums) - 1; i >= 0; i-- {
        // Count elements smaller than nums[i]
        count += query(bit, mapIndex[int64(nums[i])]-1)

        // Add nums[i] * 2 to the BIT
        update(bit, mapIndex[int64(nums[i])*2], 1)
    }

    return count
}

func update(bit []int, index, delta int) {
    for index < len(bit) {
        bit[index] += delta
        index += index & -index
    }
}

func query(bit []int, index int) int {
    sum := 0
    for index > 0 {
        sum += bit[index]
        index -= index & -index
    }
    return sum
}
```


