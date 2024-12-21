### [Leetcode Problem 523: Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

#### Approaches:
1. [Brute Force Solution](#brute-force-solution)
2. [Prefix Sum with HashMap](#prefix-sum-with-hashmap)

---

### Brute Force Solution

**Intuition**:  
The simplest way to approach this problem is to check every possible subarray within the array and see if their sum is a multiple of `k`. This involves calculating the sum from each starting point to all possible endpoints and checking if it meets the condition.

```python
def checkSubarraySum(nums, k):
    n = len(nums)
    # Iterate over each possible starting point of the subarray
    for start in range(n):
        current_sum = 0
        # For each starting point, compute the sum from start to every endpoint
        for end in range(start, n):
            current_sum += nums[end]
            # If the length of the subarray is at least 2 and the sum is a multiple of k
            if end - start >= 1 and current_sum % k == 0:
                return True
    return False

# Example usage
# print(checkSubarraySum([23, 2, 4, 6, 7], 6))  # Output: True
```

**Time Complexity**:  
- O(n^2), where n is the number of elements in the array. This is because for each element, we're checking subarrays ending in each subsequent element.

**Space Complexity**:  
- O(1), as no extra space is used besides the input and a few variables.

---

### Prefix Sum with HashMap

**Intuition**:  
To optimize, we utilize the property of modular arithmetic. If `sum[i] % k == sum[j] % k` for any `i < j`, the sum of the elements between `i` and `j` is a multiple of `k`. We use a hashmap to store the remainder of the prefix sum modulo `k` and the earliest index it was seen at. This approach ensures that we can find two such indices in linear time.

```python
def checkSubarraySum(nums, k):
    # Dictionary to store the first occurrence of prefix sum modulo k
    prefix_sum_mod_k = {0: -1}  # Accounts for the whole prefix being a multiple of k

    current_sum = 0
    
    for i in range(len(nums)):
        current_sum += nums[i]
        # Compute the mod of the current prefix sum
        mod = current_sum % k
        
        # If this mod has been seen before, check the length of the subarray
        if mod in prefix_sum_mod_k:
            if i - prefix_sum_mod_k[mod] > 1:
                return True
        else:
            # Record the first occurrence of this mod value
            prefix_sum_mod_k[mod] = i
            
    return False

# Example usage
# print(checkSubarraySum([23, 2, 4, 6, 7], 6))  # Output: True
```

**Time Complexity**:  
- O(n), where n is the number of elements in the array. We traverse the array once, maintaining the hashmap.

**Space Complexity**:  
- O(min(n, k)), in the worst case, the hashmap could contain as many keys as there are unique remainders, which is bounded by the minimum of n and k.

---

