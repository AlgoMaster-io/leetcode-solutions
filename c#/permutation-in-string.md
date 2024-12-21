[Leetcode 567: Permutation in String](https://leetcode.com/problems/permutation-in-string/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Sliding Window with Count Array](#sliding-window-with-count-array)
3. [Optimized Sliding Window with Two Pointers](#optimized-sliding-window-with-two-pointers)

---

### Brute Force Approach

#### Intuition:
The brute force approach tries every possible substring of length `s1` in `s2` and checks if it is a permutation of `s1`. This problem can be visualized by checking if any subtree of permutations rooted at `s1` exists in `s2`.

#### Steps:
1. Generate all possible substrings of `s2` that have the same length as `s1`.
2. For each substring, check if it is a permutation of `s1` by comparing character counts.

#### Code:
```csharp
public class Solution {
    public bool CheckInclusion(string s1, string s2) {
        int len1 = s1.Length;
        int len2 = s2.Length;

        if (len1 > len2) {
            return false;
        }

        // Count frequency of characters in s1
        int[] s1Count = new int[26];
        foreach (char c in s1) {
            s1Count[c - 'a']++;
        }

        // Iterate over all possible substrings of s2 of length len1
        for (int i = 0; i <= len2 - len1; i++) {
            // Count frequency of characters in the current substring of s2
            int[] s2Count = new int[26];
            for (int j = 0; j < len1; j++) {
                s2Count[s2[i + j] - 'a']++;
            }
            // Compare character counts
            if (MatchCounts(s1Count, s2Count)) {
                return true;
            }
        }
        return false;
    }

    private bool MatchCounts(int[] s1Count, int[] s2Count) {
        for (int i = 0; i < 26; i++) {
            if (s1Count[i] != s2Count[i]) {
                return false;
            }
        }
        return true;
    }
}
```

#### Complexity:
- **Time Complexity**: O((len2 - len1) * 26) due to checking for each substring and comparing character arrays.
- **Space Complexity**: O(1) since we use fixed size arrays.

---

### Sliding Window with Count Array

#### Intuition:
Instead of generating all substrings, use a sliding window approach to maintain character counts in a window over `s2`. Compare this window's character counts with that of `s1`.

#### Steps:
1. Use a fixed-size window of length `s1`.
2. Slide the window right over `s2`, updating the character count for the new character added and the character removed from the left.
3. Check for permutation match at each sliding position.

#### Code:
```csharp
public class Solution {
    public bool CheckInclusion(string s1, string s2) {
        int len1 = s1.Length, len2 = s2.Length;

        if (len1 > len2) {
            return false;
        }

        int[] s1Count = new int[26];
        int[] s2Count = new int[26];

        // Initialize the counts for s1 and the first window
        for (int i = 0; i < len1; i++) {
            s1Count[s1[i] - 'a']++;
            s2Count[s2[i] - 'a']++;
        }

        for (int i = 0; i <= len2 - len1; i++) {
            // If counts match, s2 contains a permutation of s1
            if (MatchCounts(s1Count, s2Count)) {
                return true;
            }
            // Slide the window: remove the first character and add the next character
            if (i + len1 < len2) {
                s2Count[s2[i] - 'a']--;
                s2Count[s2[i + len1] - 'a']++;
            }
        }
        return false;
    }

    private bool MatchCounts(int[] s1Count, int[] s2Count) {
        for (int i = 0; i < 26; i++) {
            if (s1Count[i] != s2Count[i]) {
                return false;
            }
        }
        return true;
    }
}
```

#### Complexity:
- **Time Complexity**: O(len2) due to maintaining constant time checks and updates for the count array.
- **Space Complexity**: O(1) because of fixed size count arrays.

---

### Optimized Sliding Window with Two Pointers

#### Intuition:
The previous sliding window method can be further improved by maintaining a single integer `diff` that counts the number of character mismatches at each point in `s2`.

#### Steps:
1. Initialize two count arrays and a `diff` count.
2. Use two pointers to update the `diff` for the window while sliding through `s2`.
3. If `diff` is zero, it means we have a match for `s1` permutation in `s2`.

#### Code:
```csharp
public class Solution {
    public bool CheckInclusion(string s1, string s2) {
        int len1 = s1.Length, len2 = s2.Length;

        if (len1 > len2) {
            return false;
        }

        int[] s1Count = new int[26];
        int[] s2Count = new int[26];
        int diff = 0;

        // Initialize the counts and diff for s1 and the first window
        for (int i = 0; i < len1; i++) {
            s1Count[s1[i] - 'a']++;
            s2Count[s2[i] - 'a']++;
        }

        for (int i = 0; i < 26; i++) {
            if (s1Count[i] != s2Count[i]) {
                diff++;
            }
        }

        for (int i = 0; i < len2 - len1; i++) {
            if (diff == 0) {
                return true;
            }

            int l = s2[i] - 'a';
            int r = s2[i + len1] - 'a';
            
            // Move the window right
            s2Count[r]++;
            if (s1Count[r] == s2Count[r]) {
                diff--;
            } else if (s1Count[r] + 1 == s2Count[r]) {
                diff++;
            }
            
            s2Count[l]--;
            if (s1Count[l] == s2Count[l]) {
                diff--;
            } else if (s1Count[l] - 1 == s2Count[l]) {
                diff++;
            }
        }

        return diff == 0;
    }
}
```

#### Complexity:
- **Time Complexity**: O(len2), similar to the previous sliding window approach, but optimized with a single check for `diff`.
- **Space Complexity**: O(1), as we utilize fixed-size count arrays and a single integer. 

This optimization maintains the O(n) complexity with reduced overhead in checking conditions, and it's crucial for performance improvements in real-world applications or competitive programming situations.

