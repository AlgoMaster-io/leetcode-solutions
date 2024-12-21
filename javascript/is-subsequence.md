[Leetcode Problem 392: Is Subsequence](https://leetcode.com/problems/is-subsequence/)

- [Approach 1: Two Pointers](#approach-1-two-pointers)
- [Approach 2: Iterative Character Search](#approach-2-iterative-character-search)

### Approach 1: Two Pointers

**Intuition**

The problem can be broken down into checking if all characters of `s` appear in `t` in the same order in which they appear in `s`. For this, we can use two pointers; one for `s` and another for `t`. We'll scan through `t` and check if we can match every character of `s` sequentially. 

The core idea here is to use the pointer on `s` to track how far we've matched the sequence in `t`.

**Algorithm**

1. Initialize two pointers, `sPointer` at the start of `s` and `tPointer` at the start of `t`.
2. Traverse through `t` using `tPointer`:
   - If the current characters pointed by `sPointer` and `tPointer` match, move `sPointer` to the next character.
   - Always move `tPointer` to continue traversing `t`.
3. If `sPointer` has reached the end of `s`, all characters of `s` have been matched in `t` in the same order.

**Code**

```javascript
function isSubsequence(s, t) {
    let sPointer = 0;
    let tPointer = 0;
    
    // Traverse `t` with `tPointer`
    while (tPointer < t.length) {
        // If characters match, move `sPointer`
        if (s[sPointer] === t[tPointer]) {
            sPointer++;
        }
        // Always move `tPointer`
        tPointer++;
    }
    
    // Check if `sPointer` reached the end of `s`
    return sPointer === s.length;
}
```

**Time Complexity**: O(n), where n is the length of `t`. We traverse `t` once while trying to match with `s`.

**Space Complexity**: O(1), no extra space is required.

### Approach 2: Iterative Character Search

**Intuition**

This approach doesn't use two pointers directly but instead iteratively searches each character of `s` in `t` using the `indexOf` method starting from the previous found index + 1. This simulates looking forward step-by-step for each character of `s` in `t`.

**Algorithm**

1. Initialize `prevIndex` to -1 to indicate we haven't yet started searching.
2. Iterate over each character in `s`:
   - For each character `c`, use `indexOf` starting the search from `prevIndex + 1` in `t`.
   - If a character `c` is not found, return false.
   - Update `prevIndex` to the index of `c` found in `t`.
3. If we successfully find all characters in `s`, return true.

**Code**

```javascript
function isSubsequence(s, t) {
    let prevIndex = -1;
    
    for (let i = 0; i < s.length; i++) {
        // Search for the next character starting from `prevIndex + 1`
        const currentChar = s[i];
        prevIndex = t.indexOf(currentChar, prevIndex + 1);
        
        // If character is not found, return false
        if (prevIndex === -1) {
            return false;
        }
    }
    
    return true;
}
```

**Time Complexity**: O(m * n), where m is the length of `s` and n is the length of `t`. In the worst case, each character search runs in O(n).

**Space Complexity**: O(1), no extra space is required outside variable space.

Although the iterative character search is easy to understand, it has worse time complexity. Hence, the two pointers method is optimal for this problem.

