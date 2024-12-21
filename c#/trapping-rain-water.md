## [Leetcode 42: Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

### Approaches:
- [Brute Force Approach](#Brute-Force-Approach)
- [Dynamic Programming](#Dynamic-Programming)
- [Two-Pointer Technique](#Two-Pointer-Technique)

### Brute Force Approach
The brute force approach involves iterating over each element in the height array and calculating the water above the current element based on the maximum heights to the left and right of the element. 

#### Intuition:
For each element in the array, the water level above it is determined by the minimum of the maximum heights to its left and right. The amount of water trapped above the current block is this minimum height minus the height of the current block. If the minimum height is less than or equal to the current block height, no water is trapped on that block.

```csharp
public int Trap(int[] height) {
    int n = height.Length;
    int totalWater = 0;

    for (int i = 0; i < n; i++) {
        // Find the maximum height to the left of the current index
        int leftMax = 0;
        for (int j = 0; j <= i; j++) {
            leftMax = Math.Max(leftMax, height[j]);
        }

        // Find the maximum height to the right of the current index
        int rightMax = 0;
        for (int j = i; j < n; j++) {
            rightMax = Math.Max(rightMax, height[j]);
        }

        // Calculate the water above the current index
        int water = Math.Min(leftMax, rightMax) - height[i];
        totalWater += water;
    }
    return totalWater;
}
```

**Time Complexity**: O(n^2), where n is the number of elements in the height array, because for every element we are doing two linear scans to find the maximum heights.  
**Space Complexity**: O(1), using constant space to store variables.

### Dynamic Programming
This optimization precomputes the highest bars on the left and right of each element to eliminate the need for repeated scanning.

#### Intuition:
We use two arrays, `leftMax` and `rightMax`.  
- `leftMax[i]` stores the maximum height to the left of index `i` (inclusive).
- `rightMax[i]` stores the maximum height to the right of index `i` (inclusive).

By filling these arrays, we can calculate water trapped on each block directly using precomputed values.

```csharp
public int Trap(int[] height) {
    int n = height.Length;
    if (n == 0) return 0;
    
    int[] leftMax = new int[n];
    int[] rightMax = new int[n];
    int totalWater = 0;

    // Fill leftMax array
    leftMax[0] = height[0];
    for (int i = 1; i < n; i++) {
        leftMax[i] = Math.Max(leftMax[i - 1], height[i]);
    }

    // Fill rightMax array
    rightMax[n - 1] = height[n - 1];
    for (int i = n - 2; i >= 0; i--) {
        rightMax[i] = Math.Max(rightMax[i + 1], height[i]);
    }

    // Calculate the total water trapped
    for (int i = 0; i < n; i++) {
        int water = Math.Min(leftMax[i], rightMax[i]) - height[i];
        totalWater += water;
    }
    
    return totalWater;
}
```

**Time Complexity**: O(n), where n is the number of elements in the height array because we iterate through the array multiple times.  
**Space Complexity**: O(n), using extra space for the `leftMax` and `rightMax` arrays.

### Two-Pointer Technique
This optimization uses two pointers to traverse the height array, effectively combining the dynamic programming arrays into one pass.

#### Intuition:
By maintaining two pointers that move towards each other from both ends of the height array, we can keep track of the current maximum heights from the left and right. This way, we decide whether the water trapped depends on the leftMax or the rightMax based on which is smaller at the current pointers' positions.

```csharp
public int Trap(int[] height) {
    int n = height.Length;
    if (n == 0) return 0;

    int left = 0, right = n - 1;
    int leftMax = 0, rightMax = 0;
    int totalWater = 0;

    while (left < right) {
        if (height[left] < height[right]) {
            if (height[left] >= leftMax) {
                leftMax = height[left];
            } else {
                totalWater += leftMax - height[left];
            }
            left++;
        } else {
            if (height[right] >= rightMax) {
                rightMax = height[right];
            } else {
                totalWater += rightMax - height[right];
            }
            right--;
        }
    }
    
    return totalWater;
}
```

**Time Complexity**: O(n), since we traverse the height array only once.  
**Space Complexity**: O(1), since we are using a constant amount of extra space.

