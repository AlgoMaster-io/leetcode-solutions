# [Leetcode 39: Combination Sum](https://leetcode.com/problems/combination-sum/)

## Approaches
- [Backtracking Approach](#backtracking-approach)

### Backtracking Approach

Backtracking is an algorithmic technique for solving problems incrementally, by trying partial solutions and then abandoning them if they are not valid. It is often seen in problems that ask for all possible combinations or permutations.

**Intuition:**
The problem asks for all unique combinations in `candidates` where the candidate numbers sum to the target. Each number in `candidates` can be used unlimited times. The idea is to explore each possibility for each number using backtracking. We'll use a recursive approach to try different combinations by either including the current candidate or skipping it. If the sum equals the target, we add the current combination to our solution.

Steps:
1. Sort the candidates to help with efficiency.
2. Use a helper function `backtrack` passing the remaining sum, a starting index to avoid duplicates, and the current path of numbers forming a combination.
3. Base case: If the remaining sum is 0, we found a valid combination.
4. If the remaining sum goes below zero, stop further exploration in this path.
5. Recursively call the function adding the current candidate as part of our combination.

**Code:**
```typescript
function combinationSum(candidates: number[], target: number): number[][] {
    // Sort candidates to possibly improve efficiency and help in pruning branches
    candidates.sort((a, b) => a - b);
    const result: number[][] = [];
    
    function backtrack(remainingSum: number, startIndex: number, currentCombination: number[]): void {
        // If the remaining sum is zero, we found a valid combination.
        if (remainingSum === 0) {
            result.push([...currentCombination]);
            return;
        }
        
        // If the remaining sum is less than zero, there's no point in continuing the exploration
        if (remainingSum < 0) {
            return;
        }
        
        // Explore further with recursive backtracking
        for (let i = startIndex; i < candidates.length; i++) {
            const currentCandidate = candidates[i];
            // Include current candidate in the combination
            currentCombination.push(currentCandidate);
            // Explore further with a reduced remaining sum (considering unlimited use of the candidate)
            backtrack(remainingSum - currentCandidate, i, currentCombination);
            // Backtrack: Remove the last added candidate and try the next
            currentCombination.pop();
        }
    }
    
    // Start backtracking with initial inputs
    backtrack(target, 0, []);
    
    return result;
}
```

**Time Complexity:**
- The time complexity is `O(2^t)`, where `t` is the target value. This is because each number can either be included or not, leading to exponential time in the number of decisions we make.
  
**Space Complexity:**
- The space complexity is also `O(t)`, where `t` is the target, due to the maximum depth of recursive calls in our stack.

This approach efficiently explores potential combinations using the candidates array, ensuring that each valid sum-targeting combination is only constructed once.

