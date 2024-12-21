# [Leetcode 39: Combination Sum](https://leetcode.com/problems/combination-sum/)

## Approaches

1. [Basic Approach: Backtracking Without Early Stopping](#basic-approach-backtracking-without-early-stopping)
2. [Optimized Backtracking with Early Stopping](#optimized-backtracking-with-early-stopping)

---

### Basic Approach: Backtracking Without Early Stopping

The **combination sum** problem involves finding all unique combinations of numbers from a given set that add up to a target. This is a classic problem suitable for a *backtracking* approach because:

1. We deal with a decision problem where we either take or don't take an element.
2. We must find all solutions, making it necessary to explore all potential combinations.

#### Intuition
- We'll use a recursive approach to explore each possible combination.
- At each step, we have the option to include the current element or skip it.
- We allow repetitions, which means that when we include an element, we should not move to the next one.
- When the exact target is met with a combination, it's added to the list of results.
- If the sum exceeds the target, we backtrack, thereby pruning that path.

#### Time and Space Complexity
- **Time Complexity**: The time complexity is \(O(2^t)\), where \(t\) is the target number, and it's exponential due to the potential combinatorial complexity.
- **Space Complexity**: \(O(n)\) to store recursive call stack and resultant combinations, where \(n\) is the maximum depth of the recursion tree.

```go
func combinationSum(candidates []int, target int) [][]int {
    var res [][]int
    var current []int
    backtrack(candidates, target, 0, &current, &res)
    return res
}

func backtrack(candidates []int, target int, start int, current *[]int, res *[][]int) {
    // If target is zero, we've found a valid combination
    if target == 0 {
        combination := make([]int, len(*current))
        copy(combination, *current)
        *res = append(*res, combination)
        return
    }
    // Iterate over remaining candidates starting from 'start'
    for i := start; i < len(candidates); i++ {
        if candidates[i] <= target {
            // Choose the current candidate
            *current = append(*current, candidates[i])
            // Explore further with reduced target
            backtrack(candidates, target-candidates[i], i, current, res)
            // Remove the last added element to backtrack
            *current = (*current)[:len(*current)-1]
        }
    }
}
```

---

### Optimized Backtracking with Early Stopping

To optimize the basic backtracking approach, we can include a sorting step. This allows us to utilize early stopping. If we find a number that surpasses the remaining target sum during iteration, we break out of the loop thus potentially reducing the number of explored paths.

#### Intuition
- Before starting the recursion, sort the list of candidates.
- Since the candidates are sorted, once a candidate number exceeds the target, further numbers would not be valid, allowing us to stop exploring further paths from that position.

#### Time and Space Complexity
- **Time Complexity**: \(O(2^t)\) in the worst case scenario for finding combinations, and \(O(n \log n)\) for sorting initially.
- **Space Complexity**: \(O(n)\) for the recursion stack and combination storage.

```go
func combinationSumOptimized(candidates []int, target int) [][]int {
    sort.Ints(candidates) // Sort the candidates to enable early stopping
    var res [][]int
    var current []int
    backtrackOptimized(candidates, target, 0, &current, &res)
    return res
}

func backtrackOptimized(candidates []int, target int, start int, current *[]int, res *[][]int) {
    if target == 0 {
        combination := make([]int, len(*current))
        copy(combination, *current) // Copy current combination to result
        *res = append(*res, combination)
        return
    }
    for i := start; i < len(candidates); i++ {
        // If current candidate exceeds the target, break out of the loop
        if candidates[i] > target {
            break
        }
        // Choose the current candidate
        *current = append(*current, candidates[i])
        // Explore further with reduced target
        backtrackOptimized(candidates, target-candidates[i], i, current, res)
        // Backtrack by removing the last element
        *current = (*current)[:len(*current)-1]
    }
}
```

These approaches provide flexible solutions to the combination sum problem by employing them through backtracking. The optimized method takes advantage of sorted inputs for early stopping and reduced recursion, offering a potential speedup in various cases.

