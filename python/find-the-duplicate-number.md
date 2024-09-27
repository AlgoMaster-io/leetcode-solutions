
# Find the Duplicate Number
You are given an array of integers nums containing n + 1 integers where each integer is in the range [1, n] inclusive. There is exactly one duplicate number in nums, return this duplicate number.

You must solve the problem without modifying the array and use only constant extra space.

### Constraints:
- 1 <= n <= 10^5
- nums.length == n + 1
- 1 <= nums[i] <= n
- All the integers in nums appear only once except for exactly one integer which appears twice.

### Follow-up:
- How can we prove that at least one duplicate number must exist?
- Can you solve the problem in O(n) time and without modifying the array?

### Examples
```javascript
Input: nums = [1,3,4,2,2]
Output: 2

Input: nums = [3,1,3,4,2]
Output: 3
```

## Approaches to Solve the Problem
### Approach 1: Sort the Array (Not Optimal)
##### Intuition:
One of the simplest approaches is to sort the array. Once the array is sorted, the duplicate number will appear next to itself.

1. Sort the array.
2. Iterate through the sorted array, checking if any two consecutive elements are the same.
##### Time Complexity:
O(n log n), because sorting the array takes O(n log n).
##### Space Complexity:
O(1), if sorting is done in place, or O(n) if using additional memory for sorting.
##### Python Code:
```python
def findDuplicate(nums):
    nums.sort()  # Sort the array
    
    # Find the first pair of consecutive duplicate elements
    for i in range(1, len(nums)):
        if nums[i] == nums[i - 1]:
            return nums[i]
```
### Approach 2: Hash Set to Track Seen Numbers
##### Intuition: 
We can use a hash set to keep track of numbers we have already encountered. As we iterate through the array, if a number is found in the set, it is the duplicate.

1. Initialize an empty set.
2. Traverse the array and check if the number is already in the set.
3. If it is, return the duplicate number.
##### Time Complexity:
O(n), as we traverse the array once.
##### Space Complexity:
O(n), because we use a set to store the numbers we have seen.
##### Python Code:
```python
def findDuplicate(nums):
    seen = set()  # Set to track seen numbers
    
    for num in nums:
        if num in seen:
            return num  # Duplicate found
        seen.add(num)
```
### Approach 3: Binary Search on the Value Range
##### Intuition: 
The key insight is that the array contains numbers in the range [1, n]. This allows us to apply binary search on the range of values instead of the array itself. For each mid-point of the range, count how many numbers in the array are less than or equal to mid. If this count exceeds mid, then the duplicate must be in the lower half. Otherwise, it must be in the upper half.

1. Perform binary search on the range of numbers [1, n].
2. For each mid value, count how many numbers in the array are less than or equal to mid.
3. Based on the count, adjust the search range.
##### Visualization:
```rust
For example, with nums = [1, 3, 4, 2, 2], we perform the following:

- Initial range: 1 to 4 (n=4)
- Mid = 2. Count of numbers <= 2 is 3 (1, 2, 2).
  Since 3 > 2, the duplicate is in the lower half.
- Narrow down the range to 1 to 2.
- Mid = 1. Count of numbers <= 1 is 1.
  The duplicate must be in the upper half.
- Range becomes 2 to 2. Found duplicate = 2.
```
##### Time Complexity:
O(n log n), because we perform binary search on the range and for each mid-point, we do a linear scan of the array.
##### Space Complexity:
O(1), since no extra space is used except for a few variables.
##### Python Code:
```python
def findDuplicate(nums):
    left, right = 1, len(nums) - 1
    
    while left < right:
        mid = (left + right) // 2
        count = sum(num <= mid for num in nums)
        
        if count > mid:
            right = mid  # Duplicate is in the lower half
        else:
            left = mid + 1  # Duplicate is in the upper half
    
    return left  # The duplicate number
```
### Approach 4: Floyd’s Tortoise and Hare (Cycle Detection)
##### Intuition: 
This approach treats the problem as detecting a cycle in a linked list. Imagine the array as a linked list where each element points to another element in the array (the value at that index). The duplicate number creates a cycle. Using Floyd's Tortoise and Hare algorithm, we can detect this cycle.

1. Initialize two pointers, slow and fast.
2. Move slow one step at a time and fast two steps at a time.
3. If a cycle exists, slow and fast will meet at some point.
4. Reset one pointer to the start and move both pointers one step at a time until they meet again, which will be at the duplicate number.
##### Why This Works:
- The duplicate number creates a cycle because it points to a number already visited. The cycle detection method will find the duplicate as the entry point of the cycle.
##### Visualization:
```rust
Array: [1, 3, 4, 2, 2]

1 -> 3 -> 2 -> 4 -> 2 (cycle starts at 2)

Step 1:
- slow = 1, fast = 3

Step 2:
- slow = 3, fast = 4

Step 3:
- slow = 2, fast = 2 (they meet)

Reset slow to the start, then move both pointers one step at a time:
- slow = 1, fast = 2
- slow = 3, fast = 2
- slow = 2, fast = 2 (found duplicate = 2)
```
##### Time Complexity:
O(n), as we traverse the array once.
##### Space Complexity:
O(1), since we only use constant extra space.
##### Python Code:
```python
def findDuplicate(nums):
    slow = nums[0]
    fast = nums[0]
    
    # First phase: detect the cycle
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break
    
    # Second phase: find the entrance to the cycle
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    
    return slow  # The duplicate number
```
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Sort the Array                    | O(n log n)      | O(1)             |
| Hash Set                          | O(n)            | O(n)             |
| Binary Search on Value Range      | O(n log n)      | O(1)             |
| Floyd’s Tortoise and Hare         | O(n)            | O(1)             |

The Floyd’s Tortoise and Hare approach is the most optimal, providing O(n) time complexity and O(1) space complexity without modifying the array