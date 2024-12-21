# [456. 132 Pattern](https://leetcode.com/problems/132-pattern/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using A Stack and a Mono Stack Argument](#approach-2-using-a-stack-and-a-mono-stack-argument)

---

## Approach 1: Brute Force

### Intuition
The simplest way to find the "132 pattern" in the array is to examine every possible triplet `nums[i], nums[j], nums[k]` where `i < j < k`. We need to check if these elements fulfill the pattern where `nums[i] < nums[k] < nums[j]`.

### Steps
1. Loop through each possible `i` from 0 to `len(nums) - 3`.
2. For each `i`, loop through each possible `j` where `j > i`.
3. For each `j`, loop through each possible `k` where `k > j`.
4. Check if `nums[i] < nums[k] < nums[j]`.
5. If such a triplet is found, return `true`.

### Code
```javascript
function find132pattern(nums) {
    const n = nums.length;
    for (let i = 0; i < n - 2; i++) {
        for (let j = i + 1; j < n - 1; j++) {
            for (let k = j + 1; k < n; k++) {
                // Check if the current triplet forms a 132 pattern
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
- **Time Complexity:** O(n^3), where n is the length of nums. This is because we have three nested loops.
- **Space Complexity:** O(1), as we aren't using any additional space besides a few loops.

---

## Approach 2: Using A Stack and a Mono Stack Argument

### Intuition
A more optimal way is to use a stack to maintain a potential "32" part of the pattern, iterating from right to left. We aim to track the largest possible "2" that is smaller than the most recently found "3". This allows us to maintain conditions to find a "132" pattern efficiently:

1. Use a stack as a decreasing mono stack (to find 3, 2 patterns).
2. Iterate backwards and use the stack to keep track of elements seen as potential "2".

### Steps
1. Initialize a variable `third` to negative infinity. This variable represents the "2" in the "132" pattern sequence.
2. Initialize an empty stack that will keep track of potential "3" values.
3. Iterate backward through the array.
4. For each element, check if it is less than the current third. If so, we've found a "132" pattern.
5. While the stack is not empty and the current element is larger than the stack's top, pop the stack and update third to the popped value (this value is a potential "3").
6. Push the current element onto the stack as a potential "3".
7. If no pattern is found after traversing all elements, return false.

### Code
```javascript
function find132pattern(nums) {
    let third = -Infinity;
    const stack = [];
    
    // Traverse from right to left
    for (let i = nums.length - 1; i >= 0; i--) {
        // If we find any number smaller than the third, pattern is found
        if (nums[i] < third) {
            return true;
        }
        
        // Maintain the decreasing order in the stack, adjust 'third'
        while (stack.length > 0 && stack[stack.length - 1] < nums[i]) {
            third = stack.pop();
        }
        
        // Push the current number as a potential "3"
        stack.push(nums[i]);
    }
    
    return false;
}
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the length of nums. We traverse the list once, with each element being pushed and popped from the stack only once.
- **Space Complexity:** O(n), due to the stack usage. In the worst case, all elements could be pushed into the stack.

