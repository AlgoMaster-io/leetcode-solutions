# [Leetcode 456: 132 Pattern](https://leetcode.com/problems/132-pattern/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Monotonic Stack Approach](#monotonic-stack-approach)

---

### Brute Force Approach

**Intuition:**  
The brute force solution involves examining every possible triplet `(i, j, k)` in the given array `nums` to check if it satisfies the "132 Pattern" condition. Specifically, `i < j < k` and `nums[i] < nums[k] < nums[j]`. While this solution will identify the 132 pattern, it is inefficient for large arrays because it checks every possible combination of triplets.

#### Code

```typescript
function find132pattern(nums: number[]): boolean {
    const n = nums.length;
    
    // Check every triplet combination in the array
    for (let i = 0; i < n - 2; i++) {
        for (let j = i + 1; j < n - 1; j++) {
            for (let k = j + 1; k < n; k++) {
                // Check if the current triplet satisfies the 132 condition
                if (nums[i] < nums[k] && nums[k] < nums[j]) {
                    return true;
                }
            }
        }
    }
    
    return false;
}
```

#### Complexity
- **Time Complexity:** O(n^3) because it involves three nested loops to evaluate all triplets.
- **Space Complexity:** O(1) as no additional data structures are used, and space usage is constant.

---

### Monotonic Stack Approach

**Intuition:**  
A more efficient approach is using a monotonic stack, leveraging the reverse iteration and maintaining the `third` element that represents `nums[k]` in the "132 Pattern". By iterating from the end of the array, one can ensure that the `stack` maintains decreasing elements which are potential `nums[k]` in descending order. The `third` variable maintains the maximum valid `nums[k]` found so far to utilize the condition `nums[i] < third`.

#### Code

```typescript
function find132pattern(nums: number[]): boolean {
    const n = nums.length;
    const stack: number[] = [];
    let third = -Infinity;  // This will represent the nums[k] in our pattern

    // Iterating from the end to find the "132" pattern
    for (let i = n - 1; i >= 0; i--) {
        if (nums[i] < third) {
            // nums[i] < nums[k] implies we found a valid pattern
            return true;
        }
        
        // Maintaining stack as a monotonic decreasing stack
        while (stack.length > 0 && nums[i] > stack[stack.length - 1]) {
            // Update `third` to be the highest value we've popped which fits the criteria
            third = stack.pop()!;
        }
        
        stack.push(nums[i]);  // Push current number to stack
    }
    
    return false;
}
```

#### Complexity
- **Time Complexity:** O(n) as each element is pushed and popped from the stack at most once.
- **Space Complexity:** O(n) due to storage in the stack, in the worst case when no element is popped.

