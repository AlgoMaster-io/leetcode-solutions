# [Leetcode 300: Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

## Approaches
- [Approach 1: Dynamic Programming](#approach-1-dynamic-programming)
- [Approach 2: Dynamic Programming with Binary Search](#approach-2-dynamic-programming-with-binary-search)

### Approach 1: Dynamic Programming

**Intuition:**

The idea is to use a dynamic programming table to keep track of the length of the longest increasing subsequence that ends with the element at each position in the array. At each element, we consider all previous elements and find the longest subsequence that this element can extend.

**Steps:**
1. Initialize a `dp` array where `dp[i]` represents the length of the longest increasing subsequence that ends at index `i`.
2. Iterate over each element in the array. For each element, check all previous elements:
   - If the current element is greater than the previous element, then it can extend the subsequence ending at that previous element.
   - Update `dp[i]` to be the maximum of its current value or `dp[j] + 1` where `j` is an index before `i`.
3. The answer is the maximum value in the `dp` array.

**Code:**

```java
public class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        // dp[i] means the length of LIS ending with nums[i]
        int[] dp = new int[nums.length];
        Arrays.fill(dp, 1); // Each element is a subsequence of length 1
        
        int maxLength = 1; // At least one element in the array

        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                // If nums[i] can extend the sequence ending with nums[j]
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            // Update max result as we go
            maxLength = Math.max(maxLength, dp[i]);
        }
        return maxLength;
    }
}
```

**Complexity:**

- Time Complexity: O(n^2), where n is the length of the array. We examine each pair of indices once.
- Space Complexity: O(n), used for the dp array.

### Approach 2: Dynamic Programming with Binary Search

**Intuition:**

Instead of maintaining an array to find the longest increasing subsequence with a direct comparison which results in O(n^2) complexity, we can use a clever trick involving binary search. The approach is to maintain an array that keeps the "minimal" possible tail for all increasing subsequences of various lengths. This makes the time complexity significantly better.

**Steps:**
1. Create an array `tails` where `tails[i]` represents the smallest tail value of all increasing subsequences of length `i+1`.
2. For each number in the input array:
   - Use binary search (or manual iteration) to find the location it can replace in the `tails` array.
   - Update the `tails` array: if the number is larger than the largest element in `tails`, append it.
3. The length of `tails` will be the length of the longest increasing subsequence.

**Code:**

```java
public class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int[] tails = new int[nums.length];
        int size = 0;

        for (int num : nums) {
            int left = 0, right = size;
            // Binary search for the location to replace in tails
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (tails[mid] < num) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            // Update tails or append to tails
            tails[left] = num;
            if (left == size) size++;
        }

        return size;
    }
}
```

**Complexity:**

- Time Complexity: O(n log n), due to binary searching within tails for each of the n elements.
- Space Complexity: O(n), used for the tails array.


