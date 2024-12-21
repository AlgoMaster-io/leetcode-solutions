## [LeetCode Problem 46: Permutations](https://leetcode.com/problems/permutations/)

### Approaches:
- [Approach 1: Recursive Solution (Backtracking)](#approach-1-recursive-solution-backtracking)
- [Approach 2: Iterative Solution using STL next_permutation](#approach-2-iterative-solution-using-stl-next_permutation)

---

### Approach 1: Recursive Solution (Backtracking)

#### Intuition:
The classic approach to generate all permutations of a list is backtracking. The idea is to fix each number at the current position, and recursively determine the permutations of the remaining numbers. This approach essentially explores the decision tree where at each level, we choose a number to be at the current position.

#### Steps:
1. Create a helper function `backtrack` that generates permutations of the provided numbers.
2. For each recursive call, swap the current element with the rest of the elements recursively and backtrack by swapping them back.
3. Base case: When the current index reaches the size of the list, add the current permutation to the result list.

#### Time Complexity:
- The time complexity is O(n*n!), where n is the length of the list. This is because there are n! permutations, and it takes O(n) time to create each permutation.

#### Space Complexity:
- The space complexity is O(n) for the recursion stack.

```cpp
class Solution {
public:
    void backtrack(vector<int>& nums, vector<vector<int>>& result, int start) {
        // If we have a complete permutation
        if(start == nums.size()) {
            result.push_back(nums);
            return;
        }
        for(int i = start; i < nums.size(); i++) {
            // Swap the current index with the element at the current position
            swap(nums[start], nums[i]);
            // Recurse to generate permutations on the next part of the list
            backtrack(nums, result, start + 1);
            // Backtrack; swap the elements back to restore the original list
            swap(nums[start], nums[i]);
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> result;
        backtrack(nums, result, 0);
        return result;
    }
};
```

### Approach 2: Iterative Solution using STL `next_permutation`

#### Intuition:
The `next_permutation` function from the STL creates permutations in lexicographical order. By calling `next_permutation` repeatedly until we've cycled through all permutations, we can generate all possible permutations iteratively.

#### Steps:
1. Start by sorting the input numbers to ensure we can generate permutations in lexicographical order.
2. Use a `do-while` loop to apply `next_permutation` until the arrangement of numbers cycles back to the sorted order, exhausting all permutations.

#### Time Complexity:
- The time complexity is O(n*n!), where n is the length of the list.

#### Space Complexity:
- The space complexity is O(1) as we are generating permutations in place without using additional data structures.

```cpp
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> result;
        // Sort the numbers to start with the smallest lexicographical permutation
        sort(nums.begin(), nums.end());
        do {
            // Add the current permutation to the list
            result.push_back(nums);
            // Generate the next permutation
        } while(next_permutation(nums.begin(), nums.end()));
        return result;
    }
};
```

Each solution provides its own trade-offs between complexity and intuitiveness. The recursive approach is intuitive and easy to implement using the backtracking paradigm, while the iterative approach leverages the powerful STL to generate permutations efficiently but may be less intuitive for learners new to combinatorial problems.

