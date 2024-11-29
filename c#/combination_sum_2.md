# 40. [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

## Approach 1: Backtracking with Duplicate Handling

### Solution
csharp
```csharp
// Time Complexity: O(2^n), where n is the number of candidates
// Space Complexity: O(k), where k is the average length of a combination
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> CombinationSum2(int[] candidates, int target) {
        IList<IList<int>> result = new List<IList<int>>();
        Array.Sort(candidates); // Sort the candidates to handle duplicates
        Backtrack(candidates, target, 0, new List<int>(), result);
        return result;
    }

    private void Backtrack(int[] candidates, int target, int start, List<int> current, IList<IList<int>> result) {
        if (target == 0) {
            result.Add(new List<int>(current)); // Add the valid combination to the result
            return;
        }

        for (int i = start; i < candidates.Length; i++) {
            if (i > start && candidates[i] == candidates[i - 1]) {
                continue; // Skip duplicates
            }
            if (candidates[i] > target) {
                break; // Stop the loop if the current candidate exceeds the target
            }

            current.Add(candidates[i]); // Choose the current candidate
            Backtrack(candidates, target - candidates[i], i + 1, current, result); // Recurse with the next index
            current.RemoveAt(current.Count - 1); // Backtrack to explore other combinations
        }
    }
}
```

## Approach 2: Iterative with Stack (Less Common)

### Solution
csharp
```csharp
// Time Complexity: O(2^n), where n is the number of candidates
// Space Complexity: O(k), where k is the average length of a combination
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> CombinationSum2(int[] candidates, int target) {
        IList<IList<int>> result = new List<IList<int>>();
        Array.Sort(candidates); // Sort the candidates to handle duplicates

        Stack<(int currentSum, int remainingTarget, int previousIndex, List<int> currentCombination)> stack = new Stack<(int, int, int, List<int>)>();
        stack.Push((0, target, -1, new List<int>())); // Initial state: [currentSum, remainingTarget, previousIndex]

        while (stack.Count > 0) {
            var (currentSum, remainingTarget, previousIndex, currentCombination) = stack.Pop();

            if (remainingTarget == 0) {
                result.Add(new List<int>(currentCombination)); // Add valid combinations
                continue;
            }

            for (int i = previousIndex + 1; i < candidates.Length; i++) {
                if (i > previousIndex + 1 && candidates[i] == candidates[i - 1]) {
                    continue; // Skip duplicates
                }
                if (candidates[i] > remainingTarget) {
                    break; // Stop the loop if the current candidate exceeds the target
                }

                List<int> nextCombination = new List<int>(currentCombination) { candidates[i] };
                stack.Push((currentSum + candidates[i], remainingTarget - candidates[i], i, nextCombination));
            }
        }

        return result;
    }
}
```

