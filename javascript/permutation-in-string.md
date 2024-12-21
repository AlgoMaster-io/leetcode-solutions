# Problem: [Leetcode 567: Permutation in String](https://leetcode.com/problems/permutation-in-string/)

## Approaches:
- [Approach 1: Brute Force with Recursion](#approach-1-brute-force-with-recursion)
- [Approach 2: Sliding Window with Frequency Counter](#approach-2-sliding-window-with-frequency-counter)
- [Approach 3: Sliding Window with Optimized Character Count](#approach-3-sliding-window-with-optimized-character-count)

---

## Approach 1: Brute Force with Recursion

### Intuition:
The simplest way to solve this problem is to generate all permutations of the first string `s1` and check each permutation against all substrings of `s2` that are the same length as `s1`. This brute force approach is simple but inefficient due to the factorial time complexity of generating permutations.

### Code:
```javascript
function checkInclusion(s1, s2) {
    const permute = (str) => {
        if (str.length === 0) return [''];
        const permutations = [];
        for (let i = 0; i < str.length; i++) {
            const char = str[i];
            const remaining = str.slice(0, i) + str.slice(i + 1);
            for (const subPermutation of permute(remaining)) {
                permutations.push(char + subPermutation);
            }
        }
        return permutations;
    };

    const permutations = permute(s1);
    const length = s1.length;

    for (let i = 0; i <= s2.length - length; i++) {
        const subString = s2.substr(i, length);
        if (permutations.includes(subString)) return true;
    }
    
    return false;
}
```

### Time Complexity:
- Generating permutations has a time complexity of O(n!), where `n` is the length of `s1`.
- Checking each substring in `s2` takes O(m * n^2), where `m` is the length of `s2` and `n` is the length of `s1`.

### Space Complexity:
- O(n!) for storing all permutations of `s1`.

---

## Approach 2: Sliding Window with Frequency Counter

### Intuition:
Instead of checking each permutation, we can check if a substring of `s2` matches the character frequency of `s1`. We use two frequency counters: one for `s1` and a sliding window counter for tracking frequency in `s2`.

### Code:
```javascript
function checkInclusion(s1, s2) {
    if (s1.length > s2.length) return false;

    const count1 = new Array(26).fill(0);
    const count2 = new Array(26).fill(0);

    for (let i = 0; i < s1.length; i++) {
        count1[s1.charCodeAt(i) - 'a'.charCodeAt(0)]++;
        count2[s2.charCodeAt(i) - 'a'.charCodeAt(0)]++;
    }

    let matches = 0;
    for (let i = 0; i < 26; i++) {
        if (count1[i] === count2[i]) matches++;
    }

    const aCharCode = 'a'.charCodeAt(0);

    for (let i = 0; i < s2.length - s1.length; i++) {
        if (matches === 26) return true;

        let r = s2.charCodeAt(i + s1.length) - aCharCode;
        let l = s2.charCodeAt(i) - aCharCode;

        count2[r]++;
        if (count1[r] === count2[r]) {
            matches++;
        } else if (count1[r] + 1 === count2[r]) {
            matches--;
        }

        count2[l]--;
        if (count1[l] === count2[l]) {
            matches++;
        } else if (count1[l] === count2[l] + 1) {
            matches--;
        }
    }

    return matches === 26;
}
```

### Time Complexity:
- O(m + n) where `m` is the length of `s2` and `n` is the length of `s1`.

### Space Complexity:
- O(1), since we use fixed space for frequency arrays (26 letters).

---

## Approach 3: Sliding Window with Optimized Character Count

### Intuition:
This is an optimized version of the frequency method that iterates over `s2` with a sliding window updating character counts while maintaining a count of matched characters only.

### Code:
```javascript
function checkInclusion(s1, s2) {
    if (s1.length > s2.length) return false;

    const count = new Array(26).fill(0);
    const aCharCode = 'a'.charCodeAt(0);

    for (const char of s1) {
        count[char.charCodeAt(0) - aCharCode]++;
    }

    let left = 0, right = 0;
    let required = s1.length;

    while (right < s2.length) {
        if (count[s2.charCodeAt(right) - aCharCode] > 0) {
            required--;
        }
        count[s2.charCodeAt(right) - aCharCode]--;
        right++;

        if (required === 0) return true;

        if (right - left === s1.length) {
            if (count[s2.charCodeAt(left) - aCharCode] >= 0) {
                required++;
            }
            count[s2.charCodeAt(left) - aCharCode]++;
            left++;
        }
    }

    return false;
}
```

### Time Complexity:
- O(m), where `m` is the length of `s2`.

### Space Complexity:
- O(1), since we use constant space for `count` array.

