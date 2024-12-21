## [Three Sum - LeetCode](https://leetcode.com/problems/3sum/)

### Approaches:

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sorting and Two-Pointer Technique](#approach-2-sorting-and-two-pointer-technique)

### Approach 1: Brute Force

**Intuition**:
A straightforward approach is to generate all possible triplets in the array and check if their sum equals zero. This means iterating over every possible combination of three numbers.

1. Loop over the array with three nested loops to select each triplet of numbers.
2. Check if the sum of each triplet is zero.
3. Store the unique triplets in a results list.

**Time Complexity**:  
O(n^3), where n is the number of elements in the array. The time complexity comes from having three nested loops.

**Space Complexity**:  
O(m), where m is the number of unique triplets that sum to zero. This is needed for storing the result list.

```python
def threeSum(nums):
    nums.sort()  # Sort the array
    triplets = set()  # Use a set to store unique triplets

    # Iterate over each triplet
    for i in range(len(nums) - 2):
        for j in range(i + 1, len(nums) - 1):
            for k in range(j + 1, len(nums)):
                # If the sum of the triplet is zero, add it to the set
                if nums[i] + nums[j] + nums[k] == 0:
                    triplet = (nums[i], nums[j], nums[k])
                    triplets.add(triplet)

    return list(triplets)
```

### Approach 2: Sorting and Two-Pointer Technique

**Intuition**:
A more efficient way is to sort the array and use the two-pointer technique. The idea is to fix one number and find the other two numbers using two pointers to minimize the sum calculation efficiently.

1. Sort the array to simplify the problem.
2. Iterate over the array, fixing one number and using two pointers for the remaining part of the array.
3. Move the left and right pointers accordingly, based on whether the sum of the triplet is less or more than zero.
4. Skip duplicate values to ensure the uniqueness of triplets.

**Time Complexity**:  
O(n^2), where n is the number of elements in the array. Sorting the array takes O(n log n), and the two-pointer technique takes O(n^2).

**Space Complexity**:  
O(m), where m is the number of unique triplets that sum to zero. This is needed for storing the result list.

```python
def threeSum(nums):
    nums.sort()  # Sort the array
    triplets = []  # Initialize a list to store unique triplets

    for i in range(len(nums) - 2):
        # Skip the same element to avoid duplicates
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        
        left, right = i + 1, len(nums) - 1
        while left < right:
            # Calculate the sum of the current triplet
            current_sum = nums[i] + nums[left] + nums[right]
            
            if current_sum == 0:
                # Triplet found, add to result list
                triplets.append([nums[i], nums[left], nums[right]])
                # Move both pointers past duplicates
                while left < right and nums[left] == nums[left + 1]:
                    left += 1
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                # Move pointers inward
                left += 1
                right -= 1
            elif current_sum < 0:
                # If the sum is less than zero, move the left pointer to increase the sum
                left += 1
            else:
                # If the sum is more than zero, move the right pointer to decrease the sum
                right -= 1

    return triplets
```

These solutions provide progressively better time efficiency while utilizing the straightforward techniques of brute force and the more efficient two-pointer approach. Use the second approach for optimal performance, especially on larger input sizes.

