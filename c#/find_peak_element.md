# 162. [Find Peak Element](https://leetcode.com/problems/find-peak-element/)

## Approach 1: Linear Scan

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int FindPeakElement(int[] nums) {
        // Traverse the array to find a peak
        for (int i = 0; i < nums.Length - 1; i++) {
            // A peak is found if the current element is greater than the next
            if (nums[i] > nums[i + 1]) {
                return i; // Return the index of the peak
            }
        }
        return nums.Length - 1; // If no peak is found, the last element is a peak
    }
}
```

## Approach 2: Binary Search

### Solution
csharp
```csharp
// Time Complexity: O(log n)
// Space Complexity: O(1)
public class Solution {
    public int FindPeakElement(int[] nums) {
        int start = 0, end = nums.Length - 1;

        // Perform binary search to find the peak
        while (start < end) {
            int mid = start + (end - start) / 2;

            // If mid element is less than the next element, peak lies on the right
            if (nums[mid] < nums[mid + 1]) {
                start = mid + 1; // Move to the right half
            } else {
                // If mid element is greater than the next element, peak lies on the left
                end = mid; // Narrow search to the left half
            }
        }

        return start; // Start and end converge to the peak element
    }
}
```

## Approach 3: Recursive Binary Search

### Solution
csharp
```csharp
// Time Complexity: O(log n)
// Space Complexity: O(log n) due to recursion stack
public class Solution {
    public int FindPeakElement(int[] nums) {
        return Search(nums, 0, nums.Length - 1);
    }

    private int Search(int[] nums, int start, int end) {
        if (start == end) {
            return start; // Base case: single element is the peak
        }

        int mid = start + (end - start) / 2;

        // Check if peak lies on the right half
        if (nums[mid] < nums[mid + 1]) {
            return Search(nums, mid + 1, end); // Recur for the right half
        } else {
            return Search(nums, start, mid); // Recur for the left half
        }
    }
}
```

