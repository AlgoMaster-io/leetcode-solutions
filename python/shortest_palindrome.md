# 214. [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Approach 1: Brute Force Reverse and Check

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)
class Solution:
    def shortestPalindrome(self, s: str) -> str:
        # Edge case for empty string
        if not s:
            return s
        
        # Reverse the string
        sb = s[::-1]
        
        # Try to find a palindrome by checking each position
        for i in range(len(s)):
            # Check if the substring of s and reversed sb is a palindrome
            if s.startswith(sb[i:]):
                return sb[:i] + s
        
        return ""  # This will never be executed
```

## Approach 2: KMP Algorithm

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def shortestPalindrome(self, s: str) -> str:
        rev = s[::-1]
        combined = s + "#" + rev

        # Compute KMP table
        kmp_table = self.compute_kmp_table(combined)
        
        # The palindrome length from using kmp_table
        palindrome_length = kmp_table[-1]
        
        # Append the part missing to form a palindrome
        return rev[:len(rev) - palindrome_length] + s

    def compute_kmp_table(self, s: str) -> [int]:
        kmp_table = [0] * len(s)
        j = 0
        
        for i in range(1, len(s)):
            while j > 0 and s[i] != s[j]:
                j = kmp_table[j - 1]
            if s[i] == s[j]:
                j += 1
            kmp_table[i] = j
        
        return kmp_table
```



