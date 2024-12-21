# [Leetcode 167: Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## Solutions:

1. [Brute Force Approach](#solution-1-brute-force-approach)
2. [Two Pointer Approach](#solution-2-two-pointer-approach)

---

## Solution 1: Brute Force Approach

### Intuition:
The simplest way to solve this problem is by checking every possible pair in the array to see if it adds up to the target. Since the array is sorted, once the sum exceeds the target, we can stop checking further elements with the current element.

### Java Code:
```java
public int[] twoSumBruteForce(int[] numbers, int target) {
    // Iterate over each element in the array
    for (int i = 0; i < numbers.length; i++) {
        // For the current 'i', iterate over every element greater than 'i'
        for (int j = i + 1; j < numbers.length; j++) {
            // If the pair adds up to the target, return their indices (1-indexed)
            if (numbers[i] + numbers[j] == target) {
                return new int[]{i + 1, j + 1};
            }
            // Since the array is sorted, no need to further check if sum exceeds target
            if (numbers[i] + numbers[j] > target) {
                break;
            }
        }
    }
    // Return an empty array if no solution is found, although the problem guarantees a solution
    return new int[]{};
}
```

### Time and Space Complexity:
- **Time Complexity**: \(O(n^2)\) - We are checking every possible pair.
- **Space Complexity**: \(O(1)\) - No additional space is used.

---

## Solution 2: Two Pointer Approach

### Intuition:
Since the array is sorted, we can use a two-pointer technique. Start with one pointer at the beginning and the other at the end of the array. Check the sum of the values at these pointers:
- If the sum equals the target, return the 1-indexed positions.
- If the sum is less than the target, move the left pointer to the right to increase the sum.
- If the sum is more than the target, move the right pointer to the left to decrease the sum.

### Java Code:
```java
public int[] twoSumTwoPointer(int[] numbers, int target) {
    int left = 0;
    int right = numbers.length - 1;

    while (left < right) {
        int sum = numbers[left] + numbers[right];
        // Check if the current sum equals the target
        if (sum == target) {
            return new int[]{left + 1, right + 1}; // Return 1-indexed positions
        }
        // Move the left pointer right to increase sum
        else if (sum < target) {
            left++;
        }
        // Move the right pointer left to decrease sum
        else {
            right--;
        }
    }
    // It is guaranteed that there will always be one solution, so this line will not be reached
    return new int[]{};
}
```

### Time and Space Complexity:
- **Time Complexity**: \(O(n)\) - Each element is visited at most once by either of the pointers.
- **Space Complexity**: \(O(1)\) - No additional space is used. 

The two-pointer approach leverages the sorted nature of the input array and is optimal for this problem.

