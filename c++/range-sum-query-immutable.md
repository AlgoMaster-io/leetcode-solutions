# [LeetCode 303: Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Prefix Sum](#approach-2-prefix-sum)

## Approach 1: Brute Force

### Intuition:
The simplest way to solve the problem is to compute the sum of elements from index `i` to `j` by iterating through the array each time a query is made. Although this approach is straightforward, it is not efficient when dealing with multiple queries.

### Steps:
1. For each query `(i, j)`, initialize a sum variable to 0.
2. Iterate over the array from index `i` to `j`.
3. Accumulate the sum of elements in this range.
4. Return the sum.

### Time Complexity:
- Query time complexity is \(O(n)\) per query, where \(n = j - i + 1\).

### Space Complexity:
- \(O(1)\) since we only use a fixed amount of extra space.

```cpp
class NumArray {
public:
    NumArray(vector<int>& nums) : data(nums) { }

    int sumRange(int i, int j) {
        int sum = 0;
        for (int k = i; k <= j; ++k) {
            sum += data[k];  // Accumulate the sum of elements from index i to j
        }
        return sum; // Return the computed sum
    }
    
private:
    vector<int> data; // Store the array to be queried
};
```

## Approach 2: Prefix Sum

### Intuition:
To optimize the query process, we can use a prefix sum array. The prefix sum is a pre-computed array where each element at index `k` contains the sum of elements from the start of the array up to the index `k`. With this array, each query can be answered in constant time.

### Steps:
1. Precompute a prefix sum array where each element `prefix[i]` is the sum of the array from the start to index `i`.
2. For a sum range query `(i, j)`, the sum can be calculated as `prefix[j + 1] - prefix[i]`.

### Time Complexity:
- Building the prefix sum takes \(O(n)\).
- Query time complexity is \(O(1)\) per query.

### Space Complexity:
- \(O(n)\) for the prefix sum array.

```cpp
class NumArray {
public:
    NumArray(vector<int>& nums) {
        int n = nums.size();
        prefixSum.resize(n + 1, 0); // Initialize prefixSum array with an extra element
        for (int i = 0; i < n; ++i) {
            prefixSum[i + 1] = prefixSum[i] + nums[i]; // Build the prefix sum array
        }
    }

    int sumRange(int i, int j) {
        // Compute the sum for the range (i, j) using the prefix sums
        return prefixSum[j + 1] - prefixSum[i];
    }

private:
    vector<int> prefixSum; // Array to store prefix sums
};
```

Both solutions solve the range sum query problem, but the prefix sum method is far more efficient for multiple query scenarios, reducing the query operation to constant time by performing the necessary computations upfront.

