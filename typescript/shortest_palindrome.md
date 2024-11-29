# 214. [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Approach 1: Brute Force Reverse and Check

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
class Solution {
    shortestPalindrome(s: string): string {
        // Edge case for empty string
        if (s.length === 0) return s;

        // Reverse the string
        const sb = s.split('').reverse().join('');

        // Try to find a palindrome by checking each position
        for (let i = 0; i < s.length; i++) {
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
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    shortestPalindrome(s: string): string {
        const rev = s.split('').reverse().join('');
        const combined = s + "#" + rev;

        // Compute KMP table
        const kmpTable = this.computeKMPTable(combined);

        // The palindrome length from using kmpTable
        const palindromeLength = kmpTable[combined.length - 1];

        // Append the part missing to form a palindrome
        return rev.substring(0, rev.length - palindromeLength) + s;
    }

    private computeKMPTable(s: string): number[] {
        const kmpTable = new Array(s.length).fill(0);
        let j = 0;

        for (let i = 1; i < s.length; i++) {
            while (j > 0 && s[i] !== s[j]) {
                j = kmpTable[j - 1];
            }
            if (s[i] === s[j]) {
                j++;
            }
            kmpTable[i] = j;
        }

        return kmpTable;
    }
}
```

