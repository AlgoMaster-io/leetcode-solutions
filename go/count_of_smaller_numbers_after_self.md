# 315. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
package main

import "fmt"

func countSmaller(nums []int) []int {
    result := make([]int, 0, len(nums))
    
    // Traverse each element and count smaller numbers to its right
    for i := 0; i < len(nums); i++ {
        count := 0
        for j := i + 1; j < len(nums); j++ {
            if nums[j] < nums[i] {
                count++
            }
        }
        result = append(result, count)
    }
    
    return result
}

func main() {
    nums := []int{5, 2, 6, 1}
    fmt.Println(countSmaller(nums))
}
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(n)
package main

import (
    "fmt"
    "sort"
)

func countSmaller(nums []int) []int {
    result := make([]int, len(nums))
    sortedNums := make([]int, len(nums))
    copy(sortedNums, nums)
    
    // Sort and remove duplicates from nums
    sort.Ints(sortedNums)
    fenwickTree := make([]int, len(nums)+1)
    
    // Traverse nums from right to left
    for i := len(nums) - 1; i >= 0; i-- {
        rank := sort.SearchInts(sortedNums, nums[i]) + 1
        result[i] = query(rank-1, fenwickTree)
        update(rank, 1, fenwickTree)
    }
    
    return result
}

// Update the Fenwick Tree
func update(index int, value int, fenwickTree []int) {
    for index < len(fenwickTree) {
        fenwickTree[index] += value
        index += index & -index
    }
}

// Query the prefix sum in the Fenwick Tree
func query(index int, fenwickTree []int) int {
    sum := 0
    for index > 0 {
        sum += fenwickTree[index]
        index -= index & -index
    }
    return sum
}

func main() {
    nums := []int{5, 2, 6, 1}
    fmt.Println(countSmaller(nums))
}
```

## Approach 3: Merge Sort

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(n)
package main

import (
    "fmt"
)

var counts []int

func countSmaller(nums []int) []int {
    counts = make([]int, len(nums))
    indices := make([]int, len(nums))
    
    // Initialize indices
    for i := range nums {
        indices[i] = i
    }
    
    // Perform merge sort
    mergeSort(nums, indices, 0, len(nums)-1)
    
    // Convert counts array to list
    return counts
}

// Perform merge sort and count smaller elements
func mergeSort(nums []int, indices []int, left int, right int) {
    if left >= right {
        return
    }
    
    mid := left + (right-left)/2
    mergeSort(nums, indices, left, mid)
    mergeSort(nums, indices, mid+1, right)
    
    merge(nums, indices, left, mid, right)
}

// Merge two sorted halves and update counts
func merge(nums []int, indices []int, left int, mid int, right int) {
    tempIndices := make([]int, right-left+1)
    i, j, k, smallerCount := left, mid+1, 0, 0
    
    // Merge the two halves
    for i <= mid && j <= right {
        if nums[indices[i]] <= nums[indices[j]] {
            counts[indices[i]] += smallerCount
            tempIndices[k] = indices[i]
            i++
        } else {
            smallerCount++
            tempIndices[k] = indices[j]
            j++
        }
        k++
    }
    
    // Copy remaining elements
    for i <= mid {
        counts[indices[i]] += smallerCount
        tempIndices[k] = indices[i]
        i++
        k++
    }
    for j <= right {
        tempIndices[k] = indices[j]
        j++
        k++
    }
    
    // Copy merged result back to indices
    copy(indices[left:right+1], tempIndices)
}

func main() {
    nums := []int{5, 2, 6, 1}
    fmt.Println(countSmaller(nums))
}
```

