# [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Approach 1: Brute Force (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(n * m), where n is the length of s and m is the length of p
// Space Complexity: O(m) for frequency array

function findAnagrams(s: string, p: string): number[] {
    const result: number[] = [];
    const pFreq: number[] = Array(26).fill(0);

    // Frequency count for string p
    for (const c of p) {
        pFreq[c.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }

    // Check every substring of length p in s
    for (let i = 0; i <= s.length - p.length; i++) {
        const sFreq: number[] = Array(26).fill(0);

        // Count frequency of the current substring
        for (let j = i; j < i + p.length; j++) {
            sFreq[s.charAt(j) - 'a'.charCodeAt(0)]++;
        }

        // Compare frequencies
        if (sFreq.every((count, index) => count === pFreq[index])) {
            result.push(i);
        }
    }

    return result;
}
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the length of s
// Space Complexity: O(1) (fixed frequency array of size 26)

function findAnagrams(s: string, p: string): number[] {
    const result: number[] = [];
    const pFreq: number[] = Array(26).fill(0);
    const sFreq: number[] = Array(26).fill(0);

    // Frequency count for string p
    for (const c of p) {
        pFreq[c.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }

    let left = 0, right = 0;

    // Sliding window over s
    while (right < s.length) {
        // Add the current character to the window
        sFreq[s.charCodeAt(right) - 'a'.charCodeAt(0)]++;

        // Check if the window size matches p
        if (right - left + 1 === p.length) {
            // Compare frequencies
            if (sFreq.every((count, index) => count === pFreq[index])) {
                result.push(left);
            }

            // Remove the leftmost character from the window
            sFreq[s.charCodeAt(left) - 'a'.charCodeAt(0)]--;
            left++;
        }

        right++;
    }

    return result;
}
```

