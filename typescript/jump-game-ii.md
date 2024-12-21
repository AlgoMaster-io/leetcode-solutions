## [Leetcode 45: Jump Game II](https://leetcode.com/problems/jump-game-ii/)

### Approaches
1. [Greedy Approach](#greedy-approach)
2. [Optimized Greedy Approach](#optimized-greedy-approach)

### Greedy Approach

#### Intuition:
The basic idea here is to use a greedy strategy to jump to the farthest reachable point in the array. We keep track of the farthest point we can reach at each step, and increment our jump count when we need to proceed beyond the current jump's reach.

#### Steps:
1. Use two variables - `currentEnd` to track the end of the current jump, and `farthest` to track the farthest point that can be reached by jumping.
2. Traverse through the array:
   - For each position, update the farthest point that can be reached.
   - If you reach the `currentEnd`, make a jump to `farthest`.
3. Repeat until you reach the last index in the list.

#### Time Complexity:
- **O(n)**, where n is the number of elements in the array. We make a single pass through the list.

#### Space Complexity:
- **O(1)**, as we're using constant extra space.

```typescript
function jump(nums: number[]): number {
    // Initialize the number of jumps to 0
    let jumps = 0;
    
    // These variables are to track how far we can currently jump and the farthest we could jump
    let currentEnd = 0;
    let farthest = 0;
    
    // Traverse through the array. Note: the loop stops before the last element
    for (let i = 0; i < nums.length - 1; i++) {
        // Update the farthest point we can reach at this step
        farthest = Math.max(farthest, i + nums[i]);
        
        // If we have reached the end of our current jump potential
        if (i === currentEnd) {
            // We need to increment jumps because we must decide to jump now
            jumps++;
            // And, update the current jump end with this new farthest reach
            currentEnd = farthest;
        }
    }
    
    return jumps;
}
```

### Optimized Greedy Approach

While the initial greedy approach idea remains effective, let's explore a more refined explanation and slightly optimized code for completeness and understanding.

#### Intuition:
We follow the greedy approach but with a refined understanding of monitoring the range within which each jump is valid. Once we reach the end of the list, we return the number of jumps.

#### Steps:
1. We manage two boundary markers:
   - One for keeping track of the current scope (`currentEnd`).
   - Another for the farthest point (`farthest`) that can possibly be reached from every index within `currentEnd`.
2. Every step, update the `farthest` and, if the current index reaches `currentEnd`, increment the jump count, signaling a need to extend the reach range.
3. Return the jumps once the endpoint is reached or exceeded.

#### Time Complexity:
- **O(n)**, efficiently scanning through the array once.

#### Space Complexity:
- **O(1)**, using a few extra variables only.

```typescript
function jump(nums: number[]): number {
    // Number of jumps taken
    let jumps = 0;
    // The end of the current jump
    let currentEnd = 0;
    // The farthest we can go with the current and the next jump
    let farthest = 0;

    // Iterate through the array except for the last element
    for (let i = 0; i < nums.length - 1; i++) {
        // Update the farthest point we can reach from the current position
        farthest = Math.max(farthest, i + nums[i]);
        
        // If we've reached the end of our current ability to jump
        if (i === currentEnd) {
            // Increase the jump count
            jumps++;
            // Move to the furthest we can get with the next jump
            currentEnd = farthest;
            
            // If we can already reach or surpass the last index, break
            if (currentEnd >= nums.length - 1) {
                break;
            }
        }
    }

    return jumps;
}
```

Both solutions efficiently identify the minimum number of jumps to the last index, leveraging a greedy approach by optimizing both forward reach and jump considerations.

