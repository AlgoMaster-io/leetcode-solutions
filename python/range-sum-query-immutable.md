# Range Sum Query - Immutable
Given an integer array nums, handle multiple queries of the following type:

Calculate the sum of the elements of nums between indices left and right inclusive, where left <= right.
Implement the NumArray class:

NumArray(int[] nums) Initializes the object with the integer array nums.
int sumRange(int left, int right) Returns the sum of the elements of nums between indices left and right inclusive (i.e., nums[left] + nums[left + 1] + ... + nums[right]).

### Constraints:
- 1 <= nums.length <= 10^4
- -10^5 <= nums[i] <= 10^5
- 0 <= left <= right < nums.length
- At most 10^4 calls will be made to sumRange.

### Examples
```javascript
Input:
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]

Output:
[null, 1, -1, -3]

Explanation:
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return 1 (-2 + 0 + 3 = 1)
numArray.sumRange(2, 5); // return -1 (3 + -5 + 2 + -1 = -1)
numArray.sumRange(0, 5); // return -3 (-2 + 0 + 3 + -5 + 2 + -1 = -3)
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Iterate Over Range)
##### Intuition:
The most straightforward approach is to iterate over the elements between indices left and right every time sumRange is called. This solution will work, but it may not be efficient if sumRange is called multiple times.

Steps:
1. Each time sumRange(left, right) is called, iterate over the elements from left to right and sum them.
2. Return the sum after iterating over the range.
##### Time Complexity:
O(n) for each query, where n is the length of the range between left and right.
##### Space Complexity:
O(1), no extra space is used beyond a few variables for the sum.
##### Python Code:
```python
class NumArray:

    def __init__(self, nums: List[int]):
        self.nums = nums  # Store the input array
    
    def sumRange(self, left: int, right: int) -> int:
        # Calculate the sum by iterating over the range
        return sum(self.nums[left:right + 1])
```
### Approach 2: Prefix Sum (Preprocessing)
##### Intuition: 
To optimize the sum range queries, we can use the concept of prefix sums. The idea is to preprocess the array and compute a cumulative sum array where each element at index i stores the sum of elements from the start of the array up to i. This allows us to compute any range sum in constant time.

- Define prefix_sum[i] as the sum of elements nums[0] + nums[1] + ... + nums[i-1].
- To find the sum of the range [left, right], we can calculate:
```python
sumRange(left, right) = prefix_sum[right + 1] - prefix_sum[left]
```

Steps:

1. Precompute the cumulative sum array prefix_sum where each entry stores the sum of all elements before index i.
2. Use this precomputed array to quickly answer range sum queries by subtracting two prefix sums.

##### Why This Works:
If a cycle exists, the fast pointer, moving twice as fast as the slow pointer, will "lap" the slow pointer and meet it again inside the cycle.
##### Time Complexity:
- O(n) for preprocessing the prefix sum array.
- O(1) for each query, since we only perform subtraction of two values.
##### Space Complexity:
O(n), for storing the prefix sum array.
##### Visualization:
```rust
For nums = [-2, 0, 3, -5, 2, -1], the prefix_sum array would be:
prefix_sum = [0, -2, -2, 1, -4, -2, -3]

To compute sumRange(0, 2), we do:
sumRange(0, 2) = prefix_sum[3] - prefix_sum[0] = 1 - 0 = 1
```
##### Python Code:
```python
class NumArray:

    def __init__(self, nums: List[int]):
        # Create a prefix sum array
        self.prefix_sum = [0] * (len(nums) + 1)
        for i in range(1, len(self.prefix_sum)):
            self.prefix_sum[i] = self.prefix_sum[i - 1] + nums[i - 1]

    def sumRange(self, left: int, right: int) -> int:
        # Calculate the sum for the range using the prefix sum
        return self.prefix_sum[right + 1] - self.prefix_sum[left]
```
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force (Iterate Over Range)	   | O(n) per query            | O(1)             |
| Prefix Sum (Preprocessing)        | O(n) to preprocess, O(1) per query	            | O(n) for storing prefix sum             |

The Prefix Sum approach is the most optimal solution as it allows querying the sum of any range in constant time after an initial preprocessing step.