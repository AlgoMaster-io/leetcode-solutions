# [Leetcode 46: Permutations](https://leetcode.com/problems/permutations/)

## Approaches
- [Approach 1: Backtracking (Recursive)](#approach-1-backtracking-recursive)
- [Approach 2: Iterative Solution (Heap's Algorithm)](#approach-2-iterative-solution-heaps-algorithm)

---

## Approach 1: Backtracking (Recursive)
Backtracking is a common technique used to solve permutation problems. It tries to generate all permutations by choosing an element to be fixed at a position and recursively permuting the remaining elements.

**Intuition:**

1. We recursively generate permutations by swapping elements.
2. For each position, we pick an element and place it at the current position.
3. Recursively solve for the rest of the positions.
4. Once a permutation is complete (when the current position is the last position), we add it to our result set.
5. Swap back to restore the array and continue generating other permutations.

**JavaScript Code:**

```javascript
function permute(nums) {
    const result = [];
    
    function backtrack(start) {
        // Base case: If we've placed all numbers, add the current permutation
        if (start === nums.length) {
            result.push([...nums]);
            return;
        }
        
        for (let i = start; i < nums.length; i++) {
            // Place i-th element at index 'start'
            [nums[start], nums[i]] = [nums[i], nums[start]];
            
            // Recursively place rest of the elements
            backtrack(start + 1);
            
            // Swap back to restore the order for the next iteration
            [nums[start], nums[i]] = [nums[i], nums[start]];
        }
    }
    
    backtrack(0);
    return result;
}
```
**Time Complexity:** O(n * n!)  
**Space Complexity:** O(n!) - For storing all permutations and recursion stack.

---

## Approach 2: Iterative Solution (Heap's Algorithm)
Heap's Algorithm is an elegant iterative solution to generate all permutations. It systematically changes the positions of the elements in order without using additional space for recursion.

**Intuition:**

1. The algorithm generates permutations by using swaps to move elements.
2. It maintains an array (or implicit call stack) of size n to manage the current state of the permutation.
3. For each element, decide whether to fix it, swap it, or do a single circular permutation (generate, swap, revert).
4. It works well iteratively while minimizing swaps.

**JavaScript Code:**

```javascript
function permute(nums) {
    const result = [];
    const c = new Array(nums.length).fill(0);  // Auxiliary array to control swaps
    
    result.push([...nums]);
    
    let i = 0;
    while (i < nums.length) {
        if (c[i] < i) {
            // Swap logic: if i is even, swap 0 and i; if i is odd, swap c[i] and i
            const j = i % 2 === 0 ? 0 : c[i];
            [nums[i], nums[j]] = [nums[j], nums[i]];
            
            result.push([...nums]);
            
            c[i] += 1; // Increment the control variable
            i = 0; // Reset i to 0
        } else {
            c[i] = 0;
            i += 1;
        }
    }
    
    return result;
}
```
**Time Complexity:** O(n * n!)  
**Space Complexity:** O(n * n!) - For storing all permutations. Auxiliary space is O(n) for the counter array.

These solutions offer a comprehensive understanding of the problem, covering both recursive and iterative approaches to generate permutations.

