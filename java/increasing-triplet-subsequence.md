## LeetCode Problem [334: Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)

### Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Linear Scan with Two Variables](#linear-scan-with-two-variables)

---

### Brute Force Approach

#### Intuition:
The simplest approach to find an increasing triplet subsequence is to use three nested loops to check all possible triplets in the array. This is a naive solution but helps to understand the underlying concept. 

In this approach, we will iterate through each element and then iterate the subsequent elements twice to form all possible triplets, checking if they are increasing.

#### Implementation:

```java
public boolean increasingTriplet(int[] nums) {
    int n = nums.length;
    // Iterate over the array to find the first number
    for (int i = 0; i < n - 2; i++) {
        // Iterate to find the second number which is greater than the first
        for (int j = i + 1; j < n - 1; j++) {
            // Iterate to find the third number which is greater than the second
            for (int k = j + 1; k < n; k++) {
                // Check if this is an increasing triplet
                if (nums[i] < nums[j] && nums[j] < nums[k]) {
                    return true;
                }
            }
        }
    }
    return false;
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n^3), due to the three nested loops over the array.
- **Space Complexity**: O(1), since we are not using any additional data structures.

---

### Linear Scan with Two Variables

#### Intuition:
The goal is to find three increasing numbers a, b, c such that a < b < c. We can achieve this in linear time with two variables:
- `first`: This will keep track of the smallest number found so far.
- `second`: This will keep track of a number larger than `first`.

The idea is to iterate through the list to look for the smallest possible 'a' and 'b'. If we find a 'c' which is greater than both, we return true.

#### Implementation:

```java
public boolean increasingTriplet(int[] nums) {
    // Initialize the first and second variables to the maximum possible value
    int first = Integer.MAX_VALUE;
    int second = Integer.MAX_VALUE;

    // Traverse the array
    for (int num : nums) {
        // If current number is less than or equal to the first, update first
        if (num <= first) {
            first = num;
        // Else if current number is less than or equal to the second, update second
        } else if (num <= second) {
            second = num;
        // Else we found a number greater than both first and second
        } else {
            return true;
        }
    }
    return false;
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of elements in the array. We only require one pass through the array.
- **Space Complexity**: O(1), since we are using only a constant amount of additional space.

By iterating through the array in a single pass and maintaining two variables, this approach offers an optimal solution for detecting an increasing triplet subsequence.

