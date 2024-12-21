# [Leetcode 214: Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Approaches:
- [Naive Approach (Brute Force)](#naive-approach)
- [Optimal Approach (KMP Algorithm)](#optimal-approach)

### Naive Approach

**Intuition:**

The naive solution involves checking for the longest palindrome starting from the first character of the string. By reversing the rest of the string and appending it to the start, the shortest palindrome can be formed. This approach checks every prefix of the string to find the longest palindrome.

**Steps:**
1. Check every prefix starting from the first character to see if it is a palindrome.
2. Find the longest such palindrome.
3. Append the reversed suffix of the string (the part after this palindrome) to the start of the string to form the shortest palindrome.

**Code in C#:**

```csharp
public class Solution {
    public string ShortestPalindrome(string s) {
        if (string.IsNullOrEmpty(s)) {
            return s;
        }
        
        int end = s.Length - 1;
        // Check each prefix to see if it's a palindrome
        for (int i = end; i >= 0; i--) {
            if (IsPalindrome(s, 0, i)) {
                // Reverse the rest of the remaining string and append it to the start
                string remaining = s.Substring(i + 1, end - i);
                char[] charArray = remaining.ToCharArray();
                Array.Reverse(charArray);
                return new string(charArray) + s;
            }
        }
        
        return "";
    }
    
    private bool IsPalindrome(string s, int start, int end) {
        while (start < end) {
            if (s[start++] != s[end--]) {
                return false;
            }
        }
        return true;
    }
}
```

**Time Complexity:** O(n^2)  
**Space Complexity:** O(n) (for the reverse operation)

### Optimal Approach (KMP Algorithm)

**Intuition:**

A more efficient solution can use the concept of the KMP (Knuth-Morris-Pratt) pattern matching algorithm. The idea is to create a new string which is the original string `s` concatenated with its reversed form, separated by a special character. This helps us construct the longest palindrome prefix efficiently.

**Steps:**
1. Create a new string by concatenating the original string, a separator like '#', and the reversed string.
2. Compute the "longest prefix which is also a suffix" (LPS) array for the combined string as in KMP algorithm.
3. The value of the last position in the LPS array gives the length of the longest palindrome prefix in the original string.
4. Use this value to determine the suffix to reverse and prepend to the original string.

**Code in C#:**

```csharp
public class Solution {
    public string ShortestPalindrome(string s) {
        int n = s.Length;
        string rev = new string(s.Reverse().ToArray());
        string l = s + "#" + rev;
        
        // Compute the LPS (longest prefix which is also a suffix) array
        int[] lps = new int[l.Length];
        for (int i = 1, len = 0; i < l.Length;) {
            if (l[i] == l[len]) {
                lps[i++] = ++len;
            } else if (len > 0) {
                len = lps[len - 1];
            } else {
                lps[i++] = 0;
            }
        }
        
        // Use the LPS array to determine the prefix length
        int heighestMatch = lps[l.Length - 1];
        string suffix = s.Substring(heighestMatch, n - heighestMatch);
        string result = new string(suffix.Reverse().ToArray()) + s;

        return result;
    }
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n) (for the LPS array and additional string manipulations)

Each approach gradually improves in terms of efficiency, with the optimal solution leveraging an insightful application of string manipulation and the KMP algorithm.

