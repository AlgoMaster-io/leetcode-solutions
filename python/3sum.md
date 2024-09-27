# 3Sum
Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

### Constraints:
0 <= nums.length <= 3000
-10^5 <= nums[i] <= 10^5

### Examples
```javascript
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = -1 + 0 + 1 = 0
nums[1] + nums[2] + nums[3] = -1 + 0 + 1 = 0
There are two unique triplets that sum to zero.

Input: nums = []
Output: []

Input: nums = [0]
Output: []
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
The brute-force solution involves checking every possible triplet in the array. We would use three nested loops to pick all combinations of three numbers and check if their sum equals zero.

Steps:
1. Loop through the array with three nested loops to check all possible combinations.
2. For each triplet, check if the sum equals zero.
3. Use a set to keep track of unique triplets to avoid duplicates.
##### Time Complexity:
O(n³), where n is the length of the array. This approach checks all possible triplets.
##### Space Complexity:
O(n) for storing unique triplets in a set.
##### Python Code:
```python
def threeSum(nums: List[int]) -> List[List[int]]:
    res = set()
    n = len(nums)
    
    for i in range(n):
        for j in range(i + 1, n):
            for k in range(j + 1, n):
                if nums[i] + nums[j] + nums[k] == 0:
                    res.add(tuple(sorted([nums[i], nums[j], nums[k]])))
                    
    return list(res)
```
### Approach 2: Sorting and Two Pointers (Optimal Solution)
##### Intuition: 
By sorting the array first, we can reduce the problem to the two-sum problem with a twist. The idea is to fix one number (nums[i]), and then use two pointers to find the other two numbers (nums[left] and nums[right]) whose sum is -nums[i].

Steps:
1. Sort the array: Sorting helps in skipping duplicate triplets and allows the two-pointer technique to work efficiently.
2. Iterate through the array: For each number nums[i], treat it as a fixed number.
3. Two-pointer approach: After fixing nums[i], use two pointers (left and right) to find two other numbers such that their sum equals -nums[i].
4. Skip duplicates: While iterating, skip duplicate elements to avoid repeating the same triplets.
##### Visualization:
```perl
For nums = [-1, 0, 1, 2, -1, -4], after sorting the array:

Sorted array: [-4, -1, -1, 0, 1, 2]

Iteration 1:
Fix nums[0] = -4
left = 1, right = 5
Sum = nums[1] + nums[5] = -1 + 2 = 1 > 0 → move right left

Iteration 2:
Fix nums[1] = -1
left = 2, right = 5
Sum = nums[2] + nums[5] = -1 + 2 = 1 > 0 → move right left
Sum = nums[2] + nums[4] = -1 + 1 = 0 → triplet found: [-1, -1, 2]

Iteration 3:
Fix nums[2] = -1 (skip since it’s a duplicate of nums[1])
...

Continue until all triplets are checked.
```
##### Time Complexity:
O(n²), where n is the length of the array. Sorting the array takes O(n log n), and the two-pointer scan for each element takes O(n).
##### Space Complexity:
O(n) for the output list of triplets.
##### Python Code:
```python
def threeSum(nums: List[int]) -> List[List[int]]:
    res = []
    nums.sort()  # Step 1: Sort the array
    
    for i in range(len(nums) - 2):
        # Step 2: Skip duplicates for the current number
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        
        # Step 3: Two-pointer approach
        left, right = i + 1, len(nums) - 1
        while left < right:
            total = nums[i] + nums[left] + nums[right]
            
            if total == 0:
                res.append([nums[i], nums[left], nums[right]])
                left += 1
                right -= 1
                # Skip duplicates for the left and right pointers
                while left < right and nums[left] == nums[left - 1]:
                    left += 1
                while left < right and nums[right] == nums[right + 1]:
                    right -= 1
            elif total < 0:
                left += 1  # Increase the sum by moving left pointer right
            else:
                right -= 1  # Decrease the sum by moving right pointer left
    
    return res
```
### Approach 3: Hashing (Alternative Approach)
##### Intuition: 
Another approach is to use a hash set to store the values we've already seen. For each pair of numbers, calculate the third number needed to make the sum zero and check if it exists in the hash set.

Steps:
1. Sort the array: Sorting helps in identifying and skipping duplicates.
2. Iterate through the array: For each number, use a hash set to check if the complement of the sum exists.
3. Check for duplicates: Ensure the result contains only unique triplets by checking for duplicates.
##### Time Complexity:
O(n²), as we iterate over the array and use hashing for each element.
##### Space Complexity:
O(n), for storing the hash set.
##### Python Code:
```python
def threeSum(nums: List[int]) -> List[List[int]]:
    nums.sort()
    res = []
    
    for i in range(len(nums)):
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        
        seen = set()
        j = i + 1
        while j < len(nums):
            complement = -nums[i] - nums[j]
            if complement in seen:
                res.append([nums[i], nums[j], complement])
                while j + 1 < len(nums) and nums[j] == nums[j + 1]:
                    j += 1
            seen.add(nums[j])
            j += 1
    
    return res
```
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force	                    | O(n²)      | O(n)             |
| Sorting + Two Pointers (Optimal)	                          | O(n²)            | O(n)             |
| Hashing with Set	                          | O(n²)            | 	O(n)             |

The Sorting + Two Pointers approach is the most efficient and optimal solution. It leverages sorting and the two-pointer technique to solve the problem in O(n²) time and handles duplicate triplets efficiently.