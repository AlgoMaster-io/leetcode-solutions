# Next Greater Element I
You are given two arrays nums1 and nums2 where nums1 is a subset of nums2. For each element in nums1, find the next greater element in nums2. The Next Greater Element of a number x in nums1 is the first greater number to its right in nums2. If it does not exist, return -1 for this number.

### Constraints:
- 1 <= nums1.length <= 1000
- 1 <= nums2.length <= 1000
- 0 <= nums1[i], nums2[i] <= 10^4
- All elements in nums1 and nums2 are unique.
- All the elements of nums1 also appear in nums2.

### Examples
```javascript
Input: nums1 = [4,1,2], nums2 = [1,3,4,2]
Output: [-1,3,-1]
Explanation:
For 4, there is no next greater number in nums2.
For 1, the next greater number is 3.
For 2, there is no next greater number.

Input: nums1 = [2,4], nums2 = [1,2,3,4]
Output: [3,-1]
Explanation:
For 2, the next greater number is 3.
For 4, there is no next greater number.
```

## Approaches to Solve the Problem
### Approach 1: Brute Force
##### Intuition:
The brute-force solution involves iterating through each element in nums1 and searching for the next greater element in nums2. For each element in nums1, we find its position in nums2, then scan to the right to find the next greater element.

Steps:
1. For each element in nums1, find its position in nums2.
2. Start from that position in nums2 and search to the right for the next greater element.
3. If no greater element is found, return -1 for that element.
##### Time Complexity:
O(m * n), where m is the length of nums1 and n is the length of nums2. For each element in nums1, we may have to search through all of nums2.
##### Space Complexity:
O(1), as we only need a few variables to track indices and the current next greater element.
##### Python Code:
```python
def nextGreaterElement(nums1, nums2):
    result = []
    
    for num in nums1:
        found = False
        for i in range(len(nums2)):
            if nums2[i] == num:
                for j in range(i + 1, len(nums2)):
                    if nums2[j] > num:
                        result.append(nums2[j])
                        found = True
                        break
                if not found:
                    result.append(-1)
                break
                
    return result
```

### Approach 2: Stack with Hash Map (Optimal)
##### Intuition: 
The brute-force approach is inefficient because it searches nums2 for the next greater element repeatedly. A more efficient approach is to use a monotonic stack and a hash map. The stack helps us keep track of elements in nums2 for which we haven't yet found the next greater element, and the hash map stores the next greater element for each number.

1. Traverse nums2 from right to left.
2. Use the stack to find the next greater element for each number:
   - If the stack is non-empty and the top of the stack is less than or equal to the current number, pop the stack.
   - The current number's next greater element is at the top of the stack.
Push the current number onto the stack.
3. After processing nums2, use the hash map to quickly find the next greater element for each number in nums1.

Steps:
1. Traverse nums2 from right to left.
2. Use a stack to maintain the next greater element candidates.
3. Use a hash map to store the next greater element for each number in nums2.
4. For each element in nums1, look up the next greater element from the hash map.
### Visualization
For nums2 = [1, 3, 4, 2]:
```rust
Stack process:
Start from the rightmost element: 2 → push 2 onto the stack.
Move to 4 → no elements in the stack are greater than 4 → push 4 onto the stack.
Move to 3 → next greater element is 4 → push 3 onto the stack.
Move to 1 → next greater element is 3 → push 1 onto the stack.

Hash map after processing: {2: -1, 4: -1, 3: 4, 1: 3}
```
##### Time Complexity:
O(m + n), where m is the length of nums1 and n is the length of nums2. We traverse nums2 once and process nums1 in constant time using the hash map.
##### Space Complexity:
O(n), where n is the length of nums2. The stack and hash map both use space proportional to the size of nums2.
##### Python Code:
```python
def nextGreaterElement(nums1, nums2):
    # Dictionary to store the next greater element for each number in nums2
    next_greater = {}
    stack = []
    
    # Traverse nums2 from right to left
    for num in reversed(nums2):
        # Pop elements from the stack until we find a greater element or the stack is empty
        while stack and stack[-1] <= num:
            stack.pop()
        
        # If the stack is not empty, the top of the stack is the next greater element
        next_greater[num] = stack[-1] if stack else -1
        
        # Push the current number onto the stack
        stack.append(num)
    
    # Use the dictionary to find the next greater element for each number in nums1
    return [next_greater[num] for num in nums1]
```
### Edge Cases:
1. Empty Input: If either nums1 or nums2 is empty, the result should also be an empty list.
2. No Greater Element: If a number in nums2 does not have a greater element to its right, the result should be -1 for that number.
3. Single Element: If nums1 or nums2 has a single element, the result will either be -1 or the only element itself.
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force                                | O(m * n)      | O(1)             |
| Stack with Hash Map (Optimal)                          | O(m + n)            | O(n)             |

The Stack with Hash Map approach is the most efficient and optimal solution. It uses a monotonic stack to keep track of elements that are waiting for their next greater element and processes the input arrays in linear time.