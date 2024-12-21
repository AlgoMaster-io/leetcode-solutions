# [Leetcode 3: Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Sliding Window Approach](#sliding-window-approach)
3. [Optimized Sliding Window with HashMap](#optimized-sliding-window-with-hashmap)

### Brute Force Approach

#### Intuition:
The simplest way to solve this problem is to generate all possible substrings of the given string and check each one to see if it has all unique characters. We ultimately want the length of the longest substring that meets this condition.

#### Steps:
1. Iterate through each character in the string, considering it as the starting character of a substring.
2. Extend the substring one character at a time, checking for duplicates with a helper function.
3. Track the maximum length among all substrings with unique characters.

```typescript
function isUnique(substring: string): boolean {
    const charSet = new Set();
    for (let char of substring) {
        if (charSet.has(char)) {
            return false;
        }
        charSet.add(char);
    }
    return true;
}

function lengthOfLongestSubstring(s: string): number {
    let maxLength = 0;

    for (let i = 0; i < s.length; i++) {
        for (let j = i + 1; j <= s.length; j++) {
            if (isUnique(s.substring(i, j))) {
                maxLength = Math.max(maxLength, j - i);
            }
        }
    }

    return maxLength;
}
```

- **Time Complexity:** O(n^3), where n is the number of characters in the input string. Checking all substrings takes O(n^2) and checking if a substring is unique takes O(n).
- **Space Complexity:** O(n), used by the set for checking unique characters.

---

### Sliding Window Approach

#### Intuition:
Instead of generating all substrings and checking them, we can utilize the sliding window technique. The idea is to use two pointers to create a window that can expand and contract as necessary to maintain a substring of unique characters.

#### Steps:
1. Use two pointers, `start` and `end`, both initially set to the start of the string.
2. Expand the `end` pointer to add characters to the current window without repeating any character.
3. If a duplicate character is found, move the `start` pointer past the duplicate.
4. Keep track of the maximum length of the window observed.

```typescript
function lengthOfLongestSubstring(s: string): number {
    let charSet = new Set<string>();
    let maxLength = 0;
    let start = 0;

    for (let end = 0; end < s.length; end++) {
        while (charSet.has(s[end])) {
            charSet.delete(s[start]);
            start++;
        }
        charSet.add(s[end]);
        maxLength = Math.max(maxLength, end - start + 1);
    }

    return maxLength;
}
```

- **Time Complexity:** O(n), because each character is visited at most twice.
- **Space Complexity:** O(min(m, n)), where m is the size of the character set and n is the size of the input string.

---

### Optimized Sliding Window with HashMap

#### Intuition:
The sliding window approach can be further optimized using a hash map to index characters. This allows direct access to skip over segments of the string when a duplicate character is encountered, effectively reducing unnecessary checks and movements of the `start` pointer.

#### Steps:
1. Use a hash map to remember the last index of each character.
2. While iterating through the string with the `end` pointer:
   - If the current character already exists in the map and its index is greater than or equal to `start`, move `start` to `last_index + 1`.
   - Update the hash map with the current character's index.
   - Calculate the length of the current window and update the maximum length if it's longer.

```typescript
function lengthOfLongestSubstring(s: string): number {
    let map = new Map<string, number>();
    let maxLength = 0;
    let start = 0;

    for (let end = 0; end < s.length; end++) {
        const char = s[end];

        if (map.has(char) && map.get(char)! >= start) {
            start = map.get(char)! + 1;
        }
        map.set(char, end);
        maxLength = Math.max(maxLength, end - start + 1);
    }

    return maxLength;
}
```

- **Time Complexity:** O(n), each character is processed at most once.
- **Space Complexity:** O(min(m, n)), where m is the size of the character set and n is the size of the input string.

These solutions cover a progression from a simple brute-force approach to an efficient sliding window optimized with a hash map. Each solution offers a unique way to understand and solve the problem of finding the longest substring without repeating characters.

