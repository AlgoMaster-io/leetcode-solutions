# 46. [Permutations](https://leetcode.com/problems/permutations/)

## Approach 1: Brute Force (Generate All Permutations)

### Solution
go
```go
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n * n!) for storing the permutations
package main

import "fmt"

func permute(nums []int) [][]int {
	var result [][]int
	visited := make([]bool, len(nums))
	generateAll(nums, []int{}, &result, visited)
	return result
}

func generateAll(nums []int, current []int, result *[][]int, visited []bool) {
	if len(current) == len(nums) {
		temp := make([]int, len(current))
		copy(temp, current)
		*result = append(*result, temp)
		return
	}

	for i := 0; i < len(nums); i++ {
		if !visited[i] {
			visited[i] = true
			current = append(current, nums[i]) // Choose the current element
			generateAll(nums, current, result, visited) // Recurse
			current = current[:len(current)-1] // Backtrack
			visited[i] = false // Reset visited state
		}
	}
}

func main() {
	nums := []int{1, 2, 3}
	fmt.Println(permute(nums))
}
```

## Approach 2: Backtracking (Optimal Solution)

### Solution
go
```go
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n * n!) for storing the permutations
package main

import "fmt"

func permute(nums []int) [][]int {
	var result [][]int
	backtrack(nums, []int{}, &result)
	return result
}

func backtrack(nums []int, current []int, result *[][]int) {
	if len(current) == len(nums) {
		temp := make([]int, len(current))
		copy(temp, current)
		*result = append(*result, temp)
		return
	}

	for _, num := range nums {
		if contains(current, num) {
			continue // Skip if the number is already in the current permutation
		}
		current = append(current, num) // Choose the current element
		backtrack(nums, current, result) // Recurse
		current = current[:len(current)-1] // Backtrack
	}
}

func contains(slice []int, num int) bool {
	for _, val := range slice {
		if val == num {
			return true
		}
	}
	return false
}

func main() {
	nums := []int{1, 2, 3}
	fmt.Println(permute(nums))
}
```

## Approach 3: Swap-Based Backtracking (In-Place Modification)

### Solution
go
```go
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n) for the recursion stack
package main

import "fmt"

func permute(nums []int) [][]int {
	var result [][]int
	backtrack(nums, 0, &result)
	return result
}

func backtrack(nums []int, start int, result *[][]int) {
	if start == len(nums) {
		current := make([]int, len(nums))
		copy(current, nums)
		*result = append(*result, current)
		return
	}

	for i := start; i < len(nums); i++ {
		swap(nums, start, i) // Swap to create a new permutation
		backtrack(nums, start+1, result) // Recurse
		swap(nums, start, i) // Backtrack to the original state
	}
}

func swap(nums []int, i, j int) {
	nums[i], nums[j] = nums[j], nums[i] // Swap two elements in the array
}

func main() {
	nums := []int{1, 2, 3}
	fmt.Println(permute(nums))
}
```

