# [Leetcode 45: Jump Game II](https://leetcode.com/problems/jump-game-ii/)

**Approaches:**
1. [Brute Force Approach](#approach-1-brute-force)
2. [Greedy Approach](#approach-2-greedy-approach)

## Approach 1: Brute Force

### Intuition
The brute force solution involves recursively calculating the minimum number of jumps needed to reach the end of the array from each possible starting position. For each starting position, you try every reachable index and compute the jumps recursively. This method guarantees you will find the minimum jumps but is highly inefficient.

### Code
```python
def jump(nums):
    def recursiveJump(index):
        if index >= len(nums) - 1:
            return 0
        minJumps = float('inf')
        maxReach = min(index + nums[index], len(nums) - 1)
        for nextIndex in range(index + 1, maxReach + 1):
            # Calculate the jumps needed from the next index
            jumps = recursiveJump(nextIndex)
            # Update the minimum jumps needed
            minJumps = min(minJumps, jumps + 1)
        return minJumps
    
    # Return the result starting from the first index
    return recursiveJump(0)

# Example usage
nums = [2, 3, 1, 1, 4]
print(jump(nums))  # Output: 2
```

### Complexity Analysis
- **Time Complexity:** \(O(2^n)\). Since at each step we can make multiple recursive calls, the time complexity is exponential.
- **Space Complexity:** \(O(n)\). Due to the recursion stack.

## Approach 2: Greedy Approach

### Intuition
A more optimized way to solve this problem is to use a greedy approach. The idea here is to keep track of the farthest position that can be reached while traversing the array. We make a jump only when we reach the end of the current jump range. This minimizes the number of jumps made by ensuring each jump leads to the maximum advancement possible.

### Code
```python
def jump(nums):
    # When array length is one, zero jumps are needed.
    if len(nums) <= 1:
        return 0
    
    # Initialize variables
    jumps = 0
    currentEnd = 0
    farthest = 0
    
    # Iterate over each element except the last one
    for i in range(len(nums) - 1):
        # Update the farthest index we can reach
        farthest = max(farthest, i + nums[i])
        # If we reach the current jump end, we need to jump
        if i == currentEnd:
            jumps += 1
            currentEnd = farthest
            # If we have already reached or surpassed the last index
            # break as we don't need any additional jumps
            if currentEnd >= len(nums) - 1:
                break
    
    return jumps

# Example usage
nums = [2, 3, 1, 1, 4]
print(jump(nums))  # Output: 2
```

### Complexity Analysis
- **Time Complexity:** \(O(n)\). We only make a single pass through the list.
- **Space Complexity:** \(O(1)\). We use a constant amount of extra space.

