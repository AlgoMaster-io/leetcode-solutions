[Leetcode Problem 673: Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

### Approaches:
1. [Basic Dynamic Programming Approach](#basic-dynamic-programming-approach)
2. [Optimized Dynamic Programming Approach with Explanation of Count Array](#optimized-dynamic-programming-approach-with-explanation-of-count-array)

---

### Basic Dynamic Programming Approach

**Intuition:**
The simplest form of the problem can be solved using Dynamic Programming (DP) similar to the Longest Increasing Subsequence (LIS) problem. The idea is to use an array `dp` where `dp[i]` stores the length of the longest increasing subsequence that ends at index `i`. We iterate through each element and check all previous elements to find if we can extend the subsequence, updating our `dp` array accordingly.

**C# code:**

```csharp
public int FindNumberOfLIS(int[] nums) {
    if (nums.Length == 0) return 0;

    int n = nums.Length;
    // Array to store the longest increasing subsequence ending at each position
    int[] dp = new int[n];
    // Initialize dp array with 1 as each number is a subsequence of itself
    Array.Fill(dp, 1);

    int maxLength = 1;

    // Compute the dp array values
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.Max(dp[i], dp[j] + 1);
            }
        }
        maxLength = Math.Max(maxLength, dp[i]);
    }

    // Count the number of sequences that have maxLength
    int count = 0;
    for (int i = 0; i < n; i++) {
        if (dp[i] == maxLength) count++;
    }

    return count;
}
```

**Complexities:**
- **Time Complexity:** O(n^2), where n is the length of the input array. We have a nested loop where each element is compared with every previous element.
- **Space Complexity:** O(n), due to the use of the `dp` array.

---

### Optimized Dynamic Programming Approach with Explanation of Count Array

**Intuition:**
An improved approach involves not only tracking the length of the longest subsequence ending at each element but also the number of such sequences. We use a `count` array parallel to `dp` where `count[i]` stores the number of longest increasing subsequences that end at index `i`.

**C# code:**

```csharp
public int FindNumberOfLIS(int[] nums) {
    if (nums.Length == 0) return 0;

    int n = nums.Length;
    // Array to store the longest increasing subsequence ending at each position
    int[] dp = new int[n];
    // Array to store the count of longest increasing subsequences ending at each position
    int[] count = new int[n];
    // Each number is a subsequence of length 1 and there's initially one way to have a subsequence of length 1
    Array.Fill(dp, 1);
    Array.Fill(count, 1);

    int maxLength = 1;

    // Compute the dp and count array values
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                // If nums[i] can extend the subsequence ending at nums[j]
                if (dp[j] + 1 > dp[i]) {
                    dp[i] = dp[j] + 1;
                    // If a longer subsequence is found, reset count to the count of j
                    count[i] = count[j];
                } else if (dp[j] + 1 == dp[i]) {
                    // If another subsequence with the same length as the current max is found
                    count[i] += count[j];
                }
            }
        }
        maxLength = Math.Max(maxLength, dp[i]);
    }

    // Summing up all the counts of subsequences that have the length equal to maxLength
    int totalCount = 0;
    for (int i = 0; i < n; i++) {
        if (dp[i] == maxLength) totalCount += count[i];
    }

    return totalCount;
}
```

**Complexities:**
- **Time Complexity:** O(n^2), similar to the basic approach, due to the nested loop structure.
- **Space Complexity:** O(n), due to the two arrays `dp` and `count`.

Both approaches solve the problem, but the second approach provides insight into the distribution of subsequences and counts them more efficiently.

