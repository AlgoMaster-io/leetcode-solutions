# [Leetcode Problem 300: Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

## Approaches
1. [Brute Force - Recursion](#approach-1-brute-force---recursion)
2. [Dynamic Programming](#approach-2-dynamic-programming)
3. [Dynamic Programming with Binary Search](#approach-3-dynamic-programming-with-binary-search)

## Approach 1: Brute Force - Recursion

The basic idea is to try all possible subsequences and maintain a variable for the longest increasing subsequence. This approach uses recursion to explore all potential subsequences and checks whether each is increasing.

### Intuition

- For each element, you have two choices: either to include it in your current subsequence if it's larger than the previously chosen element or to exclude it and move to the next element.
- Make recursive calls for each of these decisions to explore all subsequences.

### Code
```python
def lengthOfLIS(nums):
    def helper(index, prev_index):
        if index == len(nums):
            return 0
        
        # Choice 1: Don't include the current element
        max_len = helper(index + 1, prev_index)
        
        # Choice 2: Include the current element if it makes the sequence increasing
        if prev_index == -1 or nums[index] > nums[prev_index]:
            max_len = max(max_len, 1 + helper(index + 1, index))
        
        return max_len

    return helper(0, -1)
```

### Complexity Analysis
- **Time Complexity**: O(2^n), where n is the number of elements in the array. In the worst case, we check every possibility.
- **Space Complexity**: O(n) due to the recursion call stack.

## Approach 2: Dynamic Programming

The idea is to use dynamic programming to store results of subproblems and build up solutions to larger and larger problems incrementally.

### Intuition

- Use a DP array where each index `i` contains the length of the longest increasing subsequence that ends with the element at index `i`.
- For each element, check all previous elements and update the dp value if the current element can extend the increasing subsequence formed by the previous element.

### Code
```python
def lengthOfLIS(nums):
    if not nums:
        return 0
    
    n = len(nums)
    dp = [1] * n  # Each element is at least an increasing subsequence of length 1

    for i in range(1, n):
        for j in range(0, i):
            if nums[i] > nums[j]:  # Check if the subsequence can be extended
                dp[i] = max(dp[i], dp[j] + 1)
    return max(dp)  # The longest increasing subsequence is the max value in the dp array
```

### Complexity Analysis
- **Time Complexity**: O(n^2), where n is the number of elements in the array.
- **Space Complexity**: O(n) for keeping the dp array.

## Approach 3: Dynamic Programming with Binary Search

This approach optimizes the DP approach using binary search to find the appropriate position of each element in the subsequence.

### Intuition

- Maintain a list (let's call it `sub`) that keeps track of the smallest tail of all increasing subsequences with different lengths.
- For each number, use binary search to determine its place in `sub`.
- If it is larger than the largest element in `sub`, append it.
- Otherwise, replace the first element in `sub` which is larger than or equal to the number.

### Code
```python
from bisect import bisect_left

def lengthOfLIS(nums):
    sub = []
    for x in nums:
        # Find the index where x should be placed (if all elements are unique)
        i = bisect_left(sub, x)
        
        # If x is larger than any element in sub
        if i == len(sub):
            sub.append(x)
        else:
            sub[i] = x
    
    return len(sub)
```

### Complexity Analysis
- **Time Complexity**: O(n log n) due to the binary search implementation.
- **Space Complexity**: O(n) for maintaining the `sub` array.

