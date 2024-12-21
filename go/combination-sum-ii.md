# [Combination Sum II - LeetCode](https://leetcode.com/problems/combination-sum-ii/)

## Approaches
- [Backtracking Approach](#backtracking-approach)

## Backtracking Approach

**Intuition:**

The problem asks for unique combinations in `candidates` that sum up to `target`. Each number in `candidates` may only be used once in the combination. This is a classic problem that can be effectively tackled using backtracking. The key aspects of this approach are:
- Sort the `candidates` first. This helps in both identifying duplicates and stopping early when the remaining candidates cannot sum up to the target.
- Use a backtrack function to explore possible combinations, keeping track of the current combination and the remaining target sum.
- Skip duplicates to avoid repeated combinations.

```go
func combinationSum2(candidates []int, target int) [][]int {
    // Sort the candidates to manage duplicates and early stopping
    sort.Ints(candidates)
    var result [][]int
    var combination []int

    // Backtracking function
    var backtrack func(start int, target int)
    backtrack = func(start int, target int) {
        if target == 0 {
            // Found a valid combination
            temp := make([]int, len(combination))
            copy(temp, combination)
            result = append(result, temp)
            return
        }

        for i := start; i < len(candidates); i++ {
            // Skip duplicates
            if i > start && candidates[i] == candidates[i-1] {
                continue
            }
            // Break the loop if the current number is greater than target
            if candidates[i] > target {
                break
            }
            // Choose the current number
            combination = append(combination, candidates[i])
            // Explore further with reduced target
            backtrack(i+1, target-candidates[i])
            // Undo the last choice
            combination = combination[:len(combination)-1]
        }
    }

    // Start backtracking from the 0th index
    backtrack(0, target)
    return result
}
```

**Time Complexity:** O(2^N), where N is the number of candidates. In the worst case, each number could be a part of some combination (though practically, this will be much lower due to pruning, i.e., skipping numbers that exceed the target).

**Space Complexity:** O(N) for recursion stack space, where N is the depth of the recursion tree. This can be at most the number of elements in `candidates` due to modifications of `combination`. Moreover, additional space is used to store the resulting combinations, sharing the same complexity class.

