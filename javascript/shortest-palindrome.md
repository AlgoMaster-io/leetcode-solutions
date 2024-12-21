# [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [KMP Algorithm Approach](#kmp-algorithm-approach)

---

### Brute Force Approach

**Intuition:**

The simplest way to create the shortest palindrome is to repeatedly check if the string `s` is a palindrome by removing one character from the end each time until it is. Once we find the longest prefix which is a palindrome, we can reverse the remainder of the string and add it to the start of `s`.

**Detailed Steps:**
1. Reverse the string `s` to get `rev_s`.
2. Iterate over possible lengths of the string starting from the full length.
3. For each length, check if the substring from the start to this length is a palindrome.
4. If we find such a length, reverse the remainder of string `s` and prepend it to form the shortest palindrome.

**Code:**

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var shortestPalindrome = function(s) {
    function isPalindrome(str) {
        let left = 0, right = str.length - 1;
        while (left < right) {
            if (str[left] !== str[right]) return false;
            left++;
            right--;
        }
        return true;
    }

    if (isPalindrome(s)) return s; // early return if already a palindrome

    let rev_s = s.split('').reverse().join('');
    for (let i = s.length; i >= 0; i--) {
        if (isPalindrome(s.substring(0, i))) {
            return rev_s.slice(0, s.length - i) + s;
        }
    }
    return ""; // This line should never be reached
};
```

**Complexity:**

- **Time Complexity:** \(O(n^2)\) - Checking if each substring is a palindrome takes linear time, and in the worst case, we check for every length.
- **Space Complexity:** \(O(n)\) - For storing the reversed string.

---

### KMP Algorithm Approach

**Intuition:**

Instead of checking every prefix for being a palindrome, we can optimize this by utilizing the concept of the longest prefix which is also a suffix, akin to what we do with the Knuth-Morris-Pratt (KMP) pattern matching algorithm. We create a new string `rev_s + '#' + s` and calculate the longest prefix suffix (LPS) array to find the longest palindromic prefix efficiently.

**Detailed Steps:**
1. Reverse the string `s` to form `rev_s`.
2. Concatenate `s` and `rev_s` with a special character `#` in between to avoid overlaps: `temp = s + "#" + rev_s`.
3. Compute the LPS array for the `temp` string.
4. The last value in the LPS array gives the length of the palindrome that can be formed at the start of the original string `s`.
5. Use the LPS array to determine the characters to prepend to `s` to form the palindrome.

**Code:**

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var shortestPalindrome = function(s) {
    let rev_s = s.split('').reverse().join('');
    let temp = s + '#' + rev_s;
    
    // Build LPS (Longest Prefix Suffix) array
    let lps = new Array(temp.length).fill(0);
    for (let i = 1, length = 0; i < temp.length; ) {
        if (temp[i] === temp[length]) {
            lps[i++] = ++length; // Characters match
        } else if (length) {
            length = lps[length - 1]; // fall back in the pattern
        } else {
            lps[i++] = 0; // no match
        }
    }

    // The LPS of the entire string tells us the longest suffix of `s` which is palindromic
    let pal_length = lps[temp.length - 1];
    return rev_s.slice(0, rev_s.length - pal_length) + s;
};
```

**Complexity:**

- **Time Complexity:** \(O(n)\) - Building the LPS array takes linear time relative to the combined length of the strings.
- **Space Complexity:** \(O(n)\) - To store the temporary string and the LPS array.

Both approaches aim to extend `s` to the shortest possible palindrome. The brute force method is more intuitive but less efficient, while the KMP-based approach effectively finds the solution in linear time using pattern matching concepts.

