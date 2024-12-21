## [Leetcode Problem 974: Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

### Approaches
- [Brute Force Approach](#brute-force-approach)
- [Prefix Sum with Modulo Count](#prefix-sum-with-modulo-count)

---

### Brute Force Approach

#### Intuition
A naive approach is to consider all possible subarrays and check if their sum is divisible by `K`. This approach is straightforward but inefficient because it involves calculating the sum of each subarray repeatedly.

#### Approach
1. Use a nested loop to iterate through all possible subarrays.
2. For each subarray, calculate the sum and check if it is divisible by `K`.
3. Count the number of subarrays whose sum is divisible by `K`.

#### Code

```python
def subarraysDivByK(nums, K):
    count = 0
    n = len(nums)
    
    # Outer loop for the starting point of subarray
    for start in range(n):
        subarray_sum = 0
        # Inner loop for the ending point of subarray
        for end in range(start, n):
            subarray_sum += nums[end]  # calculate sum for each subarray
            if subarray_sum % K == 0:  # check divisibility by K
                count += 1  # Increment count if divisible by K
                
    return count
```

#### Complexity
- **Time Complexity**: \(O(N^2)\) where \(N\) is the length of `nums`. This is due to the nested loops iterating over potential subarray start and end indices.
- **Space Complexity**: \(O(1)\) as we only use a constant amount of extra space.

---

### Prefix Sum with Modulo Count

#### Intuition
Instead of recalculating the sum for every subarray, we utilize prefix sums. The key observation is that if two subarrays have the same modulo when divided by `K`, then the sum of the elements between these two subarrays is divisible by `K`. To achieve this, we maintain a running total of the prefix sums and use a dictionary to keep track of how many times each modulo value has appeared.

#### Approach
1. Calculate a running prefix sum for the array.
2. Compute the modulo of the current prefix sum with `K`.
3. Use a dictionary to maintain counts of how many times each modulo value has been seen so far.
4. If a particular modulo value is seen again, it means there is a subarray (end at current index and start at some previous index) that could be formed with this modulo that makes the sum divisible by `K`.
5. Sum up the count of these subarrays.

#### Code

```python
def subarraysDivByK(nums, K):
    prefix_sum = 0
    count = 0
    modulo_count = {0: 1}  # There is one way to have a sum divisible by K without using any elements
    
    for num in nums:
        prefix_sum += num  # Update the prefix sum with the current number
        # Calculate modulo to find the equivalent position in circular array formed by K
        modulo = (prefix_sum % K + K) % K  # Adjust for negative numbers for correct modulo
        
        if modulo in modulo_count:
            count += modulo_count[modulo]  # If this modulo was seen before, add those counts to result
        
        # Update the count of this specific modulo in dictionary
        modulo_count[modulo] = modulo_count.get(modulo, 0) + 1
        
    return count
```

#### Complexity
- **Time Complexity**: \(O(N)\) where \(N\) is the length of `nums`. We iterate through the list once.
- **Space Complexity**: \(O(K)\) for storing the counts of each possible modulo value, which is the max number of distinct keys the dictionary can have.

This prefix sum with modulo approach is optimal both in time and space compared to the naive brute force method.


