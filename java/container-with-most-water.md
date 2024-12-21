# [Leetcode 11: Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## Approaches
1. [Brute Force](#brute-force)
2. [Two-Pointer Technique](#two-pointer-technique)


### Brute Force

In the brute force approach, we'll iterate over all possible pairs of lines and calculate the area of water that it can contain. This involves a nested loop where the outer loop picks the first line and the inner loop tries all possible second lines. For each pair of lines, we calculate the minimum of these two heights (as water can only be stored up to the shorter line) and multiply it by the distance between them (their indices difference) to get the area. We keep track of the maximum area we've found so far.

#### Intuition:
- Check all pairs of lines (i, j), compute the container area they define.
- The area is determined by `H[i]` and `H[j]`, taking the smaller one, and the distance between walls `(j - i)`.

#### Time Complexity:
- O(n^2) due to the nested loop through the list.

#### Space Complexity:
- O(1) as we're only using a few extra variables.

#### Java Code:
```java
public class Solution {
    public int maxArea(int[] height) {
        int maxArea = 0;
        for (int i = 0; i < height.length; i++) {
            for (int j = i + 1; j < height.length; j++) {
                // Calculate area between lines i and j
                int area = Math.min(height[i], height[j]) * (j - i);
                // Update maxArea if we find a bigger one
                maxArea = Math.max(maxArea, area);
            }
        }
        return maxArea;
    }
}
```

### Two-Pointer Technique

In this more optimal approach, we use two pointers, one at the beginning and one at the end of the array. In every step, we calculate the area formed by the lines at these two pointers. Then, we move the pointer pointing to the shorter line inward because moving the taller one wouldn't possibly increase the area.

#### Intuition:
- Start with the widest container possible. Move the shorter line inwards step by step.
- By closing in the pointers, we seek better maximum areas because even though the width reduces, we might have taller lines.

#### Time Complexity:
- O(n) since we go through the list at most twice in a single pass.

#### Space Complexity:
- O(1) as no additional space is used apart from some variables for storage.

#### Java Code:
```java
public class Solution {
    public int maxArea(int[] height) {
        int maxArea = 0;
        int left = 0;
        int right = height.length - 1;
        while (left < right) {
            // Calculate area with current left and right pointer
            int area = Math.min(height[left], height[right]) * (right - left);
            // Update maxArea if current area is larger
            maxArea = Math.max(maxArea, area);

            // Move pointers inward from shorter height
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return maxArea;
    }
}
```

In the above solutions, the Two-Pointer Technique drastically reduces the complexity which is beneficial for larger input sizes, making it the preferred approach when optimizing for both time and operational efficiency.

