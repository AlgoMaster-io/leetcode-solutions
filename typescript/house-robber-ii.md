# [Leetcode 213: House Robber II](https://leetcode.com/problems/house-robber-ii/)

This problem is an extension of the "House Robber" problem. In this case, houses are arranged in a circle, meaning the first house is the neighbor of the last house. You cannot rob two adjacent houses, and your aim is to rob houses to maximize your profit while adhering to the constraints.

## Approaches

1. [Basic Dynamic Programming](#basic-dynamic-programming)
2. [Optimized Dynamic Programming with Space Reduction](#optimized-dynamic-programming-with-space-reduction)

### Basic Dynamic Programming

#### Intuition

The classic "House Robber" problem is solved using dynamic programming. However, due to the circular arrangement in this problem, if we include the first house, we cannot include the last house, and vice versa. Therefore, we evaluate two scenarios:
1. Rob houses from the 1st to the 2nd last.
2. Rob houses from the 2nd to the last.

We'll use dynamic programming for both scenarios and take the maximum of the two results.

#### Steps

1. Define a helper function to calculate the maximum money that can be robbed from a linear sequence of houses.
2. Utilize this helper to calculate two scenarios:
   - Rob houses excluding the last one.
   - Rob houses excluding the first one.
3. The result will be the maximum profit from both scenarios.

#### Code

```typescript
function rob(nums: number[]): number {
    if (nums.length === 1) return nums[0];

    // Helper function to calculate maximum rob amount for a linear street of houses
    function robLinear(start: number, end: number): number {
        let prev2 = 0;  // Max amount when skipping the current house
        let prev1 = 0;  // Max amount when robbing the current house

        // Iterate through houses from start to end
        for (let i = start; i <= end; i++) {
            // Max amount if robbing this house
            const curr = Math.max(prev1, prev2 + nums[i]);
            
            // Update prev values for next iteration
            prev2 = prev1;
            prev1 = curr;
        }
        return prev1;
    }

    // Scenario 1: Rob houses from the 1st to the 2nd last
    const robFirstSection = robLinear(0, nums.length - 2);

    // Scenario 2: Rob houses from the 2nd to the last
    const robSecondSection = robLinear(1, nums.length - 1);

    // Return the maximum of the two scenarios
    return Math.max(robFirstSection, robSecondSection);
}
```

#### Time and Space Complexity

- **Time Complexity:** O(n), where n is the number of houses. Each house is processed once in each of the two linear scenarios.
- **Space Complexity:** O(1), as we are using constant extra space.

### Optimized Dynamic Programming with Space Reduction

#### Intuition

The basic solution is already optimized space-wise by maintaining only two variables (`prev1` and `prev2`) to keep track of the dynamic programming table. Since we cannot reduce any more space or improve the time complexity further, the first solution is both time and space optimal, considering the constraints given by the problem.

The key change we have already applied is avoiding building a separate dp array for linear house rob function, which optimizes the space.

Since the first solution already utilizes the space optimization, no further optimization is needed beyond this.

