# [Leetcode 523: Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

## Approaches:
1. [Brute Force](#brute-force)
2. [HashMap for Remainders](#hashmap-for-remainders)

### Brute Force

This approach involves checking all possible subarrays and calculating their sums to determine if any of these subarrays have a sum that is a multiple of `k`.

#### Intuition:
- Iterate through all possible starting points of a subarray.
- For each starting point, determine all possible subarray sums and check if divisible by `k`.
- The straightforward solution involves two loops for generating subarrays, which might not be efficient but helps understand the problem.

#### Approach:
- Use two nested loops to generate subarrays.
- Calculate the sum of each subarray and check whether it is a multiple of `k`.
- If a valid subarray is found, return true.

#### Time Complexity:
- O(n^2), where n is the number of elements in the array.

#### Space Complexity:
- O(1), as no extra space is used.

```cpp
bool checkSubarraySum(vector<int>& nums, int k) {
    int n = nums.size();
    for (int start = 0; start < n; ++start) {
        int sum = 0;
        for (int end = start; end < n; ++end) {
            sum += nums[end];
            // Check if the sum is a multiple of k, and subarray has at least 2 elements
            if (end - start >= 1 && sum % k == 0) {
                return true;
            }
        }
    }
    return false;
}
```

### HashMap for Remainders

Optimize the solution using a HashMap to track remainders of cumulative sums. This allows us to determine if there exists a subarray sum that is a multiple of `k`.

#### Intuition:
- Instead of checking all subarrays, use the properties of modular arithmetic.
- If the cumulative sum up to two indices produces the same remainder when divided by `k`, the elements in between sum to a multiple of `k`.
- This is similar to finding the subarray with sum zero technique.

#### Approach:
- Traverse the array, maintaining the cumulative sum.
- Use a HashMap to store each unique remainder `(cumulative_sum % k)` and its index.
- If the same remainder appears again, it indicates that the intermediate sum is a multiple of `k`.
- Also, ensure the subarray size is at least 2 by checking the index difference.

#### Time Complexity:
- O(n), as we only traverse the array once.

#### Space Complexity:
- O(min(n, k)), for storing remainders.

```cpp
bool checkSubarraySum(vector<int>& nums, int k) {
    unordered_map<int, int> remainderIndex;
    remainderIndex[0] = -1; // For cases where the subarray starts from index 0
    int cumulativeSum = 0;
    for (int i = 0; i < nums.size(); ++i) {
        cumulativeSum += nums[i];
        int remainder = cumulativeSum % k;
        // Handle negative remainder
        if (remainder < 0) remainder += k;
        // If the remainder is found, check the subarray length
        if (remainderIndex.find(remainder) != remainderIndex.end()) {
            if (i - remainderIndex[remainder] > 1) {
                return true;
            }
        } else {
            // Store the first occurrence of this remainder
            remainderIndex[remainder] = i;
        }
    }
    return false;
}
```

By using the second approach, the problem becomes efficiently solvable, highlighting a clever use of modular arithmetic and hash maps to solve otherwise complex problems.

