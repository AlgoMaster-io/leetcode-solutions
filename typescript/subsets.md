# [Leetcode 78: Subsets](https://leetcode.com/problems/subsets/)

## Approaches

1. [Recursive Backtracking](#recursive-backtracking)
2. [Iterative Approach](#iterative-approach)
3. [Bit Manipulation](#bit-manipulation)

### Recursive Backtracking

In this approach, we use recursion to explore all possible combinations. We maintain a `current` subset and for each element, we decide whether to include it in the `current` subset or not. This decision leads to two recursive calls, effectively generating all subsets.

#### Intuition
- Start with an empty set and at each element, branch off into two paths: one that includes the element and one that doesn’t.
- This is a recursive tree structure where each node represents one decision point for including or not including a particular element in the current subset. 

```typescript
function subsets(nums: number[]): number[][] {
    const result: number[][] = [];

    function backtrack(start: number, current: number[]) {
        result.push([...current]);  // Add the current subset to the result (clone of current)

        for (let i = start; i < nums.length; i++) {
            current.push(nums[i]);  // Include nums[i] in the current subset
            backtrack(i + 1, current);  // Recurse with remaining elements
            current.pop();  // Backtrack: remove the last element to explore other possibilities
        }
    }
    
    backtrack(0, []);  // Start recursion with an empty set
    return result;
}
```

**Time Complexity:** \(O(2^n \times n)\) - Each subset is created in O(n) time since we create a new array for each subset.

**Space Complexity:** \(O(n)\) - The recursion stack depth is bounded by `n`.

### Iterative Approach

In the iterative approach, we build subsets layer by layer. Starting from an empty set, each element in the input array is added to all existing subsets in the result, generating new subsets.

#### Intuition
- Start with an empty subset.
- For each number in the input list, make copies of all current subsets, add the number to each copy, and add these new subsets to the result list.

```typescript
function subsets(nums: number[]): number[][] {
    let result: number[][] = [[]];  // Start with the empty subset

    for (const num of nums) {
        const newSubsets: number[][] = [];
        for (const current of result) {
            newSubsets.push([...current, num]);  // Add current number to each existing subset
        }
        result.push(...newSubsets);  // Incorporate all new subsets formed in this step
    }

    return result;
}
```

**Time Complexity:** \(O(2^n \times n)\) - We double the size of subsets in each step and each element in the subset is recreated.

**Space Complexity:** \(O(2^n \times n)\) - Store all subsets.

### Bit Manipulation

This method leverages the properties of binary numbers. Each number from `0` to `2^n - 1` can represent a decision of "include/do not include" for each element in the array.

#### Intuition
- Each bit in a number represents whether to include a particular element in the set.
- For a list of size `n`, there are `2^n` possible combinations, each represented uniquely by a bitmask from `0` to `2^n - 1`.

```typescript
function subsets(nums: number[]): number[][] {
    const result: number[][] = [];
    const n = nums.length;
    const numSubsets = 1 << n;  // Calculate 2^n

    for (let i = 0; i < numSubsets; i++) {
        const subset: number[] = [];
        for (let j = 0; j < n; j++) {
            if (i & (1 << j)) {  // Check if the jth bit is set in i
                subset.push(nums[j]);  // Include nums[j] in the subset
            }
        }
        result.push(subset);
    }
    return result;
}
```

**Time Complexity:** \(O(2^n \times n)\) - Iterate over each of the `2^n` subsets and reconstruct them efficiently.

**Space Complexity:** \(O(2^n \times n)\) - Need to store all subsets.

