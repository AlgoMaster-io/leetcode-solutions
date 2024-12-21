## [Leetcode Problem 3: Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

### Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window with Set](#approach-2-sliding-window-with-set)
- [Approach 3: Optimized Sliding Window](#approach-3-optimized-sliding-window)

---

### Approach 1: Brute Force

**Intuition:**

The simplest way to solve the problem is by evaluating each possible substring of the given string and checking whether it has all unique characters. This can be achieved by using two nested loops to explore all substrings and an auxiliary function to check if a substring contains all unique characters.

#### Steps:
1. Iterate over each possible starting character of a substring.
2. For each start character, iterate through the end characters forming substrings.
3. Check if the current substring contains all unique characters using a set or an array.
4. Keep track of the maximum length of substrings that contain all unique characters.

```javascript
var lengthOfLongestSubstring = function(s) {
    // A helper function to check if all characters in a substring are unique.
    function allUnique(substring) {
        let charSet = new Set();
        for (let i = 0; i < substring.length; i++) {
            let ch = substring.charAt(i);
            if (charSet.has(ch)) return false; // Character already seen
            charSet.add(ch);
        }
        return true;
    }

    let maxLength = 0;
    for (let i = 0; i < s.length; i++) {
        for (let j = i; j < s.length; j++) {
            if (allUnique(s.slice(i, j + 1))) {
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }
    }
    return maxLength;
};
```

**Time Complexity:** O(n^3) - Due to the generation of all substrings and checking each one for uniqueness.

**Space Complexity:** O(min(n, m)) - Based on the data structure used in `allUnique`.

---

### Approach 2: Sliding Window with Set

**Intuition:**

To improve over the brute force approach, one can use the sliding window technique. This involves using two pointers to maintain a window of characters that are unique, and moving the window when a duplicate character is encountered.

#### Steps:
1. Use two pointers, `i` (start of the window) and `j` (end of the window).
2. Move `j` to expand the window, adding characters to a set.
3. If a duplicate is found, move `i` to shrink the window until duplicates are removed.
4. Keep track of the maximum window size found.

```javascript
var lengthOfLongestSubstring = function(s) {
    let seenChars = new Set();
    let maxLen = 0, i = 0, j = 0;

    while (j < s.length) {
        if (!seenChars.has(s[j])) {
            seenChars.add(s[j]);
            maxLen = Math.max(maxLen, j - i + 1);
            j++;
        } else {
            seenChars.delete(s[i]);
            i++;
        }
    }

    return maxLen;
};
```

**Time Complexity:** O(2n) = O(n) - Each character is processed at most twice (once by `j` and once by `i`).

**Space Complexity:** O(min(n, m)) - To store characters in the set.

---

### Approach 3: Optimized Sliding Window

**Intuition:**

Optimize the sliding window even further by not retracting `i` slowly but by jumping directly to the next position of the duplicate character.

#### Steps:
1. Use a hashmap to remember the latest positions of characters within the window.
2. Traverse the string using a single pointer `j` and maintain the starting index of the window `i`.
3. If the character at position `j` has already appeared, set `i` to the maximum of `i` and the next position of the last occurrence of `s[j]`.
4. Update the maximum length after each character processing.

```javascript
var lengthOfLongestSubstring = function(s) {
    let charIndexMap = new Map();
    let maxLen = 0, i = 0;

    for (let j = 0; j < s.length; j++) {
        if (charIndexMap.has(s[j])) {
            i = Math.max(charIndexMap.get(s[j]) + 1, i); // Update the start of the window
        }
        charIndexMap.set(s[j], j); // Store/update the index of current character
        maxLen = Math.max(maxLen, j - i + 1); // Update max length
    }

    return maxLen;
};
```

**Time Complexity:** O(n) - Each character is processed only once via the hashmap operations.

**Space Complexity:** O(min(n, m)) - Storing indices of the characters in the hashmap. 

---

Each approach successively optimizes the time and/or space complexity while maintaining simplicity and readability of the code as a priority.

