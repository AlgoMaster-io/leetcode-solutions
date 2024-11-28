# 387. [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

## Approach 1: HashMap for Character Frequency

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    public int firstUniqChar(String s) {
        HashMap<Character, Integer> countMap = new HashMap<>();

        // Count the frequency of each character
        for (char c : s.toCharArray()) {
            countMap.put(c, countMap.getOrDefault(c, 0) + 1);
        }

        // Find the first character with frequency 1
        for (int i = 0; i < s.length(); i++) {
            if (countMap.get(s.charAt(i)) == 1) {
                return i;
            }
        }

        return -1; // No unique character found
    }
}
```

## Approach 2: Array for Character Frequency

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int firstUniqChar(String s) {
        int[] charCount = new int[26]; // Array to store frequency of each character

        // Count the frequency of each character
        for (char c : s.toCharArray()) {
            charCount[c - 'a']++;
        }

        // Find the first character with frequency 1
        for (int i = 0; i < s.length(); i++) {
            if (charCount[s.charAt(i) - 'a'] == 1) {
                return i;
            }
        }

        return -1; // No unique character found
    }
}
```

## Approach 3: Single Pass with Index Array

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int firstUniqChar(String s) {
        int[] index = new int[26]; // Array to track occurrences and order of characters
        for (int i = 0; i < 26; i++) {
            index[i] = -1; // Initialize with -1 (not encountered)
        }

        // Traverse string to fill occurrences and order
        for (int i = 0; i < s.length(); i++) {
            int charIndex = s.charAt(i) - 'a';
            if (index[charIndex] == -1) {
                index[charIndex] = i; // First occurrence
            } else {
                index[charIndex] = -2; // Mark as repeated
            }
        }

        // Find the smallest index of a unique character
        int result = Integer.MAX_VALUE;
        for (int i = 0; i < 26; i++) {
            if (index[i] >= 0) {
                result = Math.min(result, index[i]);
            }
        }

        return result == Integer.MAX_VALUE ? -1 : result;
    }
}
```