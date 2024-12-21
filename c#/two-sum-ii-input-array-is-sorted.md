[LeetCode 167: Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

### Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Two Pointers Approach](#two-pointers-approach)

---

### Brute Force Approach

**Intuition:**

The brute force approach involves considering every pair of numbers in the array and checking if they sum up to the given target. This approach is straightforward but not efficient for larger arrays.

**Explanation and Steps:**

- Iterate through each element in the array with a loop.
- For each element, iterate through the rest of the elements to find if its complement (target - current element) exists.
- If such a pair is found, return their indices (1-based as per problem requirement).

**Code:**

```csharp
public class Solution {
    public int[] TwoSum(int[] numbers, int target) {
        // Iterate through each element in numbers
        for (int i = 0; i < numbers.Length; i++) {
            // For each element, iterate through remaining elements
            for (int j = i + 1; j < numbers.Length; j++) {
                // Check if the sum of elements at i and j equals target
                if (numbers[i] + numbers[j] == target) {
                    // Return 1-based indices as required by the problem
                    return new int[] { i + 1, j + 1 };
                }
            }
        }
        // Return an empty array if no pair is found, although per problem constraints a solution always exists
        return new int[] { };
    }
}
```

**Time Complexity:** O(n^2)  
**Space Complexity:** O(1)

---

### Two Pointers Approach

**Intuition:**

Given that the array is sorted, we can use two pointers to find the pair efficiently. We initialize one pointer at the beginning and another at the end of the array, and move them towards each other.

**Explanation and Steps:**

- Start with two pointers: one at the beginning (`left`) and the other at the end (`right`) of the array.
- Calculate the sum of elements at the two pointers.
- If the sum equals the target, return their indices.
- If the sum is less than the target, increment the `left` pointer to increase the sum.
- If the sum is greater than the target, decrement the `right` pointer to decrease the sum.
- Continue the process until the pointers meet.

**Code:**

```csharp
public class Solution {
    public int[] TwoSum(int[] numbers, int target) {
        // Initialize two pointers
        int left = 0;
        int right = numbers.Length - 1;

        // Repeat until the pointers cross
        while (left < right) {
            int sum = numbers[left] + numbers[right];

            // If we found the target sum, return the 1-based indices
            if (sum == target) {
                return new int[] { left + 1, right + 1 };
            }
            // If sum is less than target, move the left pointer to the right
            else if (sum < target) {
                left++;
            }
            // If sum is greater than target, move the right pointer to the left
            else {
                right--;
            }
        }
        // Return an empty array as a fallback
        return new int[] { };
    }
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

With the two pointers approach, we efficiently determine the pair of indices that sum to the target, utilizing the sorted nature of the array. This method provides a significant performance improvement over the brute force technique.

