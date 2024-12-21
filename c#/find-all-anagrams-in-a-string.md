# [Leetcode 438: Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window with Array](#approach-2-sliding-window-with-array)
- [Approach 3: Optimized Sliding Window with HashMap](#approach-3-optimized-sliding-window-with-hashmap)

## Approach 1: Brute Force

### Intuition
The brute force approach involves generating all possible substrings of the string `s` of length equal to `p` and checking if each is an anagram of `p`. Given the constraints, this approach can be inefficient.

### Implementation
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public IList<int> FindAnagrams(string s, string p) {
        List<int> result = new List<int>();
        if (s.Length < p.Length) return result;

        // Sort the string p, as we're going to compare sorted strings
        char[] pChars = p.ToCharArray();
        Array.Sort(pChars);
        string sortedP = new string(pChars);

        for (int i = 0; i <= s.Length - p.Length; i++) {
            // Get the substring of length p from s
            string substring = s.Substring(i, p.Length);
            char[] subChars = substring.ToCharArray();
            Array.Sort(subChars);
            string sortedSub = new string(subChars);

            // Compare the sorted substring to the sorted p
            if (sortedSub == sortedP) {
                result.Add(i);
            }
        }

        return result;
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O((n-m+1) * m log m), where n is the length of `s` and m is the length of `p`. Sorting takes m log m for each of the n-m+1 substrings.
- **Space Complexity**: O(m), for storing the sorted version of `p` and substrings.

## Approach 2: Sliding Window with Array

### Intuition
This method utilizes a sliding window technique to efficiently check for anagrams. We use two frequency arrays to compare the character counts in the current window and the string `p`.

### Implementation
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public IList<int> FindAnagrams(string s, string p) {
        List<int> result = new List<int>();
        if (s.Length < p.Length) return result;

        int[] pCount = new int[26];
        int[] sCount = new int[26];

        // Fill the frequency array for p
        foreach (char c in p) {
            pCount[c - 'a']++;
        }

        // Initial window of size p on s
        for (int i = 0; i < p.Length; i++) {
            sCount[s[i] - 'a']++;
        }

        // Function to compare count arrays
        bool IsAnagram(int[] count1, int[] count2) {
            for (int i = 0; i < 26; i++) {
                if (count1[i] != count2[i]) return false;
            }
            return true;
        }

        if (IsAnagram(pCount, sCount)) {
            result.Add(0);
        }

        // Slide the window
        for (int i = p.Length; i < s.Length; i++) {
            // Add new character to the window
            sCount[s[i] - 'a']++;
            // Remove the first character of the previous window
            sCount[s[i - p.Length] - 'a']--;

            // Compare counts
            if (IsAnagram(pCount, sCount)) {
                result.Add(i - p.Length + 1);
            }
        }

        return result;
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O(n + m), where n is the length of `s` and m is the length of `p`. We are scanning over the length of `s` with constant time operations.
- **Space Complexity**: O(1), as the frequency arrays are of constant size.

## Approach 3: Optimized Sliding Window with HashMap

### Intuition
This method improves upon the previous method by using a more direct comparison, reducing unnecessary checks, and making use of character differences.

### Implementation
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public IList<int> FindAnagrams(string s, string p) {
        List<int> result = new List<int>();
        if (s.Length < p.Length) return result;

        int[] charCount = new int[26];
        foreach (char c in p) {
            charCount[c - 'a']++;
        }

        int left = 0, right = 0, count = p.Length;

        while (right < s.Length) {
            if (charCount[s[right++] - 'a']-- >= 1) {
                count--;
            }
            
            if (count == 0) {
                result.Add(left);
            }

            if (right - left == p.Length && charCount[s[left++] - 'a']++ >= 0) {
                count++;
            }
        }

        return result;
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O(n), where n is the length of `s`. We scan through the string `s` using a single pass.
- **Space Complexity**: O(1), as the array used is of fixed size 26, representing all lowercase letters.

In conclusion, while the brute force approach is a straightforward solution, the sliding window methods significantly improve time efficiency by leveraging character count comparisons. When dealing with anagram or substring problems especially with fixed character sets, sliding windows and count comparisons are often powerful tools.

