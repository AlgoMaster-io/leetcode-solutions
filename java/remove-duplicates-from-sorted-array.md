# [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

## Approaches:
1. [Two Pointers Approach](#two-pointers-approach)

### Two Pointers Approach

#### Intuition:
In a sorted array, duplicates occur consecutively. To remove duplicates, we can maintain two pointers - one (`i`) to track the location for the next unique element, and another (`j`) to traverse through the array checking for duplicates. For each unique element found, we place it at the `i`-th index and increment `i`. This ensures that all unique elements are moved to the front of the array, while any remaining elements beyond the `i`-th position are irrelevant as they are duplicates or unevaluated.

#### Algorithm:
1. Check if the array is empty. If so, return 0 as there are no elements.
2. Initialize `i` as 0, which will keep track of the last index of the unique array.
3. Traverse the array using pointer `j` starting from the first element.
4. If an element at index `j` is different from the element at index `i`, it means we have found a new unique element:
   - Increment `i`.
   - Place `nums[j]` at `nums[i]`.
5. Return `i + 1` as the length of the unique elements array.

#### Java Code:
```java
public class Solution {
    public int removeDuplicates(int[] nums) {
        // Check if the array is empty. If it is, return 0 as there are no elements to process.
        if (nums.length == 0) return 0;

        // Initialize i to point to the index where we should place the next unique element.
        int i = 0; 

        // Iterate through the array starting from the first element.
        for (int j = 1; j < nums.length; j++) {
            // Check if the current element nums[j] is different from nums[i].
            // If so, it indicates a new unique element.
            if (nums[j] != nums[i]) {
                // Increment i to reserve the next position for the new unique element.
                i++;
                // Place the new unique element found at nums[j] into the position at nums[i].
                nums[i] = nums[j];
            }
        }
        // Return the length of the array containing unique elements,
        // this is i + 1 because i is the index of the last unique element found.
        return i + 1;
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of elements in the array. We traverse the array with a single pass using the two pointers.
- **Space Complexity**: O(1), as we are using extra space only for the pointers and directly modifying the input array.

This approach efficiently removes duplicates in place and ensures that only unique elements are kept at the beginning of the array. The final array length of unique elements is returned.

