# [Leetcode 41: First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

## Approaches
1. [Basic Brute Force Approach](#basic-brute-force-approach)
2. [HashSet Approach](#hashset-approach)
3. [Cyclic Sort (Optimal) Approach](#cyclic-sort-optimal-approach)

---

### Basic Brute Force Approach

Intuition:
- We need to find the first missing positive integer from an unsorted array.
- Our basic approach involves checking starting from 1, then 2, and so on, whether each number is present in the array. 
- We continue this process until we find a number that is not present, which will be our answer.

Steps:
1. Start with the integer 1.
2. For each integer, check if it exists in the array by iterating through the entire array.
3. If the integer is found, increment to check the next integer.
4. If an integer is not found, it is the first missing positive number.

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        // Starting from 1, check each integer in the array
        for (int i = 1; i <= n + 1; i++) {
            boolean found = false;
            // Check if i is present in the array
            for (int num : nums) {
                if (num == i) {
                    found = true;
                    break;
                }
            }
            // If not found, then it's the missing positive
            if (!found) return i;
        }
        return -1; // The function should never reach here
    }
}
```

Time Complexity: O(n^2) - We iterate through the array for each number starting from 1.
Space Complexity: O(1) - No additional space used.

---

### HashSet Approach

Intuition:
- Rather than checking every number by iterating through the array, we can store all numbers in a HashSet for O(1) look-up time.
- Iterate through numbers starting from 1 and check if they are in the HashSet.

Steps:
1. Insert all numbers into a HashSet.
2. Start checking integers from 1 onwards.
3. If any integer is not found in the HashSet, that's the first missing positive number.

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int firstMissingPositive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        // Add all nums into the HashSet
        for (int num : nums) {
            set.add(num);
        }
        // Check from 1 to infinite for the first missing number
        for (int i = 1; i <= nums.length + 1; i++) {
            if (!set.contains(i)) {
                return i;
            }
        }
        return -1; // The function should never reach here
    }
}
```

Time Complexity: O(n) - Linear time to populate the set and checking numbers.
Space Complexity: O(n) - Space used for storing array elements in the HashSet.

---

### Cyclic Sort (Optimal) Approach

Intuition:
- Place each number at its correct index (i.e., number 1 at index 0, number 2 at index 1, etc.). Numbers out of this range can be ignored.
- Finally, the first place where the number is not equal to the index + 1 is the missing number.

Steps:
1. Iterate through the array and swap numbers to their correct positions if they are within the valid range (1 to n).
2. After sorting, iterate again to find the first index where the number is not equal to `index + 1`.
3. Return `index + 1` as the result.

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        
        for (int i = 0; i < n; i++) {
            // Swap numbers to their correct positions
            while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                int temp = nums[nums[i] - 1];
                nums[nums[i] - 1] = nums[i];
                nums[i] = temp;
            }
        }

        // Determine the first missing positive number
        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        
        return n + 1; // If all numbers 1 to n are present, return n+1
    }
}
```

Time Complexity: O(n) - Each number is visited at most twice.
Space Complexity: O(1) - In-place sorting without additional space.

---

This concludes our guide for solving the "First Missing Positive" problem using three different approaches. The Cyclic Sort Approach is the most optimal both in time and space complexity!

