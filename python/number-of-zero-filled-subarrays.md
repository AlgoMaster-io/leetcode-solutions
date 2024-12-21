# [Leetcode Problem 2348: Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Sliding Window](#optimized-sliding-window)

---

### Brute Force Approach

**Intuition:**

The simplest approach is to iterate through every possible subarray in the given array and count how many of these subarrays consist only of zeros. While this method works correctly, it is not efficient given the large possible size of the input array.

**Algorithm:**
1. Initialize a counter to zero which will eventually store the number of zero-filled subarrays.
2. Iterate through each element in the array, treating it as the starting element of a new subarray.
3. For each starting element, iterate through subsequent elements to form subarrays.
4. If a subarray contains only zeros, increment the counter.
5. Return the counter at the end of the iterations.

**Code:**

```python
def zeroFilledSubarray(nums):
    count = 0  # This will hold the number of zero-filled subarrays
    
    # Iterate through each starting element
    for i in range(len(nums)):
        # Track if we encounter a non-zero before ending the subarray
        for j in range(i, len(nums)):
            if nums[j] == 0:
                count += 1
            else:
                break  # As soon as we hit a non-zero, break the inner loop
    
    return count

# Example usage
print(zeroFilledSubarray([0, 0, 0, 2, 0, 0]))  # Output: 9
```

- **Time Complexity:** O(n^2) - Because we are evaluating all subarrays.
- **Space Complexity:** O(1) - Only constant space is used for the counter.

---

### Optimized Sliding Window

**Intuition:**

Instead of checking every possible subarray, we focus on contiguous zero segments. Once we encounter a zero, we calculate how many zero-filled subarrays it can create by looking at the current zero segment size. Every new zero extends all existing zero-filled subarrays to include itself and adds new subarrays of size 1. This can be seen as an arithmetic progression.

**Algorithm:**
1. Initialize a counter to zero to hold the number of zero-filled subarrays.
2. Initialize a current zero sequence length counter to zero.
3. Traverse through the array. For each zero encountered, increase the current zero sequence length.
4. Each zero in a sequence gives rise to an increased number of zero-filled subarrays, so add the current sequence length to the overall counter.
5. Reset the current zero sequence length counter when a non-zero is encountered.
6. Return the total count.

**Code:**

```python
def zeroFilledSubarray(nums):
    count = 0  # Total count of zero-filled subarrays
    zeroSeqLength = 0  # Length of the current sequence of zeros
    
    for num in nums:
        if num == 0:
            # Increment the zero sequence length
            zeroSeqLength += 1
            # Add the current sequence length to overall count
            count += zeroSeqLength
        else:
            # Reset the zero sequence length for a non-zero number
            zeroSeqLength = 0
    
    return count

# Example usage
print(zeroFilledSubarray([0, 0, 0, 2, 0, 0]))  # Output: 9
```

- **Time Complexity:** O(n) - We traverse through the array only once.
- **Space Complexity:** O(1) - Additional space is used for a few counters. 

By employing the Sliding Window approach, we significantly improve the efficiency of our solution.

