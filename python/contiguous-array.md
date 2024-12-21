# [LeetCode 525: Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approaches:
1. [Brute Force Solution](#approach-1-brute-force-solution)
2. [Prefix Sum with HashMap Solution (Optimal)](#approach-2-prefix-sum-with-hashmap-solution-optimal)

## Approach 1: Brute Force Solution

### Intuition
The simplest approach is to consider every subarray within the array and determine whether it contains an equal number of 0's and 1's. This approach involves generating all possible subarrays and then counting the number of 0's and 1's in each subarray to check if they are equal.

This is a classic brute force approach which does not use any advanced optimization techniques but systematically tries all possibilities.

### Code

```python
def findMaxLength(nums):
    # Initialize the maximum length to 0
    max_length = 0
    
    # Go through each possible starting point of a subarray
    for start in range(len(nums)):
        zero_count = 0  # Count of 0's
        one_count = 0   # Count of 1's
        
        # Try each ending point of the subarray starting from this start point
        for end in range(start, len(nums)):
            # Count the number of 0's and 1's
            if nums[end] == 0:
                zero_count += 1
            else:
                one_count += 1
            
            # Check if this subarray has the same number of 0's and 1's
            if zero_count == one_count:
                max_length = max(max_length, end - start + 1)
                
    return max_length
```

### Time Complexity
- **Time:** \(O(n^2)\) due to the nested loops over the range of the array.
- **Space:** \(O(1)\) as it uses constant extra space.

## Approach 2: Prefix Sum with HashMap Solution (Optimal)

### Intuition
The key observation here is to use a prefix sum strategy combined with a hashmap for efficient lookup:
- Convert the problem of finding equal numbers of 0's and 1's to finding subarrays whose elements sum to zero if we use a modified counting scheme (e.g., treat 0 as -1).
- Track cumulative sums and use a hashmap to quickly find if a particular cumulative sum has been seen before, which would indicate a subarray with zero sum.

For every element, we increment or decrement the cumulative sum. If the same sum has been seen before, it means the subarray elements between the two instances of this sum have balanced 0's and 1's.

### Code

```python
def findMaxLength(nums):
    # Dictionary to map first occurrence of cumulative sum
    sum_index_map = {0: -1}
    max_length = 0
    cumulative_sum = 0
    
    for i, num in enumerate(nums):
        # Adjust cumulative sum, treat 0 as -1
        if num == 0:
            cumulative_sum -= 1
        else:
            cumulative_sum += 1
        
        # Check if this cumulative sum has been seen before
        if cumulative_sum in sum_index_map:
            # The subarray between the first occurrence and now has equal 0’s and 1’s
            max_length = max(max_length, i - sum_index_map[cumulative_sum])
        else:
            # Store the first occurrence of this sum
            sum_index_map[cumulative_sum] = i
    
    return max_length
```

### Time Complexity
- **Time:** \(O(n)\) as we iterate through the array once and hashmap operations are \(O(1)\).
- **Space:** \(O(n)\) due to the hashmap storing cumulative sums and their indices.

This optimal solution efficiently determines the longest subarray with equal number of 0's and 1's in linear time, making it significantly faster and more scalable than the brute force approach for larger arrays.

