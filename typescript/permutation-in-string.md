# [Leetcode 567: Permutation in String](https://leetcode.com/problems/permutation-in-string/)

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Sliding Window with Frequency Count](#sliding-window-with-frequency-count)
3. [Optimal Sliding Window](#optimal-sliding-window)

### Brute Force Approach

#### Intuition:

The brute force approach involves generating all possible permutations of the smaller string `s1` and checking if any of these permutations is a substring of the larger string `s2`. 

#### Code:

```typescript
function isPermutation(s1: string, s2: string): boolean {
    if (s1.length > s2.length) return false;

    const permute = (s: string): string[] => {
        if (s.length === 1) return [s];
        const permutations: string[] = [];
        for (let i = 0; i < s.length; i++) {
            const char = s[i];
            const remaining = s.slice(0, i) + s.slice(i + 1);
            for (const perm of permute(remaining)) {
                permutations.push(char + perm);
            }
        }
        return permutations;
    };

    const s1Permutations = new Set(permute(s1));
    for (let i = 0; i <= s2.length - s1.length; i++) {
        const substring = s2.substring(i, i + s1.length);
        if (s1Permutations.has(substring)) {
            return true;
        }
    }
    return false;
}
```

#### Complexity:

- **Time Complexity:** O(n! * m * m), where n is the length of `s1` (for generating permutations), and m is the length of `s2` (for checking each substring).
- **Space Complexity:** O(n!), for storing permutations.

---

### Sliding Window with Frequency Count

#### Intuition:

We check if a permutation of `s1` exists in `s2` by using a frequency array for characters. We slide over `s2` with a window of size `s1` and compare the frequency of characters in the window to the frequency of characters in `s1`.

#### Code:

```typescript
function isPermutation(s1: string, s2: string): boolean {
    if (s1.length > s2.length) return false;

    const count1 = new Array(26).fill(0);
    const count2 = new Array(26).fill(0);
    
    const aCharCode = 'a'.charCodeAt(0);

    for (let i = 0; i < s1.length; i++) {
        count1[s1.charCodeAt(i) - aCharCode]++;
        count2[s2.charCodeAt(i) - aCharCode]++;
    }

    let matches = 0;
    for (let i = 0; i < 26; i++) {
        if (count1[i] == count2[i]) {
            matches++;
        }
    }

    let left = 0;
    for (let right = s1.length; right < s2.length; right++) {
        if (matches === 26) return true;

        const index = s2.charCodeAt(right) - aCharCode;
        count2[index]++;
        if (count1[index] === count2[index]) {
            matches++;
        } else if (count1[index] + 1 === count2[index]) {
            matches--;
        }
        
        const leftIndex = s2.charCodeAt(left) - aCharCode;
        count2[leftIndex]--;
        if (count1[leftIndex] === count2[leftIndex]) {
            matches++;
        } else if (count1[leftIndex] - 1 === count2[leftIndex]) {
            matches--;
        }
        left++;
    }
    return matches === 26;
}
```

#### Complexity:

- **Time Complexity:** O(n + m * 26), where n is the length of `s1` and m is the length of `s2`. 
- **Space Complexity:** O(1), since the frequency array size is fixed.

---

### Optimal Sliding Window

#### Intuition:

We can further optimize by reducing the number of comparisons for character frequencies. Instead of maintaining a `matches` count, we can directly compare the frequency arrays only when necessary by sliding over the window in `s2`.

#### Code:

```typescript
function isPermutation(s1: string, s2: string): boolean {
    if (s1.length > s2.length) return false;
    
    const count1 = new Array(26).fill(0);
    const count2 = new Array(26).fill(0);
    
    const aCharCode = 'a'.charCodeAt(0);

    for (const char of s1) {
        count1[char.charCodeAt(0) - aCharCode]++;
    }

    let left = 0;
    for (let right = 0; right < s2.length; right++) {
        const index = s2.charCodeAt(right) - aCharCode;
        count2[index]++;
        
        const windowSize = right - left + 1;
        if (windowSize == s1.length) {
            if (count1.every((val, idx) => val === count2[idx])) {
                return true;
            }
            // Slide the window
            const leftIndex = s2.charCodeAt(left) - aCharCode;
            count2[leftIndex]--;
            left++;
        }
    }
    return false;
}
```

#### Complexity:

- **Time Complexity:** O(m), where m is the length of `s2` because we are sliding through `s2` only once.
- **Space Complexity:** O(1), since the frequency array size is fixed.

---

This set of solutions deals with checking for permutations using progressively efficient approaches, from brute force to a more optimized sliding window technique with constant space complexity.

