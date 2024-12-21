# [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

## Approaches

1. [Backtracking Approach](#backtracking-approach)
2. [Optimized Backtracking Approach](#optimized-backtracking-approach)

---

### Backtracking Approach

The problem asks us to find all unique combinations where the candidate numbers sum to the target. In this approach, we use backtracking to explore all possible combinations.

**Intuition:**
- Sort the candidate array. This will help to easily handle duplicates and ensure our combinations are in lexicographical order.
- Use backtracking by exploring each number, adding it to the current path, and recursively trying to find combinations for the reduced target.
- To ensure uniqueness, if we use a number from the candidates, we cannot use that number or any number after it again in a new combination starting from the same point.

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        // Result variable to store all unique combinations
        vector<vector<int>> results;
        // Sort candidates to handle duplicates easily
        sort(candidates.begin(), candidates.end());
        // Current path to store current combination
        vector<int> combination;
        // Start backtracking
        backtrack(candidates, target, 0, combination, results);
        return results;
    }
    
private:
    void backtrack(vector<int>& candidates, int target, int start, vector<int>& combination, vector<vector<int>>& results) {
        // If target becomes zero, we found a valid combination
        if (target == 0) {
            results.push_back(combination);
            return;
        }
        
        // Explore each candidate starting from `start` index
        for (int i = start; i < candidates.size(); ++i) {
            // Skip duplicate elements
            if (i > start && candidates[i] == candidates[i-1]) continue;

            // Stop exploration if the candidate is greater than the remaining target
            if (candidates[i] > target) break;

            // Include the candidate in the current path
            combination.push_back(candidates[i]);
            // Recurse with reduced target and next index
            backtrack(candidates, target - candidates[i], i + 1, combination, results);
            // Backtrack step: remove last element and try the next candidate
            combination.pop_back();
        }
    }
};
```

**Time Complexity:** \(O(2^n)\). In the worst case, we explore all subsets of the candidates set.

**Space Complexity:** \(O(n)\) due to the recursion stack and potential storage in the result.

---

### Optimized Backtracking Approach

In the optimized approach, we keep all the steps of the simple backtracking approach, but reduce some of the unnecessary checks by breaking early when the candidate exceeds the target. Since we sorted the array, we can stop any further exploration immediately.

**Optimal Improvements:**
- By sorting the array, repetitive combinations are inherently avoided, reducing the need for a hashset.
- Early stopping can significantly reduce unnecessary recursion calls.

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<vector<int>> results;
        vector<int> combination;
        // Sort the candidates to handle duplicates
        sort(candidates.begin(), candidates.end());
        backtrack(candidates, target, 0, combination, results);
        return results;
    }
    
private:
    void backtrack(vector<int>& candidates, int target, int begin, vector<int>& combination, vector<vector<int>>& results) {
        if (target == 0) {
            results.push_back(combination);
            return;
        }
        
        for (int i = begin; i < candidates.size(); ++i) {
            // If current element is greater than remaining target, break to avoid unnecessary exploration
            if (candidates[i] > target) break;

            // Skip duplicates
            if (i > begin && candidates[i] == candidates[i-1]) continue;
            
            // Add candidate to the current combination
            combination.push_back(candidates[i]);
            // Recurse further, reducing the target
            backtrack(candidates, target - candidates[i], i + 1, combination, results);
            // Backtrack to try another combination
            combination.pop_back();
        }
    }
};
```

**Time Complexity:** \(O(2^n)\) as we are still exploring every subset combination, but breaks reduce unnecessary recursive calls.

**Space Complexity:** \(O(n)\) due to recursion and path storage.

