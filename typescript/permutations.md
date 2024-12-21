# [Leetcode 46: Permutations](https://leetcode.com/problems/permutations/)

## Approaches
- [Approach 1: Backtracking (Recursive)](#approach-1-backtracking-recursive)
- [Approach 2: Iterative Permutation Generation](#approach-2-iterative-permutation-generation)

### Approach 1: Backtracking (Recursive)

The recursive backtracking algorithm is a classic way to solve permutation problems. 

**Intuition:**
- The idea is to fix one element at a position and recursively generate all permutations of the remaining elements.
- For each position in the permutation, swap the current number with the number we are iterating through, recurse for the next position, and then backtrack by swapping back.

Here's a step-by-step breakdown:

1. Swap each element with the start element.
2. Recursively generate all permutations of the rest of the list.
3. Backtrack by swapping the elements back to their original positions.

This approach benefits from not requiring additional space beyond the stack and the input list.

**Code:**
```typescript
function permute(nums: number[]): number[][] {
    const results: number[][] = [];

    function backtrack(start: number): void {
        // If we reached the end, we have a complete permutation
        if (start === nums.length) {
            results.push([...nums]);
            return;
        }

        // Swap each element with the start to place it at the start position
        for (let i = start; i < nums.length; i++) {
            // Swap the current index with the start
            [nums[start], nums[i]] = [nums[i], nums[start]];
            // Recurse for the next position
            backtrack(start + 1);
            // Backtrack by swapping back
            [nums[start], nums[i]] = [nums[i], nums[start]];
        }
    }

    // Start the recursion with the first position
    backtrack(0);
    return results;
}
```

**Complexity Analysis:**
- **Time Complexity:** O(n * n!), where n! accounts for generating each permutation and n is for the swap operation.
- **Space Complexity:** O(n), considering the recursion stack depth and no extra data structures beyond the input list and result storage.

### Approach 2: Iterative Permutation Generation

This method constructs permutations iteratively by building up the list step by step.

**Intuition:**
- Start with an empty list and progressively insert elements into all positions of current permutations.
- For each number, insert it into every possible position of all existing permutations from the previous step.

This often utilizes a queue or list to build permutations in layers without recursion.

**Code:**
```typescript
function permuteIteratively(nums: number[]): number[][] {
    let results: number[][] = [[]];

    for (const num of nums) {
        const newResults: number[][] = [];
        for (const perm of results) {
            for (let i = 0; i <= perm.length; i++) {
                const newPerm = [...perm]; // Copy the current permutation
                newPerm.splice(i, 0, num); // Insert current number at position i
                newResults.push(newPerm);
            }
        }
        results = newResults;
    }

    return results;
}
```

**Complexity Analysis:**
- **Time Complexity:** O(n * n!), each level constructing permutations requires considering all current permutations and extending them.
- **Space Complexity:** O(n * n!), storing all permutations with n numbers.

Both methods efficiently generate permutations, but depending on stack size limitations or preferences for iteration, either may be preferred in different environments.

