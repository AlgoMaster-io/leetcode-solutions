Here is the solution for the Leetcode problem [Number of Good Ways to Split a String](https://leetcode.com/problems/number-of-good-ways-to-split-a-string/).

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach with Two Passes](#optimized-approach-with-two-passes)

---

### 1. Brute Force Approach

To solve the problem, the naive way would be to create every possible division of the string and check the distinct character condition for each prefix and suffix.

**Intuition:**

- For every position in the string from `1` to `n-1`, split the string into two parts.
- Count distinct characters on the left part and the right part.
- If the counts are the same, it's a valid split.

#### C# Code:

```csharp
public class Solution {
    public int NumSplits(string s) {
        int n = s.Length;
        int goodSplits = 0;

        for (int i = 1; i < n; i++) {
            string left = s.Substring(0, i);
            string right = s.Substring(i, n - i);

            // Use HashSet to count distinct characters
            HashSet<char> leftSet = new HashSet<char>(left);
            HashSet<char> rightSet = new HashSet<char>(right);

            if (leftSet.Count == rightSet.Count) {
                goodSplits++;
            }
        }

        return goodSplits;
    }
}
```

**Time Complexity:** O(n^2) - For each split point, we determine distinct characters which takes linear time.

**Space Complexity:** O(n) - To store distinct characters in hash sets.

---

### 2. Optimized Approach with Two Passes

We can optimize by counting distinct characters in two passes. 

**Intuition:**

- Use two arrays to store counts of unique characters for the left and right parts.
- Use the first pass to calculate and store distinct character counts from left to right.
- Use the second pass to calculate from right to left, comparing the counts dynamically.

This way, we can achieve the solution using just two passes through the array.

#### C# Code:

```csharp
public class Solution {
    public int NumSplits(string s) {
        int n = s.Length;
        int[] leftDistinct = new int[n];
        int[] rightDistinct = new int[n];
        
        HashSet<char> seen = new HashSet<char>();

        // First pass: Calculate distinct counts from left to right
        for (int i = 0; i < n; i++) {
            seen.Add(s[i]);
            leftDistinct[i] = seen.Count;
        }

        seen.Clear(); // Clear the set

        // Second pass: Calculate distinct counts from right to left
        for (int i = n - 1; i >= 0; i--) {
            seen.Add(s[i]);
            rightDistinct[i] = seen.Count;
        }

        int goodSplits = 0;
        // Compare the distinct counts
        for (int i = 0; i < n - 1; i++) {
            if (leftDistinct[i] == rightDistinct[i + 1]) {
                goodSplits++;
            }
        }

        return goodSplits;
    }
}
```

**Time Complexity:** O(n) - The solution involves two linear passes through the string.

**Space Complexity:** O(n) - Arrays to store distinct character counts.

---

This problem highlights how understanding the properties of prefix and suffix arrays can lead to efficient solutions even when the naive approach is computationally expensive.

