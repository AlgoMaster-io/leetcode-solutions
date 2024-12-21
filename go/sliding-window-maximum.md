# [Leetcode 239: Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Deque Based Sliding Window](#approach-2-deque-based-sliding-window)

---

## Approach 1: Brute Force

### Intuition
The simplest approach to this problem is to consider each possible window of size `k` and calculate the maximum element within that window. This involves iterating through the array and for each starting position of a window, iterating through the subsequent `k` elements to find the maximum. It's straightforward but inefficient for large inputs.

### Implementation

```go
func maxSlidingWindow(nums []int, k int) []int {
    if len(nums) == 0 || k == 0 {
        return []int{}
    }

    maxValues := []int{}
    for i := 0; i <= len(nums) - k; i++ {
        // Initialize the maximum value for the current window
        maxVal := nums[i]
        // Loop over each element in the window to find the maximum value
        for j := 1; j < k; j++ {
            if nums[i+j] > maxVal {
                maxVal = nums[i+j]
            }
        }
        // Append the maximum value to the result list
        maxValues = append(maxValues, maxVal)
    }
    return maxValues
}
```

### Complexity Analysis
- **Time Complexity:** O(n*k), where `n` is the length of `nums`. This is because for each window, we check `k` elements.
- **Space Complexity:** O(n-k+1), which is the number of windows we need to store the results for.

---

## Approach 2: Deque Based Sliding Window

### Intuition
To improve efficiency, we can utilize a deque data structure to keep track of indexes of the maximum elements for each window. The deque allows efficient removal of elements from both ends and maintaining a decreasing order of elements from front to back (i.e., the largest element is always at the front). As we slide the window, we pop elements from the back that are less than the current element (since they will not be needed anymore), and we also remove the front if it is out of bounds of the current window.

### Implementation

```go
func maxSlidingWindow(nums []int, k int) []int {
    if len(nums) == 0 || k == 0 {
        return []int{}
    }
    
    var deque []int
    result := make([]int, len(nums)-k+1)

    for i := 0; i < len(nums); i++ {
        // Remove elements out of window from front (as they are not useful anymore)
        if len(deque) > 0 && deque[0] < i-k+1 {
            deque = deque[1:]
        }
        
        // Remove elements from the back that are smaller than current element
        // (they will not be maximum for any future windows)
        for len(deque) > 0 && nums[deque[len(deque)-1]] <= nums[i] {
            deque = deque[:len(deque)-1]
        }
        
        // Add current index at the end of deque
        deque = append(deque, i)
        
        // The front of the deque is the largest element for the current window
        if i >= k-1 {
            result[i-k+1] = nums[deque[0]]
        }
    }
    
    return result
}
```

### Complexity Analysis
- **Time Complexity:** O(n), because each element is added and removed from the deque at most once.
- **Space Complexity:** O(k), where `k` is the size of the window, as the deque contains at most `k` elements.

As the deque approach efficiently finds the maximum for each sliding window in linear time, it is optimal for this problem.

