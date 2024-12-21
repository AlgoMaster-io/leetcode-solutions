# [Leetcode Problem 39: Combination Sum](https://leetcode.com/problems/combination-sum/)

## Approaches

- [Backtracking](#backtracking)
- [DFS with Backtracking and Optimization](#dfs-with-backtracking-and-optimization)


### Backtracking

The combination sum is a classical problem that can be solved using backtracking. The task is to find all possible combinations of the given candidate numbers that sum up to a target. 

**Intuition:**

1. Use backtracking to explore each candidate and attempt forming combinations that add up to the target.
2. Start from the first candidate and recursively build potential combinations.
3. If a candidate causes the sum to exceed the target, backtrack and try the next candidate.
4. If the sum matches the target, record the current combination.

```cpp
#include <vector>
#include <algorithm>

class Solution {
public:
    void backtrack(std::vector<int>& candidates, int target, std::vector<int>& currentCombination, std::vector<std::vector<int>>& combinations, int start) {
        // Base case: when the target is zero, we found a valid combination
        if (target == 0) {
            combinations.push_back(currentCombination);
            return;
        }
        
        // Iterate through the candidates starting from `start` index
        for (int i = start; i < candidates.size(); ++i) {
            if (candidates[i] <= target) { // Check if the candidate can be part of the solution
                // Choose the candidate
                currentCombination.push_back(candidates[i]);
                // Recursively explore further with the remaining target value
                backtrack(candidates, target - candidates[i], currentCombination, combinations, i);
                // Un-choose the candidate to try another possibility
                currentCombination.pop_back();
            }
        }
    }
    
    std::vector<std::vector<int>> combinationSum(std::vector<int>& candidates, int target) {
        std::vector<std::vector<int>> combinations;
        std::vector<int> currentCombination;
        backtrack(candidates, target, currentCombination, combinations, 0);
        return combinations;
    }
};

// Time Complexity: O(N^(T/M)) where N is the number of candidates, T is the target, and M is the smallest number in the candidates. 
// Space Complexity: O(T/M) due to the space required for the recursion (call stack).
```

### DFS with Backtracking and Optimization

This approach is essentially the same as the simpler backtracking one but with the added insight that we can avoid recomputing combinations and further optimize our DFS. We avoid unnecessary recursion by breaking early when the candidate list is sorted and exceeds the remaining target.

**Intuition:**

1. Sort the candidates to improve decision outcomes at each recursive call.
2. Only recur with candidates that don't exceed the current target.
3. Prune the search space by breaking out of loops when a candidate exceeds the target.

```cpp
#include <vector>
#include <algorithm>

class Solution {
public:
    void dfs(std::vector<int>& candidates, int target, std::vector<int>& currentCombination, std::vector<std::vector<int>>& combinations, int start) {
        // Base case: when the target is zero, we found a valid combination
        if (target == 0) {
            combinations.push_back(currentCombination);
            return;
        }

        for (int i = start; i < candidates.size(); ++i) {
            // Since the candidates are sorted, no point in continuing if current candidate exceeds the target
            if (candidates[i] > target) break;

            currentCombination.push_back(candidates[i]);
            // Explore further with the current candidate included
            dfs(candidates, target - candidates[i], currentCombination, combinations, i);
            // Undo the add for other combinations
            currentCombination.pop_back();
        }
    }

    std::vector<std::vector<int>> combinationSum(std::vector<int>& candidates, int target) {
        std::sort(candidates.begin(), candidates.end());
        std::vector<std::vector<int>> combinations;
        std::vector<int> currentCombination;
        dfs(candidates, target, currentCombination, combinations, 0);
        return combinations;
    }
};

// Time Complexity: O(N^(T/M)) - even if the candidates are sorted, the asymptotic complexity remains the same as the backtracking solution.
// Space Complexity: O(T/M) - similar to the previous version, depth of the recursion tree depends on the value of the target and smallest candidate.
```

