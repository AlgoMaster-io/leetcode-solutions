# [Leetcode 46: Permutations](https://leetcode.com/problems/permutations/)

## Approaches

1. [Basic Backtracking](#basic-backtracking)
2. [Backtracking with In-place Swapping](#backtracking-with-in-place-swapping)

---

### Basic Backtracking

This is the most straightforward method to generate permutations using the backtracking approach. The idea is to build permutations by progressively constructing each permutation and then backtracking if it doesn’t lead to a solution.

**Intuition:**

- Start by iterating over each number and visualize placing it in the first position.
- Continue selecting numbers for subsequent positions.
- Ensure no number is placed more than once at the same level of recursion by tracking chosen numbers.
- If all numbers are chosen, a permutation is completed and stored.

```go
func permute(nums []int) [][]int {
    var result [][]int
    var current []int
    used := make([]bool, len(nums))

    var backtrack func()
    backtrack = func() {
        // Base case: if the current permutation has as many numbers as nums, it's complete
        if len(current) == len(nums) {
            // Append a copy of the current permutation to the result
            perm := make([]int, len(current))
            copy(perm, current)
            result = append(result, perm)
            return
        }

        // Explore choices
        for i, num := range nums {
            if !used[i] {
                // Choose number num, marking it used
                used[i] = true
                current = append(current, num)

                // Recurse: explore with current number choice
                backtrack()

                // Undo choice: backtrack
                current = current[:len(current)-1]
                used[i] = false
            }
        }
    }

    // Start backtracking with an empty permutation
    backtrack()
    return result
}
```

**Time Complexity:** \(O(n \times n!)\) - There are \(n!\) permutations and each permutation of size \(n\) takes \(O(n)\) time to build a copy.

**Space Complexity:** \(O(n!)\) - Storing all permutations requires \(n!\) space.

---

### Backtracking with In-place Swapping

This technique optimizes the space by eliminating the need to track used numbers separately. Instead, we manipulate the input list directly by swapping numbers in-place.

**Intuition:**

- Treat swapping as the choice-making mechanism. For each number at a given position, swap it with any number from current to the end.
- Recursively call the function allowing the next number in line to be fixed.
- After recursion, backtrack by swapping back to restore the array.

```go
func permute(nums []int) [][]int {
    var result [][]int

    var backtrack func(first int)
    backtrack = func(first int) {
        // If first index equals length of nums, a permutation is completed
        if first == len(nums) {
            perm := make([]int, len(nums))
            copy(perm, nums)
            result = append(result, perm)
            return
        }

        for i := first; i < len(nums); i++ {
            // Place i-th integer first in the current permutation
            nums[first], nums[i] = nums[i], nums[first]

            // Use recursion to permute the remaining integers
            backtrack(first + 1)

            // Backtrack: swap back to the original
            nums[first], nums[i] = nums[i], nums[first]
        }
    }

    // Start backtracking with the first index
    backtrack(0)
    return result
}
```

**Time Complexity:** \(O(n \times n!)\) - Just like the previous method, it achieves \(n!\) permutations with linear time complexity for each permutation.

**Space Complexity:** \(O(n!)\) - Uses the input array for in-place swaps but requires storage for result permutations.

---

Both techniques effectively solve the permutations problem with recursive backtracking. The in-place swap variant provides an interesting optimization in terms of auxiliary space usage beyond the output storage.

