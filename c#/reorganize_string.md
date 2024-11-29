# [767. Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Approach 1: Max Heap

### Solution
csharp
```csharp
// Time Complexity: O(n log k), where k is the number of unique characters
// Space Complexity: O(n + k)
using System;
using System.Collections.Generic;

public class Solution {
    public string ReorganizeString(string s) {
        // Frequency map for characters
        int[] charCounts = new int[26];
        foreach (char c in s) {
            charCounts[c - 'a']++;
        }

        // Priority queue to keep the most frequent character at the top
        var maxHeap = new PriorityQueue<(int charIndex, int count), int>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
        for (int i = 0; i < 26; i++) {
            if (charCounts[i] > 0) {
                maxHeap.Enqueue((i, charCounts[i]), charCounts[i]);
            }
        }

        var result = new System.Text.StringBuilder();
        var prev = (-1, 0); // Previously used character and its count

        while (maxHeap.Count > 0) {
            var current = maxHeap.Dequeue();
            result.Append((char)(current.charIndex + 'a')); // Append current character
            current.count--; // Decrease its count

            // Re-add the previous character if it's still valid
            if (prev.count > 0) {
                maxHeap.Enqueue(prev, prev.count);
            }

            // Update the previous character
            prev = current;
        }

        // If the result length is not the same as the input, return ""
        return result.Length == s.Length ? result.ToString() : string.Empty;
    }
}
```

## Approach 2: Greedy with Frequency Array

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1) (assuming a fixed alphabet size)
public class Solution {
    public string ReorganizeString(string s) {
        int[] charCounts = new int[26];
        int maxCount = 0;
        char maxChar = ' ';

        // Count frequencies and find the most frequent character
        foreach (char c in s) {
            charCounts[c - 'a']++;
            if (charCounts[c - 'a'] > maxCount) {
                maxCount = charCounts[c - 'a'];
                maxChar = c;
            }
        }

        // If the most frequent character cannot fit, return ""
        if (maxCount > (s.Length + 1) / 2) {
            return string.Empty;
        }

        char[] result = new char[s.Length];
        int index = 0;

        // Place the most frequent character at even indices
        while (charCounts[maxChar - 'a'] > 0) {
            result[index] = maxChar;
            index += 2;
            charCounts[maxChar - 'a']--;
        }

        // Place the remaining characters
        for (int i = 0; i < 26; i++) {
            while (charCounts[i] > 0) {
                if (index >= result.Length) {
                    index = 1; // Start filling odd indices
                }
                result[index] = (char)(i + 'a');
                index += 2;
                charCounts[i]--;
            }
        }

        return new string(result);
    }
}
```

