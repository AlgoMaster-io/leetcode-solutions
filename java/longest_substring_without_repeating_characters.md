# [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(n)
import java.util.HashSet;

public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int maxLength = 0;

        // Check every possible substring
        for (int i = 0; i < s.length(); i++) {
            HashSet<Character> seen = new HashSet<>();
            int length = 0;

            for (int j = i; j < s.length(); j++) {
                if (seen.contains(s.charAt(j))) {
                    break; // Break if a duplicate character is found
                }
                seen.add(s.charAt(j));
                length++;
                maxLength = Math.max(maxLength, length); // Update maxLength
            }
        }

        return maxLength;
    }
}
```

## Approach 2: Sliding Window with HashSet

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashSet;

public class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashSet<Character> set = new HashSet<>();
        int maxLength = 0;
        int left = 0;

        // Sliding window
        for (int right = 0; right < s.length(); right++) {
            while (set.contains(s.charAt(right))) {
                set.remove(s.charAt(left)); // Shrink the window
                left++;
            }
            set.add(s.charAt(right)); // Expand the window
            maxLength = Math.max(maxLength, right - left + 1); // Update maxLength
        }

        return maxLength;
    }
}
```

## Approach 3: Approach 3: Sliding Window with HashMap (Optimal)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashMap<Character, Integer> map = new HashMap<>();
        int maxLength = 0;
        int left = 0;

        // Sliding window
        for (int right = 0; right < s.length(); right++) {
            char currentChar = s.charAt(right);

            // If the character is already in the map, update the left pointer
            if (map.containsKey(currentChar)) {
                left = Math.max(left, map.get(currentChar) + 1);
            }

            // Update the character's last index in the map
            map.put(currentChar, right);

            // Update maxLength
            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```