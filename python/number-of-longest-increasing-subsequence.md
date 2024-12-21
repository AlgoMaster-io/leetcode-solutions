# [Leetcode 673: Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Dynamic Programming with Memoization](#dynamic-programming-with-memoization)

---

### Brute Force Approach

#### Intuition
The brute force approach involves exploring all possible increasing subsequences and computing their lengths to determine the longest increasing subsequences. This solution may be implemented through recursion, generating all subsequences and counting the longest ones by comparing lengths.

#### Approach
1. Generate all subsequences.
2. Check each subsequence if it is increasing.
3. Track the lengths of all increasing subsequences.
4. Count how many subsequences have the maximum length.

```python
def length_of_lis(nums):
    def is_increasing(subseq):
        return all(subseq[i] < subseq[i + 1] for i in range(len(subseq) - 1))
    
    from itertools import combinations
    n = len(nums)
    max_length = 0
    num_of_longest = 0

    # Check all subsequences of different lengths.
    for length in range(1, n + 1):
        for subseq in combinations(nums, length):
            if is_increasing(subseq):
                if length > max_length:
                    max_length = length
                    num_of_longest = 1
                elif length == max_length:
                    num_of_longest += 1

    return num_of_longest

# Complexity Analysis: 
# Time Complexity: O(2^n * n), since we examine each of the 2^n possible subsequences and check if they are increasing.
# Space Complexity: O(1), aside from the input, we are using a few integers for keeping track of lengths and counts.
```

### Dynamic Programming with Memoization

#### Intuition
Dynamic programming can be used to efficiently solve this problem by storing intermediate results. We maintain two arrays:
1. `lengths[i]`, which stores the length of the longest increasing subsequence ending at index `i`.
2. `counts[i]`, which stores the number of longest increasing subsequences ending at index `i`.

#### Approach
1. Initialize arrays `lengths` and `counts` such that every position starts at a length of 1 and count of 1.
2. Iterate over each element in the array while checking all previous elements.
3. If `nums[j] < nums[i]`, then `nums[i]` can be appended to the increasing subsequence ending at `nums[j]`.
4. Update `lengths[i]` accordingly and update `counts[i]` based on whether we have found a longer subsequence or an equal-length subsequence.

```python
def findNumberOfLIS(nums):
    if not nums:
        return 0
    
    n = len(nums)
    lengths = [1] * n  # lengths[i] will be the length of longest ending in nums[i].
    counts = [1] * n   # counts[i] will track the number of longest increasing subsequences ending in nums[i].

    for i in range(n):
        for j in range(i):
            if nums[j] < nums[i]:  # A valid subsequence extension
                if lengths[j] + 1 > lengths[i]:
                    lengths[i] = lengths[j] + 1
                    counts[i] = counts[j]
                elif lengths[j] + 1 == lengths[i]:
                    counts[i] += counts[j]

    longest = max(lengths)
    return sum(c for i, c in enumerate(counts) if lengths[i] == longest)

# Complexity Analysis:
# Time Complexity: O(n^2), because we have a nested loop over the array.
# Space Complexity: O(n), to store the `lengths` and `counts` arrays.
```

The dynamic programming approach provides a more optimal way to solve the problem due to reduced redundant calculations, converting an exponential problem to polynomial time.

