# [LeetCode 1512: Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Approaches
- [Brute Force Solution](#brute-force-solution)
- [Optimized Solution Using a Dictionary](#optimized-solution-using-a-dictionary)

### Brute Force Solution

The simplest approach is to use a double loop to compare each pair of indices `i` and `j`. If `nums[i]` is equal to `nums[j]` and `i` is less than `j`, we have a good pair. This approach checks all possible pairs and is straightforward to implement.

#### Intuition:
- We iterate over the list with two nested loops to consider each pair `(i, j)`.
- We check if the pair is a good pair (i.e., `nums[i] == nums[j]` and `i < j`).
- Count the number of such good pairs.

```python
def numIdenticalPairs(nums):
    good_pairs = 0
    n = len(nums)
    
    # Iterate over each possible pair (i, j)
    for i in range(n):
        for j in range(i + 1, n):
            # Check if it's a good pair
            if nums[i] == nums[j]:
                good_pairs += 1
                
    return good_pairs

# Time Complexity: O(n^2) - As we are using nested loops to evaluate each pair
# Space Complexity: O(1) - No additional data structures are used
```

### Optimized Solution Using a Dictionary

To improve efficiency, we can count the occurrences of each number in the list using a dictionary. For each identical number, say that it appears `k` times, the number of good pairs is given by `k * (k - 1) / 2`, which is the combination formula for choosing 2 elements from `k`.

#### Intuition:
- Use a dictionary to track the occurrence of each element.
- For each new occurrence of an element, update the number of good pairs because each new occurrence of an element can form a new good pair with all previous instances of that element.

```python
def numIdenticalPairs(nums):
    from collections import defaultdict
    
    count = defaultdict(int)  # To store the frequency of each number
    good_pairs = 0
    
    for num in nums:
        # Additional good pairs formed is equal to the count of num
        good_pairs += count[num]
        # Increment the count of this number as it's seen one more time
        count[num] += 1
        
    return good_pairs

# Time Complexity: O(n) - We only iterate once through the list
# Space Complexity: O(n) - In the worst case, we store a count for each unique number in the list
```

In summary, while the brute force solution checks all pairs and is straightforward, the optimized dictionary-based approach leverages counting to efficiently calculate the number of good pairs, significantly reducing the time required for execution.

