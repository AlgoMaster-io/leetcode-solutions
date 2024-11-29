# [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## Approach 1: Binary Search for the Complement

### Solution
csharp
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(1)
public class Solution {
    public int[] TwoSum(int[] numbers, int target) {
        for (int i = 0; i < numbers.Length; i++) {
            int complement = target - numbers[i];
            int index = BinarySearch(numbers, i + 1, numbers.Length - 1, complement);

            if (index != -1) {
                return new int[] {i + 1, index + 1}; // 1-based index
            }
        }

        return new int[] {-1, -1}; // No solution found
    }

    private int BinarySearch(int[] numbers, int left, int right, int target) {
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (numbers[mid] == target) {
                return mid;
            } else if (numbers[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1; // Target not found
    }
}
```

## Approach 2: HashMap (For Non-Sorted Input)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int[] TwoSum(int[] numbers, int target) {
        Dictionary<int, int> map = new Dictionary<int, int>();

        for (int i = 0; i < numbers.Length; i++) {
            int complement = target - numbers[i];

            if (map.ContainsKey(complement)) {
                return new int[] {map[complement] + 1, i + 1}; // 1-based index
            }

            map[numbers[i]] = i;
        }

        return new int[] {-1, -1}; // No solution found
    }
}
```

## Approach 3: Two Pointers

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int[] TwoSum(int[] numbers, int target) {
        int left = 0; // Start pointer
        int right = numbers.Length - 1; // End pointer

        while (left < right) {
            int sum = numbers[left] + numbers[right];
            
            if (sum == target) {
                return new int[] {left + 1, right + 1}; // 1-based index
            } else if (sum < target) {
                left++; // Move left pointer to increase sum
            } else {
                right--; // Move right pointer to decrease sum
            }
        }

        return new int[] {-1, -1}; // No solution found
    }
}
```

