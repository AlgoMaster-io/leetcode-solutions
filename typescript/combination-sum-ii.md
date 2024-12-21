# [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

In this problem, we are given a collection of candidate numbers (`candidates`) and a target number (`target`), and we need to find all unique combinations in `candidates` where the candidate numbers sum to `target`. Each number in `candidates` may only be used once in the combination.

## Approaches
- ### [Approach 1: Backtracking with Sorting](#approach-1-backtracking-with-sorting)

## Approach 1: Backtracking with Sorting

**Intuition**

This problem is a classic example of using backtracking to generate combinations. With a twist, we need to handle duplicates carefully. Here's the broad plan:
1. **Sort the candidates**: By sorting, we can handle duplicates easily. If two consecutive elements are the same and the first one is not part of the current combination, skip it.
2. **Backtracking**: Begin with an empty combination and progressively add candidates, recursively attempting to reach the target.
3. **Skip duplicates**: Whenever encountering a duplicate candidate (one that is the same as the previous one in the sorted list), skip it to prevent duplicate combinations.
4. **Stop early**: If the sum of the combination exceeds the target during any stage of the decision tree, prune that path immediately.

With this approach, we'll be able to ensure that every unique combination is checked exactly once.

**Time Complexity**: O(2^n), in the worst case scenario where we check each possible combination. However, the sorting step takes O(n log n).

**Space Complexity**: O(n), where n is the number of candidates. This is the space required to maintain the recursive stack and the temporary combination list.

```typescript
function combinationSum2(candidates: number[], target: number): number[][] {
    const result: number[][] = [];
    
    // Sort the candidates to handle duplicates easily
    candidates.sort((a, b) => a - b);

    function backtrack(start: number, currentCombination: number[], remainingTarget: number): void {
        // When the remaining target is zero, we have found a valid combination
        if (remainingTarget === 0) {
            result.push([...currentCombination]);
            return;
        }

        for (let i = start; i < candidates.length; i++) {
            // Skip duplicates
            if (i > start && candidates[i] === candidates[i - 1]) continue;

            const candidate = candidates[i];

            // If the candidate is greater than the remaining target, no need to proceed further
            if (candidate > remainingTarget) break;

            // Choose the current candidate
            currentCombination.push(candidate);

            // Explore further with the chosen candidate
            backtrack(i + 1, currentCombination, remainingTarget - candidate);

            // Undo the choice for backtracking
            currentCombination.pop();
        }
    }
    
    backtrack(0, [], target);
    return result;
}
```

- **Explanation**:
  - We start by sorting the candidates array, which allows us to skip duplicates easily.
  - We use a helper function `backtrack` to attempt building combinations, starting from a given index.
  - We only add a combination to the results if it exactly meets the target, thus ensuring all combinations are valid.
  - For each recursive step, we either include a candidate in the current combination or skip it.

This method efficiently finds all unique combinations that sum to a given target, leveraging both sorting and backtracking to manage complexity and avoid duplicates.

