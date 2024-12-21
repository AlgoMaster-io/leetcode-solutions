# [LeetCode Problem 300: Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

## Approaches:

- [Approach 1: Dynamic Programming (Basic)](#approach-1-dynamic-programming)
- [Approach 2: Patience Sorting (Optimized with Binary Search)](#approach-2-patience-sorting)

---

## Approach 1: Dynamic Programming (Basic)

### Intuition

The basic idea is to use dynamic programming to build a solution. The problem is similar to finding the longest path in a directed graph, but in this case, the direction is increasing order. We'll maintain a `dp` array where `dp[i]` stores the length of the longest increasing subsequence that ends with the element at index `i`.

For each element at index `i`, we look for all elements before it (at indices `j` where `0 <= j < i`) and check if they can be part of the increasing subsequence ending at `i`. If the element at `j` is less than the element at `i`, then the sequence can be extended, and we update `dp[i]` to be the maximum of its current value and `dp[j] + 1`.

### Steps

1. Create a `dp` array with the same length as the input array, initialized to 1, because the minimum length of LIS ending at each index is 1 (the element itself).
2. Iterate through the array, and for each index, check all previous indices to extend the subsequences.
3. Update the `dp` array according to the rule mentioned above.
4. The result is the maximum value in the `dp` array.

### Time Complexity

- **Time Complexity**: O(n^2) where `n` is the length of the input array. This is due to the nested loop structure.
- **Space Complexity**: O(n) for the `dp` array.

### Code Implementation

```csharp
public int LengthOfLIS_DP(int[] nums) {
    if (nums == null || nums.Length == 0) return 0;
    
    int n = nums.Length;
    int[] dp = new int[n];
    
    // Initialize the dp array to 1, as the minimum LIS ending at each i is 1
    for (int i = 0; i < n; i++) {
        dp[i] = 1;
    }

    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            // If the current element is greater, consider it for the increasing subsequence
            if (nums[i] > nums[j]) {
                dp[i] = Math.Max(dp[i], dp[j] + 1);
            }
        }
    }

    // The length of the longest increasing subsequence
    return dp.Max();
}
```

---

## Approach 2: Patience Sorting (Optimized with Binary Search)

### Intuition

This approach draws inspiration from the game of patience sorting (a card game). It uses a `tails` array to keep track of the ends of potential increasing subsequences found during iteration. The length of this `tails` array in the end will represent the length of the longest increasing subsequence.

By using binary search, we can efficiently place each number in its correct position in the `tails` array, ensuring that the `tails` array remains sorted.

### Steps

1. Initialize an empty list `tails` which will hold the end elements of the different piles (subsequences).
2. Traverse the input array and for each number, use binary search to find its position in the `tails` list.
3. If the number can extend the largest found subsequence (i.e., it's greater than all in `tails`), append it.
4. Otherwise, replace the element in `tails` that can be increased (but is larger than the current number) with the current number to maintain the smallest possible value.
5. The length of the `tails` list at the end tells us the length of the longest increasing subsequence.

### Time Complexity

- **Time Complexity**: O(n log n) because for each number, binary search is performed.
- **Space Complexity**: O(n) since the `tails` list can have at most `n` elements.

### Code Implementation

```csharp
public int LengthOfLIS_PatienceSorting(int[] nums) {
    if (nums == null || nums.Length == 0) return 0;
    
    List<int> tails = new List<int>();

    foreach (var num in nums) {
        int left = 0, right = tails.Count;

        // Perform binary search
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (tails[mid] < num) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }

        // If left is equal to the size of tails, it means this number can extend the largest subsequence
        if (left == tails.Count) {
            tails.Add(num);
        } else {
            tails[left] = num;
        }
    }

    return tails.Count;
}
```

Each approach offers a different balance of simplicity and performance, suited for different constraints and use cases. Choose the dynamic programming approach for clarity and the patience sorting method for optimal performance.

