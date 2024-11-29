# 40. [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

## Approach 1: Backtracking with Duplicate Handling

### Solution
go
```go
// Time Complexity: O(2^n), where n is the number of candidates
// Space Complexity: O(k), where k is the average length of a combination
package main

import (
	"sort"
)

func combinationSum2(candidates []int, target int) [][]int {
	var result [][]int
	sort.Ints(candidates) // Sort the candidates to handle duplicates
	backtrack(candidates, target, 0, []int{}, &result)
	return result
}

func backtrack(candidates []int, target, start int, current []int, result *[][]int) {
	if target == 0 {
		combination := make([]int, len(current))
		copy(combination, current)
		*result = append(*result, combination) // Add the valid combination to the result
		return
	}

	for i := start; i < len(candidates); i++ {
		if i > start && candidates[i] == candidates[i-1] {
			continue // Skip duplicates
		}
		if candidates[i] > target {
			break // Stop the loop if the current candidate exceeds the target
		}

		current = append(current, candidates[i]) // Choose the current candidate
		backtrack(candidates, target-candidates[i], i+1, current, result) // Recurse with the next index
		current = current[:len(current)-1] // Backtrack to explore other combinations
	}
}
```

## Approach 2: Iterative with Stack (Less Common)

### Solution
go
```go
// Time Complexity: O(2^n), where n is the number of candidates
// Space Complexity: O(k), where k is the average length of a combination
package main

import (
	"sort"
	"container/list"
)

func combinationSum2Iterative(candidates []int, target int) [][]int {
	var result [][]int
	sort.Ints(candidates) // Sort the candidates to handle duplicates

	stack := list.New()
	stack.PushBack([]interface{}{0, target, -1, []int{}}) // Initial state: [currentSum, remainingTarget, previousIndex, currentCombination]

	for stack.Len() > 0 {
		ele := stack.Back()
		stack.Remove(ele)
		state := ele.Value.([]interface{})
		currentSum, remainingTarget, previousIndex, currentCombination := state[0].(int), state[1].(int), state[2].(int), state[3].([]int)

		if remainingTarget == 0 {
			combination := make([]int, len(currentCombination))
			copy(combination, currentCombination)
			result = append(result, combination) // Add valid combinations
			continue
		}

		sz := len(currentCombination)
		for i := previousIndex + 1; i < len(candidates); i++ {
			if i > previousIndex+1 && candidates[i] == candidates[i-1] {
				continue // Skip duplicates
			}
			if candidates[i] > remainingTarget {
				break // Stop if the candidate exceeds the remaining target
			}

			newCombination := append(make([]int, 0, sz+1), currentCombination...)
			newCombination = append(newCombination, candidates[i])
			stack.PushBack([]interface{}{currentSum + candidates[i], remainingTarget - candidates[i], i, newCombination})
		}
	}
	return result
}
```

