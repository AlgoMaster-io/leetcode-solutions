# [Leetcode 34: Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Approaches
1. [Linear Search](#linear-search)
2. [Binary Search](#binary-search)
3. [Optimal Binary Search](#optimal-binary-search)

---

## 1. Linear Search

### Intuition
In this approach, we simply iterate over each element in the array to find the first and the last occurrence of the target element. This is the most straightforward solution and works by checking each element sequentially.

### Steps
- Initialize two variables, `first` and `last`, to keep track of the first and last positions of the target.
- Iterate through the array.
- If you encounter the target and `first` is still -1, set `first` to the current index.
- Continue to set `last` to the current index while iterating over the target.
- Return an array containing `first` and `last`.

### Solution
```go
func searchRange(nums []int, target int) []int {
    first, last := -1, -1
    for i := 0; i < len(nums); i++ {
        if nums[i] == target {
            if first == -1 {
                first = i
            }
            last = i
        }
    }
    return []int{first, last}
}
```

### Time Complexity
- O(n), where n is the number of elements in the array.

### Space Complexity
- O(1), as no extra space is used beyond input size.

---

## 2. Binary Search

### Intuition
We can improve on the linear search by leveraging the fact that the input array is sorted. Using binary search, we can efficiently find an occurrence of the target. Once found, we can iterate forward and backward to determine the first and last positions.

### Steps
- Perform a binary search to find any occurrence of the target.
- If found, linearly search backwards from that index to find the first position.
- Similarly, linearly search forwards to find the last position.

### Solution
```go
func searchRange(nums []int, target int) []int {
    low, high := 0, len(nums)-1
    index := -1
    
    // Binary Search to find one occurence of target
    for low <= high {
        mid := low + (high - low) / 2
        if nums[mid] == target {
            index = mid
            break
        } else if nums[mid] < target {
            low = mid + 1
        } else {
            high = mid - 1
        }
    }
    
    if index == -1 {
        return []int{-1, -1}
    }
    
    // Find first position
    first := index
    for first > 0 && nums[first - 1] == target {
        first--
    }
    
    // Find last position
    last := index
    for last < len(nums)-1 && nums[last + 1] == target {
        last++
    }
    
    return []int{first, last}
}
```

### Time Complexity
- O(log n) for the binary search and O(n) for finding the first and last positions - effectively O(n) in the worst case.

### Space Complexity
- O(1), as no extra space is used beyond input size.

---

## 3. Optimal Binary Search

### Intuition
To ensure optimal performance strictly within O(log n), we can perform binary search twice: first to find the start position and then to find the end position. This approach leverages binary search to minimize the number of elements checked.

### Steps
- Implement a helper function for binary search to find the leftmost (first) and rightmost (last) positions of the target.
- For the leftmost search, adjust the high pointer; for the rightmost, adjust the low pointer accordingly.
- Return the indices found by the two binary searches.

### Solution
```go
func searchRange(nums []int, target int) []int {
    first := findBound(nums, target, true)
    if first == -1 {
        return []int{-1, -1}
    }
    last := findBound(nums, target, false)
    return []int{first, last}
}

func findBound(nums []int, target int, isFirst bool) int {
    low, high := 0, len(nums)-1
    for low <= high {
        mid := low + (high - low) / 2
        if nums[mid] == target {
            if isFirst {
                // Check if this is the leftmost index
                if mid == low || nums[mid-1] != target {
                    return mid
                }
                high = mid - 1
            } else {
                // Check if this is the rightmost index
                if mid == high || nums[mid+1] != target {
                    return mid
                }
                low = mid + 1
            }
        } else if nums[mid] < target {
            low = mid + 1
        } else {
            high = mid - 1
        }
    }
    return -1
}
```

### Time Complexity
- O(log n) for both finding the first and the last position using binary search.

### Space Complexity
- O(1), no additional space is required beyond input size.

---

