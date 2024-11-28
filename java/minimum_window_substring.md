# [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
import java.util.HashMap;

public class Solution {
    public String minWindow(String s, String t) {
        if (t.length() > s.length()) return "";

        String result = "";
        int minLength = Integer.MAX_VALUE;

        // Iterate over all substrings
        for (int i = 0; i < s.length(); i++) {
            for (int j = i; j < s.length(); j++) {
                String sub = s.substring(i, j + 1);

                if (containsAll(sub, t)) {
                    if (sub.length() < minLength) {
                        minLength = sub.length();
                        result = sub;
                    }
                }
            }
        }

        return result;
    }

    private boolean containsAll(String sub, String t) {
        HashMap<Character, Integer> map = new HashMap<>();

        for (char c : t.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }

        for (char c : sub.toCharArray()) {
            if (map.containsKey(c)) {
                map.put(c, map.get(c) - 1);
                if (map.get(c) == 0) {
                    map.remove(c);
                }
            }
        }

        return map.isEmpty();
    }
}
```

## Approach 2: Sliding Window with Frequency Count (Optimal)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    public String minWindow(String s, String t) {
        if (t.length() > s.length()) return "";

        HashMap<Character, Integer> tFreq = new HashMap<>();
        for (char c : t.toCharArray()) {
            tFreq.put(c, tFreq.getOrDefault(c, 0) + 1);
        }

        HashMap<Character, Integer> windowFreq = new HashMap<>();
        int left = 0, right = 0, matched = 0;
        int minLength = Integer.MAX_VALUE;
        int start = 0;

        while (right < s.length()) {
            char c = s.charAt(right);
            windowFreq.put(c, windowFreq.getOrDefault(c, 0) + 1);

            if (tFreq.containsKey(c) && windowFreq.get(c).equals(tFreq.get(c))) {
                matched++;
            }

            while (matched == tFreq.size()) {
                // Update the result if this window is smaller
                if (right - left + 1 < minLength) {
                    minLength = right - left + 1;
                    start = left;
                }

                // Shrink the window
                char leftChar = s.charAt(left);
                if (tFreq.containsKey(leftChar) && windowFreq.get(leftChar).equals(tFreq.get(leftChar))) {
                    matched--;
                }
                windowFreq.put(leftChar, windowFreq.get(leftChar) - 1);
                if (windowFreq.get(leftChar) == 0) {
                    windowFreq.remove(leftChar);
                }
                left++;
            }

            right++;
        }

        return minLength == Integer.MAX_VALUE ? "" : s.substring(start, start + minLength);
    }
}
```