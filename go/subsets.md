# [LeetCode 78: Subsets](https://leetcode.com/problems/subsets/)

## Table of Contents
1. [Backtracking Approach](#backtracking-approach)
2. [Bit Manipulation Approach](#bit-manipulation-approach)

### Backtracking Approach

The problem of generating all subsets (or power set) can be tackled by employing a backtracking approach. This method is intuitive as it involves decision-making at each step regarding whether or not to include a particular element in the current subset.

#### Intuition:
1. Start with an empty list and consider adding each element one by one.
2. For every element, we have two choices: include it in the current subset or exclude it.
3. Recursively apply the above decision until we explore all elements.
4. This method explores all paths in the decision tree, collecting valid subsets along the way.

```go
func subsets(nums []int) [][]int {
    var result [][]int
    // Helper function to backtrack
    var backtrack func(start int, current []int)
    backtrack = func(start int, current []int) {
        // Append a copy of current to the result
        result = append(result, append([]int{}, current...))
        // Try adding the next elements to the current subset
        for i := start; i < len(nums); i++ {
            // Include nums[i] in the current subset
            current = append(current, nums[i])
            // Continue with next elements
            backtrack(i+1, current)
            // Backtrack: remove the last element before trying the next option
            current = current[:len(current)-1]
        }
    }

    // Start the backtracking process
    backtrack(0, []int{})
    return result
}
```

**Time Complexity:** O(2^n), where n is the number of elements in the input array. This is because each element is either included or not included.

**Space Complexity:** O(n), which is the maximal depth of the recursion stack.

---

### Bit Manipulation Approach

The bit manipulation approach is a more mathematical method and a fascinating way to visualize the generation of subsets.

#### Intuition:
1. Treat the problem as creating a bitmask of size `n` (length of `nums`) where each bit can be either `1` (include the element) or `0` (exclude the element).
2. For each subset (from `0` to `2^n - 1`), use the binary representation of the number to decide which elements to include in the subset.
3. If bit `j` in mask `i` is set, include `nums[j]` in the current subset.

```go
func subsets(nums []int) [][]int {
    n := len(nums)
    total := 1 << n // Total number of subsets is 2^n 
    var result [][]int

    // Iterate over all possible subsets
    for i := 0; i < total; i++ {
        var subset []int
        for j := 0; j < n; j++ {
            // Check if the j-th bit in i is set (i.e., include nums[j])
            if (i>>j)&1 == 1 {
                subset = append(subset, nums[j])
            }
        }
        result = append(result, subset)
    }
    return result
}
```

**Time Complexity:** O(n * 2^n), where n is the number of elements in the input array. For each of the `2^n` subsets, we iterate through the input array.

**Space Complexity:** O(n), as we use additional space for storing each subset before adding it to the result list.

