# [Leetcode 673: Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## Approaches:
1. [Dynamic Programming with Two Arrays](#approach-1-dynamic-programming-with-two-arrays)

### Approach 1: Dynamic Programming with Two Arrays

**Intuition:**
The problem is to find the number of longest increasing subsequences in the given array. Here, we will use a dynamic programming approach where one array will keep track of the length of the longest increasing subsequence ending at each index, and another array will keep the number of such subsequences.

**Detailed Steps:**
1. Create two arrays: `lengths` and `counts`.
    - `lengths[i]` will store the length of the longest increasing subsequence ending at index `i`.
    - `counts[i]` will store the number of longest increasing subsequences ending at index `i`.
2. Initialize each element of `lengths` to 1 because the minimum length of an increasing subsequence ending at any element is 1 (the element itself).
3. Initialize each element of `counts` to 1 because there's at least one subsequence ending at each element (the element itself).
4. Iterate through the array with two nested loops:
    - For each pair of indices (j < i), check if `nums[j] < nums[i]`.
    - If it is, update the `lengths` and `counts` arrays.
        - If `lengths[j] + 1 > lengths[i]`, it means we have found a longer subsequence by adding `nums[i]` to the subsequence ending at `nums[j]`. Thus, update `lengths[i]` and set `counts[i] = counts[j]`.
        - If `lengths[j] + 1 == lengths[i]`, it means we have found another subsequence of the same maximum length. Add `counts[j]` to `counts[i]`.
5. After processing, find the maximum value in the `lengths` array, which represents the length of the longest increasing subsequence.
6. Sum up all values in `counts` where corresponding `lengths` are equal to the maximum value found in step 5.

**Java Code:**

```java
public int findNumberOfLIS(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    
    // Array to store the length of the longest increasing subsequence ending at each index
    int[] lengths = new int[nums.length];
    // Array to store the count of longest increasing subsequences ending at each index
    int[] counts = new int[nums.length];
    Arrays.fill(lengths, 1); // Initialize lengths to 1
    Arrays.fill(counts, 1);  // Initialize counts to 1

    int n = nums.length;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) { // A valid increasing subsequence
                if (lengths[j] + 1 > lengths[i]) {
                    // Found a longer length subsequence ending at i
                    lengths[i] = lengths[j] + 1;
                    counts[i] = counts[j]; // Reset count to count[j]
                } else if (lengths[j] + 1 == lengths[i]) {
                    // Found another subsequence of maximum length
                    counts[i] += counts[j];
                }
            }
        }
    }

    // Find the length of the longest subsequence
    int maxLength = 0;
    for (int length : lengths) {
        maxLength = Math.max(maxLength, length);
    }

    // Sum up the counts for subsequences which have the maxLength
    int result = 0;
    for (int i = 0; i < n; i++) {
        if (lengths[i] == maxLength) {
            result += counts[i];
        }
    }
    return result;
}
```

**Time Complexity:** O(n^2), where n is the length of the input array. This complexity arises due to the nested loops iterating over the array.

**Space Complexity:** O(n), as we are using two additional arrays `lengths` and `counts` to store information regarding the longest subsequences.

