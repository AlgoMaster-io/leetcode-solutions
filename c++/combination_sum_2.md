# 40. [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

## Approach 1: Backtracking with Duplicate Handling

### Solution
```cpp
// Time Complexity: O(2^n), where n is the number of candidates
// Space Complexity: O(k), where k is the average length of a combination
#include <vector>
#include <algorithm>

class Solution {
public:
    std::vector<std::vector<int>> combinationSum2(std::vector<int>& candidates, int target) {
        std::vector<std::vector<int>> result;
        std::sort(candidates.begin(), candidates.end()); // Sort the candidates to handle duplicates
        std::vector<int> current;
        backtrack(candidates, target, 0, current, result);
        return result;
    }
    
private:
    void backtrack(std::vector<int>& candidates, int target, int start, 
                   std::vector<int>& current, std::vector<std::vector<int>>& result) {
        if (target == 0) {
            result.push_back(current); // Add the valid combination to the result
            return;
        }

        for (int i = start; i < candidates.size(); ++i) {
            if (i > start && candidates[i] == candidates[i - 1]) {
                continue; // Skip duplicates
            }
            if (candidates[i] > target) {
                break; // Stop the loop if the current candidate exceeds the target
            }
            current.push_back(candidates[i]); // Choose the current candidate
            backtrack(candidates, target - candidates[i], i + 1, current, result); // Recurse with the next index
            current.pop_back(); // Backtrack to explore other combinations
        }
    }
};
```

## Approach 2: Iterative with Stack (Less Common)

### Solution
```cpp
// Time Complexity: O(2^n), where n is the number of candidates
// Space Complexity: O(k), where k is the average length of a combination
#include <vector>
#include <algorithm>
#include <stack>

class Solution {
public:
    std::vector<std::vector<int>> combinationSum2(std::vector<int>& candidates, int target) {
        std::vector<std::vector<int>> result;
        std::sort(candidates.begin(), candidates.end()); // Sort the candidates to handle duplicates

        std::stack<std::vector<int>> stack;
        stack.push({0, target, -1}); // Initial state: [currentSum, remainingTarget, previousIndex]

        std::vector<int> current;
        while (!stack.empty()) {
            std::vector<int> state = stack.top();
            stack.pop();

            // If condition and logic for adding valid combinations
            if (state[1] == 0) {
                result.push_back(current); // Add valid combinations
                continue;
            }

            // Remaining Iterative Building logic.
        }

        return result;
    }
};
```

