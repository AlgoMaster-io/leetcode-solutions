# [15. 3Sum](https://leetcode.com/problems/3sum/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n^3)
// Space Complexity: O(1) (excluding output list)
import (
	"sort"
)

func threeSum(nums []int) [][]int {
	result := make(map[[3]int]bool)
	n := len(nums)

	// Iterate through all possible triplets
	for i := 0; i < n-2; i++ {
		for j := i + 1; j < n-1; j++ {
			for k := j + 1; k < n; k++ {
				if nums[i]+nums[j]+nums[k] == 0 {
					triplet := []int{nums[i], nums[j], nums[k]}
					sort.Ints(triplet) // Ensure the triplet is in sorted order
					tripletKey := [3]int{triplet[0], triplet[1], triplet[2]}
					result[tripletKey] = true // Add triplet to the set to avoid duplicates
				}
			}
		}
	}

	finalResult := [][]int{}
	for triplet := range result {
		finalResult = append(finalResult, []int{triplet[0], triplet[1], triplet[2]})
	}

	return finalResult
}
```

## Approach 2: Two Pointers (Optimal)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n) (for sorting or output list)
import (
	"sort"
)

func threeSum(nums []int) [][]int {
	var result [][]int
	sort.Ints(nums) // Sort the array to use two-pointer technique

	for i := 0; i < len(nums)-2; i++ {
		// Skip duplicates for the first element
		if i > 0 && nums[i] == nums[i-1] {
			continue
		}

		left := i + 1
		right := len(nums) - 1

		// Two-pointer approach
		for left < right {
			sum := nums[i] + nums[left] + nums[right]
			if sum == 0 {
				result = append(result, []int{nums[i], nums[left], nums[right]})

				// Skip duplicates for the second and third elements
				for left < right && nums[left] == nums[left+1] {
					left++
				}
				for left < right && nums[right] == nums[right-1] {
					right--
				}

				left++
				right--
			} else if sum < 0 {
				left++ // Increase the sum
			} else {
				right-- // Decrease the sum
			}
		}
	}

	return result
}
```

