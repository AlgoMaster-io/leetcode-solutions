# [Leetcode 354: Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Approaches:
- [Approach 1: Dynamic Programming](#approach-1-dynamic-programming)
- [Approach 2: Dynamic Programming with Binary Search](#approach-2-dynamic-programming-with-binary-search)

### Approach 1: Dynamic Programming

**Intuition:**

The Russian Doll Envelopes problem is similar to the Longest Increasing Subsequence (LIS) problem. The primary goal here is to find the maximum number of envelopes that can be put inside one another. The intuition is to first sort the envelopes by width and then apply a dynamic programming approach to track the longest chain of nested envelopes based on height.

**Approach:**

1. **Sort**: First, sort the list of envelopes by width and, if they have the same width, by height in descending order. This helps in dealing with cases where envelopes have the same width because we can't nest them.
  
2. **DP Array**: Use a dynamic programming array `dp` where `dp[i]` represents the length of the longest increasing subsequence of envelopes ending with the envelope at index `i`.

3. **Transit**: For each envelope at position `i`, check all previous envelopes `j` to determine if envelope `j` can fit inside envelope `i`. If so, update the `dp[i]` by considering `dp[j] + 1`.

4. **Result:** The maximum value in the `dp` array will be the answer.

**Time Complexity:** O(n^2)  
**Space Complexity:** O(n)

```python
def maxEnvelopes(envelopes):
    # If there are no envelopes, the max number of envelopes is 0
    if not envelopes:
        return 0
    
    # Sort envelopes by width; if widths are the same, sort by height descending
    envelopes.sort(key=lambda x: (x[0], -x[1]))
    
    n = len(envelopes)
    # Initialize a dp array for storing the longest increasing sequence
    dp = [1] * n
    
    # Build the dp array
    for i in range(n):
        for j in range(i):
            # If the j-th envelope can fit into i-th envelope
            if envelopes[j][1] < envelopes[i][1]:
                # Update the dp at i
                dp[i] = max(dp[i], dp[j] + 1)

    # Maximum of the dp array is our answer
    return max(dp)

# Example usage
envelopes = [[5,4],[6,4],[6,7],[2,3]]
print(maxEnvelopes(envelopes))  # Output: 3
```

### Approach 2: Dynamic Programming with Binary Search

**Intuition:**

Using binary search optimizes the search for the next possible envelope, allowing us to efficiently build the increasing sequence of heights. By sorting envelopes in the same manner, we can manage the sequence of heights using a list where we perform a binary search to maintain the order.

**Approach:**

1. **Sort**: Similar to the previous approach, sort the envelopes by width and then by height in descending order.

2. **LIS with Binary Search**: Use a list `lis` to store the longest increasing subsequence of heights. Iterate through each envelope, and for every envelope, apply a binary search to determine its correct position in the `lis`. If it can extend the sequence, then append it; otherwise, replace the existing value at the found position.

3. **Result**: The length of the `lis` list is the result.

**Time Complexity:** O(n log n)  
**Space Complexity:** O(n)

```python
import bisect

def maxEnvelopes(envelopes):
    if not envelopes:
        return 0

    # Sort envelopes by width; if widths are the same then by height descending
    envelopes.sort(key=lambda x: (x[0], -x[1]))
    
    # Initialize the list for heights to store the increasing sequence
    lis = []

    for _, h in envelopes:
        # Use binary search to find the position to place the current height
        pos = bisect.bisect_left(lis, h)
        
        # If it's larger than the largest possible increasing subsequence so far
        if pos == len(lis):
            lis.append(h)
        else:
            lis[pos] = h
    
    # The length of the lis array is the answer
    return len(lis)

# Example usage
envelopes = [[5,4],[6,4],[6,7],[2,3]]
print(maxEnvelopes(envelopes))  # Output: 3
```

In this optimized approach, the combination of sorting and binary search allows us to achieve a more efficient solution compared to the standard dynamic programming approach.

