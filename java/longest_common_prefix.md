# 14. [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

## Approach 1: Horizontal Scanning

### Solution
```java
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(1)
public class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }

        // Start with the first string as the prefix
        String prefix = strs[0];

        // Compare the prefix with each string in the array
        for (int i = 1; i < strs.length; i++) {
            while (strs[i].indexOf(prefix) != 0) {
                // Shorten the prefix until it matches the start of the current string
                prefix = prefix.substring(0, prefix.length() - 1);
                if (prefix.isEmpty()) {
                    return ""; // No common prefix
                }
            }
        }

        return prefix;
    }
}
```

## Approach 2: Vertical Scanning

### Solution
```java
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(1)
public class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }

        // Iterate over each character of the first string
        for (int i = 0; i < strs[0].length(); i++) {
            char c = strs[0].charAt(i);

            // Compare this character with the same position in all strings
            for (int j = 1; j < strs.length; j++) {
                // If characters don't match or exceed the string's length
                if (i >= strs[j].length() || strs[j].charAt(i) != c) {
                    return strs[0].substring(0, i); // Return the common prefix
                }
            }
        }

        return strs[0]; // The entire first string is the common prefix
    }
}
```

## Approach 3: Divide and Conquer

### Solution
```java
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(m * log n) where m is the average string length
public class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }

        return longestCommonPrefix(strs, 0, strs.length - 1);
    }

    private String longestCommonPrefix(String[] strs, int left, int right) {
        if (left == right) {
            return strs[left]; // Base case: single string
        }

        // Divide the array into two halves
        int mid = (left + right) / 2;
        String lcpLeft = longestCommonPrefix(strs, left, mid);
        String lcpRight = longestCommonPrefix(strs, mid + 1, right);

        // Merge results
        return commonPrefix(lcpLeft, lcpRight);
    }

    private String commonPrefix(String left, String right) {
        int minLength = Math.min(left.length(), right.length());
        for (int i = 0; i < minLength; i++) {
            if (left.charAt(i) != right.charAt(i)) {
                return left.substring(0, i);
            }
        }
        return left.substring(0, minLength);
    }
}
```

## Approach 4: Binary Search

### Solution
```java
// Time Complexity: O(S * log m) where S is the sum of characters in strings and m is the minimum string length
// Space Complexity: O(1)
public class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }

        int minLen = Integer.MAX_VALUE;
        for (String str : strs) {
            minLen = Math.min(minLen, str.length());
        }

        int low = 1, high = minLen;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (isCommonPrefix(strs, mid)) {
                low = mid + 1; // Try for a longer prefix
            } else {
                high = mid - 1; // Try for a shorter prefix
            }
        }

        return strs[0].substring(0, (low + high) / 2);
    }

    private boolean isCommonPrefix(String[] strs, int len) {
        String prefix = strs[0].substring(0, len);
        for (int i = 1; i < strs.length; i++) {
            if (!strs[i].startsWith(prefix)) {
                return false;
            }
        }
        return true;
    }
}```