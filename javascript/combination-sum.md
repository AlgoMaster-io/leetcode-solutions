# [Leetcode 39: Combination Sum](https://leetcode.com/problems/combination-sum/)

## Approaches
1. [Backtracking Approach](#backtracking-approach)
2. [Optimized Backtracking Using Early Termination](#optimized-backtracking-using-early-termination)

---

### Backtracking Approach

The backtracking approach is a classic method for generating combinations. This problem is suitable for backtracking because it requires exploring all possible subsets of numbers and calculating their sums to find the valid combinations.

**Intuition:**
- Start with an empty combination and try to build it by including numbers.
- At each step, either pick a number or ignore it, and recursively perform the same operation on the reduced target.
- If the target becomes zero, we store the current combination since it forms a valid solution.
- If the target becomes negative, we backtrack because it means we have exceeded the target sum.
- Reuse the same number by allowing the function to continue including numbers from the current position.

```javascript
const combinationSum = (candidates, target) => {
    const result = [];

    // Helper function to perform backtracking
    const backtrack = (remain, comb, start) => {
        if (remain === 0) {
            // Found a valid combination
            result.push([...comb]);
            return;
        }

        if (remain < 0) {
            // Exceeded the sum, no need to continue
            return;
        }

        // Iterate over the candidates starting from the current position
        for (let i = start; i < candidates.length; i++) {
            comb.push(candidates[i]);
            // Since we can reuse same elements, pass the same index 'i'
            backtrack(remain - candidates[i], comb, i);
            // Backtrack by removing the last element
            comb.pop();
        }
    };

    backtrack(target, [], 0);
    return result;
};
```

**Time Complexity:** O(2^t), where t is the target value, due to the decision tree branching at each step.

**Space Complexity:** O(t), due to the call stack from recursion and the space required to store combinations.

---

### Optimized Backtracking Using Early Termination

While the brute force backtracking works well, we can optimize it slightly by considering early termination. Essentially, sort the candidates array, and if the number is greater than the remaining target, we can break early since subsequent numbers will be greater.

**Intuition:**
- Sort the candidates array to facilitate early termination during the backtracking process.
- This helps in reducing unnecessary recursive calls, especially when the numbers are significantly larger than the remaining target.

```javascript
const optimizedCombinationSum = (candidates, target) => {
    const result = [];
    candidates.sort((a, b) => a - b);

    const backtrack = (remain, comb, start) => {
        if (remain === 0) {
            result.push([...comb]);
            return;
        }

        for (let i = start; i < candidates.length; i++) {
            if (candidates[i] > remain) break; // Early termination
            comb.push(candidates[i]);
            backtrack(remain - candidates[i], comb, i);
            comb.pop();
        }
    };

    backtrack(target, [], 0);
    return result;
};
```

**Time Complexity:** O(2^t), with reductions depending on pruning due to early termination.

**Space Complexity:** O(t), similar to the basic backtracking method, mainly due to recursion stack and storage of combinations.

Both approaches are helpful in generating unique combinations that sum up to the target, with the second approach offering slight optimizations through sorting and early termination.

