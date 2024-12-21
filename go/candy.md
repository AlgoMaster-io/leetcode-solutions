# [Leetcode 135: Candy](https://leetcode.com/problems/candy/)

## Solutions
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pass Approach](#approach-2-two-pass-approach)
- [Approach 3: Single Pass Approach](#approach-3-single-pass-approach)

### Approach 1: Brute Force

#### Intuition
The simplest way to solve the problem is to initially distribute 1 candy to each child and then repeatedly make passes to adjust candy distribution, ensuring that each child has more candies than their neighbors if they have a higher rating. This approach will involve multiple refinements until the condition is satisfied for all.

#### Steps
1. Start by giving each child 1 candy.
2. Use a flag to track if any changes have been made in a pass.
3. Iterate through the list, checking the conditions from left to right and right to left.
4. If a condition is not satisfied, increment the candy count for the child with a higher rating.
5. Repeat the passes until a complete pass is made without any adjustments.

#### Code
```go
func candy(ratings []int) int {
    n := len(ratings)
    candies := make([]int, n)
    for i := range candies {
        candies[i] = 1 // Step 1: Give each child 1 candy initially
    }
    
    changed := true
    // Step 3: Repeat passes until no changes
    for changed {
        changed = false
        // Left to Right
        for i := 1; i < n; i++ {
            if ratings[i] > ratings[i-1] && candies[i] <= candies[i-1] {
                candies[i] = candies[i-1] + 1 // Increment the candy count
                changed = true
            }
        }
        // Right to Left
        for i := n-2; i >= 0; i-- {
            if ratings[i] > ratings[i+1] && candies[i] <= candies[i+1] {
                candies[i] = candies[i+1] + 1 // Increment the candy count
                changed = true
            }
        }
    }
    
    // Calculate the sum of all candies distributed
    totalCandies := 0
    for _, candy := range candies {
        totalCandies += candy
    }
    
    return totalCandies
}
```

#### Complexity Analysis
- **Time Complexity**: O(n^2) - Due to the repeated passes through the array until no changes.
- **Space Complexity**: O(n) - To store the candy count for each child.

### Approach 2: Two Pass Approach

#### Intuition
By making only two passes through the ratings list, we can efficiently calculate the minimum candies required. The idea is to satisfy the condition from left to right and then from right to left, which ensures the minimum candies distribution effectively.

#### Steps
1. Initialize a candies array with all elements as 1.
2. Perform a pass from left to right, adjusting candies if a child's rating is greater than the previous child.
3. Perform a pass from right to left, adjusting candies if a child's rating is greater than the next child while ensuring the candies are still higher than previously distributed.
4. Sum up all the candies from the candies array.

#### Code
```go
func candy(ratings []int) int {
    n := len(ratings)
    if n == 0 {
        return 0
    }
    candies := make([]int, n)
    for i := range candies {
        candies[i] = 1 // Initially give each child 1 candy
    }
    
    // Step 2: Left to Right pass
    for i := 1; i < n; i++ {
        if ratings[i] > ratings[i-1] {
            candies[i] = candies[i-1] + 1
        }
    }
    
    // Step 3: Right to Left pass
    for i := n-2; i >= 0; i-- {
        if ratings[i] > ratings[i+1] {
            candies[i] = max(candies[i], candies[i+1] + 1)
        }
    }
    
    // Calculate total candies
    totalCandies := 0
    for _, candy := range candies {
        totalCandies += candy
    }
    
    return totalCandies
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

#### Complexity Analysis
- **Time Complexity**: O(n) - Only two single passes through the list.
- **Space Complexity**: O(n) - To store candy allocations.

### Approach 3: Single Pass Approach

#### Intuition
A more optimized way uses a greedy approach. Start with the minimum candies distribution and adjust based on local increments and decrements. It utilizes a technique to keep track of the peak ratings and their corresponding candies in a single forward pass.

#### Steps
1. Initialize candies for the first child.
2. Traverse the list, keeping track of previous trend (increasing, decreasing or equal).
3. Calculate total candies based on the increments for increasing ratings and adjust during decreasing trends using a persistent sum update at each adjustment step.

#### Note
This approach is complex and requires careful handling of state transitions which can be simplified or proven via implementation and real-world performance benchmarking. Therefore, for accurate results in a simpler setup, Approach 2 is preferred unless specific constraints explicitly necessitate further optimization.

This step does not currently provide a direct code sample, as its complexity may outweigh its practical benefits depending on implementation needs. Typically, being aware of the strategies and linear patterns emerging in simpler approaches can guide effective coding especially in constrained environments, realizing a proved efficient greedy strategy could achieve O(n) time and O(1) extra space, yet more often in tandem with predefined constraints.

