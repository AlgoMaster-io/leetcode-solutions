# [Leetcode 14: Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

## Table of Contents
- [Approach 1: Horizontal Scanning](#approach-1-horizontal-scanning)
- [Approach 2: Vertical Scanning](#approach-2-vertical-scanning)
- [Approach 3: Divide and Conquer](#approach-3-divide-and-conquer)
- [Approach 4: Binary Search](#approach-4-binary-search)

## Approach 1: Horizontal Scanning

### Intuition
The idea behind horizontal scanning is to iteratively compare prefixes among the strings. We start by assuming the first string is the common prefix, then we compare this with each subsequent string to determine the actual common prefix.

### Steps
1. Initialize the first string as the initial prefix.
2. Compare this prefix with the strings one by one. Update the prefix by identifying the common prefix between the current prefix and each string.
3. If at any point the prefix becomes empty, return an empty string.

### Code
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.empty()) return "";

        // Start with the first string as the initial prefix
        string prefix = strs[0];
        
        // Compare the prefix with each string in the array
        for (int i = 1; i < strs.size(); ++i) {
            int j = 0;
            while (j < prefix.size() && j < strs[i].size() && prefix[j] == strs[i][j]) {
                j++;
            }
            prefix = prefix.substr(0, j); // Update the prefix
            if (prefix.empty()) return ""; // If prefix is empty, no common prefix exists
        }
        return prefix;
    }
};
```

### Complexity
- **Time Complexity**: O(S), where S is the sum of all characters in all strings. In the worst case, all strings are the same.
- **Space Complexity**: O(1), as we use only constant extra space.

## Approach 2: Vertical Scanning

### Intuition
In vertical scanning, we go through each character in the strings, column by column, and check if every string has the same character at that position.

### Steps
1. Compare characters at the same index across each string.
2. If any string does not match or we reach the end of a string, return the accumulated prefix up to that point.

### Code
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.empty()) return "";

        for (int i = 0; i < strs[0].size(); ++i) {
            char c = strs[0][i];
            for (int j = 1; j < strs.size(); ++j) {
                if (i >= strs[j].size() || strs[j][i] != c) {
                    return strs[0].substr(0, i);
                }
            }
        }
        return strs[0];
    }
};
```

### Complexity
- **Time Complexity**: O(S), where S is the sum of all characters in all strings.
- **Space Complexity**: O(1), as it uses constant extra space.

## Approach 3: Divide and Conquer

### Intuition
This approach involves breaking the problem into smaller subproblems, finding the longest common prefix in each, and then merging those solutions. The idea is similar to the Merge Sort algorithm.

### Steps
1. Recursively split the list of strings into two halves.
2. Find the longest common prefix for each half.
3. Merge results to get the longest common prefix for both halves.

### Code
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.empty()) return "";
        return divideAndConquer(strs, 0, strs.size() - 1);
    }

private:
    string divideAndConquer(const vector<string>& strs, int left, int right) {
        if (left == right) {
            return strs[left];
        }

        int mid = (left + right) / 2;
        string lcpLeft = divideAndConquer(strs, left, mid);
        string lcpRight = divideAndConquer(strs, mid + 1, right);

        return commonPrefix(lcpLeft, lcpRight);
    }

    string commonPrefix(const string& left, const string& right) {
        int minLength = min(left.size(), right.size());
        for (int i = 0; i < minLength; i++) {
            if (left[i] != right[i]) {
                return left.substr(0, i);
            }
        }
        return left.substr(0, minLength);
    }
};
```

### Complexity
- **Time Complexity**: O(S), where S is the sum of all characters in all strings. In practice, the recursion depth is log2(n), which leads to similar complexity as the previous methods.
- **Space Complexity**: O(m * log(n)), where m is the size of the longest common prefix being stored during recursion.

## Approach 4: Binary Search

### Intuition
By using binary search, we can determine the longest common prefix length by checking midpoints. The idea is to exploit the sorted nature of the problem, trying midpoints and adjusting search intervals.

### Steps
1. Calculate the minimum length string to know upper bound.
2. Use binary search on this length to find the longest prefix length.
3. Check if the current prefix length is common to all strings.

### Code
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.empty()) return "";

        int minLen = INT_MAX;
        for (const auto& str : strs) {
            minLen = min(minLen, static_cast<int>(str.size()));
        }

        int low = 1, high = minLen;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (isCommonPrefix(strs, mid)) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return strs[0].substr(0, (low + high) / 2);
    }

private:
    bool isCommonPrefix(const vector<string>& strs, int len) {
        string str1 = strs[0].substr(0, len);
        for (int i = 1; i < strs.size(); ++i) {
            if (strs[i].find(str1) != 0) {
                return false;
            }
        }
        return true;
    }
};
```

### Complexity
- **Time Complexity**: O(S * log(m)), where m is the length of the common prefix and S is the sum of all characters in all strings.
- **Space Complexity**: O(1), as we utilize constant space beyond inputs.

These approaches provide a spectrum of solutions based on different techniques with varying levels of optimization in terms of time and space complexity.

