# [1512. Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

func numIdenticalPairs(nums []int) int {
	count := 0

	// Compare every pair of elements
	for i := 0; i < len(nums); i++ {
		for j := i + 1; j < len(nums); j++ {
			if nums[i] == nums[j] {
				count++ // Increment count if pair is "good"
			}
		}
	}

	return count
}
```

## Approach 2: HashMap for Counting Frequencies

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func numIdenticalPairs(nums []int) int {
	countMap := make(map[int]int)
	count := 0

	// Count the occurrences of each number
	for _, num := range nums {
		if val, exists := countMap[num]; exists {
			count += val // Add the current count of this number
		}
		countMap[num]++ // Increment the count
	}

	return count
}
```

## Approach 3: Frequency Array (Optimized for Small Range)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1) (assuming the range of numbers is limited)
package main

func numIdenticalPairs(nums []int) int {
	var freq [101]int // Frequency array for numbers in range [1, 100]
	count := 0

	// Count pairs using frequency
	for _, num := range nums {
		count += freq[num] // Add the number of pairs that can be formed
		freq[num]++ // Increment the frequency of the current number
	}

	return count
}
```

