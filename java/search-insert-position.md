# [LeetCode Problem 35: Search Insert Position](https://leetcode.com/problems/search-insert-position/)

### Approaches:
1. [Linear Search](#approach-1-linear-search)
2. [Binary Search](#approach-2-binary-search)

---

## Approach 1: Linear Search

### Intuition:
A straightforward approach to solve this problem is to go through the array and find the target or the position where it can be inserted in order. This is done by iterating over the array and checking each element against the target.

### Steps:
1. Iterate through each element in the array.
2. If the current element is equal to the target, return its index.
3. If the current element is greater than the target, return the current index since that is where the target should be inserted.
4. If the loop completes without finding a position, it means the target is larger than all elements, so return the length of the array as the insertion point.

### Code:

```java
public int searchInsert(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        // If we find the target, return the index.
        if (nums[i] == target) {
            return i;
        }
        // If an element is greater than the target, return current index as insert position.
        if (nums[i] > target) {
            return i;
        }
    }
    // If the target is greater than all elements, it should be inserted at the end.
    return nums.length;
}
```

### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of elements in the array. This is because we may have to check each element in the worst case.
- **Space Complexity**: O(1), as there is no extra space required for this approach.

---

## Approach 2: Binary Search

### Intuition:
Since the array is sorted, we can use binary search to find the position for the target more efficiently. Binary search involves dividing the problem space in half during each iteration, making it much faster than a linear search.

### Steps:
1. Initialize two pointers: `left` at the start and `right` at the end of the array.
2. While `left` is less than or equal to `right`:
   - Calculate `mid` as the average of `left` and `right` (careful for overflows by using `left + (right - left) / 2`).
   - If `nums[mid]` is equal to the target, return `mid`.
   - If `nums[mid]` is less than the target, move `left` to `mid + 1`.
   - If `nums[mid]` is greater than the target, move `right` to `mid - 1`.
3. If we didn't find the target, `left` will be the position where the target should be inserted.

### Code:

```java
public int searchInsert(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2; // avoid potential overflow
        // Check if mid is the target
        if (nums[mid] == target) {
            return mid;
        }
        // Move right if target is greater than mid value
        else if (nums[mid] < target) {
            left = mid + 1;
        }
        // Move left if target is less than mid value
        else {
            right = mid - 1;
        }
    }
    // left is the position where target should be inserted
    return left;
}
```

### Complexity Analysis:
- **Time Complexity**: O(log n), where n is the number of elements in the array. Binary search reduces the problem size by half each iteration, leading to a logarithmic time complexity.
- **Space Complexity**: O(1), as binary search uses a constant amount of extra space. 

These two approaches provide a clear trajectory from a simple solution to a more efficient one, demonstrating how understanding the properties of a sorted array can lead to more optimal algorithms.

