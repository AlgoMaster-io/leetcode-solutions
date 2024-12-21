# [LeetCode 151: Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

## Approaches

1. [Basic Approach - Split, Reverse, and Join](#basic-approach---split-reverse-and-join)
2. [Optimized In-place Approach](#optimized-in-place-approach)

---

### Basic Approach - Split, Reverse, and Join

#### Intuition:
The simplest method to reverse the words in a string is to:
1. Trim the leading and trailing whitespace.
2. Split the string by spaces to get the words.
3. Reverse the order of the words.
4. Join them back into a single string with a single space separating each word.

This approach leverages built-in string and array methods which handle most of the heavy lifting.

#### Code:
```javascript
function reverseWords(s) {
    // Trim the string to remove leading and trailing whitespace
    // Split the string by spaces, the filter removes empty strings between spaces
    let words = s.trim().split(/\s+/);
  
    // Reverse the array of words
    words.reverse();
  
    // Join the words back into a string with a single space separating them
    return words.join(' ');
}
```

#### Complexity Analysis:
- **Time Complexity**: O(N), where N is the length of the string. This complexity arises from the split and join operations.
- **Space Complexity**: O(N), since we use additional space to store the words after splitting the string.

---

### Optimized In-place Approach

#### Intuition:
To improve upon the initial solution, we can manipulate the string in-place which can potentially improve performance by reducing intermediate space usage. The goal is to reduce the number of auxiliary data structures when possible.

Steps:
1. Trim whitespace and iterate over the string to split words manually.
2. Use a stack or a similar structure to hold the words.
3. Manually build the reversed string by popping from the stack.

#### Code:
```javascript
function reverseWords(s) {
    // Helper function to trim and extract the words into a stack
    const words = [];
    let word = '';
    let i = 0;
    const n = s.length;
  
    while (i < n) {
        // Skip spaces
        while (i < n && s[i] === ' ') {
            i++;
        }
        // Gather the word
        while (i < n && s[i] !== ' ') {
            word += s[i];
            i++;
        }
        // If a word was constructed, add it to the stack
        if (word.length > 0) {
            words.push(word);
            word = '';
        }
    }
    
    // Build the reversed string manually from the words stack
    const reversed = [];
    while (words.length > 0) {
        reversed.push(words.pop());
    }
  
    return reversed.join(' ');
}
```

#### Complexity Analysis:
- **Time Complexity**: O(N), where N is the length of the string. We iterate over the string once to extract words and once more to construct the reversed output.
- **Space Complexity**: O(N), for the storage required for the words extracted from the input string.

---

Each solution provides a means to achieve the desired output, with trade-offs between simplicity and potentially lower space usage in practice. The optimized approach is more manual and might perform better in environments where heap allocations are costly.

