# [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n * m), where n is the length of s and m is the length of p
// Space Complexity: O(m) for frequency array
import java.util.*;

public class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();
        int[] pFreq = new int[26];

        // Frequency count for string p
        for (char c : p.toCharArray()) {
            pFreq[c - 'a']++;
        }

        // Check every substring of length p in s
        for (int i = 0; i <= s.length() - p.length(); i++) {
            int[] sFreq = new int[26];

            // Count frequency of the current substring
            for (int j = i; j < i + p.length(); j++) {
                sFreq[s.charAt(j) - 'a']++;
            }

            // Compare frequencies
            if (Arrays.equals(sFreq, pFreq)) {
                result.add(i);
            }
        }

        return result;
    }
}
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
```java
// Time Complexity: O(n), where n is the length of s
// Space Complexity: O(1) (fixed frequency array of size 26)
import java.util.*;

public class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();
        int[] pFreq = new int[26];
        int[] sFreq = new int[26];

        // Frequency count for string p
        for (char c : p.toCharArray()) {
            pFreq[c - 'a']++;
        }

        int left = 0, right = 0;

        // Sliding window over s
        while (right < s.length()) {
            // Add the current character to the window
            sFreq[s.charAt(right) - 'a']++;

            // Check if the window size matches p
            if (right - left + 1 == p.length()) {
                // Compare frequencies
                if (Arrays.equals(sFreq, pFreq)) {
                    result.add(left);
                }

                // Remove the leftmost character from the window
                sFreq[s.charAt(left) - 'a']--;
                left++;
            }

            right++;
        }

        return result;
    }
}
```