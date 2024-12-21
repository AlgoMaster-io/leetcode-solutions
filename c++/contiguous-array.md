[Leetcode 525: Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approaches
- [Approach 1: Brute Force](#approach-1)
- [Approach 2: Using HashMap (Optimal Solution)](#approach-2)

---

### Approach 1: Brute Force

**Intuition:**

The brute force solution involves generating all possible subarrays and keeping track of the number of 0s and 1s. The goal is to find the longest subarray where the number of 0s and 1s are equal.

**Detail:**

1. Iterate through each pair of indices `(i, j)`, where `i` is the starting index and `j` is the ending index.
2. For every such pair, count the occurrences of 0s and 1s in the subarray `nums[i:j]`.
3. If counts of 0s and 1s are equal, check if the length `j - i` is greater than our current maximum length and update accordingly.

This method will check all pairs of indices and is computationally expensive due to its nested loops.

```cpp
#include <vector>
#include <algorithm>

int findMaxLength(std::vector<int>& nums) {
    int maxLength = 0;
    int n = nums.size();

    for (int i = 0; i < n; ++i) {
        int count0 = 0, count1 = 0;

        // Traverse subarray nums[i:j]
        for (int j = i; j < n; ++j) {
            // Count 0s and 1s
            if (nums[j] == 0) {
                count0++;
            } else {
                count1++;
            }
            
            // Check if counts are equal
            if (count0 == count1) {
                maxLength = std::max(maxLength, j - i + 1);
            }
        }
    }

    return maxLength;
}

// Time Complexity: O(n^2) - For each start index, we may check up to n subarrays
// Space Complexity: O(1) - Only a few extra variables used for counting
```

### Approach 2: Using HashMap (Optimal Solution)

**Intuition:**

Instead of checking every possible subarray, we can utilize a running balance where:
- Increasing the `balance` by 1 for '1'.
- Decreasing the `balance` by 1 for '0'.

Converting the problem into finding two indices with the same balance value effectively translates it to finding subarrays with an equal number of 0s and 1s.

**Detail:**

- We maintain a `balance` that is updated as we iterate over the array.
- Use a map to store the first occurrence of each `balance`.
- If the `balance` at index `i` has been seen before at index `j`, the subarray from `j+1` to `i` has a `balance` of zero, implying an equal number of 0s and 1s.
- Update the maximum length accordingly.

```cpp
#include <vector>
#include <unordered_map>

int findMaxLength(std::vector<int>& nums) {
    std::unordered_map<int, int> balanceMap;
    balanceMap[0] = -1;
    int maxLength = 0;
    int balance = 0;

    for (int i = 0; i < nums.size(); ++i) {
        // Update balance
        balance += nums[i] == 1 ? 1 : -1;

        // Check if this balance was seen before
        if (balanceMap.find(balance) != balanceMap.end()) {
            maxLength = std::max(maxLength, i - balanceMap[balance]);
        } else {
            // Store index of the first occurrence of this balance
            balanceMap[balance] = i;
        }
    }

    return maxLength;
}

// Time Complexity: O(n) - Single pass through the array
// Space Complexity: O(n) - HashMap to store first occurrence of each balance value
```

This hashmap solution is more optimal as it reduces the problem to a single traversal with constant time map operations, significantly improving efficiency.

