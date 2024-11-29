# 40. [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

## Approach 1: Backtracking with Duplicate Handling

### Solution
typescript
```typescript
// Time Complexity: O(2^n), where n is the number of candidates
// Space Complexity: O(k), where k is the average length of a combination

function combinationSum2(candidates: number[], target: number): number[][] {
    const result: number[][] = [];
    
    candidates.sort((a, b) => a - b); // Sort the candidates to handle duplicates
    
    function backtrack(start: number, target: number, current: number[]): void {
        if (target === 0) {
            result.push([...current]); // Add the valid combination to the result
            return;
        }
        
        for (let i = start; i < candidates.length; i++) {
            if (i > start && candidates[i] === candidates[i - 1]) {
                continue; // Skip duplicates
            }
            if (candidates[i] > target) {
                break; // Stop the loop if the current candidate exceeds the target
            }

            current.push(candidates[i]); // Choose the current candidate
            backtrack(i + 1, target - candidates[i], current); // Recurse with the next index
            current.pop(); // Backtrack to explore other combinations
        }
    }

    backtrack(0, target, []);
    return result;
}
```

## Approach 2: Iterative with Stack (Less Common)

### Solution
typescript
```typescript
// Time Complexity: O(2^n), where n is the number of candidates
// Space Complexity: O(k), where k is the average length of a combination

function combinationSum2Iterative(candidates: number[], target: number): number[][] {
    const result: number[][] = [];
    
    candidates.sort((a, b) => a - b); // Sort the candidates to handle duplicates
    
    const stack: [number[], number, number][] = [[[], target, -1]];
    
    while (stack.length > 0) {
        const [current, remainingTarget, previousIndex] = stack.pop()!;
        
        if (remainingTarget === 0) {
            result.push([...current]); // Add valid combinations
            continue;
        }
        
        for (let i = previousIndex + 1; i < candidates.length; i++) {
            if (i > previousIndex + 1 && candidates[i] === candidates[i - 1]) {
                continue; // Skip duplicates
            }
            if (candidates[i] > remainingTarget) {
                break; // Stop the loop if the current candidate exceeds the target
            }
            
            current.push(candidates[i]);
            stack.push([[...current], remainingTarget - candidates[i], i]);
            current.pop();
        }
    }

    return result;
}
```

