# [Word Break - LeetCode](https://leetcode.com/problems/word-break/)

## Approaches
- [Approach 1: Recursive with Memoization](#approach-1-recursive-with-memoization)
- [Approach 2: Breadth-First Search (BFS)](#approach-2-breadth-first-search-bfs)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)

### Approach 1: Recursive with Memoization

**Intuition**: 

This approach utilizes recursion with memoization to explore all possible partitions of the string `s` and determine if they are fully constructible using the words from the `wordDict`. At each recursive step, we try to split `s` into two parts: a prefix `s[0:i]` that must be a valid word from `wordDict`, and the suffix `s[i:]`. If the prefix is valid, we recursively check if the suffix can also be constructed. We use memoization to store results of previously computed substrings to avoid redundant calculations.

```python
def wordBreak(s: str, wordDict: List[str]) -> bool:
    wordSet = set(wordDict)
    memo = {}

    def canBreak(start):
        # When we are at the end of the string, it means we can break it
        if start == len(s):
            return True
        if start in memo:
            return memo[start]
        for end in range(start + 1, len(s) + 1):
            # Check if the current prefix is a valid word
            if s[start:end] in wordSet and canBreak(end):
                memo[start] = True
                return True
        memo[start] = False
        return False

    return canBreak(0)
```

- **Time Complexity**: O(n^2), where n is the length of the string `s`. Each substring is considered once due to memoization, and generating each substring takes O(n).
- **Space Complexity**: O(n), due to the recursion stack and the memoization dictionary.

---

### Approach 2: Breadth-First Search (BFS)

**Intuition**: 

This approach uses BFS to simulate the traversal of the string `s` as a graph. Each node represents a position in the string and an edge represents a valid word in the dictionary. We use a queue to perform BFS, starting from position 0 and attempting to match every possible word in `wordDict`. If we reach the end of the string, it means a valid segmentation is possible.

```python
from collections import deque

def wordBreak(s: str, wordDict: List[str]) -> bool:
    wordSet = set(wordDict)
    queue = deque([0])
    visited = set()

    while queue:
        start = queue.popleft()
        if start in visited:
            continue
        for end in range(start + 1, len(s) + 1):
            # Check if the current substring is a valid word
            if s[start:end] in wordSet:
                queue.append(end)
                # If we've processed to the end of the string
                if end == len(s):
                    return True
        visited.add(start)

    return False
```

- **Time Complexity**: O(n^2), similar reasoning to the first approach.
- **Space Complexity**: O(n), to maintain the queue and visited set.

---

### Approach 3: Dynamic Programming

**Intuition**: 

This approach uses dynamic programming to decide whether a string can be segmented into words from the dictionary. We use a boolean array `dp` where `dp[i]` is `True` if the first `i` characters of `s` can be segmented to form words from the dictionary. We initialize `dp[0]` to `True` since an empty substring is trivially segmentable, then iterate over the string using nested loops to fill the `dp` array accordingly.

```python
def wordBreak(s: str, wordDict: List[str]) -> bool:
    wordSet = set(wordDict)
    dp = [False] * (len(s) + 1)
    dp[0] = True

    for i in range(1, len(s) + 1):
        for j in range(i):
            # Update the dp table if s[j:i] is a valid word and previous part can be segmented
            if dp[j] and s[j:i] in wordSet:
                dp[i] = True
                break

    return dp[len(s)]
```

- **Time Complexity**: O(n^2), involves checking each substring `s[j:i]` for all possible `j`.
- **Space Complexity**: O(n), storing the DP array of size `n+1`.

---

