# [767. Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Approach 1: Max Heap

### Solution
```java
// Time Complexity: O(n log k), where k is the number of unique characters
// Space Complexity: O(n + k)
import java.util.*;

public class Solution {
    public String reorganizeString(String s) {
        // Frequency map for characters
        int[] charCounts = new int[26];
        for (char c : s.toCharArray()) {
            charCounts[c - 'a']++;
        }

        // Priority queue to keep the most frequent character at the top
        PriorityQueue<int[]> maxHeap = new PriorityQueue<>((a, b) -> b[1] - a[1]);
        for (int i = 0; i < 26; i++) {
            if (charCounts[i] > 0) {
                maxHeap.offer(new int[] {i, charCounts[i]});
            }
        }

        StringBuilder result = new StringBuilder();
        int[] prev = {-1, 0}; // Previously used character and its count

        while (!maxHeap.isEmpty()) {
            int[] current = maxHeap.poll();
            result.append((char) (current[0] + 'a')); // Append current character
            current[1]--; // Decrease its count

            // Re-add the previous character if it's still valid
            if (prev[1] > 0) {
                maxHeap.offer(prev);
            }

            // Update the previous character
            prev = current;
        }

        // If the result length is not the same as the input, return ""
        return result.length() == s.length() ? result.toString() : "";
    }
}
```

## Approach 2: Greedy with Frequency Array

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1) (assuming a fixed alphabet size)
public class Solution {
    public String reorganizeString(String s) {
        int[] charCounts = new int[26];
        int maxCount = 0;
        char maxChar = ' ';

        // Count frequencies and find the most frequent character
        for (char c : s.toCharArray()) {
            charCounts[c - 'a']++;
            if (charCounts[c - 'a'] > maxCount) {
                maxCount = charCounts[c - 'a'];
                maxChar = c;
            }
        }

        // If the most frequent character cannot fit, return ""
        if (maxCount > (s.length() + 1) / 2) {
            return "";
        }

        char[] result = new char[s.length()];
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
                if (index >= result.length) {
                    index = 1; // Start filling odd indices
                }
                result[index] = (char) (i + 'a');
                index += 2;
                charCounts[i]--;
            }
        }

        return new String(result);
    }
}
```