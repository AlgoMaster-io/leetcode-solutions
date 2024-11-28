# [1189. Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

## Approach 1: Frequency Count with HashMap

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
import java.util.HashMap;

public class Solution {
    public int maxNumberOfBalloons(String text) {
        HashMap<Character, Integer> countMap = new HashMap<>();

        // Count frequencies of each character in the text
        for (char c : text.toCharArray()) {
            countMap.put(c, countMap.getOrDefault(c, 0) + 1);
        }

        // Check the minimum occurrences of characters required for "balloon"
        int b = countMap.getOrDefault('b', 0);
        int a = countMap.getOrDefault('a', 0);
        int l = countMap.getOrDefault('l', 0) / 2; // 'l' appears twice in "balloon"
        int o = countMap.getOrDefault('o', 0) / 2; // 'o' appears twice in "balloon"
        int n = countMap.getOrDefault('n', 0);

        // Return the minimum number of complete "balloon" words
        return Math.min(Math.min(Math.min(b, a), l), Math.min(o, n));
    }
}
```

## Approach 2: Frequency Count with Array

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int maxNumberOfBalloons(String text) {
        int[] charCount = new int[26]; // Frequency array for all characters

        // Count frequencies of each character in the text
        for (char c : text.toCharArray()) {
            charCount[c - 'a']++;
        }

        // Check the minimum occurrences of characters required for "balloon"
        int b = charCount['b' - 'a'];
        int a = charCount['a' - 'a'];
        int l = charCount['l' - 'a'] / 2; // 'l' appears twice in "balloon"
        int o = charCount['o' - 'a'] / 2; // 'o' appears twice in "balloon"
        int n = charCount['n' - 'a'];

        // Return the minimum number of complete "balloon" words
        return Math.min(Math.min(Math.min(b, a), l), Math.min(o, n));
    }
}
```

## Approach 3: Optimized Single Pass

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int maxNumberOfBalloons(String text) {
        int[] counts = new int[5]; // Indices: 0->b, 1->a, 2->l, 3->o, 4->n

        // Count only relevant characters
        for (char c : text.toCharArray()) {
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
        for (int count : counts) {
            min = Math.min(min, count);
        }

        return min;
    }
}
```