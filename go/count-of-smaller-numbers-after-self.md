# [Count of Smaller Numbers After Self - LeetCode Problem 315](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Binary Indexed Tree (BIT)](#approach-2-binary-indexed-tree-bit)
- [Approach 3: Binary Search with Fractional Cascading](#approach-3-binary-search-with-fractional-cascading)
- [Approach 4: Merge Sort-based Solution](#approach-4-merge-sort-based-solution)

### Approach 1: Brute Force

In the brute force approach, we iterate through each element in the list, and for every element, we compare it with each of the elements to its right to check how many of them are smaller.

#### Intuition
- Traverse the list from the beginning to the end.
- For each element, compare it with every other element to its right.
- Count the number of elements that are smaller.

#### Time Complexity
- The time complexity is O(n^2) because there are two nested loops both running on the order of n.

#### Space Complexity
- The space complexity is O(n) due to the storage of the result array.

```go
func countSmaller(nums []int) []int {
    n := len(nums)
    result := make([]int, n)
    for i := 0; i < n; i++ {
        count := 0
        for j := i + 1; j < n; j++ {
            if nums[j] < nums[i] {
                count++
            }
        }
        result[i] = count
    }
    return result
}
```

### Approach 2: Binary Indexed Tree (BIT)

In this approach, we use a BIT (or Fenwick Tree) to maintain frequency counts as we iterate through the list in reverse.

#### Intuition
- Discretize the elements to be used in BIT since the values can be very large or negative.
- Traverse the list from the end to the beginning to maintain the count of seen numbers using a BIT.
- Update results array by querying the BIT for counts of numbers less than the current number.

#### Time Complexity
- O(n log n), given the operations with BIT data structure are logarithmic and we're processing each element.

#### Space Complexity
- O(n), due to the BIT storing numerical frequency information.

```go
func countSmaller(nums []int) []int {
    offset := 10000      // Offset negative to non-negative
    size := 2 * 10000 + 2
    BIT := make([]int, size)
    result := make([]int, len(nums))
    update := func(index int, delta int) {
        for i := index; i < size; i += i & -i {
            BIT[i] += delta
        }
    }
    query := func(index int) int {
        sum := 0
        for i := index; i > 0; i -= i & -i {
            sum += BIT[i]
        }
        return sum
    }
    for i := len(nums) - 1; i >= 0; i-- {
        result[i] = query(nums[i] + offset)
        update(nums[i] + offset + 1, 1)
    }
    return result
}
```

### Approach 3: Binary Search with Fractional Cascading

This approach leverages binary search by maintaining a sorted list and inserts elements as we progress.

#### Intuition
- Traverse the list from the end to the beginning.
- Insert each element in the result array into its proper position in a growing sorted array.
- Use binary search to find how many elements in the sorted array are less than the current element.

#### Time Complexity
- O(n log n), because we use binary search and insertion in a sorted list.

#### Space Complexity
- O(n), due to the auxiliary data structures used to keep track of sorted elements.

```go
import (
    "sort"
)

func countSmaller(nums []int) []int {
    result := make([]int, len(nums))
    sorted := make([]int, 0)
    
    for i := len(nums) - 1; i >= 0; i-- {
        index := sort.SearchInts(sorted, nums[i])
        result[i] = index
        sorted = append(sorted, 0)
        copy(sorted[index+1:], sorted[index:])
        sorted[index] = nums[i]
    }
    return result
}
```

### Approach 4: Merge Sort-based Solution

We modify merge sort to incorporate the counting of smaller elements efficiently during the merging process.

#### Intuition
- Use a modified merge sort to count the inversions.
- As you merge two halves, count the elements in the right half that are smaller than the elements in the left half.

#### Time Complexity
- O(n log n) due to the merge sort operations.

#### Space Complexity
- O(n), needed for the auxiliary temporary arrays used in the merge process.

```go
func countSmaller(nums []int) []int {
    n := len(nums)
    result := make([]int, n)
    indexes := make([]int, n)
    for i := range indexes {
        indexes[i] = i
    }
    mergeSort(nums, result, indexes, 0, n)
    return result
}

func mergeSort(nums, result, indexes []int, left, right int) {
    if right - left <= 1 {
        return
    }
    mid := (left + right) / 2
    mergeSort(nums, result, indexes, left, mid)
    mergeSort(nums, result, indexes, mid, right)

    temp := make([]int, right - left)
    i, j := left, mid
    for k := 0; k < right-left; k++ {
        if j == right || (i < mid && nums[indexes[i]] <= nums[indexes[j]]) {
            temp[k] = indexes[i]
            result[indexes[i]] += j - mid // this count of all that's smaller
            i++
        } else {
            temp[k] = indexes[j]
            j++
        }
    }
    copy(indexes[left:right], temp)
}
```

These are different solutions to tackle the problem of counting smaller numbers after self, starting from a brute-force approach to more optimal solutions like Merge Sort.

