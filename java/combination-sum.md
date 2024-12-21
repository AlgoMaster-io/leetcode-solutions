# [Leetcode 39: Combination Sum](https://leetcode.com/problems/combination-sum/)

## Navigation
- [Approach 1: Backtracking](#approach-1-backtracking)
- [Approach 2: Backtracking with Pruning](#approach-2-backtracking-with-pruning)

## Approach 1: Backtracking

### Intuition:
The problem requires us to find all unique combinations of numbers that sum up to a target, allowing unlimited usage of each number. This is a classical combination problem suitable for backtracking, where we try all possibilities and backtrack once we exceed the target or if we've considered the entire list of candidates.

### Steps:
1. We will sort the input array to help in potential optimizations, although not necessary in this approach.
2. Use a recursive backtracking function to explore each combination:
   - Include the current candidate in the path and recursively attempt to compute the remaining target.
   - If at any point, our current combination sums to the target, save it.
   - If the sum exceeds the target, backtrack.
3. We avoid duplicates by ensuring each combination is built in non-decreasing order.

### Code:
```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> results = new ArrayList<>();
        // Start the backtracking process with an empty list and the full target
        backtrack(candidates, target, 0, new ArrayList<>(), results);
        return results;
    }
    
    private void backtrack(int[] candidates, int target, int start, List<Integer> current, List<List<Integer>> results) {
        // If target is 0, we found a combination that sums up to the original target
        if (target == 0) {
            results.add(new ArrayList<>(current));  // Save a copy of the solution
            return;
        }
        
        for (int i = start; i < candidates.length; i++) {
            // If the candidate is greater than the remaining target, there's no point in continuing
            if (candidates[i] > target) continue;
            
            // Include candidates[i] in the current combination
            current.add(candidates[i]);
            // Continue exploring with the reduced target
            backtrack(candidates, target - candidates[i], i, current, results);
            // Backtrack and try the next candidate
            current.remove(current.size() - 1);
        }
    }
}
```

### Time Complexity: 
- \(O(N^{T/M+1})\), where \(N\) is the number of candidates, \(T\) is the target value, and \(M\) is the minimal value among the candidates.
### Space Complexity:
- \(O(T/M)\) for the call stack during the depth of recursion.

## Approach 2: Backtracking with Pruning

### Intuition:
In the previous approach, we performed unnecessary recursive calls even when these calls wouldn't have contributed to a valid combination. By sorting the array initially, we can prune these branches more aggressively and stop processing further once we encounter a candidate larger than the target or the remaining target during recursion.

### Steps:
1. Begin with sorting the candidates which can make pruning easier.
2. While exploring candidates, terminate early if the current candidate exceeds the reduced target at any point, avoiding wasting computation on invalid paths.
3. This slight change saves processing time and makes the solution more efficient.

### Code:
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);  // Sorting helps in pruning
        List<List<Integer>> results = new ArrayList<>();
        backtrack(candidates, target, 0, new ArrayList<>(), results);
        return results;
    }
    
    private void backtrack(int[] candidates, int target, int start, List<Integer> current, List<List<Integer>> results) {
        if (target == 0) {
            results.add(new ArrayList<>(current));
            return;
        }
        
        for (int i = start; i < candidates.length; i++) {
            // Important pruning: if current candidate exceeds the target, further candidates will also exceed
            if (candidates[i] > target) break;
            
            current.add(candidates[i]);
            backtrack(candidates, target - candidates[i], i, current, results);
            current.remove(current.size() - 1);
        }
    }
}
```

### Time Complexity:
- The time complexity remains \(O(N^{T/M+1})\), but pruning helps in practical scenarios.
### Space Complexity:
- \(O(T/M)\), still for the recursion stack.

In both solutions, we ensure that the approach finds all possible unique combinations, allowing each number to be used multiple times without repeats and ensuring valid paths are stored when they sum up to the target.

