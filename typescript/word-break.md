[Leetcode 139: Word Break](https://leetcode.com/problems/word-break/)

### Approaches:
- [Approach 1: Recursive Brute Force](#approach-1-recursive-brute-force)
- [Approach 2: Recursive with Memoization](#approach-2-recursive-with-memoization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)

---

### Approach 1: Recursive Brute Force

The brute force approach is to recursively check all possible substrings of the string `s` and determine if they can be segmented using the words in `wordDict`.

#### Intuition:
1. Start by checking every possible prefix of the string `s`. If a prefix exists in `wordDict`, recursively check the suffix for word break.
2. If the entire string can be segmented into dictionary words, return true; otherwise return false.

#### Time Complexity:
- Exponential, as it checks all possible partitions of the string.

#### Space Complexity:
- O(n) due to the recursion stack, where n is the length of string `s`.

```typescript
function wordBreakRecursive(s: string, wordDict: string[]): boolean {
    const wordSet = new Set(wordDict);

    function canBreak(start: number): boolean {
        if (start === s.length) return true; // Base case: reached the end of the string

        for (let end = start + 1; end <= s.length; end++) {
            const word = s.substring(start, end);

            // Check if the prefix is in the word dictionary
            if (wordSet.has(word) && canBreak(end)) {
                return true;
            }
        }

        return false; // No valid segmentation found
    }

    return canBreak(0);
}
```

---

### Approach 2: Recursive with Memoization

To improve the recursive solution, we employ memoization to cache results of subproblems. This prevents redundant computations for the same substring.

#### Intuition:
1. Use a map to store the results of subproblems to avoid repeated work.
2. If a substring starting at `start` index has already been computed, use the cached result.

#### Time Complexity:
- O(n^2 * m), where n is the length of `s` and m is the maximum length of the word in `wordDict`.

#### Space Complexity:
- O(n + m), to store the memoization map and recursion stack.

```typescript
function wordBreakMemo(s: string, wordDict: string[]): boolean {
    const wordSet = new Set(wordDict);
    const memo = new Map<number, boolean>();

    function canBreak(start: number): boolean {
        if (start === s.length) return true;
        if (memo.has(start)) return memo.get(start)!;

        for (let end = start + 1; end <= s.length; end++) {
            const word = s.substring(start, end);
            if (wordSet.has(word) && canBreak(end)) {
                memo.set(start, true);
                return true;
            }
        }

        memo.set(start, false);
        return false;
    }

    return canBreak(0);
}
```

---

### Approach 3: Dynamic Programming

This approach uses a dynamic programming array to store subproblem solutions. The dp array is of size `n+1` where `n` is the length of string `s`, and `dp[i]` stores whether the substring `s[0:i]` can be segmented.

#### Intuition:
1. Initialize `dp[0]` to true since an empty string can be segmented trivially.
2. For each position `i`, check all substrings `s[j:i]` and see if `s[0:j]` can be segmented and `s[j:i]` is in `wordDict`.
3. If true, set `dp[i]` to true, indicating that `s[0:i]` can be segmented.

#### Time Complexity:
- O(n^2), where n is the length of `s`.

#### Space Complexity:
- O(n), for the dp array.

```typescript
function wordBreakDP(s: string, wordDict: string[]): boolean {
    const wordSet = new Set(wordDict);
    const dp = new Array(s.length + 1).fill(false);
    
    // Base case: empty string can always be segmented
    dp[0] = true;

    for (let i = 1; i <= s.length; i++) {
        for (let j = 0; j < i; j++) {
            // Check if s[j:i-1] is a word and s[0:j] can be segmented properly
            if (dp[j] && wordSet.has(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }

    return dp[s.length];
}
```

---

