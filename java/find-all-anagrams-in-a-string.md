# [Leetcode 438: Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Approaches
1. [Sliding Window with Two Hash Maps](#approach-1-sliding-window-with-two-hash-maps)
2. [Optimized Sliding Window with Single Array](#approach-2-optimized-sliding-window-with-single-array)

---

## Approach 1: Sliding Window with Two Hash Maps

### Intuition

The problem of finding all anagrams can be interpreted as finding all starting indices of substrings in `s` that are permutations of `p`. One direct way to approach this problem is by using a sliding window technique with the help of hash maps (or frequency arrays) to count character occurrences.

The idea is to maintain two frequency maps:
- One for the characters in the string `p`.
- Another for characters in the current window of `s` with the same length as `p`.

By sliding the window across `s`, we compare the frequency of characters in the current window to those in `p`. If they match, then the start index of this window is an anagram's starting point.

### Code

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();
        if (s.length() < p.length()) return result;

        // Frequency map of characters in p
        Map<Character, Integer> pCount = new HashMap<>();
        for (char c : p.toCharArray()) {
            pCount.put(c, pCount.getOrDefault(c, 0) + 1);
        }

        // Frequency map for the current window in s
        Map<Character, Integer> sCount = new HashMap<>();
        int windowSize = p.length();
        
        // Initialize the window with the first 'windowSize' characters of s
        for (int i = 0; i < windowSize; i++) {
            sCount.put(s.charAt(i), sCount.getOrDefault(s.charAt(i), 0) + 1);
        }
        
        // Iterate over s
        for (int i = 0; i < s.length() - windowSize + 1; i++) {
            // Compare frequency maps
            if (pCount.equals(sCount)) result.add(i);
            
            // Slide the window forward:
            // Remove the old character going out of the window
            char oldChar = s.charAt(i);
            sCount.put(oldChar, sCount.get(oldChar) - 1);
            if (sCount.get(oldChar) == 0) sCount.remove(oldChar);
            
            // Add the new character coming into the window
            if (i + windowSize < s.length()) {
                char newChar = s.charAt(i + windowSize);
                sCount.put(newChar, sCount.getOrDefault(newChar, 0) + 1);
            }
        }
        
        return result;
    }
}
```

### Complexity Analysis

- **Time Complexity:** O(n + m), where n is the length of `s` and m is the length of `p`. We have to scan both strings completely.
- **Space Complexity:** O(1), as the size of the frequency map is constrained by the number of distinct characters (which is fixed at 26 for lowercase English letters).

---

## Approach 2: Optimized Sliding Window with Single Array

### Intuition

By utilizing a fixed-size frequency count array instead of hash maps, we can reduce the constant factors in our solution. Since we're dealing only with lowercase English letters, we can use arrays of size 26 to store frequencies.

The idea remains the same as above: maintain a sliding window of length `p` across the string `s`, but instead of using two hash maps, use two arrays to track the frequencies. This not only brings clarity but also slightly optimizes access and modification times since operations on arrays are generally faster than on hash maps.

### Code

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();
        if (s.length() < p.length()) return result;

        // Frequency arrays for s and p
        int[] pCount = new int[26];
        int[] sCount = new int[26];

        // Initialize the frequency arrays
        for (int i = 0; i < p.length(); i++) {
            pCount[p.charAt(i) - 'a']++;
            sCount[s.charAt(i) - 'a']++;
        }

        // Sliding window over s
        for (int i = 0; i <= s.length() - p.length(); i++) {
            // Check if the current window is an anagram
            if (areArraysEqual(pCount, sCount)) result.add(i);

            // Slide the window
            if (i + p.length() < s.length()) {
                sCount[s.charAt(i) - 'a']--; // Remove old char from the count
                sCount[s.charAt(i + p.length()) - 'a']++; // Add new char to the count
            }
        }

        return result;
    }
    
    private boolean areArraysEqual(int[] arr1, int[] arr2) {
        for (int i = 0; i < arr1.length; i++) {
            if (arr1[i] != arr2[i]) return false;
        }
        return true;
    }
}
```

### Complexity Analysis

- **Time Complexity:** O(n + m), as we are still looping through both strings in their entirety.
- **Space Complexity:** O(1), due to the constant size of the frequency count arrays (26 elements each). 

This approach is more space efficient and slightly faster due to array optimizations, making it the preferred solution for this problem on environments that support fast array operations.

