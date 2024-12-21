# [Leetcode 300: Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

## Approaches:
1. [Dynamic Programming Approach](#dynamic-programming-approach)
2. [Dynamic Programming with Binary Search](#dynamic-programming-with-binary-search)

---

## Dynamic Programming Approach

### Intuition:
The idea is to use dynamic programming to keep track of the longest increasing subsequence ending at each index. We initialize a `dp` array where `dp[i]` represents the length of the longest increasing subsequence that ends at the `i-th` index. For each element at index `i`, we check all the previous elements (`0` to `i-1`), and if the current element is greater than any of those elements, we update the `dp[i]` as the maximum of `dp[i]` and `dp[j] + 1`.

#### Steps:
1. Create a `dp` array initialized to 1. Each element itself is a subsequence.
2. Loop through each element of the array.
3. For each element, loop through all previous elements.
4. If a previous element is smaller than the current element, update the `dp` value to reflect the longest subsequence.
5. Finally, the answer will be the maximum value in the `dp` array indicating the length of the longest increasing subsequence.

```go
func lengthOfLIS(nums []int) int {
    n := len(nums)
    if n == 0 {
        return 0
    }

    // Initialize dp array where each element is a subsequence by itself.
    dp := make([]int, n)
    for i := 0; i < n; i++ {
        dp[i] = 1
    }

    // Iterate through each element
    for i := 1; i < n; i++ {
        // Compare with previous elements
        for j := 0; j < i; j++ {
            if nums[i] > nums[j] {
                dp[i] = max(dp[i], dp[j] + 1)
            }
        }
    }

    // Find the max value in the dp array
    longest := 0
    for _, length := range dp {
        if length > longest {
            longest = length
        }
    }

    return longest
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

**Time Complexity**: O(n^2) - We iterate over all pairs of elements.  
**Space Complexity**: O(n) - We use a `dp` array of size `n`.

---

## Dynamic Programming with Binary Search

### Intuition:
We can optimize the above approach using binary search. Instead of maintaining a `dp` array, we maintain a `sub` array that helps us track the minimal value of the last element of the increasing subsequences of different lengths. For each element in the input list, we can use binary search to locate the position at which this element can replace an existing element in the `sub` array or extend the `sub` array to form a new subsequence. This approach leverages the efficiency of binary search to achieve a lower time complexity.

#### Steps:
1. Initialize an empty `sub` array.
2. For each element in the array:
   - Use binary search to find the position where this element can fit into the `sub` array.
   - If the position is at the end of the `sub`, append the element.
   - Otherwise, replace the element in the `sub` array at the found position (to maintain a smaller potential end element).
3. The length of the `sub` array will be the length of the longest increasing subsequence.

```go
import "sort"

func lengthOfLIS(nums []int) int {
    // Initialize a sub array
    var sub []int

    // Iterate over each number in the given array
    for _, num := range nums {
        // Find the position in the sub array using binary search
        i := sort.SearchInts(sub, num)
        // If index is equal to the length of sub, append new number
        if i == len(sub) {
            sub = append(sub, num)
        } else {
            // Otherwise, replace element in sub with the new number
            sub[i] = num
        }
    }

    // Length of sub array is the length of the longest increasing subsequence
    return len(sub)
}
```

**Time Complexity**: O(n log n) - Binary search for each of the n elements.  
**Space Complexity**: O(n) - Maintain the `sub` array that could potentially hold all elements in the worst case.

