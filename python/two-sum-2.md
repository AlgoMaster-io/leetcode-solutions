# Two Sum II - Input Array Is Sorted
Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be numbers[index1] and numbers[index2] where 1 <= index1 < index2 <= numbers.length.

Return the indices of the two numbers, index1 and index2, added by one as index1 + 1 and index2 + 1.

### Constraints:
- 2 <= numbers.length <= 3 * 10^4
- -1000 <= numbers[i] <= 1000
- numbers is sorted in non-decreasing order.
- -1000 <= target <= 1000
- The tests are generated such that there is exactly one solution.

### Examples
```javascript
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2.

Input: numbers = [2,3,4], target = 6
Output: [1,3]

Input: numbers = [-1,0], target = -1
Output: [1,2]
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
The brute-force approach involves trying every possible pair of numbers and checking if their sum equals the target. This is a basic approach and works but is inefficient for large arrays.

Steps:
1. Loop over each number in the array.
2. For each number, loop over the rest of the array to find a second number that adds up to the target.
3. If such a pair is found, return their indices.
##### Time Complexity:
O(n²), where n is the length of the array. Each pair of numbers is checked.
##### Space Complexity:
O(1), no additional space is used beyond variables for storing indices.
##### Python Code:
```python
def twoSum(numbers: List[int], target: int) -> List[int]:
    n = len(numbers)
    for i in range(n):
        for j in range(i + 1, n):
            if numbers[i] + numbers[j] == target:
                return [i + 1, j + 1]  # 1-indexed
```
### Approach 2: Two Pointers (Optimal Solution)
##### Intuition: 
Since the array is sorted, we can use the two-pointer technique to optimize the solution. The two pointers, left and right, start at the beginning and end of the array, respectively. Depending on the sum of the values at these pointers, we can adjust them:

- If the sum of the numbers at left and right is less than the target, increment the left pointer to increase the sum.
- If the sum is greater than the target, decrement the right pointer to decrease the sum.
- If the sum equals the target, return the current indices (adjusted for 1-indexing).

Steps:
1. Initialize two pointers, left at the start and right at the end of the array.
2. Loop while left is less than right.
3. Calculate the sum of the numbers at left and right.
4. If the sum equals the target, return the indices left + 1 and right + 1.
5. If the sum is less than the target, move the left pointer to the right.
6. If the sum is greater than the target, move the right pointer to the left.
##### Visualization:
```perl
For numbers = [2, 7, 11, 15] and target = 9:

Initial pointers:
left = 0 (points to 2)
right = 3 (points to 15)

Iteration 1:
Sum = 2 + 15 = 17, which is greater than the target.
Move right pointer left.

Iteration 2:
left = 0 (points to 2)
right = 2 (points to 11)
Sum = 2 + 11 = 13, which is still greater than the target.
Move right pointer left again.

Iteration 3:
left = 0 (points to 2)
right = 1 (points to 7)
Sum = 2 + 7 = 9, which matches the target.
Return indices [1, 2].
```
##### Time Complexity:
O(n), where n is the length of the array. We traverse the array once with the two pointers.
##### Space Complexity:
O(1), as we use only two pointers and no additional data structures.
##### Python Code:
```python
def twoSum(numbers: List[int], target: int) -> List[int]:
    left, right = 0, len(numbers) - 1
    
    while left < right:
        current_sum = numbers[left] + numbers[right]
        
        if current_sum == target:
            return [left + 1, right + 1]  # Return 1-indexed positions
        elif current_sum < target:
            left += 1  # Move left pointer to increase sum
        else:
            right -= 1  # Move right pointer to decrease sum
```
### Approach 3: Binary Search (Alternative Approach)
##### Intuition: 
Since the array is sorted, we can also use binary search. For each element in the array, we calculate the complement needed to reach the target (target - numbers[i]). Then, we perform a binary search for the complement in the remainder of the array (i.e., starting from the next element).

Steps:
1. Loop over each element in the array.
2. For each element numbers[i], calculate the complement target - numbers[i].
3. Perform a binary search for the complement in the subarray starting from i + 1.
4. If found, return the indices i + 1 and the index of the complement.
##### Visualization:
```perl
For numbers = [2, 7, 11, 15] and target = 9:

Initial pointers:
left = 0 (points to 2)
right = 3 (points to 15)

Iteration 1:
Sum = 2 + 15 = 17, which is greater than the target.
Move right pointer left.

Iteration 2:
left = 0 (points to 2)
right = 2 (points to 11)
Sum = 2 + 11 = 13, which is still greater than the target.
Move right pointer left again.

Iteration 3:
left = 0 (points to 2)
right = 1 (points to 7)
Sum = 2 + 7 = 9, which matches the target.
Return indices [1, 2].
```
##### Time Complexity:
O(n log n), where n is the length of the array. The binary search runs in logarithmic time, and we perform it for each element in the array.
##### Space Complexity:
O(1), as we only use variables for storing indices.
##### Python Code:
```python
def twoSum(numbers: List[int], target: int) -> List[int]:
    for i in range(len(numbers)):
        complement = target - numbers[i]
        left, right = i + 1, len(numbers) - 1
        
        # Binary search for the complement
        while left <= right:
            mid = (left + right) // 2
            if numbers[mid] == complement:
                return [i + 1, mid + 1]
            elif numbers[mid] < complement:
                left = mid + 1
            else:
                right = mid - 1
```
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force	                    | O(n²)      | O(1)             |
| Two Pointers	                          | O(n)            | O(1)             |
| Binary Search	                          | O(n logn)            | O(1)             |

The Two Pointers approach is the most efficient and optimal solution since it only requires a single pass over the array and uses constant space.