# [Leetcode 40: Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

## Approaches:
1. [Backtracking with Pruning](#backtracking-with-pruning)
2. [Backtracking with Early Stopping](#backtracking-with-early-stopping)

### Backtracking with Pruning

The problem requires us to find unique combinations that sum to a target, considering different candidates where each candidate number can only be used once. To approach this problem, we can use backtracking, a common technique for exploring all potential solutions.

#### Intuition
- First, sort the input array. Sorting helps to easily handle duplicates, ensuring that we don't consider the same number more than once. 
- Use a recursive backtracking function to explore all possible combinations.
- At each step, decide whether to include or not include a candidate number.
- Use a loop to iterate through the candidates from the current position onward, recursively calling the backtracking function with the reduced target.
- Skip over duplicate numbers after taking a number.

#### Code

```javascript
function combinationSum2(candidates, target) {
    // Sort the candidates to make it easy to avoid duplicates
    candidates.sort((a, b) => a - b);
    const results = [];
    const currentCombination = [];

    function backtrack(start, target) {
        // Base case: if target is 0, we found a combination
        if (target === 0) {
            // Make a copy of the current combination and add it to results
            results.push([...currentCombination]);
            return;
        }

        for (let i = start; i < candidates.length; i++) {
            // Skip duplicates
            if (i > start && candidates[i] === candidates[i - 1]) {
                continue;
            }

            // If the number is greater than the target, break the loop as
            // further numbers will be greater as well due to sorting
            if (candidates[i] > target) {
                break;
            }

            // Include the candidate number in the current combination
            currentCombination.push(candidates[i]);

            // Recursively call with reduced target and move position forward
            backtrack(i + 1, target - candidates[i]);

            // Backtrack, remove the last element added
            currentCombination.pop();
        }
    }

    // Initial call to backtracking function
    backtrack(0, target);

    return results;
}
```

#### Complexity Analysis
- **Time Complexity:** O(2^n), where n is the number of elements in the candidates array. In the worst case, we explore all subsets.
- **Space Complexity:** O(n), used for the recursion stack and temporary storage of current combinations.

### Backtracking with Early Stopping

This approach continues from the previous method but adds a strategy to stop exploring paths early if they exceed the current target.

#### Intuition
- By examining the possible sums during early phases of recursion, we can stop exploring certain paths early, thus optimizing the search.
- This approach saves unnecessary computations by checking the feasibility of sums before deep exploration, effectively pruning futile paths.

#### Code

```javascript
function combinationSum2(candidates, target) {
    candidates.sort((a, b) => a - b);
    const results = [];

    const dfs = (start, target, path) => {
        if (target === 0) {
            // Target reached, add current path to results
            results.push([...path]);
            return;
        }
        if (target < 0) return;

        for (let i = start; i < candidates.length; i++) {
            if (i > start && candidates[i] === candidates[i - 1]) {
                continue; // Skip duplicates
            }

            if (candidates[i] > target) {
                break; // Future elements would also exceed target
            }

            path.push(candidates[i]);
            dfs(i + 1, target - candidates[i], path);
            path.pop(); // Backtrack
        }
    };

    dfs(0, target, []);
    return results;
}
```

#### Complexity Analysis
- **Time Complexity:** O(2^n), similar to the first approach due to backtracking over possible subsets.
- **Space Complexity:** O(n), used for storing the current combination path and the recursion stack.

The early stopping in the implementation improves practical runtime by reducing the number of recursive calls in cases where the target becomes evidently unreachable within a specific path.

