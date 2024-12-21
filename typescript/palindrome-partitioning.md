[**Leetcode Problem 131: Palindrome Partitioning**](https://leetcode.com/problems/palindrome-partitioning/)

---

## Approaches

1. [Backtracking Approach](#backtracking-approach)

---

### Backtracking Approach

The goal is to partition the string into all possible palindrome partitions. The backtracking approach is excellent for exploring all possible combinations where each combination is validated for palindromic substrings.

#### Intuition

The idea is to use backtracking to partition the string by trying to cut it at each position and recursively checking if the left side is a palindrome. If it is, we continue to find partitions for the right side.

1. Start by iterating over the string `s`.
2. For each iteration, check if the substring from the start to the current position is a palindrome.
3. If the substring is a palindrome, add it to the current path.
4. Recursively call the function for the remaining substring.
5. If the entire string is traversed, the current path is a valid palindrome partition.
6. Backtrack by removing the last element added to the path and explore further.

#### Steps in Code

- Define a helper function `isPalindrome` to check if a given string is a palindrome.
- Define a backtracking function `backtrack` that tries to make partitions such that each partition is a palindrome.

#### Time and Space Complexity

- **Time Complexity:** O(N * 2^N), where N is the length of the string. In the worst case, we are creating every possible partition and checking each substring.
- **Space Complexity:** O(N), due to the recursion depth and space used by the `currentPath`.

```typescript
function isPalindrome(s: string, left: number, right: number): boolean {
    while (left < right) {
        if (s[left] !== s[right]) return false;
        left++;
        right--;
    }
    return true;
}

function backtrack(result: string[][], currentPath: string[], s: string, start: number): void {
    // If we reach the end of the string, add the current path as one of the solutions.
    if (start === s.length) {
        result.push([...currentPath]);
        return;
    }
    
    // Explore each cut position
    for (let end = start; end < s.length; end++) {
        // Check if the current substring is a palindrome
        if (isPalindrome(s, start, end)) {
            // Make a choice
            currentPath.push(s.substring(start, end + 1));
            
            // Recursively call for the next start position
            backtrack(result, currentPath, s, end + 1);
            
            // Backtrack by removing the last choice
            currentPath.pop();
        }
    }
}

function partition(s: string): string[][] {
    const result: string[][] = [];
    backtrack(result, [], s, 0);
    return result;
}
```

This code correctly partitions the input string into all possible lists of palindromic substrings using a recursive backtracking strategy. Each recursive call attempts to find valid palindromic partitions and then backtracks to attempt new pathways.

