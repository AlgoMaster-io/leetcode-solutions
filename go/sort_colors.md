# [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

## Approach 1: Two-Pass Counting Sort

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
func sortColors(nums []int) {
    count := make([]int, 3)

    // Count the occurrences of 0, 1, and 2
    for _, num := range nums {
        count[num]++
    }

    // Overwrite the array based on the counts
    index := 0
    for i := 0; i < 3; i++ {
        for count[i] > 0 {
            nums[index] = i
            index++
            count[i]--
        }
    }
}
```

## Approach 2: One-Pass (Dutch National Flag Algorithm)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
func sortColors(nums []int) {
    low, mid, high := 0, 0, len(nums)-1

    for mid <= high {
        if nums[mid] == 0 {
            // Swap nums[low] and nums[mid] and move pointers
            nums[low], nums[mid] = nums[mid], nums[low]
            low++
            mid++
        } else if nums[mid] == 1 {
            // Just move the mid pointer
            mid++
        } else {
            // Swap nums[mid] and nums[high] and move high pointer
            nums[mid], nums[high] = nums[high], nums[mid]
            high--
        }
    }
}
```

