# [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## Approach 1: Binary Search for the Complement

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(1)
public class Solution {
    public int[] twoSum(int[] numbers, int target) {
        for (int i = 0; i < numbers.length; i++) {
            int complement = target - numbers[i];
            int index = binarySearch(numbers, i + 1, numbers.length - 1, complement);

            if (index != -1) {
                return new int[] {i + 1, index + 1}; // 1-based index
            }
        }

        return new int[] {-1, -1}; // No solution found
    }

    private int binarySearch(int[] numbers, int left, int right, int target) {
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
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    public int[] twoSum(int[] numbers, int target) {
        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < numbers.length; i++) {
            int complement = target - numbers[i];

            if (map.containsKey(complement)) {
                return new int[] {map.get(complement) + 1, i + 1}; // 1-based index
            }

            map.put(numbers[i], i);
        }

        return new int[] {-1, -1}; // No solution found
    }
}
```

## Approach 3: Two Pointers

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0; // Start pointer
        int right = numbers.length - 1; // End pointer

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