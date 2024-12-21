# [Leetcode 78: Subsets](https://leetcode.com/problems/subsets/)

## Approaches

1. [Iterative Approach](#iterative-approach)
2. [Recursive Backtracking Approach](#recursive-backtracking-approach)
3. [Bit Manipulation Approach](#bit-manipulation-approach)

### Iterative Approach

**Intuition:**

In the iterative method, one can generate each subset by adding one element from the given set to the existing subsets. We start with an initial empty subset and for each number in the input list, add it to each existing subset to create new subsets.

**Detailed Steps:**

1. Start with a list of subsets containing just the empty set.
2. Iterate over each number in the input list.
3. For each existing subset, create a new subset by adding the current number.
4. Merge the newly created subsets with the existing ones.

```cpp
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> subsets = {{}};

    // Iterating through each number in nums.
    for (int num : nums) {
        // For each number, we will add it to all the existing subsets.
        int n = subsets.size();
        for (int i = 0; i < n; ++i) {
            // Create a new subset by adding the current number to the existing ones.
            vector<int> new_subset = subsets[i];
            new_subset.push_back(num);
            subsets.push_back(new_subset);
        }
    }

    return subsets;
}
```

**Time Complexity:** `O(2^n * n)`  
**Space Complexity:** `O(2^n * n)`

### Recursive Backtracking Approach

**Intuition:**

The recursive approach utilizes backtracking to explore all possible subsets. It involves considering each element either included or excluded from the subset and recursively exploring these choices.

**Detailed Steps:**

1. Define a recursive function to add current subset to results.
2. For each element, first include it in the current subset then exclude it, exploring both branches recursively.
3. Base case: If we've iterated through all elements, add the current subset to results.

```cpp
void backtrack(int start, vector<int>& nums, vector<int>& current, vector<vector<int>>& subsets) {
    // Add the current subset to the list of subsets.
    subsets.push_back(current);

    for (int i = start; i < nums.size(); ++i) {
        // Include nums[i] in the current subset.
        current.push_back(nums[i]);
        // Move on to the next element.
        backtrack(i + 1, nums, current, subsets);
        // Exclude nums[i] from the current subset and backtrack.
        current.pop_back();
    }
}

vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> subsets;
    vector<int> current;
    backtrack(0, nums, current, subsets);
    return subsets;
}
```

**Time Complexity:** `O(2^n * n)`  
**Space Complexity:** `O(n)`

### Bit Manipulation Approach

**Intuition:**

Each subset corresponds to a binary number where each bit represents whether to include a particular element from the input set. By iterating from 0 to \(2^n - 1\), all possible combinations of the original set can be represented.

**Detailed Steps:**

1. Calculate the total number of subsets as \(2^n\).
2. For each number from 0 to \(2^n - 1\):
   - Use the binary bits of the number to decide inclusion of elements in subset.
3. If a bit is set in the binary representation, include the corresponding element from the input list.

```cpp
vector<vector<int>> subsets(vector<int>& nums) {
    int n = nums.size();
    int subset_count = 1 << n;  // Total possible subsets = 2^n.
    vector<vector<int>> subsets;

    // Generate all possible subsets.
    for (int i = 0; i < subset_count; ++i) {
        vector<int> subset;
        for (int j = 0; j < n; ++j) {
            // If j-th bit is set in i, include nums[j] in the current subset.
            if (i & (1 << j)) {
                subset.push_back(nums[j]);
            }
        }
        subsets.push_back(subset);
    }

    return subsets;
}
```

**Time Complexity:** `O(2^n * n)`  
**Space Complexity:** `O(2^n * n)` 

All three methods have their own advantages and can be selected based on the situation or preference for recursion versus iteration. Bit manipulation is often considered elegant and compact, making it an interesting approach for those familiar with bitwise operations.

