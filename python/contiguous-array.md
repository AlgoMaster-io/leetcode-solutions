
# Contiguous Array
Given a binary array nums, return the maximum length of a contiguous subarray with an equal number of 0 and 1.

### Constraints:
- 1 <= nums.length <= 10^5
- nums[i] is either 0 or 1.

### Examples
```javascript
Input: nums = [0,1]
Output: 2
Explanation: [0, 1] is the longest contiguous subarray with an equal number of 0 and 1.

Input: nums = [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a contiguous subarray with equal number of 0 and 1.
```

## Approaches to Solve the Problem
### Approach 1: Brute Force
##### Intuition:
In the brute-force solution, we try every possible subarray and check if it has an equal number of 0s and 1s. For each subarray, count the number of 0s and 1s, and if they are equal, update the maximum length.

- Loop over every subarray starting at each index.
- For each subarray, count the number of 0s and 1s.
- Update the maximum length if the number of 0s and 1s are equal.
##### Time Complexity:
O(n^2), where n is the length of the array. This is because we check every possible subarray.
##### Space Complexity:
O(1), no extra space is used.
##### Python Code:
```python
def findMaxLength(nums: List[int]) -> int:
    max_len = 0
    n = len(nums)
    
    for i in range(n):
        zeroes = ones = 0
        for j in range(i, n):
            if nums[j] == 0:
                zeroes += 1
            else:
                ones += 1
            if zeroes == ones:
                max_len = max(max_len, j - i + 1)
                
    return max_len
```
### Approach 2: Prefix Sum with Hash Map
##### Intuition: 
To optimize the solution, we can use the prefix sum technique. The key observation is that if we treat 0 as -1, then finding a subarray with an equal number of 0s and 1s is equivalent to finding a subarray whose sum is 0. Using this insight:

- Convert 0 to -1.
- Use a hash map (sum_index) to store the first occurrence of each cumulative sum.
- If the same sum appears again at a later index, it means the subarray between these two indices has a sum of 0.
- Calculate the length of this subarray and update the maximum length.
##### Visualization:
For nums = [0, 1, 0]:

- Convert nums to [-1, 1, -1].
- Maintain a prefix_sum and check:
   - At index 0: prefix_sum = -1, store it in the hash map.
   - At index 1: prefix_sum = 0, subarray from index 0 to 1 has sum 0.
   - At index 2: prefix_sum = -1, found the same sum at index 0, so the subarray from index 1 to 2 has sum 0.

Steps:
1. Initialize prefix_sum as 0 and store prefix_sum = 0 at index -1 in the hash map.
2. Traverse through the array, updating prefix_sum by adding -1 for 0 and 1 for 1.
3. Check if the current prefix_sum has been seen before:
   - If yes, calculate the subarray length from the previous index to the current index and update the maximum length.
   - If no, store the current prefix_sum and index in the hash map.
##### Time Complexity:
O(n), where n is the length of the array. We traverse the array once.
##### Space Complexity:
O(n), because we use a hash map to store the prefix sums.
##### Python Code:
```python
def findMaxLength(nums: List[int]) -> int:
    # Convert 0 to -1 and then find subarray with sum 0
    sum_index = {0: -1}  # Store the first occurrence of each prefix_sum
    prefix_sum = 0
    max_len = 0
    
    for i, num in enumerate(nums):
        # Convert 0 to -1
        if num == 0:
            prefix_sum += -1
        else:
            prefix_sum += 1
        
        if prefix_sum in sum_index:
            # If the prefix_sum was seen before, calculate subarray length
            max_len = max(max_len, i - sum_index[prefix_sum])
        else:
            # Store the index of the first occurrence of this prefix_sum
            sum_index[prefix_sum] = i
    
    return max_len
```
### Approach 3: Optimized Prefix Sum with Early Exit
##### Intuition: 
This is an optimization over the previous approach, where we exit early if we find the maximum possible subarray (i.e., the array itself or the longest subarray so far). This approach doesn't improve time complexity in the worst case but can be useful in certain cases.

1. As we build the prefix_sum, we track the length of the longest valid subarray.
2. If the longest subarray already equals half of the current length, we can exit early.
##### Time Complexity:
O(n), since we still traverse the array once.
##### Space Complexity:
O(n), due to the hash map.
##### Python Code:
```python
def findMaxLength(nums: List[int]) -> int:
    sum_index = {0: -1}
    prefix_sum = 0
    max_len = 0
    
    for i, num in enumerate(nums):
        prefix_sum += -1 if num == 0 else 1
        
        if prefix_sum in sum_index:
            max_len = max(max_len, i - sum_index[prefix_sum])
            # Early exit if the maximum possible subarray is found
            if max_len == i + 1:
                break
        else:
            sum_index[prefix_sum] = i
    
    return max_len
```
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force                    | O(n^2)      | O(1)             |
| Prefix Sum with Hash Map                          | O(n)            | O(n)             |
| Optimized Prefix Sum with Early Exit      | O(n)      | O(n)             |

The Prefix Sum with Hash Map approach is the most efficient solution as it allows finding the maximum length subarray in linear time using constant updates to the prefix sum and hash map.