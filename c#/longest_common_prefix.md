# 14. [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

## Approach 1: Horizontal Scanning

### Solution
csharp
```csharp
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(1)
public class Solution {
    public string LongestCommonPrefix(string[] strs) {
        if (strs == null || strs.Length == 0) {
            return "";
        }

        // Start with the first string as the prefix
        string prefix = strs[0];

        // Compare the prefix with each string in the array
        for (int i = 1; i < strs.Length; i++) {
            while (!strs[i].StartsWith(prefix)) {
                // Shorten the prefix until it matches the start of the current string
                prefix = prefix.Substring(0, prefix.Length - 1);
                if (prefix == "") {
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
csharp
```csharp
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(1)
public class Solution {
    public string LongestCommonPrefix(string[] strs) {
        if (strs == null || strs.Length == 0) {
            return "";
        }

        // Iterate over each character of the first string
        for (int i = 0; i < strs[0].Length; i++) {
            char c = strs[0][i];

            // Compare this character with the same position in all strings
            for (int j = 1; j < strs.Length; j++) {
                // If characters don't match or exceed the string's length
                if (i >= strs[j].Length || strs[j][i] != c) {
                    return strs[0].Substring(0, i); // Return the common prefix
                }
            }
        }

        return strs[0]; // The entire first string is the common prefix
    }
}
```

## Approach 3: Divide and Conquer

### Solution
csharp
```csharp
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(m * log n) where m is the average string length
public class Solution {
    public string LongestCommonPrefix(string[] strs) {
        if (strs == null || strs.Length == 0) {
            return "";
        }

        return LongestCommonPrefix(strs, 0, strs.Length - 1);
    }

    private string LongestCommonPrefix(string[] strs, int left, int right) {
        if (left == right) {
            return strs[left]; // Base case: single string
        }

        // Divide the array into two halves
        int mid = (left + right) / 2;
        string lcpLeft = LongestCommonPrefix(strs, left, mid);
        string lcpRight = LongestCommonPrefix(strs, mid + 1, right);

        // Merge results
        return CommonPrefix(lcpLeft, lcpRight);
    }

    private string CommonPrefix(string left, string right) {
        int minLength = Math.Min(left.Length, right.Length);
        for (int i = 0; i < minLength; i++) {
            if (left[i] != right[i]) {
                return left.Substring(0, i);
            }
        }
        return left.Substring(0, minLength);
    }
}
```

## Approach 4: Binary Search

### Solution
csharp
```csharp
// Time Complexity: O(S * log m) where S is the sum of characters in strings and m is the minimum string length
// Space Complexity: O(1)
public class Solution {
    public string LongestCommonPrefix(string[] strs) {
        if (strs == null || strs.Length == 0) {
            return "";
        }

        int minLen = int.MaxValue;
        foreach (string str in strs) {
            minLen = Math.Min(minLen, str.Length);
        }

        int low = 1, high = minLen;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (IsCommonPrefix(strs, mid)) {
                low = mid + 1; // Try for a longer prefix
            } else {
                high = mid - 1; // Try for a shorter prefix
            }
        }

        return strs[0].Substring(0, (low + high) / 2);
    }

    private bool IsCommonPrefix(string[] strs, int len) {
        string prefix = strs[0].Substring(0, len);
        for (int i = 1; i < strs.Length; i++) {
            if (!strs[i].StartsWith(prefix)) {
                return false;
            }
        }
        return true;
    }
}
```


