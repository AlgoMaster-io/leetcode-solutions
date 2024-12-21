# [LeetCode 209: Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pointers / Sliding Window](#approach-2-two-pointers--sliding-window)

### Approach 1: Brute Force

#### Intuition:
A straightforward approach is to find every possible subarray and check if its sum is at least `s`. This involves checking all subarrays starting from each element and calculating their sum until it satisfies the condition. Though simple, this approach is inefficient because it examines all possible subarrays, leading to a high time complexity.

#### Code:
```go
func minSubArrayLen(target int, nums []int) int {
	n := len(nums)
	min_length := n + 1  // Initialize min_length to an invalid large value

	// Iterate over all possible starting points of the subarrays
	for start := 0; start < n; start++ {
		current_sum := 0

		// Sum elements from the current starting point to the end
		for end := start; end < n; end++ {
			current_sum += nums[end]

			// Once sum is at least target, update the min_length if needed
			if current_sum >= target {
				min_length = min(min_length, end-start+1)
				break
			}
		}
	}

	// If min_length was updated from its initial value, return it, else return 0
	if min_length == n+1 {
		return 0
	}
	return min_length
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

#### Time Complexity:
- **O(n^2)**, where `n` is the number of elements in `nums`. We examine all subarrays.

#### Space Complexity:
- **O(1)**, we use only a constant amount of extra space.

### Approach 2: Two Pointers / Sliding Window

#### Intuition:
The optimal approach uses the sliding window (or two pointers) technique to maintain a window that satisfies the sum condition. As we expand the window to include more elements, we also contract it from the start to find the minimal length whenever the target condition is met. This is far more efficient than the brute force approach because it effectively reduces unnecessary recalculations by maintaining a running sum.

#### Code:
```go
func minSubArrayLen(target int, nums []int) int {
	n := len(nums)
	min_length := n + 1  // Initialize to an invalid large value
	start := 0
	current_sum := 0

	// Use end to expand the window
	for end := 0; end < n; end++ {
		current_sum += nums[end]

		// Contract the window from the start as long as the sum >= target
		for current_sum >= target {
			min_length = min(min_length, end-start+1)
			current_sum -= nums[start]
			start++
		}
	}

	// Check if we've found a valid subarray
	if min_length == n+1 {
		return 0
	}
	return min_length
}
```

#### Time Complexity:
- **O(n)**, where `n` is the number of elements in `nums`. Each element is processed at most twice (once when the end pointer is moved, and once when the start pointer moves).

#### Space Complexity:
- **O(1)**, as the algorithm uses a constant amount of extra space.

