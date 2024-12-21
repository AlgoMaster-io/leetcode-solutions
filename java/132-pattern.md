# [Leetcode 456: 132 Pattern](https://leetcode.com/problems/132-pattern/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Stacks](#approach-2-using-stacks)

## Approach 1: Brute Force

### Intuition
The brute force approach involves checking every possible triplet to see if it forms a 132 pattern. The 132 pattern means finding three indices `i, j, k` such that `i < j < k` and `nums[i] < nums[k] < nums[j]`.

### Steps
1. Loop over the array and fix `i`.
2. For every fixed `i`, fix `j` and ensure `j > i`.
3. For every fixed `j`, loop over the array to find an element `k`, where `k > j` and `nums[i] < nums[k] < nums[j]`.
4. If such a triplet is found, return true.
5. If after checking all possibilities, no triplet is found, return false.

### Code
```java
public boolean find132pattern(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            for (int k = j + 1; k < nums.length; k++) {
                // Check the pattern condition
                if (nums[i] < nums[k] && nums[k] < nums[j]) {
                    return true;
                }
            }
        }
    }
    return false;
}
```

### Complexity Analysis
- **Time Complexity**: O(n^3), where `n` is the length of the array, due to the three nested loops.
- **Space Complexity**: O(1), no extra space used.

## Approach 2: Using Stacks

### Intuition
To optimize, we can use a stack to maintain candidates for the second largest number (`nums[j]`) in the pattern 132 by iterating from the right. We will also maintain a variable `nums[k]` as the second number in the 132 pattern.

### Steps
1. Traverse the array from the right.
2. Keep a variable `third` to track the maximum number less than `nums[j]` which can be a candidate for `nums[k]`.
3. Use a stack to maintain the numbers which can potentially be `nums[j]`.
4. For each element `nums[i]`, check:
    - If `nums[i]` is less than `third`, a valid 132 pattern is found.
5. If no 132 pattern is found by the end of the loop, return false.

### Code
```java
public boolean find132pattern(int[] nums) {
    if (nums.length < 3) return false;
    
    int third = Integer.MIN_VALUE;
    Stack<Integer> stack = new Stack<>();
    
    for (int i = nums.length - 1; i >= 0; i--) {
        // Check if the current number can be nums[i] in 132 pattern
        if (nums[i] < third) {
            return true;
        }
        
        // Maintain the stack such that any number in it is a potential nums[j]
        // And update third to be the maximum valid nums[k]
        while (!stack.isEmpty() && nums[i] > stack.peek()) {
            third = stack.pop();
        }
        
        // Push the current number as potential nums[j]
        stack.push(nums[i]);
    }
    
    return false;
}
```

### Complexity Analysis
- **Time Complexity**: O(n), since each element is pushed and popped from the stack at most once.
- **Space Complexity**: O(n), for the stack used to store potential `nums[j]` candidates.

