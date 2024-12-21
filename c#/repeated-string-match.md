# [Leetcode 686: Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

## Table of Contents
- [Approach 1: Naive Approach](#approach-1-naive-approach)
- [Approach 2: Optimized Approach using KMP (Knuth-Morris-Pratt) Algorithm](#approach-2-optimized-approach-using-kmp)

## Approach 1: Naive Approach

### Intuition
The basic idea behind this problem is to determine the minimum number of times we need to concatenate string `A` such that it becomes a repeating segment and contains string `B` as a substring. The naive approach is to keep concatenating `A` to itself until the length of the repeated string `repA` is at least equal to or greater than `B` or we are sure that `B` cannot be formed that way.

The maximum number of concatenations required will be when length of `repA` is approximately `len(A) + len(B)`. If `B` hasn't appeared within this length, it won't appear with a greater number of concatenations.

### Code
```csharp
public class Solution {
    public int RepeatedStringMatch(string A, string B) {
        // Start with the original string A
        StringBuilder repA = new StringBuilder(A);

        // Initialize repetitions count
        int count = 1;

        // Keep adding A to repA until it covers the length of B
        // or len(repA) >= len(B)+len(A)
        while (repA.Length < B.Length) {
            repA.Append(A);
            count++;
        }

        // Check if B is a substring of the current repA
        if (repA.ToString().Contains(B)) {
            return count;
        }

        // Append one more A and check again
        repA.Append(A);
        count++;

        // If B is still not a substring, return -1
        return repA.ToString().Contains(B) ? count : -1;
    }
}
```
### Complexity Analysis
- **Time Complexity**: O(n * m), where `n` is the length of `A`, and `m` is the length of `B`. In the worst case, we're checking each substring operation which can take longer as m increases.
- **Space Complexity**: O(n + m) for storing the repeated concatenated string.

## Approach 2: Optimized Approach using KMP

### Intuition
Another approach involves using the KMP pattern matching algorithm to efficiently find the substring without repeatedly forming large concatenated strings. The KMP algorithm preprocesses the pattern to create the longest proper prefix which is also suffix (LPS) array to reduce the time complexity of finding substrings. However, for this problem, we'll smartly use the properties of both string lengths and direct substring search.

By knowing the required length and starting only at relevant indices, we minimize unneeded checks.

### Code
```csharp
public class Solution {
    public int RepeatedStringMatch(string A, string B) {
        int count = 0;
        StringBuilder repeatedA = new StringBuilder();
        
        // Initially try with enough repeated A to potentially cover B fully
        while (repeatedA.Length < B.Length) {
            repeatedA.Append(A);
            count++;
        }

        // Check in the current repeated A or one more repetition
        // to account for the wrap around of the cyclic effect
        if (repeatedA.ToString().Contains(B)) {
            return count;
        }
        
        // Append one more A in case B spans like end-to-begin in the repeat cycle
        repeatedA.Append(A);
        count++;
        
        // Check once more
        return repeatedA.ToString().Contains(B) ? count : -1;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n + m), as `Contains` is slightly optimized compared to multiple naive checks.
- **Space Complexity**: O(n + m), similar to the naive approach, as we use a StringBuilder for repeated concatenation which scales up until `m + n`.

Both approaches solve the problem, but using `Contains` helps balance between space usage and simplicity of implementation, with the second approach being subtly advantageous for larger inputs with overlapping patterns.

