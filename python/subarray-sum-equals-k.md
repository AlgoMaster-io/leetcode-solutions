
# Subarray Sum Equals K
Given an array of integers nums and an integer k, return the total number of continuous subarrays whose sum equals to k.

### Constraints:
- 1 <= nums.length <= 2 * 10^4
- -1000 <= nums[i] <= 1000
- -10^7 <= k <= 10^7

### Examples
```javascript
Input: nums = [1,1,1], k = 2
Output: 2
Explanation: [1,1] and [1,1] are the two subarrays with a sum equal to 2.

Input: nums = [1,2,3], k = 3
Output: 2
Explanation: [1, 2] and [3] are the two subarrays with a sum equal to 3.
```

## Approaches to Solve the Problem
### Approach 1: Brute Force
##### Intuition:
A straightforward approach would be to iterate through every possible subarray in nums and check if the sum equals k. This solution works but is inefficient due to the repeated summation of subarray elements.

1. Use a nested loop to consider every possible subarray in the array.
2. For each subarray, calculate the sum.
3. If the sum equals k, increment the count of valid subarrays.
##### Time Complexity:
O(n²), where n is the length of the array because we consider all subarrays.
##### Space Complexity:
O(1), no extra space is used beyond the variables for sum and count.
##### Python Code:
```python
def subarraySum(nums: List[int], k: int) -> int:
    count = 0
    n = len(nums)
    
    for i in range(n):
        current_sum = 0
        for j in range(i, n):
            current_sum += nums[j]
            if current_sum == k:
                count += 1
                
    return count
```
### Approach 2: Prefix Sum with Hash Map
##### Intuition: 
The key idea in optimizing the brute force approach is to use prefix sums. A prefix sum is the cumulative sum of elements from the start of the array to any index i. If the difference between two prefix sums equals k, then the subarray between these two indices has a sum of k.

1. Define prefix_sum[i] as the cumulative sum of elements from the start of the array to index i.
2. To find the number of subarrays with sum k, check if there is a previous prefix sum such that: ```current_prefix_sum - previous_prefix_sum = k```
3. Rearranging this gives: ```previous_prefix_sum = current_prefix_sum - k```
4. Use a hash map to store the frequency of each prefix sum as we iterate through the array.

Steps:
1. Initialize a prefix_sum as 0 and a hash map prefix_sum_count to store how often each prefix sum occurs.
2. For each element in the array, update the prefix_sum.
3. Check if prefix_sum - k exists in the hash map. If it does, the count of valid subarrays is incremented by the frequency of this sum.
4. Add the current prefix_sum to the hash map.

##### Visualization:
For nums = [1, 1, 1], k = 2:

```perl
prefix_sum = 0 initially
Iterating through the array:
  At index 0, prefix_sum = 1, prefix_sum - k = 1 - 2 = -1 (not found in hash map)
  At index 1, prefix_sum = 2, prefix_sum - k = 2 - 2 = 0 (found in hash map, count += 1)
  At index 2, prefix_sum = 3, prefix_sum - k = 3 - 2 = 1 (found in hash map, count += 1)

Total count = 2
```
##### Time Complexity:
O(n), where n is the length of the array, since we traverse the array once and use hash map operations that are O(1) on average.
##### Space Complexity:
O(n), because we use a hash map to store prefix sums.
##### Python Code:
```python
def subarraySum(nums: List[int], k: int) -> int:
    prefix_sum_count = {0: 1}  # To handle the case when prefix_sum equals k
    prefix_sum = 0
    count = 0
    
    for num in nums:
        prefix_sum += num
        
        # Check if there is a previous prefix_sum such that prefix_sum - k exists
        if prefix_sum - k in prefix_sum_count:
            count += prefix_sum_count[prefix_sum - k]
        
        # Add the current prefix_sum to the map
        if prefix_sum in prefix_sum_count:
            prefix_sum_count[prefix_sum] += 1
        else:
            prefix_sum_count[prefix_sum] = 1
    
    return count
```
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force                    | O(n^2)      | O(1)             |
| Prefix Sum with Hash Map                          | O(n)            | O(n)             |

The Prefix Sum with Hash Map approach is the most efficient solution as it computes the number of valid subarrays in linear time, making use of cumulative sums and hash maps to store and check the required prefix sums.