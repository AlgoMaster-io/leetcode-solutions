# 40. [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

## Approach 1: Backtracking with Duplicate Handling

### Solution
```javascript
// Time Complexity: O(2^n), where n is the number of candidates
// Space Complexity: O(k), where k is the average length of a combination
function combinationSum2(candidates, target) {
    const result = [];
    candidates.sort((a, b) => a - b); // Sort the candidates to handle duplicates
    backtrack(candidates, target, 0, [], result);
    return result;
}

function backtrack(candidates, target, start, current, result) {
    if (target === 0) {
        result.push([...current]); // Add the valid combination to the result
        return;
    }

    for (let i = start; i < candidates.length; i++) {
        if (i > start && candidates[i] === candidates[i - 1]) {
            continue; // Skip duplicates
        }
        if (candidates[i] > target) {
            break; // Stop the loop if the current candidate exceeds the target
        }

        current.push(candidates[i]); // Choose the current candidate
        backtrack(candidates, target - candidates[i], i + 1, current, result); // Recurse with the next index
        current.pop(); // Backtrack to explore other combinations
    }
}
```

## Approach 1: Iterative with Stack (Less Common)

### Solution
```javascript
// Time Complexity: O(2^n), where n is the number of candidates
// Space Complexity: O(k), where k is the average length of a combination
function combinationSum2(candidates, target) {
    const result = [];
    candidates.sort((a, b) => a - b); // Sort the candidates to handle duplicates

    const stack = [];
    stack.push([0, target, -1]); // Initial state: [currentSum, remainingTarget, previousIndex]

    const current = [];
    while (stack.length > 0) {
        const [currentSum, remainingTarget, prevIndex] = stack.pop();

        if (remainingTarget === 0) {
            result.push([...current]); // Add valid combinations
            continue;
        }

        // Remaining Iterative Building logic.
    }
}
```

