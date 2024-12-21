# [Leetcode 1438: Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window with Two Priority Queues](#approach-2-sliding-window-with-two-priority-queues)

### Approach 1: Brute Force

#### Intuition
The basic idea is to generate all subarrays and check if each one satisfies the condition that the maximum element minus the minimum element in the subarray is less than or equal to the given limit. This approach ensures that every possible subarray is considered, but it is very inefficient for large arrays due to its high time complexity.

#### Solution
```go
func longestSubarray(nums []int, limit int) int {
    maxLength := 0
    n := len(nums)
    
    for i := 0; i < n; i++ {
        minVal, maxVal := nums[i], nums[i]
        for j := i; j < n; j++ {
            // Update min and max in current subarray nums[i:j+1]
            if nums[j] < minVal {
                minVal = nums[j]
            }
            if nums[j] > maxVal {
                maxVal = nums[j]
            }
            
            // Check if the condition is satisfied
            if maxVal - minVal <= limit {
                maxLength = max(maxLength, j-i+1)
            }
        }
    }
    return maxLength
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

#### Complexity Analysis
- **Time Complexity**: O(n^2), where n is the number of elements in the array because we are considering all possible subarrays.
- **Space Complexity**: O(1), only a constant amount of extra space is used.

### Approach 2: Sliding Window with Two Priority Queues

#### Intuition
Instead of recalculating the min and max for every subarray, maintain two deques (double-ended queues) to store the indices of the minimum and maximum values in the current window. The deques help track the maximum and minimum values efficiently as the window slides. This approach optimizes the search for the longest subarray drastically by using the sliding window technique along with auxiliary data structures for quick max-min queries.

#### Solution
```go
func longestSubarray(nums []int, limit int) int {
    var left, right, result int
    maxDeque, minDeque := make([]int, 0), make([]int, 0)
    
    for right < len(nums) {
        // Maintain decreasing order in maxDeque
        for len(maxDeque) > 0 && nums[maxDeque[len(maxDeque)-1]] <= nums[right] {
            maxDeque = maxDeque[:len(maxDeque)-1]
        }
        maxDeque = append(maxDeque, right)

        // Maintain increasing order in minDeque
        for len(minDeque) > 0 && nums[minDeque[len(minDeque)-1]] >= nums[right] {
            minDeque = minDeque[:len(minDeque)-1]
        }
        minDeque = append(minDeque, right)
        
        // Check the window validity
        for nums[maxDeque[0]] - nums[minDeque[0]] > limit {
            left++
            // Remove elements outside the window
            if maxDeque[0] < left {
                maxDeque = maxDeque[1:]
            }
            if minDeque[0] < left {
                minDeque = minDeque[1:]
            }
        }
        
        // Calculate the max length
        result = max(result, right-left+1)
        right++
    }
    
    return result
}
```

#### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the array because each element is added and removed from the deques at most once.
- **Space Complexity**: O(n), due to the space used by the deques.

Each approach builds upon the understanding of the limitations of the previous methods and refines it for better performance while retaining clarity and correctness.

