# [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

func dailyTemperatures(temperatures []int) []int {
	n := len(temperatures)
	result := make([]int, n)

	// Compare each day's temperature with the subsequent days
	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			if temperatures[j] > temperatures[i] {
				result[i] = j - i // Days until a warmer temperature is found
				break
			}
		}
	}

	return result
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func dailyTemperatures(temperatures []int) []int {
	n := len(temperatures)
	result := make([]int, n)
	stack := []int{} // Stores indices of temperatures

	// Traverse the temperatures array
	for i := 0; i < n; i++ {
		// Process indices in the stack with temperatures less than the current
		for len(stack) > 0 && temperatures[i] > temperatures[stack[len(stack)-1]] {
			index := stack[len(stack)-1]
			stack = stack[:len(stack)-1]
			result[index] = i - index // Calculate the days until a warmer temperature
		}
		stack = append(stack, i) // Push the current day's index onto the stack
	}

	return result
}
```

## Approach 3: Reverse Traversal (Optimized for Space)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func dailyTemperatures(temperatures []int) []int {
	n := len(temperatures)
	result := make([]int, n)
	hottest := 0 // Track the hottest temperature seen from the right

	// Traverse the array in reverse
	for i := n - 1; i >= 0; i-- {
		if temperatures[i] >= hottest {
			result[i] = 0 // No warmer temperature in the future
			hottest = temperatures[i]
		} else {
			days := 1
			// Find the next warmer day using result array
			for temperatures[i+days] <= temperatures[i] {
				days += result[i+days]
			}
			result[i] = days
		}
	}

	return result
}
```

