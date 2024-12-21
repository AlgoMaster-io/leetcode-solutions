# [Leetcode 560: Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Cumulative Sum with HashMap](#approach-2-cumulative-sum-with-hashmap)

### Approach 1: Brute Force

**Intuition:**

The simplest approach is to consider all possible subarrays and for each subarray, calculate the sum and check if it is equal to `k`. If it is, increase the count. This involves two nested loops, one for the start of the subarray and another for the end of the subarray.

**Time Complexity:** \(O(n^2)\)  
**Space Complexity:** \(O(1)\)

```python
def subarraySum(nums, k):
    # Initialize a variable to store the count of subarrays
    count = 0
    # Iterate over each possible starting index of the subarray
    for start in range(len(nums)):
        # Initialize the sum for this starting index
        curr_sum = 0
        # Iterate over each possible ending index of the subarray
        for end in range(start, len(nums)):
            # Add the current number to the current sum
            curr_sum += nums[end]
            # Check if the current sum equals k
            if curr_sum == k:
                # Increment the count if the sum equals k
                count += 1
    return count
```

### Approach 2: Cumulative Sum with HashMap

**Intuition:**

A more efficient approach involves using a hash map to store cumulative sums and their frequencies. The idea is to calculate the cumulative sum as we iterate through the array. For each element, we check if there is a previous cumulative sum so that current cumulative sum minus this sum equals `k`. If such a sum exists, it means there is a subarray ending at the current index with sum `k`.

**Time Complexity:** \(O(n)\)  
**Space Complexity:** \(O(n)\)

```python
def subarraySum(nums, k):
    # Initialize a dictionary to store the frequencies of cumulative sums
    sum_freq = {0: 1}  # Base case for a cumulative sum that directly equals k
    # Initialize variables
    curr_sum = 0
    count = 0
    
    # Iterate over each number in the input list
    for num in nums:
        # Update the cumulative sum
        curr_sum += num
        # Calculate the difference needed to get the target sum k
        diff = curr_sum - k
        
        # If the difference exists in the dictionary, it means there are
        # some subarrays that sum up to k ending at the current index
        if diff in sum_freq:
            # Increment the count by the number of times the difference 
            # has occurred
            count += sum_freq[diff]
        
        # Record the current sum in the dictionary, incrementing 
        # its frequency
        if curr_sum in sum_freq:
            sum_freq[curr_sum] += 1
        else:
            sum_freq[curr_sum] = 1
    
    return count
```

In this approach, we efficiently count the subarrays summing to `k` using a hash map to track the cumulative sum frequencies, thus leading to an optimal time complexity of \(O(n)\).

