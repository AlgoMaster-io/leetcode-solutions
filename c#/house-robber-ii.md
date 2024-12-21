# [Leetcode 213: House Robber II](https://leetcode.com/problems/house-robber-ii/)

## Solutions

- [Approach 1: Divide and Conquer](#approach-1-divide-and-conquer)
- [Approach 2: Dynamic Programming Bottom-Up with Reduced Space](#approach-2-dynamic-programming-bottom-up-with-reduced-space)

### Approach 1: Divide and Conquer 

#### Intuition

In the "House Robber II" problem, the houses are arranged in a circle, which means robbing the first house disables the option to rob the last house and vice-versa. This problem can be split into two simple "House Robber I" problems:

1. Consider the first to second last houses.
2. Consider the second to the last house.

For each of the above arrangements, compute the maximum possible robbery amount and choose the maximum of these two values.

#### Code

```csharp
public class Solution {
    public int Rob(int[] nums) {
        if (nums.Length == 0) return 0;
        if (nums.Length == 1) return nums[0];
        
        // Solve the problem twice based on two scenarios:
        // 1. Rob from 0 to n-2
        // 2. Rob from 1 to n-1
        return Math.Max(RobLine(nums, 0, nums.Length - 2), RobLine(nums, 1, nums.Length - 1));
    }
    
    private int RobLine(int[] nums, int start, int end) {
        int prev1 = 0, prev2 = 0;
        for (int i = start; i <= end; i++) {
            int temp = prev1;
            prev1 = Math.Max(prev2 + nums[i], prev1);
            prev2 = temp;
        }
        return prev1;
    }
}
```

#### Time Complexity

- **O(n)**: We perform two linear scans of the houses.
  
#### Space Complexity

- **O(1)**: No extra space proportional to input size, just a few variables.

### Approach 2: Dynamic Programming Bottom-Up with Reduced Space

#### Intuition

We can approach this problem using dynamic programming, similar to the classic "House Robber I", with the twist of considering the circular aspect by dividing the problem into two linear scenarios. We use two pointers to track the maximum rob amount avoiding extra space for the entire DP array.

#### Code

```csharp
public class Solution {
    public int Rob(int[] nums) {
        if (nums.Length == 1) return nums[0];
        
        // Using the helper function to solve two scenarios:
        int maxExcludeFirst = RobHelper(nums, 1, nums.Length - 1);
        int maxExcludeLast = RobHelper(nums, 0, nums.Length - 2);

        // The answer to the problem is the maximum of these two scenarios
        return Math.Max(maxExcludeFirst, maxExcludeLast);
    }
    
    private int RobHelper(int[] nums, int start, int end) {
        int previous = 0;
        int current = 0;
        
        for (int i = start; i <= end; i++) {
            int newCurrent = Math.Max(current, previous + nums[i]);
            previous = current;
            current = newCurrent;
        }
        
        return current;
    }
}
```

#### Time Complexity

- **O(n)**: Linear scan done twice for each scenario.
  
#### Space Complexity

- **O(1)**: Only a fixed number of variables are used.

