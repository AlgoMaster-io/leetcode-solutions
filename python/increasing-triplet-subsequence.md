# [Leetcode 334: Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)

## Approaches
- [Approach 1: Brute Force Solution](#approach-1-brute-force-solution)
- [Approach 2: Optimized Linear Scan Using Auxiliary Variables](#approach-2-optimized-linear-scan-using-auxiliary-variables)

## Approach 1: Brute Force Solution

### Intuition
The naive approach to solving this problem is to consider all possible triplets in the array and check if any of them are in strictly increasing order. This involves trying every triplet combination within the array.

### Steps
1. Use three nested loops to explore every triplet (i, j, k) where i < j < k.
2. Check if `nums[i] < nums[j] < nums[k]`.
3. If such a triplet is found, return `True`.
4. If no triplet is found by the end of the search, return `False`.

### Code
```python
def increasingTriplet(nums):
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):
            for k in range(j + 1, n):
                # Check if the triplet (i, j, k) is strictly increasing
                if nums[i] < nums[j] < nums[k]:
                    return True
    return False
```

### Complexity Analysis
- **Time Complexity:** \(O(n^3)\), where \(n\) is the number of elements in the array, due to the three nested loops.
- **Space Complexity:** \(O(1)\), as no extra space is used apart from loop variables.

## Approach 2: Optimized Linear Scan Using Auxiliary Variables

### Intuition
The goal is to find a subsequence of three increasing numbers in linear time. One path is to utilize two variables to track the smallest and second smallest numbers encountered so far as we iterate through the list. When we find a number greater than our second smallest, we've found our increasing triplet.

### Steps
1. Initialize two variables `first` and `second` to infinity. These will track the smallest and the second smallest numbers found.
2. Traverse the array:
   - If the current number is less than `first`, update `first` to this number.
   - Else if the number is less than `second` but greater than `first`, update `second`.
   - If we find a number greater than both `first` and `second`, return `True`.
3. If no such triplet is found, return `False`.

### Code
```python
def increasingTriplet(nums):
    first = second = float('inf')
    for num in nums:
        if num <= first:
            # Update the smallest number seen so far
            first = num
        elif num <= second:
            # Update the second smallest number that is greater than 'first'
            second = num
        else:
            # We have found a number greater than both first and second
            return True
    return False
```

### Complexity Analysis
- **Time Complexity:** \(O(n)\), where \(n\) is the number of elements in the array. We make a single pass through the array.
- **Space Complexity:** \(O(1)\), as we use a constant amount of extra space. 

Using auxiliary variables is a very efficient way to solve this problem by carefully tracking only what's necessary, leading to a very elegant solution.

