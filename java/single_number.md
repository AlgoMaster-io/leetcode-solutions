# 136. [Single Number](https://leetcode.com/problems/single-number/)

## Approach 1: Using HashMap

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    public int singleNumber(int[] nums) {
        HashMap<Integer, Integer> countMap = new HashMap<>();

        // Count the frequency of each number
        for (int num : nums) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }

        // Find the number with frequency 1
        for (int num : nums) {
            if (countMap.get(num) == 1) {
                return num;
            }
        }

        return -1; // No unique number found
    }
}
```

## Approach 2: Using Sorting

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(1)
import java.util.Arrays;

public class Solution {
    public int singleNumber(int[] nums) {
        Arrays.sort(nums); // Sort the array

        // Traverse the array looking for unique element
        for (int i = 0; i < nums.length - 1; i += 2) {
            if (nums[i] != nums[i + 1]) {
                return nums[i];
            }
        }

        return nums[nums.length - 1]; // The last element is the single number
    }
}
```

## Approach 3: Using XOR

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int singleNumber(int[] nums) {
        int result = 0;

        // XOR all elements, duplicate elements will cancel out
        for (int num : nums) {
            result ^= num;
        }

        return result; // The remaining single number
    }
}
```

