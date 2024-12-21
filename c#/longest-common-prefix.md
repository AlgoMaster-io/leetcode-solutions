# [Leetcode 14: Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

## Solutions

- [Solution 1: Horizontal Scanning](#solution-1-horizontal-scanning)
- [Solution 2: Vertical Scanning](#solution-2-vertical-scanning)
- [Solution 3: Divide and Conquer](#solution-3-divide-and-conquer)
- [Solution 4: Binary Search](#solution-4-binary-search)

### Solution 1: Horizontal Scanning

The simplest approach to solving "Longest Common Prefix" is to start with the prefix of the first string and compare it with each of the subsequent strings, updating the prefix as necessary. This approach is intuitive but not optimal for very large input sizes.

#### Intuition:
1. Begin with the first string in the list as the potential longest common prefix.
2. Iterate through the rest of the strings.
3. Update the prefix by comparing it with each string's prefix.
4. If at any point, the prefix becomes an empty string, terminate as there's no common prefix.

```csharp
public string LongestCommonPrefix(string[] strs) {
    if (strs == null || strs.Length == 0) return "";
    
    // Start with the first string as the prefix
    string prefix = strs[0];
    for (int i = 1; i < strs.Length; i++) {
        // Iterate until the current prefix matches the start of strs[i]
        while (strs[i].IndexOf(prefix) != 0) {
            prefix = prefix.Substring(0, prefix.Length - 1);
            if (prefix == "") return "";
        }
    }
    return prefix;
}
```

- **Time Complexity**: O(S), where S is the sum of all characters in all strings.
- **Space Complexity**: O(1), only constant space is used.

### Solution 2: Vertical Scanning

This solution entails comparing characters column by column, across all strings simultaneously. The idea is to continue scanning characters column-wise until a mismatch is found.

#### Intuition:
1. Compare character by character from each string.
2. If all characters at the current position are the same, continue to the next position.
3. Stop the moment a mismatch is found or any string is exhausted.

```csharp
public string LongestCommonPrefix(string[] strs) {
    if (strs == null || strs.Length == 0) return "";

    for (int i = 0; i < strs[0].Length; i++) {
        char c = strs[0][i]; // Current character to check
        for (int j = 1; j < strs.Length; j++) {
            // Check if we have gone out of bounds or found a differing character
            if (i == strs[j].Length || strs[j][i] != c) {
                return strs[0].Substring(0, i);
            }
        }
    }
    return strs[0]; // All strings matched the first string completely
}
```

- **Time Complexity**: O(S), where S is the sum of all characters in all strings.
- **Space Complexity**: O(1), only constant space is used.

### Solution 3: Divide and Conquer

This approach involves dividing the array into two parts, finding the longest common prefix for each half, and then merging the results.

#### Intuition:
1. Split the list of strings into two halves.
2. Recursively find the longest common prefix for each half.
3. Merge the results by comparing the prefix obtained from each half.

```csharp
public string LongestCommonPrefix(string[] strs) {
    if (strs == null || strs.Length == 0) return "";
    return LongestCommonPrefix(strs, 0, strs.Length - 1);
}

private string LongestCommonPrefix(string[] strs, int left, int right) {
    if (left == right) {
        return strs[left];
    } else {
        int mid = (left + right) / 2;
        string lcpLeft = LongestCommonPrefix(strs, left, mid);
        string lcpRight = LongestCommonPrefix(strs, mid + 1, right);
        return CommonPrefix(lcpLeft, lcpRight);
    }
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
```

- **Time Complexity**: O(S), where S is the sum of all characters in all strings.
- **Space Complexity**: O(m log n), where m is the length of the shortest string and n is the number of strings.

### Solution 4: Binary Search

This method uses a binary search on the length of the common prefix. It reduces the search space by half each time checking if a particular length is feasible as a common prefix.

#### Intuition:
1. Use binary search to find the minimum length that all strings share as a prefix.
2. Check feasibility of a prefix of a given length by validation across all strings.

```csharp
public string LongestCommonPrefix(string[] strs) {
    if (strs == null || strs.Length == 0) return "";

    int minLen = int.MaxValue;
    foreach (string str in strs) minLen = Math.Min(minLen, str.Length);

    int low = 0;
    int high = minLen;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (IsCommonPrefix(strs, mid)) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    
    return strs[0].Substring(0, (low + high) / 2);
}

private bool IsCommonPrefix(string[] strs, int length) {
    string str1 = strs[0].Substring(0, length);
    for (int i = 1; i < strs.Length; i++) {
        if (!strs[i].StartsWith(str1)) return false;
    }
    return true;
}
```

- **Time Complexity**: O(S log m), where m is the minimum length of strings and S is the sum of all characters in all strings.
- **Space Complexity**: O(1), only constant space is used.

