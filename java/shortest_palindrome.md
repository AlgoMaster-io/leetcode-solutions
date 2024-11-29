# 214. [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Approach 1: Brute Force Reverse and Check

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(n)
public class Solution {
    public String shortestPalindrome(String s) {
        // Edge case for empty string
        if (s.isEmpty()) return s;
        
        // Reverse the string
        StringBuilder sb = new StringBuilder(s);
        sb.reverse();
        
        // Try to find a palindrome by checking each position
        for (int i = 0; i < s.length(); i++) {
            // Check if the substring of s and reversed s is palindrome
            if (s.startsWith(sb.substring(i))) {
                return sb.substring(0, i) + s;
            }
        }
        
        return ""; // This will never be executed
    }
}
```

## Approach 2: KMP Algorithm

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
public class Solution {
    public String shortestPalindrome(String s) {
        String rev = new StringBuilder(s).reverse().toString();
        String combined = s + "#" + rev;

        // Compute KMP table
        int[] kmpTable = computeKMPTable(combined);
        
        // The palindrome length from using kmpTable
        int palindromeLength = kmpTable[combined.length() - 1];
        
        // Append the part missing to form a palindrome
        return rev.substring(0, rev.length() - palindromeLength) + s;
    }

    private int[] computeKMPTable(String s) {
        int[] kmpTable = new int[s.length()];
        int j = 0;
        
        for (int i = 1; i < s.length(); i++) {
            while (j > 0 && s.charAt(i) != s.charAt(j)) {
                j = kmpTable[j - 1];
            }
            if (s.charAt(i) == s.charAt(j)) {
                j++;
            }
            kmpTable[i] = j;
        }
        
        return kmpTable;
    }
}
```



