# [Leetcode 792: Number of Matching Subsequences](https://leetcode.com/problems/number-of-matching-subsequences/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using HashMap](#approach-2-using-hashmap)
- [Approach 3: Optimized Using Buckets](#approach-3-optimized-using-buckets)

## Approach 1: Brute Force

### Intuition
The problem presents us with a string `s` and a list of words. We need to determine how many of these words are subsequences of `s`. A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

The simplest approach is to iterate over each word and check if it is a subsequence of `s`. We can do this by using a two-pointer technique: one pointer traversing the string `s` and the other traversing the current word.

### Implementation

```javascript
function isSubsequence(s, word) {
    let i = 0; // pointer for the string 's'
    for (let char of word) {
        while (i < s.length && s[i] !== char) {
            i++; // increment until we find a matched character
        }
        if (i === s.length) {
            return false; // if we exhaust 's', return false
        }
        i++;
    }
    return true;
}

function numMatchingSubseq(s, words) {
    let count = 0;
    for (let word of words) {
        if (isSubsequence(s, word)) {
            count++; // increment count if word is a subsequence
        }
    }
    return count;
}
```

### Time Complexity
- **O(n * m):** where `n` is the length of `s` and `m` is the total number of characters across all words in `words`. Each word is checked sequentially against `s`.
    
### Space Complexity
- **O(1):** We only use pointers and counters, requiring constant space.

---

## Approach 2: Using HashMap

### Intuition
Instead of checking the subsequence for each word independently, we can leverage a shared data structure to optimize. For every character in `s`, we record where they occur in `s`. We can then use this information to check subsequences more efficiently.

Store all the indices in the string `s` where each character occurs. As we process each word, find the position for each character in the word using these indices.

### Implementation

```javascript
function numMatchingSubseq(s, words) {
    const map = new Map();

    // Populate the map with the positions of each character in 's'
    for (let i = 0; i < s.length; i++) {
        if (!map.has(s[i])) map.set(s[i], []);
        map.get(s[i]).push(i);
    }

    function isSubsequence(word) {
        let prevIndex = -1; // initialize previous index to -1
        for (let char of word) {
            if (!map.has(char)) return false; // if char not in 's'

            // binary search to find the smallest index >= prevIndex
            const indices = map.get(char);
            let left = 0, right = indices.length;
            while (left < right) {
                let middle = Math.floor((left + right) / 2);
                if (indices[middle] > prevIndex) right = middle;
                else left = middle + 1;
            }
            
            if (left === indices.length) return false; // if no valid index found
            prevIndex = indices[left];
        }
        return true; // if all characters matched in sequence
    }
    
    let count = 0;
    for (let word of words) {
        if (isSubsequence(word)) {
            count++;
        }
    }
    return count;
}
```

### Time Complexity
- **O(n + m * log(L)):** where `L` is the maximum number of occurrences of any character in `s`, and `m` is the number of characters across all words.
    
### Space Complexity
- **O(n):** for storing indices of each character in `s`.

---

## Approach 3: Optimized Using Buckets

### Intuition
Using a bucket method: where we enqueue possible word completions by the first character needed and advance them efficiently when a character from `s` is seen.

We create "buckets" for each character in `s` that store iterators processing the respective words that are pending to match the current character.

### Implementation

```javascript
function numMatchingSubseq(s, words) {
    const map = new Map();
    
    for (let word of words) {
        const firstChar = word[0];
        if (!map.has(firstChar)) map.set(firstChar, []);
        map.get(firstChar).push(word);
    }
    
    let count = 0;
    for (let char of s) {
        const currentWords = map.get(char) || [];
        map.set(char, []);

        for (let word of currentWords) {
            if (word.length === 1) {
                count++; // we have a full match
            } else {
                const nextChar = word[1];
                if (!map.has(nextChar)) map.set(nextChar, []);
                map.get(nextChar).push(word.slice(1));
            }
        }
    }
    
    return count;
}
```

### Time Complexity
- **O(n + m):** We process each character in `s` once, and each word in `words` once as well.

### Space Complexity
- **O(m):** As we may need to store the words across the buckets.

Through these approaches, you can start with a simple technique and progress to more optimized solutions as you understand the problem deeply.

