# [Leetcode 214: Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Approaches

- [Basic Approach: Brute Force](#basic-approach-brute-force)
- [Optimized Approach: KMP Algorithm](#optimized-approach-kmp-algorithm)

---

## Basic Approach: Brute Force

### Intuition

The problem asks us to create the shortest palindrome by adding characters to the front of a string. A simple brute force way to create a palindrome is to repeatedly check each prefix of the string and find out if it forms a palindrome extending towards the rest of the string. If not, we keep adding the minimum necessary characters at the front, checking each time if the result is a palindrome.

### Approach

1. Start by reversing the given string `s` to get `rev_s`.
2. If `s` itself is a palindrome, return it.
3. Otherwise, iterate through each index `i` of the string `s`:
   - Form a string with `rev_s.substring(0, i)` appended to the front of the original `s`.
   - Check if this new string is a palindrome.
   - If it is, it means `s.substring(i)` is the largest palindromic prefix.
4. Continue this until a palindrome is formed.

### Code

```java
class Solution {
    public String shortestPalindrome(String s) {
        String rev_s = new StringBuilder(s).reverse().toString();
        int n = s.length();

        // Try to match the reversed string with the original string
        for (int i = 0; i < n; i++) {
            // Check if substring from i to end of rev_s matches start of s
            if (s.substring(0, n - i).equals(rev_s.substring(i))) {
                // Append unmatched part of the reversed string at the front
                return rev_s.substring(0, i) + s;
            }
        }
        return "";
    }
}
```

### Time Complexity

- **Time Complexity**: O(n^2)   
  Each substring comparison takes O(n) time and there are n such attempts.

- **Space Complexity**: O(n)   
  For storing the reversed string and additional substring storage.

---

## Optimized Approach: KMP Algorithm

### Intuition

To reduce the time complexity, we can use the Knuth-Morris-Pratt algorithm (KMP) prefix function. The idea is to find the greatest palindrome prefix in the string using the prefix and suffix match pattern from the reversed string. This helps us quickly determine which part of the string needs to be mirrored from the end.

### Approach

1. Concatenate the string `s` with `#` and its reversed version `rev_s` to create a new string `concat`.
   - The `#` is a delimiter ensuring we don't accidentally match across `s` and `rev_s`.
2. Compute the KMP table (longest prefix that is also a suffix) for this concatenated string.
3. The value at the last index of the KMP table will tell how many characters from the start of `s` form a valid palindrome.
4. The rest of the characters of `s` can be added in reverse at the beginning to form the shortest palindrome.

### Code

```java
class Solution {
    public String shortestPalindrome(String s) {
        String rev_s = new StringBuilder(s).reverse().toString();
        String concat = s + "#" + rev_s;
        
        int n = concat.length();
        int[] kmp = new int[n];
        
        for (int i = 1; i < n; i++) {
            int j = kmp[i - 1];
            
            while (j > 0 && concat.charAt(i) != concat.charAt(j)) {
                j = kmp[j - 1];
            }
            
            if (concat.charAt(i) == concat.charAt(j)) {
                j++;
            }
            
            kmp[i] = j;
        }
        
        int palindromeLen = kmp[n - 1];
        String prefixToAdd = s.substring(palindromeLen);
        return new StringBuilder(prefixToAdd).reverse().toString() + s;
    }
}
```

### Time Complexity

- **Time Complexity**: O(n)  
  The KMP algorithm runs in linear time relative to the length of the concatenated string.

- **Space Complexity**: O(n)  
  Space used for the KMP table and additional reversed string.

---

