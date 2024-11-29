# [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

## Approach 1: Using Extra Array

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
func rotate(nums []int, k int) {
    n := len(nums)
    k = k % n // Handle cases where k > n
    rotated := make([]int, n)

    // Copy rotated elements to a new array
    for i := 0; i < n; i++ {
        rotated[(i+k)%n] = nums[i]
    }

    // Copy elements back to the original array
    for i := 0; i < n; i++ {
        nums[i] = rotated[i]
    }
}
```

## Approach 2: Rotate One-by-One (Brute Force)

### Solution
go
```go
// Time Complexity: O(n * k)
// Space Complexity: O(1)
func rotate(nums []int, k int) {
    n := len(nums)
    k = k % n // Handle cases where k > n

    // Rotate the array k times
    for i := 0; i < k; i++ {
        last := nums[n-1] // Save the last element
        // Shift elements to the right
        for j := n - 1; j > 0; j-- {
            nums[j] = nums[j-1]
        }
        nums[0] = last // Place the saved element at the beginning
    }
}
```

## Approach 3: Reverse Array (Optimal)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
func rotate(nums []int, k int) {
    n := len(nums)
    k = k % n // Handle cases where k > n

    // Step 1: Reverse the entire array
    reverse(nums, 0, n-1)

    // Step 2: Reverse the first k elements
    reverse(nums, 0, k-1)

    // Step 3: Reverse the remaining n-k elements
    reverse(nums, k, n-1)
}

func reverse(nums []int, start, end int) {
    for start < end {
        nums[start], nums[end] = nums[end], nums[start]
        start++
        end--
    }
}
```

## Approach 4: Cyclic Replacements

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
func rotate(nums []int, k int) {
    n := len(nums)
    k = k % n // Handle cases where k > n
    count := 0 // Number of elements rotated

    for start := 0; count < n; start++ {
        current := start
        prev := nums[start]

        for {
            next := (current + k) % n
            temp := nums[next]
            nums[next] = prev
            prev = temp
            current = next
            count++

            if start == current {
                break // Cycle ends when we return to the start
            }
        }
    }
}
```

