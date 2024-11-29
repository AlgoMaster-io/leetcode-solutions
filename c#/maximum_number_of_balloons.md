# 1189. [Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

## Approach 1: Frequency Count with HashMap

### Solution
c#
// Time Complexity: O(n)
// Space Complexity: O(1)
using System.Collections.Generic;

public class Solution {
    public int MaxNumberOfBalloons(string text) {
        Dictionary<char, int> countMap = new Dictionary<char, int>();

        // Count frequencies of each character in the text
        foreach (char c in text) {
            if (countMap.ContainsKey(c)) {
                countMap[c]++;
            } else {
                countMap[c] = 1;
            }
        }

        // Check the minimum occurrences of characters required for "balloon"
        int b = countMap.GetValueOrDefault('b', 0);
        int a = countMap.GetValueOrDefault('a', 0);
        int l = countMap.GetValueOrDefault('l', 0) / 2; // 'l' appears twice in "balloon"
        int o = countMap.GetValueOrDefault('o', 0) / 2; // 'o' appears twice in "balloon"
        int n = countMap.GetValueOrDefault('n', 0);

        // Return the minimum number of complete "balloon" words
        return Math.Min(Math.Min(Math.Min(b, a), l), Math.Min(o, n));
    }
}

## Approach 2: Frequency Count with Array

### Solution
c#
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int MaxNumberOfBalloons(string text) {
        int[] charCount = new int[26]; // Frequency array for all characters

        // Count frequencies of each character in the text
        foreach (char c in text) {
            charCount[c - 'a']++;
        }

        // Check the minimum occurrences of characters required for "balloon"
        int b = charCount['b' - 'a'];
        int a = charCount['a' - 'a'];
        int l = charCount['l' - 'a'] / 2; // 'l' appears twice in "balloon"
        int o = charCount['o' - 'a'] / 2; // 'o' appears twice in "balloon"
        int n = charCount['n' - 'a'];

        // Return the minimum number of complete "balloon" words
        return Math.Min(Math.Min(Math.Min(b, a), l), Math.Min(o, n));
    }
}

## Approach 3: Optimized Single Pass

### Solution
c#
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int MaxNumberOfBalloons(string text) {
        int[] counts = new int[5]; // Indices: 0->b, 1->a, 2->l, 3->o, 4->n

        // Count only relevant characters
        foreach (char c in text) {
            switch (c) {
                case 'b': counts[0]++; break;
                case 'a': counts[1]++; break;
                case 'l': counts[2]++; break;
                case 'o': counts[3]++; break;
                case 'n': counts[4]++; break;
            }
        }
        
        // 'l' and 'o' need to be divided by 2 as they appear twice in "balloon"
        counts[2] /= 2;
        counts[3] /= 2;

        // Return the minimum count
        int min = counts[0];
        foreach (int count in counts) {
            min = Math.Min(min, count);
        }

        return min;
    }
}

