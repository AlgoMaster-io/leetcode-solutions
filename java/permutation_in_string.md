# [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n * m), where n is the length of s1 and m is the length of s2
// Space Complexity: O(n)
import java.util.Arrays;

public class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int n = s1.length(), m = s2.length();

        if (n > m) return false;

        // Sort s1
        char[] s1Arr = s1.toCharArray();
        Arrays.sort(s1Arr);
        String sortedS1 = new String(s1Arr);

        // Check every substring of length n in s2
        for (int i = 0; i <= m - n; i++) {
            String sub = s2.substring(i, i + n);
            char[] subArr = sub.toCharArray();
            Arrays.sort(subArr);

            if (sortedS1.equals(new String(subArr))) {
                return true;
            }
        }

        return false;
    }
}
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
```java
// Time Complexity: O(m), where m is the length of s2
// Space Complexity: O(1)
public class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length()) return false;

        int[] s1Freq = new int[26];
        int[] s2Freq = new int[26];

        // Frequency count for s1
        for (char c : s1.toCharArray()) {
            s1Freq[c - 'a']++;
        }

        // Sliding window
        for (int i = 0; i < s2.length(); i++) {
            s2Freq[s2.charAt(i) - 'a']++;

            // Remove the leftmost character from the window
            if (i >= s1.length()) {
                s2Freq[s2.charAt(i - s1.length()) - 'a']--;
            }

            // Compare the frequency arrays
            if (Arrays.equals(s1Freq, s2Freq)) {
                return true;
            }
        }

        return false;
    }
}
```