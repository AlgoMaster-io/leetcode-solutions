# [Leetcode 33: Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## Approaches:
1. [Brute Force](#brute-force)
2. [Optimized Linear Search](#optimized-linear-search)
3. [Binary Search](#binary-search)

### Brute Force

#### Intuition:
The simplest way to solve this problem is to iterate through the array and compare each element with the target. Though this method ensures a solution, it's inefficient since it doesn't take advantage of the sorted nature of the array.

#### Solution:
```csharp
public class Solution {
    public int Search(int[] nums, int target) {
        // Traverse the array
        for (int i = 0; i < nums.Length; i++) {
            // If the target is found, return its index
            if (nums[i] == target) {
                return i;
            }
        }
        // Return -1 if the target is not found
        return -1;
    }
}
```

#### Time Complexity:
- O(n): We might need to check each element once.

#### Space Complexity:
- O(1): We are using a constant amount of extra space.

---

### Optimized Linear Search

#### Intuition:
As the array is rotated at a pivot unknown to you beforehand, the idea is to find the point of rotation, then perform a binary search on the appropriate half of the array where the target is likely located. However, a simpler way before going to the optimal is optimizing linear search by reducing unnecessary checks.

#### Solution:
```csharp
public class Solution {
    public int Search(int[] nums, int target) {
        int n = nums.Length;
        // Traverse the array to find the smallest element
        for (int i = 0; i < n; i++) {
            // If the current element is the target, return the index
            if (nums[i] == target) return i;
            // End search when a possible rotation is detected
            if (i < n - 1 && nums[i] > nums[i + 1]) break;
        }
        // Target not present
        return -1;
    }
}
```

#### Time Complexity:
- O(n): Similar to the brute force approach, just stops early on detecting the rotated part which might not always help.

#### Space Complexity:
- O(1): Constant space used.

---

### Binary Search

#### Intuition:
The optimal solution makes use of binary search by understanding that one of the halves of the array will always be sorted. By comparing the middle value with the target and adjusting the half region accordingly, we can determine which half to vanquish at each step.

#### Solution:
```csharp
public class Solution {
    public int Search(int[] nums, int target) {
        int left = 0, right = nums.Length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            // Check if the middle element is the target
            if (nums[mid] == target) return mid;
            
            // Determine if the left side is sorted
            if (nums[left] <= nums[mid]) {
                // Check if the target lies within the sorted left side
                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1; // Narrow search to left half
                } else {
                    left = mid + 1; // Search in right half
                }
            } else {
                // Right side must be sorted
                // Check if the target lies within the sorted right side
                if (target > nums[mid] && target <= nums[right]) {
                    left = mid + 1; // Narrow search to right half
                } else {
                    right = mid - 1; // Search in left half
                }
            }
        }
        // If the target is not found, return -1
        return -1;
    }
}
```

#### Time Complexity:
- O(log n): Binary search reduces the search space by half at each step.

#### Space Complexity:
- O(1): Constant space used.

---

