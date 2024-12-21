# [Leetcode 78: Subsets](https://leetcode.com/problems/subsets/)

## Approaches
1. [Recursive Backtracking](#recursive-backtracking)
2. [Iterative (Cascading)](#iterative-cascading)
3. [Bit Manipulation](#bit-manipulation)

---

### 1. Recursive Backtracking

The idea of this approach is to use recursion to generate all possible subsets. Start by choosing or not choosing each number and continue this process recursively until you have considered all numbers. This approach explores all possibilities.

#### Intuition

- Treat this problem as a decision tree where at each level you decide whether to include a specific element in the current subset.
- Each time you 'pick' an element, you move to a deeper level until you reach the end of the list.

```javascript
var subsets = function(nums) {
    const result = [];
    
    const backtrack = (start, subset) => {
        // Add the current subset to the result
        result.push([...subset]);
        
        for (let i = start; i < nums.length; i++) {
            // Include this element
            subset.push(nums[i]);
            // Move onto the next element
            backtrack(i + 1, subset);
            // Backtrack, remove the last element
            subset.pop();
        }
    };
    
    // Start backtracking with an empty subset
    backtrack(0, []);
    
    return result;
};

// Time Complexity: O(2^n) - Each element can either be present or absent.
// Space Complexity: O(n) - The recursion stack can go as deep as the length of the nums array.
```

---

### 2. Iterative (Cascading)

Instead of generating subsets recursively, we build up the list of subsets iteratively. For each number in the nums list, add it to every existing subset to create new subsets.

#### Intuition

- Start with an empty subset. As you iterate over the elements in nums, add each element to all existing subsets to create new subsets.
- This approach is visualized as a cascading process where for every element, subsets expand by including that element.

```javascript
var subsets = function(nums) {
    const result = [[]]; // Start with the empty subset

    for (let num of nums) {
        // Get the current size of the result
        const size = result.length;
        
        for (let i = 0; i < size; i++) {
            // Create a new subset by copying the existing subset and adding the current number
            const newSubset = [...result[i], num];
            result.push(newSubset);
        }
    }
    
    return result;
};

// Time Complexity: O(2^n) - For each number, we double the number of subsets (by copying and expanding existing subsets).
// Space Complexity: O(2^n) - We store all possible subsets.
```

---

### 3. Bit Manipulation

The key observation is that there are `2^n` possible subsets for a list with `n` elements. Each element can be either included (1) or excluded (0) in a subset. Thus, every subset can be represented as a binary string of length n.

#### Intuition

- Use the binary representation of numbers from `0` to `2^n - 1` to determine whether to include each element.
- If the i-th bit of the number is set, include nums[i] in the subset.

```javascript
var subsets = function(nums) {
    const n = nums.length;
    const totalSubsets = Math.pow(2, n);
    const result = [];

    for (let i = 0; i < totalSubsets; i++) {
        const subset = [];
        
        for (let j = 0; j < n; j++) {
            // Check if the j-th bit in integer i is set
            if (i & (1 << j)) {
                subset.push(nums[j]);
            }
        }
        
        result.push(subset);
    }
    
    return result;
};

// Time Complexity: O(n * 2^n) - We have to iterate through 2^n subsets and for each subset, we may check up to n elements.
// Space Complexity: O(2^n) - Storing all subsets.
```

---

Each of these approaches provides a different perspective on solving the problem, from learning recursion in the Backtracking method, understanding iteration in Cascading, to appreciating the elegance of Bit Manipulation for generating subsets. Choose the one best suited for your understanding or optimization needs!

