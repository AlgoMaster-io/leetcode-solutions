# [Leetcode 35: Search Insert Position](https://leetcode.com/problems/search-insert-position/)

## Approaches
1. [Linear Search](#linear-search)
2. [Binary Search](#binary-search)

---

### Linear Search

**Intuition**:
The simplest approach is to iterate through the list and find the position where the target needs to be inserted to maintain the sorted order. This involves checking each element one-by-one until you find a suitable position.

**Approach**:
- Start from the beginning of the array and move through each element.
- For each element, compare it with the target.
- If the current element is greater than or equal to the target, the target should be inserted at this position.
- If no appropriate position is found by the end of the array, the target should be inserted at the end.

```csharp
public class Solution {
    public int SearchInsert(int[] nums, int target) {
        // Iterate through each element in the array
        for (int i = 0; i < nums.Length; i++) {
            // Check if the current element is greater than or equal to the target
            if (nums[i] >= target) {
                // Return the current index as the insert position
                return i;
            }
        }
        // If no position is found, return the length of the array (insert at the end)
        return nums.Length;
    }
}
```

**Time Complexity**: O(n) - In the worst case, you might have to traverse the entire array.

**Space Complexity**: O(1) - Only a constant amount of space is used.


### Binary Search

**Intuition**:
For a sorted array, a more efficient way is to utilize the binary search method, which can reduce the number of comparisons significantly. This method requires repeatedly dividing the array in half and determining which half the target would lie in, ultimately narrowing down the possible position.

**Approach**:
- Initialize two pointers, `left` and `right`, to the start and end of the array, respectively.
- While the `left` pointer does not surpass the `right` pointer:
  - Calculate the middle index `mid`.
  - If the element at `mid` is the target, return `mid`.
  - If the element at `mid` is less than the target, narrow the search to the right half by updating `left` to `mid + 1`.
  - Otherwise, narrow to the left half by updating `right` to `mid - 1`.
- If the loop ends, `left` will be the position to insert the target.

```csharp
public class Solution {
    public int SearchInsert(int[] nums, int target) {
        int left = 0, right = nums.Length - 1;
        
        // Continue searching while the left index does not surpass the right
        while (left <= right) {
            // Calculate middle index
            int mid = left + (right - left) / 2;
            
            // Check if the middle element is the target
            if (nums[mid] == target) {
                return mid;
            } 
            // If target is greater, ignore left half by adjusting left
            else if (nums[mid] < target) {
                left = mid + 1;
            } 
            // If target is smaller, ignore right half by adjusting right
            else {
                right = mid - 1;
            }
        }
        
        // If no position was returned, left is the insert position
        return left;
    }
}
```

**Time Complexity**: O(log n) - The use of binary search reduces the number of comparisons logarithmically relative to the input size.

**Space Complexity**: O(1) - Only a constant amount of space is used.

