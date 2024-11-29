# 214. [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Approach 1: Brute Force Reverse and Check

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
var shortestPalindrome = function(s) {
    // Edge case for empty string
    if (s === "") return s;
    
    // Reverse the string
    let reversedString = s.split("").reverse().join("");
    
    // Try to find a palindrome by checking each position
    for (let i = 0; i < s.length; i++) {
        // Check if the substring of s and reversed s is palindrome
        if (s.startsWith(reversedString.substring(i))) {
            return reversedString.substring(0, i) + s;
        }
    }
    
    return ""; // This will never be executed
};
```

## Approach 2: KMP Algorithm

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
var shortestPalindrome = function(s) {
    let rev = s.split("").reverse().join("");
    let combined = s + "#" + rev;

    // Compute KMP table
    let kmpTable = computeKMPTable(combined);
    
    // The palindrome length from using kmpTable
    let palindromeLength = kmpTable[combined.length - 1];
    
    // Append the part missing to form a palindrome
    return rev.substring(0, rev.length - palindromeLength) + s;
};

function computeKMPTable(s) {
    let kmpTable = new Array(s.length).fill(0);
    let j = 0;
    
    for (let i = 1; i < s.length; i++) {
        while (j > 0 && s.charAt(i) !== s.charAt(j)) {
            j = kmpTable[j - 1];
        }
        if (s.charAt(i) === s.charAt(j)) {
            j++;
        }
        kmpTable[i] = j;
    }
    
    return kmpTable;
}
```

