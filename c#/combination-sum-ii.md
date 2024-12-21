# [LeetCode 40: Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

## Table of Contents

- [Approach 1: Recursive Backtracking](#approach-1-recursive-backtracking)
- [Approach 2: Optimized Backtracking with Early Termination](#approach-2-optimized-backtracking-with-early-termination)

## Approach 1: Recursive Backtracking

### Intuition

The problem is essentially about finding unique combinations that sum to a target, and each number in the input array may only be used once in the combination. This is a backtracking problem, typically solved by exploring potential candidates recursively.

### Steps:

1. **Sort the candidate array**: This helps in easily skipping duplicates.
2. **Recursive Backtracking Function**: Define a recursive function that tries to form combinations.
3. **Skip Duplicates**: Before picking a number in the candidate array, check if it's a duplicate of a previously considered number at the same level.
4. **Base Case**: If the target is reduced to zero, add the current combination to the result list.
5. **Recursion and Backtracking**: For each number, either:
   - Include it in the current path and recurse.
   - Exclude it and move to the next.

### Code

```csharp
public class Solution {
    public IList<IList<int>> CombinationSum2(int[] candidates, int target) {
        Array.Sort(candidates); // Sort candidates to handle duplicates easily
        IList<IList<int>> result = new List<IList<int>>();
        List<int> currentCombination = new List<int>();
        
        Backtrack(candidates, target, 0, currentCombination, result);
        return result;
    }
    
    private void Backtrack(int[] candidates, int target, int start, List<int> currentCombination, IList<IList<int>> result) {
        if (target == 0) {
            result.Add(new List<int>(currentCombination)); // Add a copy of the current combination
            return;
        }

        for (int i = start; i < candidates.Length; i++) {
            if (i > start && candidates[i] == candidates[i - 1]) {
                continue; // Skip duplicates
            }
            
            if (candidates[i] > target) {
                break; // Early termination since array is sorted
            }
            
            currentCombination.Add(candidates[i]);
            Backtrack(candidates, target - candidates[i], i + 1, currentCombination, result); // i+1 because a number can't be reused
            currentCombination.RemoveAt(currentCombination.Count - 1); // Backtrack
        }
    }
}
```

### Complexity Analysis

- **Time Complexity**: \(O(2^n \times n)\), where \(n\) is the number of candidates. The sorting step takes \(O(n \log n)\) and the worst-case exploration is \(O(2^n)\). Each combination takes up to \(O(n)\) to construct.
- **Space Complexity**: \(O(n)\), the depth of the recursion stack and space required for holding current combinations.

## Approach 2: Optimized Backtracking with Early Termination

### Intuition

We can further optimize by leveraging sorted arrays more effectively for early termination and skipping unnecessary recursive calls once our candidate surpasses the target.

### Enhanced Steps:

1. **Skip Duplicates Early**: Efficiently skip duplicates at each recursion level to prevent the forming of repeated combinations.
2. **Terminate Early**: As soon as we encounter a candidate greater than the current remaining target, break out of the loop.

### Code

```csharp
public class Solution {
    public IList<IList<int>> CombinationSum2(int[] candidates, int target) {
        Array.Sort(candidates);
        IList<IList<int>> result = new List<IList<int>>();
        List<int> currentCombination = new List<int>();
        
        Backtrack(candidates, target, 0, currentCombination, result);
        return result;
    }
    
    private void Backtrack(int[] candidates, int target, int start, List<int> currentCombination, IList<IList<int>> result) {
        if (target == 0) {
            result.Add(new List<int>(currentCombination));
            return;
        }
        
        for (int i = start; i < candidates.Length; i++) {
            // Skip duplicates
            if (i > start && candidates[i] == candidates[i - 1]) continue;
            // Terminate early if candidate exceeds target
            if (candidates[i] > target) break;
            
            currentCombination.Add(candidates[i]);
            Backtrack(candidates, target - candidates[i], i + 1, currentCombination, result);
            currentCombination.RemoveAt(currentCombination.Count - 1);
        }
    }
}
```

### Complexity Analysis

- **Time Complexity**: \(O(2^n \times n)\), similar reasoning as before but often runs faster in practice due to fewer recursive calls.
- **Space Complexity**: \(O(n)\), related to the maximum depth of the recursion stack.

