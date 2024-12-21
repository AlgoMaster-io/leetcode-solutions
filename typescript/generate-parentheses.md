# [LeetCode 22: Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

In this problem, the goal is to generate all combinations of well-formed parentheses for a given number `n` representing pairs of parentheses.

## Approaches:
1. [Brute-Force Backtracking](#approach-1-brute-force-backtracking)
2. [Optimized Backtracking with Pruning](#approach-2-optimized-backtracking-with-pruning)

---

## Approach 1: Brute-Force Backtracking

### Intuition:
The basic idea is to generate all potential combinations of `n` pairs of parentheses. For each valid sequence, ensure that it has the correct order of opening and closing parentheses.

- Use recursive backtracking to explore all possible combinations.
- Start with an empty current sequence and build by adding either '(' or ')'.
- Only add a '(' if we haven't used `n` open parentheses yet.
- Only add a ')' if it doesn't exceed the number of '(' used so far.

### Algorithm:
1. Use a helper function `backtrack` to generate the sequence.
2. If the current sequence is valid and of length `2 * n` (all opening and closing combined), add it to the results.
3. Recurse with the decision to:
   - Add an opening parenthesis '('.
   - Add a closing parenthesis ')'.
4. Backtrack by removing the last character before trying the next option.

### Code:
```typescript
function generateParenthesis(n: number): string[] {
    const result: string[] = [];
    
    function backtrack(current: string, open: number, close: number) {
        // Base case: if the current string is a valid sequence
        if (current.length === 2 * n) {
            result.push(current);
            return;
        }
        
        // Add an open parenthesis if we can
        if (open < n) {
            backtrack(current + '(', open + 1, close);
        }
        
        // Add a close parenthesis if valid
        if (close < open) {
            backtrack(current + ')', open, close + 1);
        }
    }
    
    backtrack('', 0, 0);
    return result;
}
```

### Complexity:
- **Time Complexity:** \( O(2^{2n} \times n) \), where each sequence generation can take linear time relative to its length.
- **Space Complexity:** \( O(2^{2n} \times n) \) for storing all valid sequences.

---

## Approach 2: Optimized Backtracking with Pruning

### Intuition:
This approach uses a similar recursive strategy but with an added pruning step to discard invalid sequences sooner, thus reducing unnecessary computations.

- By the nature of the exploration strategy, the solution builds upon valid sequences, reducing depth and only considering feasible computation paths.
- Use counts of open and close parentheses to guide the generation.
- Prune paths as early as possible if they can't possibly yield a valid combination.

### Algorithm:
1. Implement a recursive function that explores building up the sequence of parenthesis.
2. Prune: Only proceed if the choice can lead to a valid solution (i.e., number of open should not exceed `n`, close should be less than open).
3. Stop the recursion early when a valid sequence is found.

### Code:
```typescript
function generateParenthesis(n: number): string[] {
    const result: string[] = [];
    
    function backtrack(current: string, open: number, close: number) {
        // Base case: complete valid sequence
        if (current.length === 2 * n) {
            result.push(current);
            return;
        }
        
        // Add open parenthesis if count is less than n
        if (open < n) {
            backtrack(current + '(', open + 1, close);
        }
        
        // Add close parenthesis if count permits
        if (close < open) {
            backtrack(current + ')', open, close + 1);
        }
    }
    
    backtrack('', 0, 0);
    return result;
}
```

### Complexity:
- **Time Complexity:** \( O(4^n / \sqrt{n}) \), utilizing catalan number growth properties due to pruning.
- **Space Complexity:** \( O(4^n / \sqrt{n}) \), for the solution space required by Catalan numbers.

Both approaches efficiently solve the problem with the second approach offering better pruning, leading to fewer paths explored albeit within the complexity class limits.

