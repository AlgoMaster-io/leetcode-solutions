# 387. [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

## Approach 1: HashMap for Character Frequency

### Solution
c#
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int FirstUniqChar(string s) {
        Dictionary<char, int> countMap = new Dictionary<char, int>();

        // Count the frequency of each character
        foreach (char c in s) {
            if (countMap.ContainsKey(c)) {
                countMap[c]++;
            } else {
                countMap[c] = 1;
            }
        }

        // Find the first character with frequency 1
        for (int i = 0; i < s.Length; i++) {
            if (countMap[s[i]] == 1) {
                return i;
            }
        }

        return -1; // No unique character found
    }
}

## Approach 2: Array for Character Frequency

### Solution
c#
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int FirstUniqChar(string s) {
        int[] charCount = new int[26]; // Array to store frequency of each character

        // Count the frequency of each character
        foreach (char c in s) {
            charCount[c - 'a']++;
        }

        // Find the first character with frequency 1
        for (int i = 0; i < s.Length; i++) {
            if (charCount[s[i] - 'a'] == 1) {
                return i;
            }
        }

        return -1; // No unique character found
    }
}

## Approach 3: Single Pass with Index Array

### Solution
c#
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int FirstUniqChar(string s) {
        int[] index = new int[26]; // Array to track occurrences and order of characters
        for (int i = 0; i < 26; i++) {
            index[i] = -1; // Initialize with -1 (not encountered)
        }

        // Traverse string to fill occurrences and order
        for (int i = 0; i < s.Length; i++) {
            int charIndex = s[i] - 'a';
            if (index[charIndex] == -1) {
                index[charIndex] = i; // First occurrence
            } else {
                index[charIndex] = -2; // Mark as repeated
            }
        }

        // Find the smallest index of a unique character
        int result = int.MaxValue;
        for (int i = 0; i < 26; i++) {
            if (index[i] >= 0) {
                result = Math.Min(result, index[i]);
            }
        }

        return result == int.MaxValue ? -1 : result;
    }
}

