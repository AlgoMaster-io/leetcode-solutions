# [Leetcode 164: Maximum Gap](https://leetcode.com/problems/maximum-gap/)

## Approaches:
1. [Brute Force - Sort and Compare](#approach-1-brute-force---sort-and-compare)
2. [Pigeonhole Principle - Bucket Sort](#approach-2-pigeonhole-principle---bucket-sort)


### Approach 1: Brute Force - Sort and Compare

Intuition:
- The naive solution is to sort the array and then compare the differences between consecutive numbers to find the maximum gap.
- Sorting the array will rearrange the numbers in ascending order, hence the largest difference can only be between successive elements.

```python
def maximum_gap(nums):
    # If the array is too small to form any gap, return 0
    if len(nums) < 2:
        return 0

    # Sort the array
    nums.sort()
    
    # Initialize max_gap to be the smallest possible value
    max_gap = 0
    
    # Iterate through the sorted array to find the maximum gap
    for i in range(1, len(nums)):
        # Calculate the gap between the current element and the previous one
        gap = nums[i] - nums[i - 1]
        # Update max_gap if the current gap is larger
        max_gap = max(max_gap, gap)

    return max_gap
```

- **Time Complexity**: O(n log n) due to the sorting step.
- **Space Complexity**: O(1) if the sort is in place, else it is O(n).


### Approach 2: Pigeonhole Principle - Bucket Sort

Intuition:
- Using the properties of bucket sort, we can achieve a linear time complexity.
- If there are n elements in the array, then the gaps will be distributed over n-1 intervals. Therefore, the maximum gap is at least `(max_value - min_value) / (n - 1)`.
- By creating buckets and grouping numbers, we avoid unnecessary computations by ensuring the maximum gap must span at least two consecutive buckets, rather than within one bucket.

Steps:
1. Identify the minimum (`min_val`) and maximum (`max_val`) values of the array.
2. Calculate the bucket size, where `bucket_size = max(1, (max_val - min_val) // (len(nums) - 1))`.
3. Create buckets to categorize the numbers based on calculated `bucket_size`.
4. Initialize each bucket to keep track of minimum and maximum in that range.
5. Iterate over the numbers and place them into appropriate buckets, updating the min and max within each bucket.
6. Calculate the maximum gap by iterating over the buckets and checking the gap between the max of one bucket and the min of the next occupied bucket.

```python
def maximum_gap(nums):
    if len(nums) < 2:
        return 0
        
    # Get the minimum and maximum element from the nums
    min_val, max_val = min(nums), max(nums)

    # Determine the maximum possible gap
    bucket_size = max(1, (max_val - min_val) // (len(nums) - 1))
    bucket_count = (max_val - min_val) // bucket_size + 1
    
    # Initialize the buckets
    buckets_min = [float('inf')] * bucket_count
    buckets_max = [float('-inf')] * bucket_count
    
    # Place each number in a bucket
    for num in nums:
        index = (num - min_val) // bucket_size
        buckets_min[index] = min(buckets_min[index], num)
        buckets_max[index] = max(buckets_max[index], num)
    
    # Calculate the maximum gap
    max_gap = 0
    previous_max = min_val
    
    for i in range(bucket_count):
        if buckets_min[i] == float('inf'):
            continue
        max_gap = max(max_gap, buckets_min[i] - previous_max)
        previous_max = buckets_max[i]
        
    return max_gap
```

- **Time Complexity**: O(n) because we iterate over the array and buckets in linear time.
- **Space Complexity**: O(n) due to the auxiliary space for the buckets.

