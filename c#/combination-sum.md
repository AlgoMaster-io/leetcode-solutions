# [Leetcode 39: Combination Sum](https://leetcode.com/problems/combination-sum/)

## Approaches
- [Backtracking Approach](#backtracking-approach)

### Backtracking Approach

**Intuition:**  
The problem requires exploring different combinations whose sum equals the target. The best fit for such types of problems is backtracking, which is a depth-first search through the solution space. The approach will recursively build candidates, extend them, and backtrack as needed.

1. Start with an empty list (combination) and zero as a current sum.
2. Try to pick an element from the `candidates` list and add it to the combination, only if adding it doesn't exceed the target.
3. If the current sum equals the target, a valid combination is found; add the combination to the results.
4. If the current sum exceeds the target, there's no point in continuing with this combination.
5. Backtrack - remove the last added element and try the next candidates.

**Time Complexity:** \(O(S)\) where \(S\) is the number of all possible combinations. In the worst case, it could be exponential if you have multiple ways to reach the result.
**Space Complexity:** \(O(T/M)\) where \(T\) is the target sum, and \(M\) is the minimal value among the candidates in recursion depth, with additional space for storing results.

```csharp
public class Solution {

    public IList<IList<int>> CombinationSum(int[] candidates, int target) {
        List<IList<int>> results = new List<IList<int>>();
        List<int> combination = new List<int>();
        // Start the backtracking process with an initial index of 0.
        Backtrack(candidates, target, 0, combination, results);
        return results;
    }
    
    private void Backtrack(int[] candidates, int target, int start, List<int> combination, List<IList<int>> results) {
        // If the target is reached, we have found a valid combination.
        if (target == 0) {
            results.Add(new List<int>(combination));
            return;
        }
        
        // If the target is negative, we have overshot it and should backtrack.
        if (target < 0) {
            return;
        }
        
        // Iterate through all candidates starting from the current index.
        for (int i = start; i < candidates.Length; i++) {
            // Choose the candidate
            combination.Add(candidates[i]);
            // Recursive call, with the target reduced by the chosen candidate value.
            // We pass 'i' rather than 'i + 1' to allow the same index to be reused in combination.
            Backtrack(candidates, target - candidates[i], i, combination, results);
            // Undo the move (backtrack), remove the last added candidate
            combination.RemoveAt(combination.Count - 1);
        }
    }
}
```

Each call to `Backtrack` explores including a candidate and not including it in the combination, and steps back once a valid or invalid state is reached. This ensures we explore all valid paths within the constraints of this problem.

