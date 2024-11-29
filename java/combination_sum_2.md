# 40. [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

## Approach 1: Backtracking with Duplicate Handling

### Solution
```java
// Time Complexity: O(2^n), where n is the number of candidates
// Space Complexity: O(k), where k is the average length of a combination
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(candidates); // Sort the candidates to handle duplicates
        backtrack(candidates, target, 0, new ArrayList<>(), result);
        return result;
    }

    private void backtrack(int[] candidates, int target, int start, List<Integer> current, List<List<Integer>> result) {
        if (target == 0) {
            result.add(new ArrayList<>(current)); // Add the valid combination to the result
            return;
        }

        for (int i = start; i < candidates.length; i++) {
            if (i > start && candidates[i] == candidates[i - 1]) {
                continue; // Skip duplicates
            }
            if (candidates[i] > target) {
                break; // Stop the loop if the current candidate exceeds the target
            }

            current.add(candidates[i]); // Choose the current candidate
            backtrack(candidates, target - candidates[i], i + 1, current, result); // Recurse with the next index
            current.remove(current.size() - 1); // Backtrack to explore other combinations
        }
    }
}
```

## Approach 1: Iterative with Stack (Less Common)

### Solution
```java
// Time Complexity: O(2^n), where n is the number of candidates
// Space Complexity: O(k), where k is the average length of a combination
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Stack;

public class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(candidates); // Sort the candidates to handle duplicates

        Stack<int[]> stack = new Stack<>();
        stack.push(new int[]{0, target, -1}); // Initial state: [currentSum, remainingTarget, previousIndex]

        List<Integer> current = new ArrayList<>();
        while (!stack.isEmpty()) {
            int[] state = stack.pop();

            if (state[1] == 0) {
                result.add(new ArrayList<>(current)); // Add valid combinations
                continue;
            }

            // Remaining Iterative Building logic.
        }
    }
}
```