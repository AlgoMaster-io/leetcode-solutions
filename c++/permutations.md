# 46. [Permutations](https://leetcode.com/problems/permutations/)

## Approach 1: Brute Force (Generate All Permutations)

### Solution
```cpp
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n * n!) for storing the permutations
#include <vector>
using namespace std;

class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> result;
        vector<bool> visited(nums.size(), false);
        vector<int> current;
        generateAll(nums, current, result, visited);
        return result;
    }

private:
    void generateAll(vector<int>& nums, vector<int>& current, vector<vector<int>>& result, vector<bool>& visited) {
        if (current.size() == nums.size()) {
            result.push_back(current); // Add the current permutation
            return;
        }

        for (int i = 0; i < nums.size(); ++i) {
            if (!visited[i]) {
                visited[i] = true;
                current.push_back(nums[i]); // Choose the current element
                generateAll(nums, current, result, visited); // Recurse
                current.pop_back(); // Backtrack
                visited[i] = false; // Reset visited state
            }
        }
    }
};
```

## Approach 2: Backtracking (Optimal Solution)

### Solution
```cpp
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n * n!) for storing the permutations
#include <vector>
using namespace std;

class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> result;
        vector<int> current;
        backtrack(nums, current, result);
        return result;
    }

private:
    void backtrack(vector<int>& nums, vector<int>& current, vector<vector<int>>& result) {
        if (current.size() == nums.size()) {
            result.push_back(current); // Add the current permutation
            return;
        }

        for (int i = 0; i < nums.size(); ++i) {
            if (find(current.begin(), current.end(), nums[i]) != current.end()) {
                continue; // Skip if the number is already in the current permutation
            }
            current.push_back(nums[i]); // Choose the current element
            backtrack(nums, current, result); // Recurse
            current.pop_back(); // Backtrack
        }
    }
};
```

## Approach 3: Swap-Based Backtracking (In-Place Modification)

### Solution
```cpp
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n) for the recursion stack
#include <vector>
using namespace std;

class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> result;
        backtrack(nums, 0, result);
        return result;
    }

private:
    void backtrack(vector<int>& nums, int start, vector<vector<int>>& result) {
        if (start == nums.size()) {
            result.push_back(nums); // Add the current permutation
            return;
        }

        for (int i = start; i < nums.size(); ++i) {
            swap(nums, start, i); // Swap to create a new permutation
            backtrack(nums, start + 1, result); // Recurse
            swap(nums, start, i); // Backtrack to the original state
        }
    }

    void swap(vector<int>& nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp; // Swap two elements in the array
    }
};
```

