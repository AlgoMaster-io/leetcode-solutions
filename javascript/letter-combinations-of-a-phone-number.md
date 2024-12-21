# [Leetcode Problem 17: Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

### Approaches:
1. [Basic Approach: Backtracking Recursive Solution](#basic-approach-backtracking-recursive-solution)
2. [Optimized Approach: Iterative Solution using Queue](#optimized-approach-iterative-solution-using-queue)

## Basic Approach: Backtracking Recursive Solution

### Intuition:
- The problem is about generating combinations, which is well suited for a backtracking strategy.
- Each digit maps to some letters, and combinations can be formed by recursively picking one letter from each digit.
- Think of this as traversing a decision tree, where each level corresponds to a digit, and branches represent the choices (letters) for that digit.

### Detailed Explanation:
- Use a map to associate each digit with its corresponding letters.
- Create a helper function `backtrack(index, path)`.
  - If the `path` is complete (i.e., it has the same length as the input digits), add it to the results.
  - If `path` is not yet complete, extend it by iterating through all possible letters for the current digit and recursively call `backtrack` for each of these extended paths.
  
```javascript
var letterCombinations = function(digits) {
    if (!digits.length) return [];
    
    const digitToLetter = {
        '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
        '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
    };
    
    const results = [];
    
    const backtrack = (index, path) => {
        // If the current combination is complete, add to results
        if (path.length === digits.length) {
            results.push(path.join(''));
            return;
        }
        
        // Get the letters that the current digit maps to, and loop through them
        for (const letter of digitToLetter[digits[index]]) {
            path.push(letter);  // Choose a letter
            backtrack(index + 1, path);  // Explore further
            path.pop();  // Unchoose the letter, i.e., backtrack
        }
    };
    
    backtrack(0, []);
    return results;
};
```

**Time Complexity:** O(3^N * 4^M), where N is the number of digits that map to 3 letters and M is the number of digits that map to 4 letters.  
**Space Complexity:** O(N + M) due to the recursion stack and the path storage.

## Optimized Approach: Iterative Solution using Queue

### Intuition:
- We can use a BFS-like method with a queue to construct each combination iteratively.
- Start with an empty string in the queue, for each digit in digits, extend the combinations in the queue by adding all possible letters for that digit to each item.

### Detailed Explanation:
- Initialize a results array and a queue with an empty string.
- For each digit in the input string:
  - Pop each combination from the front of the queue.
  - Extend this combination by appending each possible letter for the current digit and push the new combination back into the queue.
- At the end, all completed combinations will be in the queue.

```javascript
var letterCombinations = function(digits) {
    if (!digits.length) return [];
    
    const digitToLetter = {
        '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
        '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
    };
    
    let queue = [''];
    
    for (const digit of digits) {
        const letters = digitToLetter[digit];
        const tempQueue = [];  // Temporary queue to hold new combinations
        while (queue.length > 0) {
            const currentCombination = queue.shift();
            for (const letter of letters) {
                tempQueue.push(currentCombination + letter);
            }
        }
        queue = tempQueue;
    }
    
    return queue;
};
```

**Time Complexity:** O(3^N * 4^M), the same as the recursive approach, as all combinations need to be generated.  
**Space Complexity:** O(3^N * 4^M) due to storing all combinations in the queue.

In both methods, the goal is to iterate through the decision paths (or use a queue to simulate that) until we build all possible letter combinations, storing them in a results array to return at the end.

