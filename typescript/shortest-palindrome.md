# [Leetcode 214: Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [KMP-based Approach](#kmp-based-approach)

### Brute Force Approach

#### Intuition:
The idea is to attempt to convert the string into a palindrome by adding characters to the beginning. We'll repeatedly check the longest palindromic substring starting at the beginning until we find a substring that covers the entire string.

#### Steps:
1. Reverse the given string and append it to the original with a separator.
2. Using a loop, generate substrings from the beginning and check if they are a palindrome.
3. Determine how many characters we need to prepend, which will be the non-overlapping part after the palindrome.
4. Return the shortest palindrome by adding the necessary characters in front.

#### Code:

```typescript
function shortestPalindrome(s: string): string {
    // Helper function to check if a string is a palindrome
    const isPalindrome = (str: string): boolean => {
        let left = 0, right = str.length - 1;
        while (left < right) {
            if (str[left] !== str[right]) return false;
            left++;
            right--;
        }
        return true;
    };

    // Iterate over the string
    for (let i = s.length; i >= 0; i--) {
        // Check if the substring s[0:i] is a palindrome
        if (isPalindrome(s.substring(0, i))) {
            // If it is, then we add the reverse of the remaining part in front
            return s.substring(i).split('').reverse().join('') + s;
        }
    }
    return "";  // Edge case, should not occur
}
```

#### Complexity:
- **Time Complexity**: O(n^2), where n is the length of the string, due to checking each substring.
- **Space Complexity**: O(n) required for generating substrings and reverse.

---

### KMP-based Approach

#### Intuition:
This optimal approach leverages the KMP (Knuth-Morris-Pratt) pattern matching algorithm to identify the longest prefix of the string which is also a palindrome. We create a temporary string that includes the original string and its reverse to facilitate using the KMP table to find the overlap.

#### Steps:
1. Create a temporary string consisting of the original string, a separator, and the reversed original string.
2. Generate the LPS (longest prefix suffix) array for the temporary string using the KMP preprocessing approach.
3. The value in the last position of the LPS array gives the count of characters belonging to the longest palindromic prefix.
4. Append the necessary characters from the reverse of the original in front to form the shortest palindrome.

#### Code:

```typescript
function shortestPalindrome(s: string): string {
    if (s.length <= 1) return s; // Edge case for single character or empty string

    // Construct the temporary KMP string format
    const combinedStr = s + '#' + s.split('').reverse().join('');
    const lps = new Array(combinedStr.length).fill(0);

    // Fill the lps array based on KMP preprocessing logic
    for (let i = 1, length = 0; i < combinedStr.length; ) {
        if (combinedStr[i] === combinedStr[length]) {
            lps[i++] = ++length;
        } else if (length > 0) {
            length = lps[length - 1];
        } else {
            lps[i++] = 0;
        }
    }

    // The last element of lps array gives the length of the longest palindromic prefix
    const toAdd = s.substring(lps[combinedStr.length - 1]).split('').reverse().join('');
    return toAdd + s;
}
```

#### Complexity:
- **Time Complexity**: O(n), where n is the length of the string, taking advantage of the KMP algorithm.
- **Space Complexity**: O(n), required for constructing the concatenated string and LPS array. 

This solution efficiently leverages the power of the KMP algorithm, transforming the palindrome detection problem into one of string matching!

