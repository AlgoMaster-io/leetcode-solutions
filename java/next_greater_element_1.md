# [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approach 1: Brute Force

### Solution
```java
// Time Complexity: O(n * m)
// Space Complexity: O(1)
public class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] result = new int[nums1.length];

        // Iterate over each element in nums1
        for (int i = 0; i < nums1.length; i++) {
            int current = nums1[i];
            int indexInNums2 = -1;

            // Find the index of current in nums2
            for (int j = 0; j < nums2.length; j++) {
                if (nums2[j] == current) {
                    indexInNums2 = j;
                    break;
                }
            }

            // Look for the next greater element to the right in nums2
            result[i] = -1; // Default if no greater element is found
            for (int j = indexInNums2 + 1; j < nums2.length; j++) {
                if (nums2[j] > current) {
                    result[i] = nums2[j];
                    break;
                }
            }
        }
        return result;
    }
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
```java
// Time Complexity: O(n + m)
// Space Complexity: O(m)
import java.util.*;

public class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        // Map to store the next greater element for each number in nums2
        Map<Integer, Integer> nextGreaterMap = new HashMap<>();
        Stack<Integer> stack = new Stack<>();

        // Traverse nums2 in reverse to populate the nextGreaterMap
        for (int num : nums2) {
            // Maintain the stack in decreasing order
            while (!stack.isEmpty() && stack.peek() <= num) {
                stack.pop();
            }
            // If stack is not empty, the top of the stack is the next greater element
            nextGreaterMap.put(num, stack.isEmpty() ? -1 : stack.peek());
            stack.push(num);
        }

        // Build the result for nums1 using the map
        int[] result = new int[nums1.length];
        for (int i = 0; i < nums1.length; i++) {
            result[i] = nextGreaterMap.get(nums1[i]);
        }

        return result;
    }
}
```