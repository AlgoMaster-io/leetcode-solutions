# [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approach 1: Brute Force

### Solution
csharp
```csharp
// Time Complexity: O(n * m)
// Space Complexity: O(1)
public class Solution {
    public int[] NextGreaterElement(int[] nums1, int[] nums2) {
        int[] result = new int[nums1.Length];

        // Iterate over each element in nums1
        for (int i = 0; i < nums1.Length; i++) {
            int current = nums1[i];
            int indexInNums2 = -1;

            // Find the index of current in nums2
            for (int j = 0; j < nums2.Length; j++) {
                if (nums2[j] == current) {
                    indexInNums2 = j;
                    break;
                }
            }

            // Look for the next greater element to the right in nums2
            result[i] = -1; // Default if no greater element is found
            for (int j = indexInNums2 + 1; j < nums2.Length; j++) {
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
csharp
```csharp
// Time Complexity: O(n + m)
// Space Complexity: O(m)
using System.Collections.Generic;

public class Solution {
    public int[] NextGreaterElement(int[] nums1, int[] nums2) {
        // Dictionary to store the next greater element for each number in nums2
        Dictionary<int, int> nextGreaterMap = new Dictionary<int, int>();
        Stack<int> stack = new Stack<int>();

        // Traverse nums2 in reverse to populate the nextGreaterMap
        foreach (int num in nums2) {
            // Maintain the stack in decreasing order
            while (stack.Count > 0 && stack.Peek() <= num) {
                stack.Pop();
            }
            // If stack is not empty, the top of the stack is the next greater element
            nextGreaterMap[num] = stack.Count == 0 ? -1 : stack.Peek();
            stack.Push(num);
        }

        // Build the result for nums1 using the map
        int[] result = new int[nums1.Length];
        for (int i = 0; i < nums1.Length; i++) {
            result[i] = nextGreaterMap[nums1[i]];
        }

        return result;
    }
}
```

