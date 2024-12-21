# LeetCode Problem - [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Greedy Approach](#greedy-approach)

---

### Brute Force Approach

#### Intuition:
The brute force approach involves exploring all possible ways to jump through the array to reach the end. We recursively try every possible jump from each position and choose the one that results in the minimum number of jumps.

#### Solution:
```go
// Helper function to perform recursive jumping
func jumpFromPosition(position int, nums []int) int {
    if position >= len(nums) - 1 {
        return 0
    }
    
    steps := nums[position]
    minJumps := int(^uint(0) >> 1) // Initialize to maximum integer value
    
    // Try each possible jump length from current position
    for i := 1; i <= steps; i++ {
        newPosition := position + i
        jumps := jumpFromPosition(newPosition, nums)
        minJumps = min(minJumps, jumps + 1)
    }
    
    return minJumps
}

// Wrapper function
func jump(nums []int) int {
    return jumpFromPosition(0, nums)
}

// Utility function to find minimum of two integers
func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

#### Time Complexity:
- **O(n^n)**, where `n` is the length of the array. This occurs because each jump can be up to the entire length minus the current position.

#### Space Complexity:
- **O(n)** due to the recursion stack.

---

### Greedy Approach

#### Intuition:
The greedy approach involves iteratively finding the farthest point that can be reached by taking one jump at a time. We only update our "jump count" once we must leave the current "coverage area" and move to the next one.

#### Solution:
```go
func jump(nums []int) int {
    if len(nums) <= 1 {
        return 0
    }
    
    jumps := 0
    currentEnd := 0
    furthest := 0

    for i := 0; i < len(nums)-1; i++ {
        // Determine the furthest point that can be reached from here
        furthest = max(furthest, i + nums[i])
        
        // If we reach the end of the range that was initialized by keeping
        // the `currentEnd`, a jump has to be made to continue
        if i == currentEnd {
            jumps++
            currentEnd = furthest
            
            // If we have already reached or can go beyond the last index
            if currentEnd >= len(nums)-1 {
                break
            }
        }
    }
    return jumps
}

// Utility function to find maximum of two integers
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

#### Time Complexity:
- **O(n)**, where `n` is the length of the array. We process each element exactly once.

#### Space Complexity:
- **O(1)**, since we are using a constant amount of space.

---

In conclusion, the greedy approach provides a much more efficient solution in terms of time complexity compared to the brute force approach. This problem illustrates how recognizing a greedy strategy can significantly optimize certain types of problems, especially when dealing with scenarios involving "linear progression" like reaching the end of an array.

