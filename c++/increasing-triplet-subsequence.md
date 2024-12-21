# [Leetcode 334: Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Single Pass with Two Pointers](#approach-2-single-pass-with-two-pointers)

---

## Approach 1: Brute Force

### Intuition
The problem asks if there is a triplet (i, j, k) such that i < j < k and nums[i] < nums[j] < nums[k]. A straightforward way to solve this problem is to check all possible triplets in the array. Although this is not efficient for large arrays, it's a good starting point to understand the problem.

### Solution Steps
1. Use three nested loops to iterate through every possible triplet (i, j, k).
2. Check if the current triplet satisfies the condition nums[i] < nums[j] < nums[k].
3. If a triplet is found, return `true`.
4. If no such triplet is found after checking all possibilities, return `false`.

### Code
```cpp
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int n = nums.size();
        // Iterate through all possible triplets (i, j, k)
        for (int i = 0; i < n - 2; ++i) {
            for (int j = i + 1; j < n - 1; ++j) {
                for (int k = j + 1; k < n; ++k) {
                    // Check if the triplet (i, j, k) satisfies the condition
                    if (nums[i] < nums[j] && nums[j] < nums[k]) {
                        return true;
                    }
                }
            }
        }
        return false;
    }
};
```

### Complexity
- **Time Complexity:** \(O(n^3)\) because we are using three nested loops.
- **Space Complexity:** \(O(1)\) since we are using a constant amount of extra space.

---

## Approach 2: Single Pass with Two Pointers

### Intuition
To improve efficiency, we can use two pointers to track potential candidates for the first two elements of the increasing triplet subsequence. We'll traverse the list once and maintain these two pointers as we go:
- `first`: The smallest element found so far.
- `second`: The smallest element found that is greater than `first`.

Whenever we find an element greater than `second`, we know a triplet exists.

### Solution Steps
1. Initialize `first` and `second` to the maximum possible integer value.
2. Traverse the list:
   - If the current element is smaller than or equal to `first`, update `first`.
   - Else, if the current element is smaller than or equal to `second`, update `second`.
   - Else, return `true` since the current element is larger than both `first` and `second`.
3. If the loop completes without finding a triplet, return `false`.

### Code
```cpp
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int first = INT_MAX;
        int second = INT_MAX;

        for (int num : nums) {
            if (num <= first) {
                // Found a new minimum value
                first = num;
            } else if (num <= second) {
                // Found a number larger than `first` but smaller than `second`
                second = num;
            } else {
                // Found a number larger than both `first` and `second`
                return true;
            }
        }
        return false;
    }
};
```

### Complexity
- **Time Complexity:** \(O(n)\) as we only traverse the array once.
- **Space Complexity:** \(O(1)\) since we use a constant amount of extra space.

