[Problem: Leetcode 438 - Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two-pointer with Frequency Count (Sliding Window)](#approach-2-two-pointer-with-frequency-count-sliding-window)
- [Approach 3: Optimized Sliding Window using Hashing](#approach-3-optimized-sliding-window-using-hashing)

---

### Approach 1: Brute Force

The simplest way to solve this problem is to generate all possible substrings of `s` that are the same length as `p` and check if these substrings are anagrams of `p`.

**Intuition:**
1. Brute force through every substring of length `p`.
2. For each substring, compare it against `p` to determine if it is an anagram by sorting and comparing equality.
3. Return the start indices of the substrings that are anagrams.

```typescript
function findAnagrams(s: string, p: string): number[] {
    const result: number[] = [];
    const pSorted = p.split('').sort().join('');

    // Traverse 's' up to the point where a substring of length 'p' can fit
    for (let i = 0; i <= s.length - p.length; i++) {
        const substring = s.substring(i, i + p.length);
        if (substring.split('').sort().join('') === pSorted) {
            result.push(i); // If sorted substring is the same as sorted 'p', it's an anagram
        }
    }
    
    return result;
}
```

**Time Complexity:** O(n * m log m) where `n` is the length of `s` and `m` is the length of `p`.

**Space Complexity:** O(m) for storing sorted characters of `p`.

---

### Approach 2: Two-pointer with Frequency Count (Sliding Window)

Instead of checking each substring independently, we can maintain a frequency count of characters. Use a sliding window to update the state as you iterate through `s`.

**Intuition:**
1. Keep a frequency count of characters in `p`.
2. As you iterate through `s`, keep a window (of size `p`) and adjust the frequency counts.
3. If the window's frequency matches `p`, it is an anagram.

```typescript
function findAnagrams(s: string, p: string): number[] {
    const result: number[] = [];
    const pCount = Array(26).fill(0);
    const sCount = Array(26).fill(0);
    
    // Populate character frequency array for p
    for (const char of p) {
        pCount[char.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }

    // Sliding window over s
    for (let i = 0; i < s.length; i++) {
        // Add current character to sCount
        sCount[s[i].charCodeAt(0) - 'a'.charCodeAt(0)]++;
        if (i >= p.length) {
            // Remove the character that's sliding out of the window
            sCount[s[i - p.length].charCodeAt(0) - 'a'.charCodeAt(0)]--;
        }
        
        // Compare frequency arrays
        if (pCount.toString() === sCount.toString()) {
            result.push(i - p.length + 1); // Add starting index of the current window
        }
    }
    
    return result;
}
```

**Time Complexity:** O(n + m) where `n` is the length of `s` and `m` is the length of `p`.

**Space Complexity:** O(1) since we use fixed arrays for character counts.

---

### Approach 3: Optimized Sliding Window using Hashing

This approach optimizes the frequency match check without explicitly comparing arrays as strings.

**Intuition:**
1. Initialize frequency arrays as before.
2. Maintain a count of matches between the current window and `p`.
3. Only update match count when we change character frequencies.

```typescript
function findAnagrams(s: string, p: string): number[] {
    const result: number[] = [];
    if (s.length < p.length) return result;
    
    const pCount = Array(26).fill(0);
    const sCount = Array(26).fill(0);
    let matches = 0;
    
    // Populate pCount array
    for (const ch of p) {
        pCount[ch.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }
    
    // Initial window population
    for (let i = 0; i < p.length; i++) {
        const index = s[i].charCodeAt(0) - 'a'.charCodeAt(0);
        sCount[index]++;
    }
    
    // Initial match comparison
    for (let i = 0; i < 26; i++) {
        if (pCount[i] === sCount[i]) {
            matches++;
        }
    }

    // Slide the window
    for (let i = 0; i < s.length - p.length; i++) {
        if (matches === 26) {
            result.push(i);
        }
        
        const charToRemoveIndex = s[i].charCodeAt(0) - 'a'.charCodeAt(0);
        const charToAddIndex = s[i + p.length].charCodeAt(0) - 'a'.charCodeAt(0);
        
        // Remove the char going out of the window
        sCount[charToRemoveIndex]--;
        if (sCount[charToRemoveIndex] === pCount[charToRemoveIndex]) {
            matches++;
        } else if (sCount[charToRemoveIndex] + 1 === pCount[charToRemoveIndex]) {
            matches--;
        }
        
        // Add the char coming into the window
        sCount[charToAddIndex]++;
        if (sCount[charToAddIndex] === pCount[charToAddIndex]) {
            matches++;
        } else if (sCount[charToAddIndex] - 1 === pCount[charToAddIndex]) {
            matches--;
        }
    }
    
    // Check the last window
    if (matches === 26) {
        result.push(s.length - p.length);
    }
    
    return result;
}
```

**Time Complexity:** O(n) where `n` is the length of `s`.

**Space Complexity:** O(1) as we are using fixed-size arrays for character counts.

