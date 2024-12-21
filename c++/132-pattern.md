# [Leetcode 456: 132 Pattern](https://leetcode.com/problems/132-pattern/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Min Array](#approach-2-using-min-array)
- [Approach 3: Using a Monotonic Stack](#approach-3-using-a-monotonic-stack)

---

## Approach 1: Brute Force

### Intuition

The brute force approach checks every possible combination of three indices to see if they form the 132 pattern. We iterate through each triplet `(i, j, k)` and check the conditions: `nums[i] < nums[k] < nums[j]`. This approach is straightforward but inefficient for large arrays.

### Code
```cpp
bool find132pattern(vector<int>& nums) {
    int n = nums.size();
    // Consider every triplet (i, j, k) to check if it forms a 132 pattern
    for (int i = 0; i < n; ++i) {
        for (int j = i + 1; j < n; ++j) {
            for (int k = j + 1; k < n; ++k) {
                // Check if the current triplet satisfies the 132 pattern
                if (nums[i] < nums[k] && nums[k] < nums[j]) {
                    return true;  // Found a valid 132 pattern
                }
            }
        }
    }
    return false;  // No valid pattern found
}
```

### Complexity
- **Time Complexity**: O(n^3) - We check each triplet, resulting in cubic time complexity.
- **Space Complexity**: O(1) - Uses a constant amount of space.

---

## Approach 2: Using Min Array

### Intuition

This approach uses an auxiliary array `min_i` to keep track of the smallest value up to the current index. For a pattern to exist, there should be some `min_i < nums[k] < nums[j]`. This reduces the unnecessary iterations compared to the brute force by focusing only on valid `(i, j, k)` triplets.

### Code
```cpp
bool find132pattern(vector<int>& nums) {
    int n = nums.size();
    if (n < 3) return false;  // At least 3 numbers are needed for 132 pattern

    vector<int> min_i(n);
    min_i[0] = nums[0];
    // Create min_i array to store the smallest element till current position
    for (int i = 1; i < n; ++i) {
        min_i[i] = min(min_i[i - 1], nums[i]);
    }

    // Iterate considering j and k, where j < k
    for (int j = n - 1, k = n; j >= 0; --j) {
        if (nums[j] > min_i[j]) {
            while (k < n && nums[k] <= min_i[j]) {
                ++k;
            }
            if (k < n && nums[k] < nums[j]) {
                return true;  // Found a valid 132 pattern
            }
            nums[--k] = nums[j];
        }
    }
    return false;  // No valid pattern found
}
```

### Complexity
- **Time Complexity**: O(n) - Efficient linear traversal of indices and auxiliary operations.
- **Space Complexity**: O(n) - Additional `min_i` array used which stores minimum values.

---

## Approach 3: Using a Monotonic Stack

### Intuition

The most efficient approach leverages a stack to maintain potential candidates for the `2` element of the pattern, scanning from right to left. We track the maximum valid `ak` (`nums[k]`) and use the stack to manage potential `aj` (`nums[j]`) candidates. This ensures the conditions of the 132 pattern are met by maintaining ordered potential candidates of aj, where the top of the stack is always the candidate of ak and the numbers still in the stack are potential candidates of aj.

### Code
```cpp
bool find132pattern(vector<int>& nums) {
    int n = nums.size();
    if (n < 3) return false;

    stack<int> st;
    int ak = INT_MIN;  // This will store our potential ak (nums[k]) value

    // Traverse the list from right to left
    for (int i = n - 1; i >= 0; --i) {
        if (nums[i] < ak) return true;  // Found a valid pattern
        // Pop elements from stack while they are less than or equal to nums[i]
        while (!st.empty() && st.top() < nums[i]) {
            ak = st.top();  // Update our ak to the last popped element
            st.pop();       // Pop element as it can be our potential nums[k]
        }
        st.push(nums[i]);  // Add current element as a potential aj
    }
    return false;  // No valid pattern found
}
```

### Complexity
- **Time Complexity**: O(n) - Despite inner while loop, each element is pushed and popped from the stack at most once.
- **Space Complexity**: O(n) - Stack potentially holds all elements in the worst case.

--- 

