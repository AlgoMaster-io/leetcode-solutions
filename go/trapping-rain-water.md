You can view the problem on LeetCode [here](https://leetcode.com/problems/trapping-rain-water/).

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)
3. [Two-Pointers Approach](#two-pointers-approach)

---

## Brute Force Approach

### Intuition
The idea is to traverse each element of the height array and for each element, calculate the maximum height on its left and the maximum height on its right. The water that can be trapped at that element is the minimum of these two heights minus the height of the current element.

### Code

```go
func trap(height []int) int {
    // Total amount of water trapped
    totalWater := 0
    
    // Iterate each element to calculate trapped water at that index
    for i := 0; i < len(height); i++ {
        // Maximum height on the left side
        leftMax := 0
        for j := 0; j <= i; j++ {
            if height[j] > leftMax {
                leftMax = height[j]
            }
        }
        
        // Maximum height on the right side
        rightMax := 0
        for j := i; j < len(height); j++ {
            if height[j] > rightMax {
                rightMax = height[j]
            }
        }
        
        // Calculate the trapped water at index i
        totalWater += min(leftMax, rightMax) - height[i]
    }
    
    return totalWater
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

### Complexity
- **Time Complexity**: O(n^2) - Two nested loops for each element.
- **Space Complexity**: O(1) - No extra space is used apart from variables.

---

## Dynamic Programming Approach

### Intuition
Instead of recalculating the left and right maximum heights for each element, we can store these in two separate arrays. This allows us to calculate this information in linear time and save it for later use, improving our overall performance compared to the brute force approach.

### Code

```go
func trap(height []int) int {
    n := len(height)
    if n == 0 {
        return 0
    }

    // Arrays to store the maximum heights to the left and right of each element
    leftMax := make([]int, n)
    rightMax := make([]int, n)

    // Fill leftMax array
    leftMax[0] = height[0]
    for i := 1; i < n; i++ {
        leftMax[i] = max(leftMax[i-1], height[i])
    }

    // Fill rightMax array
    rightMax[n-1] = height[n-1]
    for i := n-2; i >= 0; i-- {
        rightMax[i] = max(rightMax[i+1], height[i])
    }

    // Calculate the trapped water
    totalWater := 0
    for i := 0; i < n; i++ {
        totalWater += min(leftMax[i], rightMax[i]) - height[i]
    }

    return totalWater
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Complexity
- **Time Complexity**: O(n) - Single pass to fill leftMax and rightMax arrays and another to calculate trapped water.
- **Space Complexity**: O(n) - Additional space for leftMax and rightMax arrays.

---

## Two-Pointers Approach

### Intuition
This approach uses two pointers to save space compared to the dynamic programming approach. We start with two pointers (left and right) at the beginning and end of the array. We move the pointer with the lower height inward and calculate how much water can be trapped at each step, constantly updating the maximum heights on each side.

### Code

```go
func trap(height []int) int {
    left, right := 0, len(height)-1
    leftMax, rightMax := 0, 0
    totalWater := 0

    for left < right {
        if height[left] < height[right] {
            if height[left] >= leftMax {
                leftMax = height[left]
            } else {
                totalWater += leftMax - height[left]
            }
            left++
        } else {
            if height[right] >= rightMax {
                rightMax = height[right]
            } else {
                totalWater += rightMax - height[right]
            }
            right--
        }
    }

    return totalWater
}
```

### Complexity
- **Time Complexity**: O(n) - We make a single pass through the array with two pointers.
- **Space Complexity**: O(1) - Only uses a constant amount of extra space.

This two-pointers approach is the most optimal due to its minimal space usage while still maintaining linear time execution.

