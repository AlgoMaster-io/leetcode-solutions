# [LeetCode 219: Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

## Navigation
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Hash Map](#approach-2-hash-map)

---

## Approach 1: Brute Force

### Intuition:
The brute force method is to check all possible pairs of elements in the array to see if any two numbers are equal and their indices difference is at most `k`. Although straightforward, it is not efficient for large arrays as it checks combinations in a nested loop.

### Steps:
1. Iterate over each element in the array.
2. For each element, iterate over the subsequent elements up to `k` indices away.
3. Check if the two elements are the same and their positions satisfy the index constraint.
4. If such a pair is found, return `True`.
5. If no such pair is found after all iterations, return `False`.

```python
def containsNearbyDuplicate(nums, k):
    n = len(nums)
    # Iterate through each number in the array
    for i in range(n):
        # Iterate over the next k elements
        for j in range(i + 1, min(i + k + 1, n)):
            # Check for duplicates
            if nums[i] == nums[j]:
                return True
    return False
```

### Complexity Analysis:
- **Time Complexity:** O(n * k), where `n` is the length of the array and `k` is the maximum allowed index difference. The nested loop causes quadratic time complexity when `k` approaches `n`.
- **Space Complexity:** O(1), as no extra space is required beyond the input array. Only a couple of loop counters are used.

---

## Approach 2: Hash Map

### Intuition:
To improve efficiency, a hash map can be used to store the most recent index of each element previously encountered. This allows us to quickly verify if there is a duplicate within the allowed index range by checking if the stored index minus the current index is within the threshold `k`.

### Steps:
1. Use a dictionary to map each element to the last index it was encountered at.
2. Iterate through the list:
   - If the element has been seen before and the difference between the current index and the stored index is `<= k`, return `True`.
   - Otherwise, update the dictionary with the current index to keep track of its most recent occurrence.
3. If no such elements are found, return `False`.

```python
def containsNearbyDuplicate(nums, k):
    # Dictionary for tracking last seen index of numbers
    num_index_map = {}
    for index, number in enumerate(nums):
        # Check if the number is in the map and within allowed index difference
        if number in num_index_map and index - num_index_map[number] <= k:
            return True
        # Update the dictionary with current index of the number
        num_index_map[number] = index
    return False
```

### Complexity Analysis:
- **Time Complexity:** O(n), where `n` is the length of the array. Each lookup and update are performed in constant time due to the hash map.
- **Space Complexity:** O(n), in the worst case, we store every element in the hash map if all elements are unique.

