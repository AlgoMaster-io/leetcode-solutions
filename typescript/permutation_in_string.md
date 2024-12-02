# [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)

## Approach 1: Brute Force (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(n * m), where n is the length of s1 and m is the length of s2
// Space Complexity: O(n)
function checkInclusion(s1: string, s2: string): boolean {
    const n = s1.length, m = s2.length;

    if (n > m) return false;

    // Sort s1
    const sortedS1 = s1.split('').sort().join('');

    // Check every substring of length n in s2
    for (let i = 0; i <= m - n; i++) {
        const sub = s2.substring(i, i + n);
        const sortedSub = sub.split('').sort().join('');

        if (sortedS1 === sortedSub) {
            return true;
        }
    }

    return false;
}
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(m), where m is the length of s2
// Space Complexity: O(1)
function checkInclusion(s1: string, s2: string): boolean {
    if (s1.length > s2.length) return false;

    const s1Freq: number[] = new Array(26).fill(0);
    const s2Freq: number[] = new Array(26).fill(0);

    // Frequency count for s1
    for (const c of s1) {
        s1Freq[c.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }

    // Sliding window
    for (let i = 0; i < s2.length; i++) {
        s2Freq[s2.charAt(i).charCodeAt(0) - 'a'.charCodeAt(0)]++;

        // Remove the leftmost character from the window
        if (i >= s1.length) {
            s2Freq[s2.charAt(i - s1.length).charCodeAt(0) - 'a'.charCodeAt(0)]--;
        }

        // Compare the frequency arrays
        if (s1Freq.toString() === s2Freq.toString()) {
            return true;
        }
    }

    return false;
}
```

